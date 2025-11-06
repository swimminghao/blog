---
title: HIFI日记：HQPlayer embedded （HQPlayer OS）嵌入版使用教程
tags:
  - nas
  - HIFI
categories: 娱乐
abbrlink: 7ac8a987
date: 2025-11-06 10:59:05
---
# HIFI日记：HQPlayer embedded （HQPlayer OS）嵌入版使用教程

## [HQPlayer播放器系列](https://www.rainlain.com/index.php/2022/01/30/1168/)

## 一、前言

这篇文章继承自“[DIY高性能音乐服务器](https://www.rainlain.com/index.php/2020/12/22/839/)”和“[ROON+HQPlayer+NAA串流数播/网播完整教程](https://www.rainlain.com/index.php/2020/12/26/847/)”两篇文章，如果对于自己制作一台Core感兴趣的，可以跳转了解。经过大半年对于X86 Core的搭建，BLOG主在当前有限的环境下有了一些新的感悟。

## 二、什么是HQPlayer embedded

HQPlayer embedded是HQPlayer Linxu版本的延伸，是一个完全客制化的Linux系统，里面搭载了专门制作的HQPlayer OS系统，其定制的Network Audio Adapter协议是当前世界上最强的网络音频传输协议，同时它还提供了丰富的可调节选项，是当前实际上功能最多、性能最强、音质最好的播放系统。

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/aUD108.png)官方提供的NAA拓扑图

具体的请查阅官方介绍：[点我](https://www.signalyst.com/custom.html)

## 三、如何安装HQPlayer embedded

1、打开官方下载链接：[点我](https://www.signalyst.eu/bins/hqplayerd/images/)
试用版每半小时将自动关机一次，其他功能与付费版一致

2、下载最新版本镜像并解压得到img文件
注意，当前HQPlayer embedded分为两个INTEL和AMD两个版本，各位可以根据自己的CPU自行选择，根据官方介绍，这似乎是指令集上有些不同，可能影响某些时候的运行效率。他们一般名字命名为：
INTEL：hqplayer-embedded-4.29.3-x64gen.7z
AMD：hqplayer-embedded-4.29.3-x64amd.7z

PS：国外论坛中有人提出AMD支持的指令集更完善，对音质的影响未知。intel系列CPU的玩家可以直接选择AMD版本，也没什么问题。

3、在主板上安装sata/nvme硬盘，或者插入USB设备，系统安装将完全清空整个硬盘，且不可继续做他用。根据安装方式不同，我们可以在硬盘中制作HQPlayer embedded系统，开机即可使用。也可以制作USB随身携带版本，但经过USB后，音质必然存在劣化。

4、下载镜像烧录软件
我们采用的是“balenaEtcher”进行烧录，他的优势是可以直接对硬盘和USB设备进行烧录，速度快且稳定。
官网下载：[点我](https://www.balena.io/etcher/)

5、安装balenaEtcher，并根据下方提示进行烧录镜像

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/GM6gCC.png)

（1）点击Flash from file选择下载好的HQE的img镜像

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/JLgHty.png)

（2）选择需要写入的硬盘，注意这将完全破坏写入硬盘的所有数据。考虑到HQE本身就是个常驻运行系统，因此建议使用高性能的SSD作为启动盘为好。这里BLOG主选择的是INTEL傲腾，也是理论上同级别SSD中延迟最低、4K性能最好对硬盘，但考虑到BLOG主以前做过不同SSD测评，实在找不到什么音质规律，因此选择仅供参考。

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/sBRJOX.png)

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/wzhr2B.png)

（3）最后点击Flash！按钮，在弹出框中选择Yes，I’am sure，然后等待完成即可。

6、完成后将制作好的SSD装入音频服务器（Core）中，开机在主板设置成该硬盘启动，等待约30秒即可完成系统载入。

## 四、如何使用HQPlayer embedded

当前HQPlayer embedded本地播放功能难用到极点，而且本地播放到音质在没处理的情况下还不如NAA，因此这里仅推荐联网使用。很多朋友可能对网络处理没有特别多的概念，下面提供一下BLOG主当前系统部分拓扑图，各位可以参考，也可以搜索BLOG主之前写的相关文章。

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/YvAwou.png)

## 五、HQE设置

安装完成HQE后，各位首先需要进入路由器界面，找到HQE服务器的内网IP地址，如下方，BLOG主自动分配的IP地址为：192.168.1.197

然后，将这个IP地址复制到浏览器中打开，即可进入HQE后台，在这里可以对HQE进行各种设置。在这个界面上，如果出现账号和密码需求，默认账号为“hqplayer”，默认密码则是“password”。如果需要修改密码则可以点到“Authentication”页面进行设置。

接下来，我们详细介绍设置页面下，各个选项的意义：

1、设置Backend（后端）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/vxKFOT.png)

如果需要本地播放选择ALSA，如果需要通过网络进行NAA推送播放，则选择Network Audio

2、设置Output mode（音频输出模式）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/RiJF8o.png)

Auto是根据音频本身的格式，输出PCM或者SDM。但如果希望强制音频转换成PCM或者SDM，可以选择下面两项强制输出。

3、Fixed volume（绝对音量）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/jjvOcF.png)

该选项可以将音量锁定在某一个响度，非必要选项。如果不希望数字音量调节，可以勾选“enabled”（开启）并填入数字“0”。

4、Max volume/Min volume（最大音量/最小音量）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/pSDSyI.png)

该选项可以自行决定音量响度的刻度尺，设定最小和最大两个数值，后面就可以在这个规定的范围内进行调节。如果打开了上面的选项，则此选项不生效。

5、Startup volume （启动音量）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/PN9lya.png)

本选项相当于音乐开头提供一个衰减，以降低对人耳的刺激，对音质无影响，可以不设置。

6、Adaptive volume（适应性音量）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/46wecv.png)

自动平衡各音频的响度，保证每首曲子的音量基本一致，可能对音质有负面影响，建议不开启。

7、PCM gain compensation（PCM增益补偿）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/DZf03u.png)

播放PCM音频时，为每首曲子固定增加的音量，，可能对音质有负面影响，建议不开启。

8、Channels（声道）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/fhtWDU.png)

选择输出声道数量，一般填写“2”，即两声道。如果是多声道音频，在硬件条件允许情况下，需要对此进行配对设置。

9、Options（其它设置）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/3TBJt0.png)

Auto rate family：根据音频原采样率进行倍数升频，如果不选，则是根据设置的最高采样率进行升频。建议一定要开启，以保证不会出现SRC偏差。

Quick pause：快速暂停，允许在播放音频时迅速启动暂停/重新启动播放的功能。

Short buffer：短缓冲，可以有效降低播放延迟，但可能影响最终音质。除非是观看视频时出现明显音画不同步的情况，一般听音乐不选择此选项。

10、Log file（日志文件）

记录HQE系统运行时的日志文件，一般用户无需理会该选项，甚至可以选择关闭。

11、DSD sources（DSD信号源）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/gCNtWc.png)

该部分为DSD信号源部分的设置：

Direct SDM：直接输出SDM信号，关闭升频。

Gain +6 dB：为所有DSD音频信号默认追加6dB响度。

Intergrator：低通滤波器，当前提供了5种可选，请根据听感自行选择。

SDM-SDM Conversion：DSD升频滤波设置，当前有wide和narrow两种选择。

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/9LFEZx.png)仅供参考

Noise filter：噪声滤波器，当前有9种可选，请根据实际听感选择。

SDM-PCM Conversion：DSD升频PCM滤波选择，当前有14个可选，请根据实际听感选择。

12、PCM settings/SDM settings（PCM播放设置/SDM播放设置）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/uLJ9FC.png)

该部分设置与桌面版HQPlayer完全一致，请参考[这里](https://www.rainlain.com/index.php/2020/02/06/109/)。

13、ALSA backend（ALSA后端设置/本地播放设置）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/vSmiVW.png)

Device：输出设备选择

Channel offset：通道偏移，不知道有什么用，默认为0，请勿改动。

DAC bits：DAC的转换位深，根据硬件实际性能选择，如果不知道填数字“0”

Buffer time：缓冲时间，默认为100ms，可以根据硬件和体验实际进行调整。

DoP：将SDM信号打包成PCM信号输出

48k DSD：强制PCM升频DSD时采用48k整数倍升频（44.1Khz进行DSD升频为非整数倍升频，SRC可能导致失真），一般不开启，因为不推荐将PCM升频至DSD。

14、Network Audio backend（网络后端设置/NAA播放设置）

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/06/yl6O5r.png)

IPv6：打开对IPv6的IP地址支持，需要路由支持。

## 六、连接APP推送音乐

所有东西设置好！我们就可以开始听音乐了。将手机/平板连接进入与HQE同一个网络，然后下载合适的软件，选择HQE推送即可！下面推荐几个BLOG主尝试过后认为合适的方案供参考：

1、BubbleUPnP（Android）

支持连接Qobuz、TIDAL、Google Drive、Dropbox、Box、OneDrive以及本地NAS进行流媒体NAA推送播放。功能全面、响应迅速、软件免费、无广告，如果不需要ROON的数据功能，可以全面替代。

2、mconnect（Android/iOS）

持连接Qobuz、TIDAL、OneDrive以及本地NAS进行流媒体NAA推送播放。界面美观、功能全面、响应稍慢、有付费和免费版本可选择。

3、AirMusic（Android）

可以将手机/平板中所有音频信号通过UPNP协议发送，这样就可以通过整套HIFI系统看Youtube、Bilibili、抖音等视频了！注意本软件有付费和免费版，可能需要ROOT支持。

4、QQ音乐（Android/iOS）

在最新版本的QQ音乐中，HQ（含）以下音质可以直接推送到HQE中播放了！虽然并非无损格式，整体音质受损，但是瑕不掩瑜，毕竟大量无版权音乐可能只能在这里听到！顺便BS一下网易云，客户端稀烂，音质为所有播放器中垫底。