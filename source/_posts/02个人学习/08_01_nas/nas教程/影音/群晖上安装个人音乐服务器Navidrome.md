---
title: 群晖上安装个人音乐服务器Navidrome
tags:
  - nas
categories: 工具
abbrlink: 57d15014
date: 2023-03-02 08:56:00
---

# 群晖上安装个人音乐服务器Navidrome

## 综述

audiostation 感觉不够轻量，客户端也不够轻量，打开登录都较慢，但有歌词插件
同时audiostation 没有PC端，手机端的美观、操作方便性等，也不能拉取专辑封面，当然大多数时候是自己下载好相关封面

虽然群晖的软件都做得不错，en 还是在很多细节上差强人意

个人音乐服务有了NAS ，是肯定要有一个专业的、好用的，主角登场：Navidrome

## Navidrome

Navidrome 是一个开源的基于网络的音乐收藏和流媒体服务器，与 Subsonic/Airsonic 兼容。它让您可以自由地从任何浏览器或移动设备收听您的音乐收藏。就像您的个人 Spotify！

Navidrome特点

- 处理非常大的音乐收藏
- 流式传输几乎任何可用的音频格式
- 读取并使用您精心策划的所有元数据（id3 标签）
- 多用户，每个用户都有自己的播放次数、播放列表、收藏夹等。
- 非常低的资源使用率：例如：具有 300GB（~29000 首歌曲）的库，它使用不到 50MB 的 RAM
- 多平台，可在 macOS、Linux 和 Windows 上运行。还提供了 Docker 镜像
- 准备使用 Raspberry Pi 二进制文件和可用的 docker 镜像
- 自动监视您的库的更改、导入新文件和重新加载新元数据
- 基于 Material UI 的主题化、现代和响应式 Web 界面，用于管理用户和浏览您的图书馆
- 与所有 Subsonic/Madsonic/Airsonic 客户端兼容。查看经过测试的客户列表
- 即时转码/下采样。可以为每个用户/玩家设置。支持 Opus 编码
- 集成音乐播放器

Subsonic API 支持的功能

- 基于标签的浏览/搜索
- 播放列表
- 书签（用于有声读物）
- 出演（收藏）艺术家/专辑/曲目
- 五星级
- 转码
- 获取/保存播放队列（继续在不同的设备上收听）
- Last.fm 和 ListenBrainz 搜刮
- 来自 Last.fm 的艺术家简历
- 来自Spotify 的艺术家图像（需要配置）
- 歌词（来自嵌入标签）

## 安装

网上有 套件版的，但我还是喜欢docker版的，升级、更新、备份方便

在群晖上以 Docker 方式安装。
在注册表中搜索 navidrome ，选择第一个 deluan/navidrome，版本选择 latest

安装时容器端口映射设置：4533，外网访问还需路由器映射
挂载设置：/data 挂载到自己的统一目录下，/music 挂载到自己的音乐文件目录
![image.png](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/02/S5r5zK.png)

环境变量

主要针对 last.fm 和spotify API 进行设置

在 https://www.last.fm/api/account/create 中创建API 账户

application name:Navidrome

![image.png](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/02/UJcacz.png)

获取 api key 与 Shared secret ,保存下来

在docker 的安装环境界面新增

```language
ND_LASTFM_ENABLED :true
ND_LASTFM_APIKEY: 获取的 api key
ND_LASTFM_SECRET：获取的 Shared secret 
ND_LASTFM_LANGUAGE：zh
```

## 访问

[http://域名：4533](http://xn--:4533-y08h91u/) 访问，首次需注册管理员

登录就可以看见扫描的专辑

![image.png](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/02/T46m6O.png)

转码可以利用默认配置，如果需要更改需在环境变量里增加以下变量，就可以在网页客户端进行更改，更改完后一定改为false，否则有安全隐患

```language
ND_ENABLETRANSCODINGCONFIG=true
```

然后在需要转码的客户端上进行设置，不需要转码的就不设置

![image.png](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/02/I4qOpH.png)

## 客户端

支持的应用
除了可以使用搭建的网页端 Web UI，Navidrome 还可以与以下所有 Subsonic 客户端兼容。以下客户端经过测试并确认可以正常工作：
iOS：play:Sub、 substreamer、 Amperfy和 iSub
安卓：DSub， Subtracks， substreamer， Ultrasonic和 Audinaut
网络：Subplayer、 Airsonic Refix、 Aurial、 Jamstash和 Subfire
桌面：Sublime Music (Linux) 和Sonixd (Windows/Linux/macOS)
CLI：Jellycli (Windows/Linux) 和STMP (Linux/macOS)
连接的扬声器：
Sonos: bonob
Alexa：AskSonic
其他：
Subsonic Kodi 插件、 Navidrome Kodi 插件、 HTTP目录文件系统

在这里，我用的是安卓，把安卓推荐的都试用了一下，还是Ultrasonic 支持中文，也好用，其它substreamer 不错
win 桌面用 Sonixd ，项目地址 https://github.com/jeffvli/sonixd，下载安装即可

针对每个用户、每个终端进行转码品质设置，各使用各的，一切OK 享受吧

## PS

转码我使用的OPUS ，相同比特率下 OPUS更好！
变量详细设置：https://www.navidrome.org/docs/usage/configuration-options/ 根据官网说明可对每个细节进行详细设置