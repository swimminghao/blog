---
title: OpenCore添加ubuntu引导，引导双系统
tags:
  - hackintosh
categories: 工具
abbrlink: eb18152
date: 2025-11-06 09:34:11
---

# OpenCore添加ubuntu引导，引导双系统

## 问题描述

在此前已经安装好了Mac，并能正常引导但是Ubuntu的引导需要手动按F10进行选择，比较麻烦，尝试将Ubuntu的引导也加入到OC中。

参考国外大佬[Ayush Sahay Chaudhary](https://kextcache.com/author/ayushere/)的博客[[GUIDE] Opencore Dualboot/ Multiboot Guide](http://kextcache.com/opencore-dualboot-guide/)进行的配置，该博客中有完整的win+mac+linux的安装步骤。

## 实现步骤

请确保你的OC中存在OpenShell.efi文件，没有可前往opencore官方网站进行抄作业

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/2MhddB.png)

### 下载OpenCore Configurator

进入MAC，在网上下载OpenCore Configurator工具，工具版本要和OC文件版本对应。

### 在OpenShell中找到EFI的位置

启动到 OpenCore 并选择 OpenShell.efi

如果在OC中无法找到该选项而EFI中确实放置了OpenShell.efi，那么进入Mac中启动OpenCore Configurator。

转到工具> 挂载EFI

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/iDO59h.png)

选择自己电脑EFI所在的硬盘并挂载

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/aT3E4Y.png)

找到其中的EFI> OC> config.plist，在OpenCore Configurator中打开该文件

将其启用保存再进入OC中就可以看到OpenShell.efi

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/AYqSCo.png)

重启到 OpenCore 并选择 OpenShell.efi

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/w02Nr7.png)


你会看到这样的屏幕（这时请注意及时按一下按键）

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/up6xni.png)

这里的东西是，FS1, FS2,… FS7: 是所有连接到系统的驱动器的分区，你必须确定哪个是Linux 分区。

要查找分区开始输入

```
FS1:
```


然后输入

```bash
dir
```

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/yB82Q4.png)

继续这个操作一直到你在输入 DIR 后找到 EFI 的文件夹

输入

```bash
FS2:
```


然后输入

```bash
dir
```


![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/NDw47P.png)


再次这样输入

我在 FS7中找到了我的 总 EFI 分区：

检查你的EFI文件夹

```bash
cd EFI

dir
```

如果ubuntu的文件夹是在这个分区里面

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/R9mPlO.png)


输入

```bash
cd ../
```


回到你的根 EFI 分区，即 FS7

再次输入

```bash
map > map-table-linux.txt
```


将地址信息导出到txt文件

### 制作启动项地址

现在重新启动系统并从启动菜单中选择 macOS 驱动器启动到 macOS。

同样挂载EFI分区

打开 EFI分区可以看到，map-table-linux.txt

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/918Nox.png)

将其剪切到桌面，打开它

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/Q1AEeo.png)


查到你在OpenShell中找到的驱动器号（我的是FS7），复制 PCI 行（图中选中的那一行）并将其粘贴到一个新的文本文件中，然后在 PCI 行之后添加这一行，如下所示，

```bash
/\EFI\ubuntu\grubx64.efi
```

**注意：前面不能有空格**

之后可以看到是这样子

```bash
PciRoot(0x0)/Pci(0x1D,0x0)/Pci(0x0,0x0)/NVMe(0x1,20-60-A0-49-8B-44-1B-00)/HD(1,GPT,B2C56269-054C-45C4-8ACS-C321A6B52982,0x800,0xAF000)/\EFI\ubuntu\grubx64.efi
```

### 在OpenCore Configurator中添加启动项

同样打开OpenCore Configurator，进入目录EFI> OC> config.plist，将config.plist在OpenCore Configurator中打开，然后去Misc-其它设置中，选择Entries-自定义条目。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/6gSgi2.png)


在右下角添加自定义条目

路径中添加创建好的路径，名称填写Ubuntu，风格自动，勾选启用。

## 大功告成

效果展示

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/tl2IIU.png)