---
title: 建立Python多版本隔离开发环境
tags: Python
---

在日常使用的时候，可能需要不同的*python*版本或者不同的依赖库，那解决方法就是可以使用 pyenv 来建立多个虚拟环境，在虚拟环境中可以使用不同的*python*版本和依赖库。

<!--more-->

## 安装

[官方安装指南](https://link.jianshu.com/?t=https://github.com/pyenv/pyenv#installation)

我们需要安装2个工具，*pyenv*和*pyenv-virtualenv*。

*pyenv*是让不同版本*python*共存，*pyenv-virtualenv*是 *pyenv*的插件，解决在不同项目所依赖的软件包之间可能产生冲突的问题。在实际使用*python*时，经常出现这种情况：

> 通过`pip`安装软件包 A 时安装了 A 所依赖的软件包 B；之后又通过`pip`安装软件包 C 时再次安装了 B 并将之前的覆盖，但是因为 C 和 A 所依赖的 B 版本不同，安装完 C 后导致 A 无法运行。

*pyenv-virtualenv* 可以为每个项目创建单独的虚拟环境来解决上面的问题。

在Mac上使用 [Homebrew](https://brew.sh) 进行安装：

```shell
$ brew install pyenv
$ brew install pyenv-virtualenv
```

## 使用

安装需要的python版本：

```shell
$ pyenv install 3.6.1
```

创建一个虚拟环境：

```shell
$ pyenv virtualenv 3.6.1 mpython-3.6.1
```

直接进入需要使用虚拟环境的目录，可以手动打开和退出虚拟环境：

```shell
$ pyenv activate mpython-3.6.1
$ pyenv deactivate
```

打开后所有的操作就是在这个环境下进行的了，但是每次都手动打开比较麻烦，我们想每次进入文件夹后自动切换，添加`pyenv virtualenv-init`到*shell*的配置文件中：

```shell
$ echo 'eval "$(pyenv init -)"' >> ~/.bash_profile
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bash_profile
```

**注意**：如果是zsh用**~/.zshenv**或者**~/.zshrc**，而不是**~/.bash_profile**。不同系统会有区别注意修改

接着就直接在目录下创建一个`.python-version`的文本文件将虚拟环境名称（如`mpython-3.6.1`）写在里面，就可以自动打开与关闭了。

## 常用命令

```shell
$ pyenv install --list                            #列出可供安装的 python 版本
$ pyenv install <version>                         #安装指定版本的 python
$ pyenv local <version>                           #在当前目录下设置 python 版本
$ pyenv versions                                  #列出系统中安装的 python 版本
$ pyenv version                                   #显示当前目录下采用的 python 版本
$ pyenv virtualenv [version] <venv-name>          #创建虚拟环境
$ pyenv activate <venv-name>                      #打开虚拟环境
$ pyenv deactivate                                #退出虚拟环境
```

## 有问题看这里

我遇到的问题：

一定要注意终端用的是什么，我应该配置到**~/.zshenv**。

有其他问题参考：

[Python版本管理：pyenv和pyenv-virtualenv(MAC、Linux)、virtualenv和virtualenvwrapper(windows)](https://www.jianshu.com/p/60f361822a7e)

[使用pyenv与pyenv-virtualenv管理python环境](https://www.jianshu.com/p/9f6a9456ad5f)

