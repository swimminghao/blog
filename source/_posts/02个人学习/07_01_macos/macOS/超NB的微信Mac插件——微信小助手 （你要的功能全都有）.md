---
title: 超NB的微信Mac插件——微信小助手
tags:
  - mac
categories: 工具
abbrlink: dfdccc4
date: 2023-10-07 19:57:47
---
# 超NB的微信Mac插件——微信小助手

## **功能**

- 消息自动回复
- 消息防撤回
- 远程控制(已支持语音)
- 微信多开
- 第二次登录免认证
- 聊天置底功能(类似置顶)
- 微信窗口置顶
- 会话多选删除
- 自动登录开关
- 通知中心快捷回复
- 聊天窗口表情包复制 & 存储
- 小助手检测更新提醒
- alfred 快捷发送消息 & 打开窗口 (需安装：[wechat-alfred-workflow](https://link.zhihu.com/?target=https%3A//github.com/TKkk-iOSer/wechat-alfred-workflow))
- 会话一键已读
- 一键清除空会话
- 支持国际化
- 新增一键更新
- 新增关于小助手
- 去除微信url转链（从此直接打开抖音链接
- 史上最强 alfred 扩展
- 新增移除会话(不删除聊天记录)
- 菜单栏(关于小助手)新增 alfred 开关
- 新增是否使用微信自带浏览器开关
- 新增[LaunchBar 扩展](https://link.zhihu.com/?target=https%3A//github.com/VDeamoV/WeChatHelper)

**若无使用 alfred，则不必打开 alfred 开关**

远程控制：

- 屏幕保护
- 清空废纸篓
- 锁屏、休眠、关机、重启
- 退出QQ、WeChat、Chrome、Safari、所有程序
- 网易云音乐(播放、暂停、下一首、上一首、喜欢、取消喜欢)
- 小助手(获取指令、防撤回开关、自动回复开关、免认证登录开关)



## **不多说，直接看Demo演示看有多强大吧。**

## **Demo演示**

- 消息防撤回



- 自动回复

![img](https://pic2.zhimg.com/80/v2-e493eb81b7bd026222e1fe84267b0735_1440w.webp)



- 微信多开

![img](https://pic2.zhimg.com/80/v2-8602028aaef247d7ce89d4f28b0286f1_1440w.webp)



- 远程控制 (测试关闭Chrome、QQ、开启屏幕保护)

![img](https://pic4.zhimg.com/80/v2-31811b77d0ef52c7e6bcc8a62c93d733_1440w.webp)



- 免认证 & 置底 & 多选删除

![img](https://pic4.zhimg.com/80/v2-51f5af94e7c0dfbd5992b5bdd58fec8f_1440w.webp)

- 通知中心快捷回复

![img](https://pic2.zhimg.com/80/v2-c99bb68b6d39d726a81eda5a0f0921a5_1440w.webp)

- 聊天窗口表情复制 & 存储



![img](https://pic4.zhimg.com/80/v2-b56c09d321626cf2c6222f7f559e843b_1440w.webp)



- 语音远程控制 mac

![img](https://pic2.zhimg.com/80/v2-6135af8df0858496a9e42230af192d59_1440w.webp)



- Alfred 快速搜索 [wechat-alfred-workflow](https://link.zhihu.com/?target=https%3A//github.com/TKkk-iOSer/wechat-alfred-workflow)

![img](https://pic1.zhimg.com/80/v2-20f99aa9d458c1e055f78de042457528_1440w.webp)



- Alfred 搜索最近聊天列表 & 查看聊天记录

![img](https://pic2.zhimg.com/80/v2-325881833d1d49aad6f576a36e771775_1440w.webp)



- 一键已读 & 一键清除空回话

![img](https://pic2.zhimg.com/80/v2-0a74d9e754f5941b35e27b963f96eac1_1440w.webp)



------

## 使用

- 消息防撤回：点击`开启消息防撤回`或者快捷键`command + t`,即可开启、关闭。
- 自动回复：点击`开启自动回复`或者快捷键`conmand + k`，将弹出自动回复设置的窗口，点击红色箭头的按钮设置开关。

> 若关键字为 `*`，则任何信息都回复； 若关键字为`x|y`,则 x 和 y 都回复； 若关键字**或者**自动回复为空，则不开启该条自动回复； 可设置延迟回复，单位：秒； 若开启正则，请确认正则表达式书写正确，[在线正则表达式测试](https://link.zhihu.com/?target=http%3A//tool.oschina.net/regex/) **若开启特定联系人回复，则原先的群聊&私聊回复无效**



![img](https://pic1.zhimg.com/80/v2-92e77748c5be8b312a8ef6280217292c_1440w.webp)



- 微信多开：点击`登录新微信`或者快捷键`command + shift + n`,即可多开微信。
- 远程控制：点击`远程控制 Mac OS`或者快捷键`command + shift + c`,即可打开控制窗口。

**注意：仅向自己账号发送指令有效**



![img](https://pic3.zhimg.com/80/v2-abacd15f545d672b691817ba85fcb916_1440w.webp)



- Alfred 使用：请查看 [wechat-alfred-workflow](https://link.zhihu.com/?target=https%3A//github.com/TKkk-iOSer/wechat-alfred-workflow)

------

## 安装

详细安装方法(或者需要重新编译)请查阅 [Install.md](https://link.zhihu.com/?target=https%3A//github.com/TKkk-iOSer/WeChatPlugin-MacOS/blob/master/Install.md)

## 1. 懒癌版安装

## 1.1 无需安装Git

打开`应用程序-实用工具-Terminal(终端)`，执行下面的命令安装 [Oh My WeChat](https://link.zhihu.com/?target=https%3A//github.com/lmk123/oh-my-wechat)：

```text
curl -o- -L https://raw.githubusercontent.com/lmk123/oh-my-wechat/master/install.sh | bash -s
```

然后运行 `omw` 即可。

> 可以访问 [Oh My WeChat 的项目主页](https://link.zhihu.com/?target=https%3A//github.com/lmk123/oh-my-wechat%23oh-my-wechat)查看更多用法。

## 1.2 需要安装Git

打开`应用程序-实用工具-Terminal(终端)`，执行下面的命令安装

```
cd ~/Downloads && rm -rf WeChatPlugin-MacOS && git clone https://github.com/TKkk-iOSer/WeChatPlugin-MacOS.git --depth=1 && ./WeChatPlugin-MacOS/Other/Install.sh
```

## 2. 普通安装

- 点击`clone or download`按钮下载 WeChatPlugin 并解压，打开Terminal(终端)，拖动解压后`Install.sh` 文件(在 Other 文件夹中)到 Terminal 回车即可。

## 3. 安装完成

- 重启微信，在**菜单栏**中看到**微信小助手**即安装成功。



GitHub官方地址：[TKkk-iOSer/WeChatPlugin-MacOS](https://link.zhihu.com/?target=https%3A//github.com/TKkk-iOSer/WeChatPlugin-MacOS)

感谢插件作者：[tkk.ioser](mailto:tkk.ioser@gmail.com) ，这个插件真的棒。

## 卸载

打开Terminal(终端)，拖动解压后`Uninstall.sh` 文件(在 Other 文件夹中)到 Terminal 回车即可。