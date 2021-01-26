---
title: Gitbook的使用教程
tags: gitbook
aside:
  toc: true
---

### 简介

GitBook 是使用 GitHub / Git 和 Markdown 构建漂亮书籍的命令行工具（和Node.js库）。GitBook 可以将我们书写的文档作为网站或电子书（PDF，ePub或Mobi）输出。还有 [GitBook.com](https://link.zhihu.com/?target=https%3A//www.gitbook.com/) 网站是使用 GitBook 格式创建和托管图书的在线平台。

<!--more-->

### GitBook安装

gitbook是一个基于Node.js的命令行工具，所以要先安装[Node.js](https://nodejs.org/zh-cn/download/releases/)，找到对应平台的版本安装即可。(Node.js 版本不要使用最新，最新版可能不支持，测试使用v10.23.1版本)

Node.js都会默认安装npm（node包管理工具），所以不需要单独安装npm，打开命令行，执行以下命令安装GitBook:

```bash
npm install -g gitbook-cli
```

### 安装 Markdown 编辑工具

我比较习惯使用 [Typora](https://www.typora.io/) ，使用其他编辑工具也是可以的，比如，[Mou](http://25.io/mou/)，[MarkdownPad](http://markdownpad.com/)，[Atom](https://atom.io/)，[Cmd Markdown](https://www.zybuluo.com/mdeditor)。

### Gitbook的使用

gitbook 的基本用法非常简单，先打开命令行(cmd)，cd到文档的文件夹下，再执行命令：

```bash
$ gitbook init
```

执行后会在目录中创建2个文件 README.md（书籍的介绍）和 SUMMARY.md（书籍的目录结构），打开SUMMARY.md 编写一个简单的目录

```markdown
# Summary

* [Introduction](README.md)
* [前言](readme.md)
* [第一章](part1/README.md)
    * [第一节](part1/1.md)
    * [第二节](part1/2.md)
    * [第三节](part1/3.md)
    * [第四节](part1/4.md)
* [第二章](part2/README.md)
* [第三章](part3/README.md)
* [第四章](part4/README.md)
```

写完目录再次执行`gitbook init` ，gitbook 会为我们创建 SUMMARY.md 中的目录结构。然后就可以编写文章中的内容了。

### 生成书籍

目录创建完后可以使用`gitbook serve` 来预览，执行后会把Markdown格式转换为html格式，打开浏览器访问 http://localhost:4000，就可以看到这本书了

```bash
$ gitbook serve
Press CTRL+C to quit ...

Live reload server started on port: 35729
Starting build ...
Successfully built!

Starting server ...
Serving book on http://localhost:4000
```

### 使用Pages服务托管书籍

Pages服务简单说就是一个用来托管静态网站的。gitbook 可以将文档生成为HTML静态页面，所以可以直接将构建好的书籍放到代码仓库。

#### 在仓库中构建书籍

首先，我们先新建一个doc仓库：

```bash
$ mkdir doc
$ cd doc
$ git init
```

然后，我们根据上面的方法创建书籍：

```bash
$ gitbook init
```

接着，我们使用 `gitbook build` 将书籍进行编译，会看到生成了一个_book的目录，这个目录中存放的就是静态网页。

```bash
$ gitbook build
```

#### 创建 gh-pages 分支

下面我们创建 gh-pages 的分支，并删除不需要的文件：

```bash
$ git checkout --orphan gh-pages
$ git rm --cached -r .
$ git clean -df
$ rm -rf *~
```

我们的目录下应该剩下了 _book 目录，先编写 `.gitignore` 文件，忽略一些文件：

```bash
$ echo "*~" > .gitignore
$ echo "_book" >> .gitignore
$ git add .gitignore
$ git commit -m "update gitignore"
```

然后，将 _book 中的内容添加到分支中：

```bash
$ cp -r _book/* .
$ git add .
$ git commit -m "rebuild pages"
```

最后将修改上传到仓库。