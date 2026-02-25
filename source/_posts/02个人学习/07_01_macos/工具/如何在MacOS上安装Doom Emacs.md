---
title: 如何在MacOS上安装Doom Emacs
tags:
  - mac
categories: 工具
abbrlink: cdb8f127
date: 2025-08-25 11:56:00
---

# 如何在MacOS上安装Doom Emacs
## 1. Emacs安装

### 1.1 通过Homebrew安装

首先我们利用Mac的包管理工具hombrew，安装的是Emacs-plus，[传送门在此](https://blogv1.seanzou.com/archives/(https://github.com/d12frosted/homebrew-emacs-plus#emacs-28))。 因为当前Doom Emacs不支持29.+， 所以只安装28版本的Emacs（`Emacs 27.1+ (28.1 is recommended, or native-comp. 29+ is not supported).`）

所以，我们用命令安装

```SHELL
brew tap d12frosted/emacs-plus
brew install emacs-plus@28 --HEAD --with-modern-doom3-icon # 我们想换一个好看的头像，嘻嘻
```

如下图，doom3的icon，好看！
[![Screen Shot 2023-04-11 at 17.18.14](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/08/25/1W6D02.png)](http://blogv1.seanzou.com/upload/2023/04/Screen Shot 2023-04-11 at 17.18.14-d3897770cd794c9ca49df2fbdc9b4f38.png)

### 1.2 给Emacs创建快捷方式

利用homebrew安装完后Emacs，我们去将Emacs的app连接。我们首先去安装了emacs的根目录。可以从Summary里面找到，e.g., `/opt/hombrew/Cellar/emacs-plus@27/HEAD-abedf3a`。然后使用命令`cp`进行快捷方式的链接。

```SHELL
cd /opt/hombrew/Cellar/emacs-plus@27/HEAD-abedf3a
cp -r Emacs.app /Applications
```

这个时候我们就会发现我们有一个可爱的图标的Emacs了。

## 2. Doom Emacs配置覆盖

### 2.1 安装依赖

```sql
brew install git ripgrep coreutils fd
xcode-select --install
```

### 2.2 覆盖Emacs的配置

这个时候Emacs的配置很重要，[DoomEmacs Github](https://github.com/doomemacs/doomemacs#install)给的地方是`~/.config/emacs`，但是我的配置完全不是这个，所以导致我最开始即使安装成功了Doom 和 Emacs，但是打开的仍旧是原版的Emacs，所以对于我来说（MacOS）用户来说命令应该如下，正确位置上`~/.emacs.d`，而不是`~/.config/emacs`。

```SHELL
git clone https://github.com/doomemacs/doomemacs ~/.emacs.d
cd ~/.emacs.d
bin/doom install # 安装过程中有y 点y
bin/doom doctor  # 检查一下有没有问题
bin/doom sync    # 同步一下，我没有运行这行命令甚至无法打开Emacs
```

至此我们的Doom Emacs已经安装完成了，接下来为了方便起见，可以将Doom加入环境变量。

**[关于Emcas配置] 来自Dason大佬的建议：删掉.emacs.d，就会自动读取.config里面的配置。**

### 2.3 将Doom命令加入

使用`vim ~/.bashrc`或者`vim ~/.bash_profile`，然后加入我们通过命令`pwd`找到我们当前Doom的目录，加入进去就可以啦。嘿嘿。

# 总结

完成了，昨天在火车上思考了一路，最后得出结论，既然Doom和Emacs都安装成功了，但是打开还是原版的Emacs，很明显就是Doom的配置文件没有覆盖成功，于是今天重新试了一遍，找了一个MacOS版本的Youtube视频，结果成功啦。[参考视频链接在此。](https://www.youtube.com/watch?v=eyYxuIGF8-g)