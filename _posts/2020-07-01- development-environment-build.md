---
title: Jetson Nano 开发环境搭建
tags: JetsonNano 
aside:
  toc: true
---
此处记录 Nano 开发环境搭建时候遇到的问题。
<!--more-->

## （一）系统安装

先上[Nvidia官方教程](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit)

### 0 准备工作

开始前需要准备

- 一个大于16GB的TF卡
- HDMI或DP显示器
- USB键盘和鼠标
- 5v⎓2A Micro-USB电源

### 1 烧写镜像到TF卡

下载[镜像](https://developer.nvidia.com/jetson-nano-sd-card-image)，烧写过程与树莓派是相同的，先格式化SD卡，使用 [Etcher](https://www.balena.io/etcher) 烧写镜像。详细的烧写过程参照[官方教程](https://developer.nvidia.com/embedded/learn/get-started-jetson-nano-devkit#write)，Windows，Mac和Linux都有说明。

烧写完成后，将SD卡插入 Nano，接上鼠标键盘和显示器，开机，配置时区，语言，输入法，就大功告成了。

![Jetbot_animation](https://developer.nvidia.com/sites/default/files/akamai/embedded/images/jetsonNano/gettingStarted/Jetbot_animation_500x282_2.gif)



## （二）跟换软件源

**更改软件源有时会遇到软件版本问题，不是必要的情况下就不更换软件源。**

### 0 备份 source.list 文件

```shell
$ sudo cp sources.list sources.list.bak
```

### 1 对 source.list 文件进行修改

直接清空source.list文件内容，根据个人喜好选择下述中科大或者清华的arm64源，粘进文件，保存。（**Note**：ARM源和一般源不同，需要将地址中的ubuntu改为ubuntu-ports）

```
中科大

deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe

deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-proposed main multiverse restricted universe

deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe

deb http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe

deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe

deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe

deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-proposed main multiverse restricted universe

deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe

deb-src http://mirrors.ustc.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe
```



```
清华

deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe

deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe

deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe

deb http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe

deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial main multiverse restricted universe

deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-security main multiverse restricted universe

deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-updates main multiverse restricted universe

deb-src http://mirrors.tuna.tsinghua.edu.cn/ubuntu-ports/ xenial-backports main multiverse restricted universe
```

### 2 更新源

```shell
$ sudo apt update
$ sudo apt upgrade
```



## （三）SSH远程登录

### 1 确定自己板子的IP地址

方法1：进入系统后在终端中输入 `ifconfig` 

方法2：直接网线连接自己的电脑cmd命令行输入 `arp -a`，注意设置网络共享

### 2 使用putty连接Nano

[PuTTY下载地址](https://www.chiark.greenend.org.uk/~sgtatham/putty/)，默认系统是开启了ssh服务的，输入上面查询到的ip地址，端口号22



## （四）安装组件

根据需要安装所需的组件

### 0 测试已安装组件

### 测试OpenCV

JetPack4.4中opencv已经升级到了4，使用下面命令检测是否安装

```shell
$ pkg-config opencv4 --modversion
```

测试Python3

```shell
$ python3 -V
```

### 1 安装pip

使用pip来下载安装和管理pyhton的各种第三方库就非常方便了，后续在python上安装一般都通过pip

```shell
# 安装pip
$ sudo apt install python3-pip

# 升级pip
$ pip3 install –upgrade pip
```



### 2 安装 JupyterLab

[官方文档](https://jupyterlab.readthedocs.io/en/stable/index.html)

```bash
# 安装依赖
$ sudo apt install nodejs npm

# pip安装jupyter lab
$ pip3 install jupyter jupyterlab
```

安装 jupyter lab 中提示错误：

```shell
c/_cffi_backend.c:15:10: fatal error: ffi.h: No such file or directory
	#include <ffi.h>
			^~~~~~~
	compilation terminated.
	error: command 'aarch64-linux-gnu-gcc' failed with exit status 1
```

解决方法，将缺少的组件安装上：

```shell
$ sudo apt install libffi-dev
```

安装完成没有错误后，重启

```shell
$ sudo reboot
```

Jupyter lab 已经安装好了，在命令行输入 `jupyter lab`，即可启动，在浏览器输入 localhost:8888 就可以访问 Jupyter lab了，但只能在本机访问，局域网其他主机还不能访问。

先关闭已经启动的 Jupyter lab 进程，在当前终端窗口按 `ctrl+c`，再执行：

```shell
$ jupyter lab --generate-config
```

会看到如下输出信息，会提示配置文件保存路径，x-robot是用户名：

```shell
Writing default config to: /home/x-robot/.jupyter/jupyter_notebook_config.py
```

编辑上面的配置文件：

```shell
$ vi /home/x-robot/.jupyter/jupyter_notebook_config.py
```

找到下面两项，去掉前面的#，并修改成下面的参数

```shell
...
c.NotebookApp.allow_origin = '*' # allow all origins 48行
...
c.NotebookApp.ip = '0.0.0.0' # listen on all IPs 207行
```

设置密码

```shell
$ jupyter notebook password
[NotebookPasswordApp] Wrote hashed password to /home/x-robot/.jupyter/jupyter_notebook_config.json
```

设置成功会提示密码已经保存到配置文件中 `/home/x-robot/.jupyter/jupyter_notebook_config.json`

这样就配置完成了，在终端中启动 `jupyter lab`，现在局域网主机也可以访问了，在浏览器输入 ip 地址加端口8888就可以访问了。

最后一步添加自启动，没有自启动每次开机后都得手动开启，先确定安装位置:

```shell
$ which jupyter
/home/x-robot/.local/bin/jupyter
```

创建 jupyter.service 文件

```shell
$ sudo vi /etc/systemd/system/jupyter.service
```

填入如下文件内容，注意修改 `User`、`ExesStart` 路径，并保存。

```shell
[Unit]
Description=Jupyter Notebook

[Service]
Type=simple
User=x-robot
ExecStart=/home/x-robot/.local/bin/jupyter-lab

[Install]
WantedBy=default.target
```

启动服务

```shell
$ sudo systemctl enable jupyter
$ sudo systemctl start jupyter
```

检查服务是否正常

```shell
$ sudo systemctl status jupyter
```





