---
title: 如何下载和安装 macOS
tags:
  - mac
categories: 工具
abbrlink: a1192901
date: 2026-11-07 13:11:08
---
# 如何下载和安装 macOS

使用下面任一方式在兼容的 Mac 电脑上下载并安装最新或以前版本的 Mac 操作系统。每个 macOS 版本都包含适用于该版本的最新内置 App，包括 Safari 浏览器、备忘录、信息和 Apple Music。

- [使用“软件更新”](https://support.apple.com/zh-cn/102662#softwareupdate)
- [使用“macOS 恢复”](https://support.apple.com/zh-cn/102662#recovery)
- [使用“终端”](https://support.apple.com/zh-cn/102662#terminal)
- [使用可引导的安装器](https://support.apple.com/zh-cn/102662#bootable)
- [适用于旧版 macOS 的其他方法](https://support.apple.com/zh-cn/102662#othermethods)
- [检查 macOS 的兼容性](https://support.apple.com/zh-cn/102662#compatibility)

## 使用“软件更新”

“软件更新”是获取 macOS 更新最快捷、最简单的方法，而且它只会显示与你的 Mac 兼容的更新。与其他方法相比，“软件更新”在下载和安装更新时占用的储存空间也更少。

[如何使用“软件更新”更新或升级 macOS](https://support.apple.com/zh-cn/108382)

## 使用“macOS 恢复”

如果你想要重新安装 macOS，或者使用“软件更新”进行安装未成功，又或者你的 Mac 因没有正常运行的操作系统而无法完全启动，你可以从“macOS 恢复”启动，然后重新安装 macOS。

[如何通过“macOS 恢复”重新安装 macOS](https://support.apple.com/zh-cn/102655)

## 使用“终端”

你可以使用“终端”来下载兼容的 macOS 版本（需不低于 Mac 出厂时安装的版本，或者为仍然可用的最接近版本）的安装器。

1. 确保你的 Mac 运行的是 macOS Catalina 10.15 或更高版本。
2. 打开“终端”，它位于“应用程序”文件夹的“实用工具”文件夹中。或者使用“聚焦”![img](https://cdsassets.apple.com/live/7WUAS350/images/inline-icons/macos-catalina-spotlight-search-menu-icon.png)或 Siri AI ![img](https://cdsassets.apple.com/live/7WUAS350/images/inline-icons/siri-gen2.png)在菜单栏中查找并打开“终端”。
3. 在“终端”中键入或粘贴这个命令，并将“[number]”替换为你想要下载的兼容 macOS 的[最新 macOS 版本号](https://support.apple.com/zh-cn/109033#latest)：`softwareupdate --fetch-full-installer --full-installer-version [number]`
   - *例如：*softwareupdate --fetch-full-installer --full-installer-version 26.6.2
4. 按下 Return 键以输入这个命令。
   - 当 macOS 下载到“应用程序”文件夹时，“终端”会显示安装（下载）的百分比。
   - 如果“终端”显示未找到更新，则表示这个版本的 macOS 不适用于你的 Mac。你可以输入 `softwareupdate --list-full-installers` 来获取当前适用于你 Mac 机型的 macOS 版本列表。
5. 当“终端”显示安装成功完成时，下载即为完成。打开“应用程序”文件夹，然后连按两下名为“安装 [版本名称]”的 macOS 安装器，接着按照屏幕上的安装说明操作。或者，将安装器留在“应用程序”文件夹中供以后使用。

## **使用可引导的安装器**

如果你想在多台电脑上安装 macOS 且无需每次都下载安装器，或者无法使用其他方法安装，可以使用可引导的安装器。

1. [使用“终端”](https://support.apple.com/zh-cn/102662#terminal)或[适用于旧版 macOS 的其他方法](https://support.apple.com/zh-cn/102662#othermethods)来下载 macOS 安装器。
2. 按照步骤[创建可引导的安装器](https://support.apple.com/zh-cn/101578)，其中包括有关如何使用这个安装器来安装 macOS 的说明。

## 适用于旧版 macOS 的其他方法

某些旧版 macOS 也可通过 App Store 获取，或通过网页浏览器直接下载。

### 通过 App Store 下载

1. 确保你用来下载 macOS 的 Mac [与相应 macOS 兼容](https://support.apple.com/zh-cn/102662#compatibility)。
2. 如果你下载 macOS 是为了[创建可引导的安装器](https://support.apple.com/zh-cn/102662#bootable)，请确保你的 Mac 使用的是 macOS Mojave 10.14 或更高版本。
3. 使用以下链接在 App Store 中找到相应 macOS。如果这些链接无法正常工作，请使用位于“应用程序”文件夹中的 Safari 浏览器再试一次。
   - [macOS Sequoia 15](macappstores://apps.apple.com/app/macos-sequoia/id6596773750?mt=12)
   - [macOS Sonoma 14](macappstores://apps.apple.com/app/macos-sonoma/id6450717509?mt=12)
   - [macOS Ventura 13](macappstores://apps.apple.com/app/macos-ventura/id1638787999?mt=12)
   - [macOS Monterey 12](macappstores://apps.apple.com/app/macos-monterey/id1576738294?mt=12)
   - [macOS Big Sur 11](macappstores://apps.apple.com/app/macos-big-sur/id1526878132?mt=12)
   - [macOS Catalina 10.15](macappstores://apps.apple.com/app/macos-catalina/id1466841314?mt=12)
   - [macOS Mojave 10.14](macappstores://apps.apple.com/app/macos-mojave/id1398502828?mt=12)
   - [macOS High Sierra 10.13](macappstores://apps.apple.com/app/macos-high-sierra/id1246284741?mt=12)
4. 点按 App Store 中的“获取”按钮以开始下载。
   - 如果“软件更新”打开并询问你是不是要下载 macOS，请允许通过“软件更新”进行下载。
   - 如果你尝试下载的 macOS 与你的 Mac 不兼容，App Store 可能会将“获取”按钮显示为灰色，或者提示该版本 macOS 与这台设备不兼容，或者提示所请求的 macOS 版本不可用。
5. 下载到“应用程序”文件夹后，安装器会自动打开。按照屏幕上显示的安装说明进行操作，或者退出安装器，将它留在“应用程序”文件夹中以便日后使用。

### 通过网页浏览器下载

1. 确保你用来下载 macOS 的 Mac 与相应 macOS 兼容。如果不兼容，你将无法完成这些步骤。
2. 使用以下链接下载 macOS 磁盘映像 (.dmg) 文件。如果这些链接无法正常使用，请用 Safari 浏览器再试一次；Safari 浏览器位于“应用程序”文件夹中。
   - [Sierra 10.12](http://updates-http.cdn-apple.com/2019/cert/061-39476-20191023-48f365f4-0015-4c41-9f44-39d3d2aca067/InstallOS.dmg)
   - [El Capitan 10.11](http://updates-http.cdn-apple.com/2019/cert/061-41424-20191024-218af9ec-cf50-4516-9011-228c78eda3d2/InstallMacOSX.dmg)
   - [Yosemite 10.10](http://updates-http.cdn-apple.com/2019/cert/061-41343-20191023-02465f92-3ab5-4c92-bfe2-b725447a070d/InstallMacOSX.dmg)
   - [Mountain Lion 10.8](https://updates.cdn-apple.com/2021/macos/031-0627-20210614-90D11F33-1A65-42DD-BBEA-E1D9F43A6B3F/InstallMacOSX.dmg)
   - [Lion 10.7](https://updates.cdn-apple.com/2021/macos/041-7683-20210614-E610947E-C7CE-46EB-8860-D26D71F0D3EA/InstallMacOSX.dmg)
3. 默认情况下，Safari 浏览器会将磁盘映像下载到你的“下载”文件夹，你可以从程序坞或“访达”窗口的边栏中打开这个文件夹。[进一步了解如何使用 Safari 浏览器进行下载](https://support.apple.com/guide/safari/sfri40598/mac)。
4. 连按两下 .dmg 文件以打开它，然后找到其中的软件包 (.pkg) 文件。
5. 连按两下 .pkg 文件，然后按照屏幕上的说明将 macOS 安装器安装到“应用程序”文件夹中。
6. 打开“应用程序”文件夹，然后连按两下名为“安装 [*版本名称*]”的 macOS 安装器。请按照屏幕上的安装说明操作。

## 检查 macOS 的兼容性

确定哪些电脑与最新的 macOS 版本兼容：

- [macOS 27 Golden Gate 与这些电脑兼容](https://support.apple.com/zh-cn/127255)
- [macOS Tahoe 26 与这些电脑兼容](https://support.apple.com/zh-cn/122867)
- [macOS Sequoia 15 与这些电脑兼容](https://support.apple.com/zh-cn/120282)
- [macOS Sonoma 14 与这些电脑兼容](https://support.apple.com/zh-cn/105113)
- [macOS Ventura 13 与这些电脑兼容](https://support.apple.com/zh-cn/102861)
- [macOS Monterey 12 与这些电脑兼容](https://support.apple.com/zh-cn/103260)
- [macOS Big Sur 11 与这些电脑兼容](https://support.apple.com/zh-cn/103111)
- [macOS Catalina 10.15 与这些电脑兼容](https://support.apple.com/zh-cn/102765)

## 进一步了解

- [了解 Apple 软件更新](https://support.apple.com/zh-cn/125039)，包括了解更新软件为什么很重要以及软件更新的工作方式。
- [如果在更新或安装 macOS 时发生错误](https://support.apple.com/zh-cn/102531)
- [如果你的 Mac 无法顺利完成启动](https://support.apple.com/zh-cn/102675)
- [如何将外置储存设备用作 Mac 启动磁盘](https://support.apple.com/zh-cn/111336)

## 需要更多协助？

请详细描述一下你遇到的问题，我们会建议你接下来可以怎么做。

[获取建议](https://getsupport.apple.com/?caller=ikb)