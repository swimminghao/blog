---
title: WindowsPE和MacOS安装盘二合一制作方法
tags:
  - hackintosh
  - winpe
  - Ventura
categories: 工具
abbrlink: 4b8ad329
date: 2025-11-09 16:30:29
---

# WindowsPE和MacOS安装盘二合一制作方法

## 一、准备U盘

准备一个U盘，安装包一般大小都在12G以上，所以建议至少16G，或者直接使用移动硬盘。

插入U盘，打开Mac的磁盘工具

选择U盘-抹掉

- 名称：随意

- 格式：格式Mac OS扩展(日志式)

- 方案：GUID分区图

![notion image](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/09/6xrrzP.png)

抹掉完成之后，再对U盘进行分区用于制作Mac OS安装盘，点击`+`号，增加新的分区

分区信息：

- 名称：UOS

- 格式：Mac OS扩展(日志式)

- 大小：必须大于你的安装包，我这里直接分了16G

新增分区之后，就可以准备制作Mac OS安装盘了。

## 二、制作Mac OS安装盘

下载好的Mac OS安装包默认放在应用程序中按照一下方式进行安装盘制作：

1. 敲“`sudo`”（无引号）
2. 空格
3. 拖拽文件“`createinstallmedia`”到终端（文件位置在“`安装macOS Ventura.app`”-右键-`显示包文件`-`Contents`-`Resources`里）
4. 敲“`--volume`”（无引号）
5. 空格
6. 从桌面把你之前插入的U盘的图标拖进终端（我这里U盘名字为UOS）
7. 敲“`--nointeraction`”（无引号）
8. 回车
9. 输入密码（密码不显示）
10. 回车，静待安装盘建立完成。

以上步骤综合为这么一串代码，请根据自己系统版本和U盘名字对应修改：

```bash
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/UOS --nointeraction
```

整个制作过程如下，到最后显示出这个就表示安装盘制作完成。

```bash
sudo /Applications/Install\ macOS\ Ventura.app/Contents/Resources/createinstallmedia --volume /Volumes/Install\ macOS\ Ventura --nointeraction
Password:
Erasing disk: 0%... 10%... 20%... 30%... 100%
Copying essential files...
Copying the macOS RecoveryOS...
Making disk bootable...
Copying to disk: 0%... 10%... 20%... 30%... 40%... 50%... 60%... 70%... 80%... 90%... 100%
Install media now available at "/Volumes/Install macOS Ventura"
```

到这里，如果你的引导没有屏蔽多余的启动项，在启动中已经可以看到这个安装盘了，选择既可以使用。

## 三、加入黑苹果EFI

安装盘制作完成之后，为了可以以后将U盘作为启动盘从BIOS的启动界面启动安装盘，就需要将自己配置好的引导文件EFI文件放到EFI分区中。

使用工具挂载EFI分区，比如用OpenCore Configurator 或者Hackintool 或者 OCAuxiliaryTools都可以，挂载U盘的EFI分区之后，将自己的配置好的EFI文件放进去，就成为了自己黑苹果的专用安装盘了。

那如果需要用于不同的机器来安装黑苹果，或者调试的时候修改EFI文件，总是需要挂载EFI分区，就比较麻烦了。

我们可以在引导分区之外，新建一个公开的引导分区。

打开磁盘工具，选择U盘，点分区，选择之前制作Mac OS安装盘剩下没有使用的部分，点`+`号，增加新的分区。

分区信息：

- 名称：MAC_EFI（随意，不能有中文）

- 格式：MS-DOS (FAT32)

- 大小：1G（引导文件很小，一般大于200M即可）

![notion image](https://www.notion.so/image/https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2F0433d702-a1fc-4acb-8cce-dcc5102def31%2F1b2e46e6-0c80-47ac-aece-764c55a2799f%2FUntitled.png?table=block&id=e32eaa36-c151-4d2b-b164-0994b00519e2&t=e32eaa36-c151-4d2b-b164-0994b00519e2&width=702&cache=v2)

这个新建的引导分区同样也可以放黑苹果的EFI引导文件，而且是无需挂载的，适合频繁修改调试引导配置和更换文件。

## 四、制作Windows PE

按照上面的方式同样新增一个分区

分区信息：

- 名称：WINPE（随意，同样不能有中文）

- 格式：MS-DOS (FAT32)

- 大小：大于你的PE文件

新增分区完成之后，这里就需要一个支持UEFI启动的WinPE，一般PE主流格式都是exe格式，不符合我们的需要，所以需要在Window系统下先将PE制作成iso文件。

这里推荐老毛桃PE，功能比较多，可以联网，适合大多数PE需求。

然后在Mac下，打开iso文件，将里面的文件复制到新增的WINPE分区中。

## 结束

这样，一个Mac OS的安装盘和PE盘二合一的U盘就制作好了，剩下还有多余空间，可以当做正常存储U盘使用。