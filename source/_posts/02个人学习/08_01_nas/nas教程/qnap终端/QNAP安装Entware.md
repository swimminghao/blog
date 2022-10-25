---
title: QNAP安装Entware
tags:
  - nas
categories: 工具
abbrlink: '25003010'
date: 2022-10-19 23:01:00
---

# QNAP安装Entware

入坑qnap(威联通)，在ssh到NAS后，发现没有常用的zsh、git、sudo，这可让用惯了centos，debian系统的老高情何以堪。

一番查阅后，老高发现原来QNAP所使用的系统QTS可以使用Entware，也就是opkg最为包管理系统，这不就简单了，老高以前在OpenWrt上用的就是这个玩意儿！

本篇内容可以让你学会如何在QNAP上安装Entware-ng，以及zsh，git，sudo还有ohmyzsh等常用工具，并且保证他们不会被系统还原！

## 安装

想要在你的QNAP上安装Entware，可以访问[Qnapclub Store - Entware-ng](https://www.qnapclub.eu/en/qpkg/286)，找到对应的架构的下载地址。

比如老高的机器是`TS-551`，和`TS-453Bmini`一样是x86_64，所以下载`TS-NASX86_64`版本，对应下载链接为`https://www.qnapclub.eu/en/qpkg/model/download/11369/Entware-ng_0.97.qpkg`

最后我们ssh到NAS中，执行下面的命令：

```bash
cd /tmp
# 下载
wget https://www.qnapclub.eu/en/qpkg/model/download/11369/Entware-ng_0.97.qpkg
# 执行安装
sh Entware-ng_0.97.qpkg
# 最后删除安装包
rm Entware-ng_0.97.qpkg
```

这样就完成了安装过程。

## 使用

以安装 zsh + oh-my-zsh 为例

```
# 首先更新opkg
opkg update
# 安装zsh
opkg install zsh
# 安装git
opkg install git-http

# 安装oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

到这里我们就完成了一半了！为什么是一半呢？因为每次重启时，文件系统就会被还原，所以我们需要在系统加载的时候HACK一下，通过观察挂载的规则，老高发现`/etc/init.d/Entware.sh`这个脚本可以为我所用，所以后面我们就改一下这个脚本！

```
l /etc/init.d/Entware.sh
/etc/init.d/Entware.sh -> /share/CACHEDEV1_DATA/.qpkg/Entware/Entware.sh


# 首先，我们需要把oh-my-zsh的配置放在硬盘中，否则重启就会被删除
# 这里假设硬盘路径为/share/CACHEDEV1_DATA/
mkdir -p /share/CACHEDEV1_DATA/.zsh
cd ~
mv .zsh_history .zshrc .oh-my-zsh /share/CACHEDEV1_DATA/.zsh

## 最重要的一步！
vi /share/CACHEDEV1_DATA/.qpkg/Entware/Entware.sh

## 找到start位置，在创建link的时候加入以下代码

/bin/ln -sf /share/CACHEDEV1_DATA/.zsh/.zshrc /root/.zshrc
/bin/ln -sf /share/CACHEDEV1_DATA/.zsh/.oh-my-zsh /root/.oh-my-zsh
/bin/ln -sf /share/CACHEDEV1_DATA/.zsh/.zsh_history /root/.zsh_history
```

![hack启动脚本](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/1OTfcx_20221019230016.png)