---
title: WSL2移除PATH中Windows的环境变量
tags: wsl
---

<!--more-->

### 没有修改前，wsl启动查看`PATH`变量内容如下，包含大量的`/mnt/c`这些都是Windows系统的`PATH`变量

![windows path](https://github.com/Zhb-Wave/Zhb-Wave.github.io/tree/master/assets/images/windows_path.png)

### 可以通过修改`wsl.conf`文件来保持wsl系统环境变量的干净

```bash
$ sudo vim /etc/wsl.conf
```

再`wsl.conf`中添加

```bash
# 不加载Windows中的PATH内容
[interop]
enabled=false
appendWindowsPath = false

# 不自动挂载Windows系统所有磁盘分区
[automount]
enabled = false
```

然后保持并退出，这时候还没有生效还需要在windows下执行，以管理员身份运行`powershell`

```bash
wsl --terminate Ubuntu-18.04
```

<div style="border:2px solid; font-size:12px; padding:8px; margin-top: auto;">
<i>
    <h4><i>Tip：</i></h4>
    Ubuntu-18.04 替换成安装的版本
</i>
</div>