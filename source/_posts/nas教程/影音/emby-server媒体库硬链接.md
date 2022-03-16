---
title: emby-server媒体库硬链接
tags:
  - nas
categories: 工具
abbrlink: 61fc1c97
date: 2022-03-16 21:39:00
---

# emby-server媒体库硬链接.md

现在emby和plex等媒体库对于刮削的命名要求很高，而PT下载后我们却很难将下载的媒体资源添加到影音库中，因为要保持源文件名进行保种，而复制一份文件的话，又很占硬盘空间，造成浪费。
在这种情况下，使用Linux连接就能便捷得一石二鸟。

## 软链接和硬链接

软连接可以便捷地创立整个文件夹的连接，但软连接类似快捷方式，删除源文件后，连接就会失效，这种情况下，如果你删除PT保种的文件，在媒体库中整理好的资源也会失效。
相比起来，硬链接更加方便，在创立连接后，即便删除了源文件，只要没有删除所有的硬链接文件，硬链接仍然有效。
但Linux的硬链接却有一个小小的缺点：为了避免递归问题，硬链接只能创建单个文件的连接，而无法连接整个文件夹。

## linux硬链接脚本

Windows下有一些好用的硬链接工具，但在Linux系统下，却没有找到类似的工具，于是我就就从零搞起，网上东抄抄西抄抄，自己写了个简单的批量硬链接脚本，进行对文件夹的硬链接，在我的openmediavault系统(基于debian)下测试了几周，还没出过问题。
还没有在别的系统上测试过，但理论上适用于所有Linux系统，包括群晖、铁骑马、威联通、openmediavault、unas等等。

## 使用教程

1. 编辑两个bash脚本文件hardlink.bash和2.bash，复制到Linux下的/usr/local/bin/文件夹中。
2. 使用cd命令切换到想要让硬链接文件存在的文件夹，如test。
3. 以root账户或具有root权限的账户执行：sudo bash hardlink.bash [你的PT下载影音资源所在的目录]

** 注：root权限下不需要输入sudo，链接文件和源文件必须处于同一个硬盘之下，不能跨硬盘执行硬链接操作。**

如此一来，在test这个文件夹下，就出现了你想要硬链接的文件夹下的所有子文件夹和文件。
对链接文件进行修改文件名，删除操作，均不会影响源文件，仍然能pt保种，但修改链接文件内容，会造成源文件内容改变，同时，对于大部分的程序来说，硬链接文件和源文件是相同的。
如果感觉每次都需要输入很烦，可以创建一个计划任务，让系统自动创建硬链接。

`hardlink.bash`

```bash
#!/bin/bash
PRE_IFS=$IFS
IFS=$'\n'
distdir=$1
echo $distdir
newdir=`basename $distdir`
mkdir $newdir
cd $newdir
filename=$(ls $distdir)
currentdir=`pwd`
mod=$currentdir
echo $currentdir
for file in $filename
do
    echo $file
    if [ -f $distdir/$file ]
    then
        ln $distdir/$file $currentdir/$file
    fi
    if [ -d $distdir/$file ]
    then
        cd $currentdir
        mkdir $currentdir/$file
        cd $currentdir/$file
        bash /usr/local/bin/2.bash $distdir/$file
    fi
done
# 任务执行完毕，把IFS还原回默认值
chmod -R 777 $mod
IFS=$PRE_IFS
```

`2.bash`

```bash
#!/bin/bash
PRE_IFS=$IFS
IFS=$'\n'
currentdir=`pwd`
echo $currentdir
distdir=$1
echo $distdir
filename=$(ls $distdir)
for file in $filename
do
    echo $file
    if [ -f $distdir/$file ]
    then
        ln $distdir/$file $currentdir/$file
    fi
    if [ -d $distdir/$file ]
    then
        cd $currentdir
        mkdir $currentdir/$file
        cd $currentdir/$file
        bash /usr/local/bin/${0##*/} $distdir/$file
    fi
done
# 任务执行完毕，把IFS还原回默认值
IFS=$PRE_IFS
```