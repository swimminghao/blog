---
title: parallels Desktop共享本机目录给虚拟机
tags:
  - mac
categories: 工具
abbrlink: 42c28135
date: 2025-10-04 10:30:39
---
# parallels+desktop+linux共享文件夹,Parallels Desktop共享本机目录给虚拟机

本文详细介绍了在CentOS7虚拟机环境中安装ParallelsTools的过程，包括设置CD/DVD源、调整启动顺序及手动挂载镜像文件等步骤，并提供了升级ParallelsTools的方法。

我的虚拟机环境：centOS7

需要先安装Parallels Tools

先停止centos并设置cd/dvd源

![8ca47c6e08db940329488b788cd0968e.png](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/10/04/dakxNk.png)

Parallels Tools镜像文件在 位置 /Applications/Parallels Desktop.app/Contents/Resources/Tools/

Parallels Tools区分多个平台(windows,mac,linux)

由于用的是centOS，所以这里选择 prl-tools-lin.iso

进入安装Parallels Tools流程的方法

方法一：在Virtual Machine上点击 安装 Parallels Tools (有时候会不生效，建议使用方法二)

方法二：调整启动顺序，把CD/DVD调到第一位，接着是硬盘，然后reboot重启虚拟机

![6a2b4247e3cb0a8a7987a490d8b85b13.png](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/10/04/p7ws4A.png)

cd /media

ls

此时看不到任何内容

手动创建

mkdir cdrom

再挂载到cdrom

mount -o exec /dev/cdrom /media/cdrom

(这里需要注意的是/dev/cdrom是否存在，有时是cdrom1)

此时看到 mount: block device /dev/sr0 is write-protected, mounting read-only 提示说明挂载成功

cd /media/cdrom

ls

可以看到‘ install installer install-gui kmods tools version ’等文件

执行安装

./install

![dd44b494d24d4c3a48d02435c541d484.png](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/10/04/KmAWz6.png)

等待安装完成即可

![ab2391807425d4e7f58ed6f55dec6f55.png](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/10/04/6rHyNH.png)

在Parallels Desktop菜单栏先停止该虚拟机

设置本机共享文件夹

![55239c1dca6e8b1b3f229751488bc1ad.png](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/10/04/GxCYKE.png)

![97562408f5e1fddbae3db6c6d209ff28.png](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/10/04/onHx1W.png)

设置完共享目录再重启该虚拟机

cd /media/psf

ls

即可看到本机共享的文件夹

建立软连接

一般为了方便访问，我都是通过/website来访问本机的共享文件夹的

ln -s /media/psf/website /

cd /

ls

看到website就算建立连接了。

如果升级了Parallels Desktop版本，那么也要对应升级Parallels Tools，升级Parallels Tools的方法跟前面安装Parallels Tools的方法是一样的，界面会提示进行的操作是upgrade

正常使用建议改成优先硬盘启动

详情可查看官方文档