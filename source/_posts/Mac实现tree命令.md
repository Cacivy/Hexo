---
title:  Mac实现tree命令
date: 2016-07-14
---

tree命令可以很方便的展示目录结构，但是macos并不支持，这时我们需要安装这个命令

首先需要安装homebrew

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

然后安装tree

```bash
brew npm install tree
```

安装完成，试试使用tree命令吧

```bash
tree -L 1
```

<!-- more -->

输出

```bash
.
├── Applications
├── Desktop
├── Documents
├── Downloads
├── Library
├── Movies
├── Music
├── Pictures
└── Public
```

