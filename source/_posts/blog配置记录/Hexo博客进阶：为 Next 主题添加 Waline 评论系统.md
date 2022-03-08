---
title: Hexo博客进阶：为 Next 主题添加 Waline 评论系统
tags:
  - 博客
  - 记录
categories: 技术
abbrlink: a07389a8
date: 2022-03-02 15:10:00
---
# Hexo博客进阶：为 Next 主题添加 Waline 评论系统

 发表于 2022-01-20 分类于 [Hexo博客](https://qianfanguojin.top/categories/Hexo博客/) 阅读次数： 44 Waline： 本文字数： 2.2k 阅读时长 ≈ 4 分钟

文章发出之后，往往我们想要得到读者更多地反馈，那么拥有一个评论系统是至关重要的。

本篇带大家通过一些简单的配置，在 Hexo Next 主题下添加 Waline 评论系统。



## 前言

在之前的 [Hexo博客进阶：为Next主题添加Valine评论系统 | 谢同学的博客 (qianfanguojin.top)](https://qianfanguojin.top/2019/07/23/Hexo博客进阶：为Next主题添加Valine评论系统/?highlight=valine) 文章中，我叙述了如何 在 Next主题下配置 Valine 评论系统。

但是，根据读者反馈，Valine 评论系统在 Next 主题高版本 (7.+) 以上已没有支持，且 Valine 已经很久没有更新维护了。不过，有大佬在 Valine 的基础之上开发了 [Waline](https://waline.js.org/) 。
这次，我们就来描述如何快速上手安装配置更加人性化且带后端的 [Waline](https://waline.js.org/) 评论系统。

## 1. 第一步，配置评论数据库

`Waline` 和 Valine 一样，也是支持基于 [LeanCloud](https://leancloud.app/) 作为数据存储的，但是 `Waline` 支持的部署方式更多：

|                                                          | Waline                                    |                                                              |
| -------------------------------------------------------- | ----------------------------------------- | ------------------------------------------------------------ |
| **Client**                                               | **Server**                                | **Storage**                                                  |
| [@waline/client](https://waline.js.org/)                 | [Vercel](https://vercel.com/)             | [LeanCloud](https://leancloud.app/)                          |
| [MiniValine](https://minivaline.js.org/)                 | [Deta](https://deta.sh/)                  | [CloudBase](https://clodbase.net/)                           |
| [AprilComment](https://github.com/asforest/AprilComment) | [CloudBase](https://cloudbase.net/)       | [MongoDB](https://mongodb.com/)                              |
|                                                          | [InspireCloud](https://inspirecloud.com/) | MySQL                                                        |
|                                                          | [Railway](https://railway.app/)           | SQLite                                                       |
|                                                          | [Render](https://render.com/)             | PostgreSQL                                                   |
|                                                          | Docker                                    | [GitHub](https://github.com/)                                |
|                                                          | Virtual Host                              | [Deta Base](https://docs.deta.sh/docs/base/about)            |
|                                                          |                                           | [InspireCloud](https://inspirecloud.com/docs/nodejs/database/quickstart.html) |

为了方便，这里我只讲述最简单，零成本的数据库建立方法。

我们需要注册一个 [Leancloud 国际版 ](https://console.leancloud.app/register)的账号，注意，一定要是 **国际版**，国内版需要绑定备案的域名，比较麻烦。具体可以在注册时的左上角看到：

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201202316763.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201202316763.png)

注册完成后，登录，然后我们找到`创建应用`

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/others/20210815161252.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/others/20210815161252.png)

在这里填写你的应用名称,名称可以自己定义，然后，下面选择`开发版` 点击`创建`。

然后点击应用进入设置。

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/others/202108151614201.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/others/202108151614201.png)

点击应用凭证，取得我们 `AppKey` 、`App id` 、以及 `MasterKey` ：

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211139993.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211139993.png)

数据库配置完毕，接下来安装服务端。

## 2. 安装服务端

由上面的表格可以看到，`Waline` 支持多种服务端，为了最简便上手，我们使用第一种方式，即在 `Vercl` 上安装服务端。首先，点击下面的按钮，一键部署：

[![Vercel](https://vercel.com/button)](https://vercel.com/import/project?template=https://github.com/walinejs/waline/tree/main/example)

应该需要注册一个账号，支持使用 `Github` 账号直接登录：

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211306146.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211306146.png)

登录后重新点进来，点击 `Create`：

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211309809.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211309809.png)

然后等待下面 `Deploy` 构建完成，点击 `Go to Dashboard`

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211313406.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211313406.png)

找到 Settings => Environment Variables，配置环境变量：

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211314501.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211314501.png)

我们需要配置三个环境变量，对应如下表：

| Lean Cloud | Vercel Environment |
| ---------- | ------------------ |
| AppID      | LEAN_ID            |
| AppKey     | LEAN_KEY           |
| MasterKey  | LEAN_MASTER_KEY    |

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211330222.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211330222.png)

> 提示
>
> 如果你使用 LeanCloud 国内版，请额外配置 `LEAN_SERVER` 环境变量，值为你绑定好的域名。

为了使环境变量生效，我们需要重新构建一次。在上方找到 Deployments ，选择第一个右边的三个点，点击 Redeploy 。

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211333672.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211333672.png)

等待其构建结束，然后记住 `DOMAINS` 中的域名地址：

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211413737.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211413737.png)

好了，服务端部署到此结束，下面我们开始在 `Hexo Next` 主题中配置客户端。

## 3. 在Hexo Next主题中配置

由于 Next 主题中并不自带 `Waline` 的评论配置，我们需要安装官方提供的插件。在 `Hexo` 根目录执行：

```
npm install @waline/hexo-next
```

找到 Next 的主题配置文件，在最后加上

```
# Waline
# For more information: https://waline.js.org, https://github.com/walinejs/waline
waline:
  enable: true #是否开启
  serverURL: waline-server-pearl.vercel.app # Waline #服务端地址，我们这里就是上面部署的 Vercel 地址
  placeholder: 请文明评论呀 # #评论框的默认文字
  avatar: mm # 头像风格
  meta: [nick, mail, link] # 自定义评论框上面的三个输入框的内容
  pageSize: 10 # 评论数量多少时显示分页
  lang: zh-cn # 语言, 可选值: en, zh-cn
  # Warning: 不要同时启用 `waline.visitor` 以及 `leancloud_visitors`.
  visitor: false # 文章阅读统计
  comment_count: true # 如果为 false , 评论数量只会在当前评论页面显示, 主页则不显示
  requiredFields: [] # 设置用户评论时必填的信息，[nick,mail]: [nick] | [nick, mail]
  libUrl: # Set custom library cdn url
```

重新部署 `Hexo` ，就可以看到结果了。

> 据反馈，Hexo 似乎在 8.x 的版本使用 waline 比较稳定，如果出现 `hexo g` 出错，可尝试升级 hexo 版本。

## 4. 登录服务端

由于 `Waline` 有服务端，支持评论管理。我们需要注册一个账号作为管理员。

找到评论框，点击 `登录` 按钮，会弹出一个窗口，找到用户注册，默认第一个注册的用户为管理员，所以部署好一定要记得及时注册。

[![img](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211432511.png)](https://cdn.jsdelivr.net/gh/qianfanguojin/ImageHosting_1/hexo/202201211432511.png)

注册好，登录之后即可进入评论管理的后台，可以对评论进行管理。
