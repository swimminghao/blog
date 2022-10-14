---
title: 手把手教你黑白群晖NAS安装破解版ROON音乐播放器1.6
tags:
  - nas
categories: 工具
abbrlink: a70bb1ff
date: 2022-03-30 16:56:00
---

# 手把手教你黑白群晖NAS安装破解版ROON音乐播放器1.6

## 什么是Roon？

roon不能说是一个播放软件，它是一个系统，由Roon Server为核心构建的，Roon Server本身不存储任何音乐文件，你也不需要把任何的音乐文件存储到它里面，他可以读取你本地的任何共享文件夹里的音乐文件，然后从他庞大的数据库中，帮你归类音乐文件，只要是信息齐全的，它没什么不认识的。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330161552.png)

然后，重点来了。他可以通过IPAD、手机、PC这些设备，来控制你的其他设备播放你本地的别的设备里的音乐，并且可以接入你的家庭智能系统。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330162528.png)

然后你可以通过手机APP，平板电脑和普通电脑去管理你的Roon，并且输出到任何接入这个Roon平台的音频设备，也就是说他可以实现局域网内多平台操控和多房间系统播放。

硬件要求：
1：有一台群晖
2：CPU最好是I5或者I7级别，官方这么说的，因为有的计算需要CPU强劲一些，实测呢，蜗牛也能跑，如果你不搞升频什么的。
3：一块SSD，Roon安装在这块SSD里，比较好。速度真的快很多，要不恶心死你。我用的一块240还是256的INTEL的SSD，而且计划这块SSD只跑Roon。
当然，你也可以装在你的机械硬盘里，如果你不太在乎速度，或者你没那么多DSD音乐的话，实测这也不是必须的。

安装准备：
1：群晖开启Root权限（怎么开启我就不介绍了，网上教程太多了）
2：Winscp
3：最好有梯子，否则安装的你想哭

## 安装1.7版本的Roon官方套件

我们在群晖安装Roon Server时，由于涉及到程序自启动以及环境变量的配置问题，一般都选择运行官方的安装脚本方式进行安装。
roon官方的有关于群晖安装的链接：https://roononnas.org/de/roon-auf-nas/
按照链接下载安装文件：RoonServer_Synology_x86_64_2018-03-07.spk
然后到群晖后台去手动安装插件!

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330163010.png)

这里会有第一个坑。因为即使本地安装，他也需要连接服务器，不知道为什么，如果你没梯子，速度因地方而异了。我是开了梯子，顺利安装，非常快。这里打开你的梯子，并且让群晖可以走梯子。你有耐心慢慢等也是可以的，有人慢慢等也装上了。

到这里。第一部分结束，安装好了最新的1.7的Roon Server，但是现在老毛子只破解了1.6的，我们现在得替换文件。

## 版本文件替换

1.6文件下载：

链接: https://pan.baidu.com/s/1NiRZCHdrHXisZyE2wTLKBA 密码: j05p


1：进入套件中心，停用，上面绿色的已启动会变成停用。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330163241.png)

2：打开你的群晖的SSH那些，控制面板--终端机和SNMP1，两个打钩。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330163323.png)

3：解压缩你刚才下载的RoonServer那个压缩包。

4：运行WINSCP。第一个是SCP模式，然后HOSTNAME填写你的群晖的IP。端口如果你没改的话是22，usernama填root，后面password，就是你的root账号的密码了。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330163450.png)

5：连接进去以后，找到你的RoonServer套件所在的位置。
我的是/volume7/@appstore/RoonServer/RoonServer。找到这个目录，然后左边本地找到你刚才解压缩的那个roonserver1.6下面的roonserver文件夹。你会发现左右两边的文件是一样的。这个时候我们覆盖他就好了。
怎么覆盖？左边的一起框柱，然后往右边啦过去。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330163532.png)

这里会问你是不是要overwrite，就是问你是不是覆盖，选yes to all 就是是的全部。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330163557.png)

自此覆盖全部结束，到这还没全结束，我们只是降级了，现在我们要破解了。

## 破解服务器端

相关文件：

链接: https://pan.baidu.com/s/1f5Kg2XSi4sAF9ygmu07pSA 密码: uw01


1：修改群晖的HOST文件
SSH修改方式很好，但是怕有的同学不会，咱们弄点简单的。打开刚才那个WINSCP软件，进入根目录下，找ETC目录，然后往下翻，找到hosts文件，拖到左边去，下载下来，软件不要关。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330163644.png)

2：找到下载下来的hosts文件，双击，用写字板打开，当然用notepod更好。
最后加上两行

```
127.0.0.1 accounts5.roonlabs.com
127.0.0.1 updates.roonlabs.com
```

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330163802.png)

保存，然后从WINSCP软件里面，找到修改好的文件，再拖会群晖里面就好了。会提示你是不是覆盖，覆盖掉就好了。这样RoonServer就没办法升级和验证了。

3：修改电脑的HOSTS文件

进入 `C:/Windows/System32/drivers/etc`目录

把hosts文件COPY到桌面，然后记事本打开，加两行：

```
127.0.0.1 accounts5.roonlabs.com
127.0.0.1 updates.roonlabs.com
```

保存，然后覆盖回 `C:/Windows/System32/drivers/etc`目录。会提示你拒绝访问，要管理员权限，继续。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330163854.png)

覆盖完成，准备工作就好了。

4：开始正式破解
这里面我们需要找一个注册文件
所以我们需要安装RoonServer服务器端的PC版，找到roon.rar文件，解压缩，得到一个文件夹和两个文件，我为了方便，新建了一个ROON目录，在C盘，把文件都放进去了。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330163920.png)

RoonKeyMaker是破解文件，RoonInstaller64是播放软件，我们现在要用的是RoonServerInstaller64.exe，双击运行RoonServerInstaller64.exe。这里没什么讲究的，反正你一会会卸载他，如图按顺序即可。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330163956.png)

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330164019.png)


然后你要注意你电脑的右下角，多了一个图标，是ROONSERVER的。显示是ROON。这个就是服务器端开始运行了。不用管他了。
运行CMD，调出命令行。



进入里的ROON破解文件的目录，如果你跟我的一样，按顺序输入

```
cd\
cd roon\RoonKeyMaker\rkm_win
```

进入目录(注意不要输入错，你可以直接复制我上面的命令运行）

然后输入

```
rkm_win -i rs
```

会跳出Enter your name,包括后面的。你可以随便输，不过这个后面你运行软件的时候会这么显示。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330164358.png)

看到successfully我们知道成功了。（如果是PC端安装server的同学，这里你就破解成功了。右下角那个下图标，右键，quit，关闭一下，然后桌边的roonserver,重新启动一下就好了）
然后我们去找KEY去吧。看下面的图，找到如图文件，复制出来。
`C:/Users/用户名/AppData/Local/RoonServer/Database/Core`

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330164448.png)

还是得用WINSCP，把文件复制过来即可。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330164542.png)



这里有一个提示，开始我们找的那个目录是APPSTONE下满这个目录不一样了，是你最早新建的那个目录，我的目录是 `/volume7/RoonServer/RoonServer/Database/Core`，你不是一定是volume7，根据你自己的情况，把文件拖过去，自此，RoonServer,安装破解成功。

## 安装播放控制端

找到开始那个roon压缩包的解压缩文件夹。
1：运行RoonInstaller64.exe安装播放控制端。安装没什么好说的了。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330164631.png)

2：ROON就运行了。这时候我们什么都不要动。不管他，也别关软件，一定不要关软件，一定不要关软件，一定不要关软件，一定不要关软件，一定不要关软件，一定不要关软件。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330164708.png)

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330164751.png)

3：破解PC的播放端
首先运行CMD，调出命令行
然后依次输入

```bash
cd \
cd roon\RoonKeyMaker\rkm_win
rkm_win -i r
```

看清楚了。这次是r不是rs了,这两个命令一个是破解服务器端，一个是破解播放端的。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330164838.png)

4：关闭电脑上的Roon软件，然后重新运行。因为破解了啊。要重新运行一下。这个时候再重开软件哦
然后左下角选择语言。

选择你喜欢的语言，比如马来语什么的，然后会提示你语言改了。现在重启。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330164906.png)

5：这边，因为破解了。说一堆什么的，不管他，但是我们还是要谢谢一下。选择不，谢谢！

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330165002.png)

然后，这边我同意！

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330165036.png)

6：连接你的服务器吧

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/30/20220330165119.png)

自此安装全部完成。