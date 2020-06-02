---
title: 搭建我的学习站
tags: Jekyll TeXt
---

为了能够记录下学习时遇到的问题和收获，也为了以后碰到相同问题可以快速解决，于是这个网站就应运而生了。

那在搭建这个网站的时候踩了挺多坑的，正好先将搭建的方式做一个记录。

<!--more-->

## 简单了解

一开始想使用简书来进行记录，偶然的机会了解到使用 [GitHub Pages](https://pages.github.com) + [jekyll](http://jekyllcn.com) 可以搭建静态网站，于是决定尝试下。使用 [GitHub Pages](https://pages.github.com) + [jekyll](http://jekyllcn.com) 可以快速的搭建出项目页面、博客或者网站，而且依托GitHub的服务器是**完全免费**的。

在开始搭建之前最好先学习一定的 Web 编程知识（后面也会进行总结），推荐 [W3School](https://www.w3school.com.cn) 很全面的 Web 技术学习网站。

推荐一篇写的很详细的搭建教程 [《快速搭建个人博客》](http://qiubaiying.vip/2017/02/06/快速搭建个人博客/)。

写文章我使用的是 Markdown，语法的学习看的是[*Markdown*—入门指南 ](https://www.jianshu.com/p/1e402922ee32)。

Github的使用可以看[Git和Github使用教程]({% post_url 2020-06-01-Git %})。

## 正式开始

### 1.克隆仓库

模版我选择的是 **[TeXt](https://github.com/kitian616/jekyll-TeXt-theme)**，整个的文档也写非常的详细，[文档戳这里](https://tianqi.name/jekyll-TeXt-theme/docs/en/quick-start)，跟着文档也能轻松完成搭建。我选择直接Clone仓库，有一堆文件，这些文件都是干什么的可以查看jekyll文档：[目录结构](https://www.jekyll.com.cn/docs/structure/)。

### 2.修改仓库

Clone的文件中还有一些是我们自己网站并不需要的文件，我们可以只保留有用的文件，参照[我的学习站]()。再根据前面的几篇文档修改`_config.yml`文件。

### 3.预览网站

修改完成后，我们如果要预览自己的网站需要安装一个开发环境，也可以跳过这个步骤，安装的详细操作可以看[jekyll快速入门](https://www.jekyll.com.cn/docs/)。

当我们开启`jekyll serve`服务时会自动跟踪我们的修改，意思就是我们修改完我们的网站保存文件后直接刷新我们的界面就行了，但是更新并没有那么快多刷新几次就好了。

### 4.Push仓库

将网站仓库推送到我们自己的GitHub仓库，仓库名要与自己的用户名相同。

### 5.开始码字

修改完后我们就可以开始码字了，注意点是我们的文章都要放在`_posts` 的文件夹中，文件的命名使用时间+标题`2020-05-11-my-learn-site.md`。

