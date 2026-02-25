---
title: mac10.15 下 git2.33 安装 git-gui
tags:
  - mac
categories: 工具
abbrlink: 701fa192
date: 2025-10-11 18:48:44
---

# mac10.15 下 git2.33 安装 git-gui

## 1.软件版本

macOS系统：10.15.7

Git：2.33.0

Git-gui：2.42.1

Tcl-tk：8.6.12

## 2.环境安装

1. Mac10.15安装git2.33的dmg包

git2.33下载链接[git-2.33.0-intel-universal-mavericks.dmg (huaweicloud.com)](https://mirrors.huaweicloud.com/git-for-macos/git-2.33.0-intel-universal-mavericks.dmg)

2. Tcl-tk环境安装

`git-gui 2.42.1`依赖tcl-tk@8版本的UI库，使用`brew install tcl-tk@8`安装。

记住`tcl-tk@8`的安装目录，下面需要用到。brew安装的默认目录：

```bash
/usr/local/Cellar/tcl-tk@8
```



## 3.安装git-gui 2.42.1



1. brew 不支持旧版git-gui安装了，需要到git-gui仓库下载`2.42.1`版本的 `git-gui.rb`文件

这是下载链接：[git-gui.rb](https://github.com/Homebrew/homebrew-core/commits/3831c91a4807c802fe1db291af9682d89ed566f1/Formula/g/git-gui.rb?after=3831c91a4807c802fe1db291af9682d89ed566f1+34)，如下图所示。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/10/11/EWBlLL.png)

文件下载完，如果下载目录在`~/Download`里，就用`brew install ~/Download/git-gui.rb`安装。

2. git-gui安装完，可能报`git-gui 依赖tcl-tk 8.5,现在使用的是9.0版本`的错误。

使用以下2条指令解决，这里就要用到上面第2步`tcl-tk@8`的安装目录

```bash
sudo sed -i '' '1s|.*|#!/usr/local/Cellar/tcl-tk@8/8.6.17/bin/wish8.6|' /usr/local/bin/git-gui
sudo sed -i '' '1s|.*|#!/usr/local/Cellar/tcl-tk@8/8.6.17/bin/wish8.6|' /usr/local/bin/gitk
```

