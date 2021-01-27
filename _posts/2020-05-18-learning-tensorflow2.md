---
title: TensorFlow2学习与实践
tags: TensorFlow2
---

记录我学习*TensorFlow2*时遇到的问题与解决方法。学习可以看coursera上的[TensorFlow in Practice 专项课程](https://www.coursera.org/specializations/tensorflow-in-practice)（学习中）。

<!--more-->

## 0 安装TF2

*TensorFlow 2*的安装还是比较方便的，直接使用*pip*安装就可以了。最好使用虚拟*python*环境，与系统*python*隔离开来。虚拟环境有*pyenv*、*virtualenv*和*Docker*选择自己喜欢的用就好了。官方的安装教程写的很详细了，跟着操作就不会出问题。[官方安装链接](https://tensorflow.google.cn/install)

```shell
$ pip install --upgrade pip
$ pip install tensorflow
```

**注意**：系统需要64位（都是血和泪），*python*>3.5。

## 1 初体验

安装好后，可以先跑个[MNIST例子](https://tensorflow.google.cn/tutorials/quickstart/beginner)试试安装是否正确：

```python
import tensorflow as tf

mnist = tf.keras.datasets.mnist

(x_train, y_train), (x_test, y_test) = mnist.load_data()
x_train, x_test = x_train / 255.0, x_test / 255.0

model = tf.keras.models.Sequential([
  tf.keras.layers.Flatten(input_shape=(28, 28)),
  tf.keras.layers.Dense(128, activation='relu'),
  tf.keras.layers.Dropout(0.2),
  tf.keras.layers.Dense(10, activation='softmax')
])

model.compile(optimizer='adam',
              loss='sparse_categorical_crossentropy',
              metrics=['accuracy'])

model.fit(x_train, y_train, epochs=5)

model.evaluate(x_test,  y_test, verbose=2)
```

运行结果：

```shell
Epoch 1/5
1875/1875 [==============================] - 3s 2ms/step - loss: 0.3005 - accuracy: 0.9135
Epoch 2/5
1875/1875 [==============================] - 3s 2ms/step - loss: 0.1435 - accuracy: 0.9584
Epoch 3/5
1875/1875 [==============================] - 3s 2ms/step - loss: 0.1079 - accuracy: 0.9673
Epoch 4/5
1875/1875 [==============================] - 3s 2ms/step - loss: 0.0870 - accuracy: 0.9729
Epoch 5/5
1875/1875 [==============================] - 3s 2ms/step - loss: 0.0747 - accuracy: 0.9770
313/313 - 0s - loss: 0.0746 - accuracy: 0.9763
```

如果顺利就会输出上面结果，虽然现在还不知道在干什么。

### 我遇到的问题

由于网络问题`mnist.load_data()`无法下载[MNIST 数据集](http://yann.lecun.com/exdb/mnist/)。解决方法，将数据集下载下来[链接](https://github.com/guangfuhao/Deeplearning/blob/master/mnist.npz)，从本地加载数据集。

```python
import numpy as np

def load_data(path):
  with np.load(path, allow_pickle=True) as f:
    x_train, y_train = f['x_train'], f['y_train']
    x_test, y_test = f['x_test'], f['y_test']
    return (x_train, y_train), (x_test, y_test)

(x_train, y_train), (x_test, y_test) = load_data("./datasets/mnist.npz")
```

## 2 深度学习基础

### 什么是机器学习？

我的理解是计算机从提供的数据中总结经验（模型），并利用经验对未知的数据进行预测。

> Tom Mitchell provides a more modern definition: "A computer program is said to learn from experience E with respect to some class of tasks T and performance measure P, if its performance at tasks in T, as measured by P, improves with experience E."

