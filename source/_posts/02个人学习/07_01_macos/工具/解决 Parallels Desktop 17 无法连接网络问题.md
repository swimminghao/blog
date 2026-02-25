---
title: 解决 Parallels Desktop 17 无法连接网络问题 从纯纯粹粹v  
tags:
  - mac
categories: 工具
abbrlink: ce2466fb
date: 2025-02-25 15:59:00
---

# 解决 Parallels Desktop 17 无法连接网络问题

## 前言

不少小伙伴在 macOS Big Sur 或 Monterey 中安装 Parallels Desktop 16/17 之后，都遇到了**初始化网络失败**，无法连接网络的问题。



## 正文

我们只要修改两个文件的配置即可：



> - `/Library/Preferences/Parallels/dispatcher.desktop.xml`
> - `/Library/Preferences/Parallels/network.desktop.xml`



可以通过 Finder 的**前往文件夹**功能直达:



1. 打开 `dispatcher.desktop.xml` 文件，找到 `<Usb>0</Usb>`，修改为 `<Usb>1</Usb>` 并保存。



> 另外，有些小白朋友可能按截图中的行号来找，这是不对的。可通过 ⌘ + F 键，然后输入关键词，来快速定位。

![3.webp.jpg](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2024/09/13/CleanShot 2024-09-13 at 10.23.36@2x.png)



2. 打开 `network.desktop.xml` 文件，找到 `<UseKextless>1</UseKextless>` 或 `<UseKextless>-1</UseKextless>`，修改为 `<UseKextless>0</UseKextless>` 并保存。

![2.webp.jpg](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2024/09/13/CleanShot 2024-09-13 at 10.24.17@2x.png)



如果找不到 `<UseKextless>1</UseKextless>` 或 `<UseKextless>-1</UseKextless>`，那么需要新增一行。



```
<ParallelsNetworkConfig schemaVersion="1.0" dyn_lists="VirtualNetworks 1">
  <!-- 在第一行新增即可 -->
  <UseKextless>0</UseKextless>
</ParallelsNetworkConfig>
```



3. 完全退出 Parallels Desktop，重新打开，就能正常连接网络了。