---
title: 关闭OneDrive内部的自动更新机制
tags:
  - mac
categories: 工具
abbrlink: b7168604
date: 2024-04-13 22:30:00
---

# 关闭 OneDrive 内部的自动更新机制

## 

我有一台用了十年的 mac mini，这台电脑的质量还是不错的。虽然也升级了内存和硬盘。 这个没怎么清过灰的家伙也一直没坏。稳定地发挥一个 HTPC 的作用。 不过苹果是已经放弃了它，最终它停留在 catalina。 不过最近我遇到了点麻烦：onedrive 打不开了。

## 目录

本文使用到了 homebrew，如果你没有或不能使用 brew，请参考另一篇：

- 使用 homebrew 安装旧版 OneDrive（本文）
- [使用 installer 安装旧版 OneDrive](https://tricks.one/post/install-onedrive-wo-brew-on-catalina/)

## 更新

目前看来，这是微软推送了一个未在 catalina 上验证过的更新，目前问题已修复， 可以直接安装到最新版，如果您已经使用了本文的方法关闭了自动更新， 那么删掉现有 OneDrive 程序重新安装即可。下次微软再犯相同错误，可以如法炮制。

## 问题

某天，OneDrive 突然提示我它已经损坏了，重启也没什么反应。 我的第一反应是 mini 用的这块 ssd 大限到了， 不过后来看看其他程序和文件似乎也都正常，感觉又不太像是 ssd 的寿命问题。

如果不是 ssd 的寿命问题，之前 OneDrive 也是好好的，那么估计就是更新出问题了， 我的 OneDrive 是在 app store 安装的，打开商店看了一下，最近 OneDrive 果然有更新。

第一回合我们自然是先重装试试看能不能修复，顺带一提， OneDrive 内部有一个重置的命令，可以把 OneDrive 重置到出厂状态， 右键点击 OneDrive 应用，点击「显示包内容」：

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2024/04/13/NaMymm.png)

接下来就可以在 Contents/Resources 下找到 ResetOneDriveApp.command：

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2024/04/13/P75WRD.png)

双击运行就好，不过写在这里你应该可以猜到，这个和重装都没能解决问题。

## 解决

首先我们要安装旧版本的应用，很显然 app store 是没办法更新到指定版本的， 不过 brew 可以，只是需要一些小手段。

### 使用 HomeBrew 安装旧版应用

首先我们在 [Homebrew Formulae](https://formulae.brew.sh/) 中找到 onedrive 的[页面](https://formulae.brew.sh/cask/onedrive)， 从这个页面我们可以找到 OneDrive 的 [Cask code](https://github.com/Homebrew/homebrew-cask/blob/853aa6382660a4e2386c29a9c770e5f135211264/Casks/onedrive.rb)， 点击 history：

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2024/04/13/UGJuni.png)

这时我们就可以看到近期的更新，我打算更新到 2 月 23 日的版本， 我们点击 #141750 右边「[查看当时的代码](https://github.com/Homebrew/homebrew-cask/blob/7f2c7e50e7eff325536efedadb5877d2c1e96b85/Casks/onedrive.rb)」

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2024/04/13/RTd4B6.png)

接下来 raw 按钮给出的就是 当时的 [cask code](https://github.com/Homebrew/homebrew-cask/raw/7f2c7e50e7eff325536efedadb5877d2c1e96b85/Casks/onedrive.rb) 了，我们可以下载下来，使用以下命令安装：

```bash
brew install --cask onedrive.rb
```

安装好以后，**千万不要立即执行 OneDrive**！因为 OneDrive 自身也内嵌了更新机制， 一旦启动了 OneDrive，它就会自作聪明地自作主张， 把我们好不容易装上去的旧版更新为无法启动的最新版。

## 关闭自动更新

一个 OneDrive，三个更新进程，我们索性把它们全关掉：

```bash
launchctl remove com.microsoft.OneDriveStandaloneUpdater
sudo launchctl remove com.microsoft.OneDriveStandaloneUpdaterDaemon
sudo launchctl remove com.microsoft.OneDriveUpdaterDaemon
```

一不做二不休，配置文件也删了好了：

```bash
sudo rm /Library/LaunchAgents/com.microsoft.OneDriveStandaloneUpdater.plist
sudo rm /Library/LaunchDaemons/com.microsoft.OneDriveStandaloneUpdaterDaemon.plist
sudo rm /Library/LaunchDaemons/com.microsoft.OneDriveUpdaterDaemon.plist
```

这时我们再启动 OneDrive，整个世界清净了。

# 总结

如果你还在使用 catalina，并且也在使用 OneDrive 时，那么大概率会遇到这个问题， 其实我理解微软的做法，淘汰某个古老系统是正常的，更新也是按正常逻辑运行的， 但是两个加在一起，老系统就不正常了，这不能作为让用户承担问题的理由， 起码应该下发一个版本，停止旧版本的自动更新，再接下来向新系统下发更新。 不过我的牢骚微软应该是听不到的，如果你也有这个问题，那么请按以下步骤尝试：

1. 重装旧版本应用，重装后不要立即启动
2. 停用所有更新相关服务