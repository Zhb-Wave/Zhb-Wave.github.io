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
$ sudo apt-get update
$ sudo apt-get upgrade
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

测试OpenCV

JetPack4.4中opencv已经升级到了4，使用下面命令检测是否安装

```shell
$ pkg-config opencv4 --modversion
```

测试Python3

```shell
$ python3 -V
```



