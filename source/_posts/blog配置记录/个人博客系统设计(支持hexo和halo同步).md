---
title: 个人博客系统设计(支持hexo和halo同步)
tags:
  - 博客
  - 感悟
categories: 技术
abbrlink: d2168e68
date: 2022-02-28 19:57:47
---
# 个人博客系统设计(支持hexo和halo同步)


1. 本文主要介绍自己的博客系统是如何设计的，并使用[Halo博客同步器](https://github.com/linshenkx/haloSyncServer) 将hexo（git pages: [https://linshenkx.github.io](https://linshenkx.github.io/) ）文章自动同步到halo( [http://linshenkx.cn](http://linshenkx.cn/) )。
   实现一次编写、两套博客系统并存、多个网址访问的效果。

## 一 总览

### 达到效果

| 个人博客网址                                                | 介绍                                 | 对应git仓库/管理界面                             |
| ----------------------------------------------------------- | ------------------------------------ | ------------------------------------------------ |
| [https://linshenkx.gitee.io](https://linshenkx.gitee.io/)   | hexo next gitee pages                | https://gitee.com/linshenkx/linshenkx            |
| [https://linshenkx.github.io](https://linshenkx.github.io/) | hexo next github pages               | https://github.com/linshenkx/linshenkx.github.io |
| [https://linshen.netlify.app](https://linshen.netlify.app/) | netlify加速，文章同步自blog源码仓库  | https://app.netlify.com/teams/linshenkx          |
| [https://linshenkx.cn](https://linshenkx.cn/)               | halo个人网站，文章同步自blog源码仓库 | https://linshenkx.cn/admin/index.html#/dashboard |

blog博客源码仓库（核心，私有）：https://github.com/linshenkx/blog

### 博客发布流程

1. 编写博客
   在blog工程下写博客，工程为标准hexo，博客为markdown文件放在source/_posts目录下，使用多层级分类存放
2. 发布到git pages
   完成博客的增删改后，在工程目录下执行`hexo clean && hexo d -g`部署到git pages。
   这里我配置了同时发布到github和gitee，需要注意的是，gitee的git pages需要手动去触发更新才能生效。
3. 提交并推送工程
   提交并推送blog工程的修改。
   netlify将自动获取blog工程，并执行hexo部署脚本（效果和git pages一样，只是用netlify访问据说会快一点）
   自己开发的[Halo博客同步器](https://github.com/linshenkx/haloSyncServer)也会检测到blog工程更新，根据更新情况将变化同步到halo博客系统中。

## 二 设计思路

### 1 起因

本来我一直是在使用csdn的，但是网页端写作确实不方便，而且还可能受网络情况限制。
所以我后面一般都是用印象笔记做记录，在印象笔记写好再看心情整理到csdn上去。
但是悄不注意的，在21年初csdn改版，同时也改变了排名和引流规则。
之前一个星期2500到3000的访问量现在只剩1500到2000了。

嗯，不可忍。换。

### 2 调研

市面上的博客系统可根据对Git Pages的支持（即是否支持生成静态网站）分为两大类：

一是以hexo为代表的静态网站生成器：如hexo、hugo、jekyll，较成熟，有较多第三方主题和插件，可与git pages搭配使用，也可自行部署。

二是以halo为代表的五花八门的个人博客系统，功能更加强大，自由度更高，通常带后台管理，但不支持git pages，需自行部署。

### 3 分析

个人博客的话使用git pages比较稳定，网址固定，可以永久使用，而且可以通过搭配不同的git服务商来保证访问速度。
但是git pages的缺点也很明显，是静态网站，虽然可以搭配第三方插件增强，但说到底还是个静态网站。

而如果自己买服务器，买域名，用第三方个人博客系统，就可以玩得比较花里胡哨了，但谁知道会用多久呢。
服务器、域名都要自己负责，三五年之后还能不能访问就比较难说了。
但是年轻人嘛，总还是花里胡哨点才香。

那我就全都要。

git pages作为专业性较强的个人网站可以永久访问，
然后再弄个服务器放个博客系统自己玩。

### 4 选型

静态网站生成器选的是hexo，传统一点，支持的插件和主题比较多。
hugo虽然也不错，但似乎国内用的不多，支持可能还不够完善。

然后hexo的主题用的最经典的next，比较成熟，功能也很完善
虽然整体比较严肃压抑，但可以自己加个live2d增添点活力，
作为一个展示专业性的博客网站这样也就够了

自定义博客系统的话我选的是halo，最主要原因是它是java写的，利于二次开发（事实上后面用着也确实有问题，还提交了一个issue）
而且功能比较强大，生态比较完善，虽然第三方主题少且基本都没更新，但是...实在是找不出其他一个能打的了
另外halo支持导入markdown，且功能基本都通过rest接口放开，适合开发者使用

## 三 设计实现

### 1 hexo

hexo本身只是静态网站生成器，你可以把hexo项目本身发布成为git pages项目，
像github、gitee这些会识别出这是一个hexo项目，然后进行编译，得到静态资源供外部访问。
这也是最简单的用法。

但是不推荐。

因为git pages项目一般都要求是public的（且名称固定，一个git账号只有一个git pages仓库），
hexo项目包含你的博客markdown源文件和其他的个人信息。
我们只是想把必要的生成后的静态网页放出去而已，至于项目的配置信息和markdown源文件应该藏起来。

所以需要使用 hexo-deployer-git 插件进行git pages的部署。
即放到git公开的文件只有生成后的网页文件而已，git只是把你生成后的index.html进行直接展示，不会再去编译了
（需要在source目录下添加.nojekyll文件表明为静态网页，无须编译）

而项目本身为了更好地进行管理和记录，还是要发布到git上面的，作为一个普通的私有仓库，名称可以任意（如 blog）

这样，每次要增删改完文章只需要执行`hexo clean && hexo d -g`即可发布到git仓库上
注意，不同git服务商git pages规则不一样。
比方说我gitee和github的用户名都是linshenkx
但是gitee要求的仓库名是linshenkx，而github的仓库名就必须是linshenkx.github.io了
而github的git pages仓库在接收到推送后就自动（编译）部署
gitee则需要到仓库web界面手动触发更新

截至到这一步是大多数人的做法，即git上两个仓库并存，一（或多）个git pages公有仓库做展示，一个blog仓库存放博客源码
注意：如果git pages仓库允许私有，则可以使用一个仓库多个分支来实现相同效果。
但还是推荐使用两个仓库，因为这样更通用，设计上也更合理。

工程总体结构如下，为普通hexo工程：
[![img](https://lian-gallery.oss-cn-guangzhou.aliyuncs.com/img/1631718173(1).png)](https://lian-gallery.oss-cn-guangzhou.aliyuncs.com/img/1631718173(1).png)
博客源码目录结构如下，为多层级结构：
[![img](https://lian-gallery.oss-cn-guangzhou.aliyuncs.com/img/1631718305(1).png)](https://lian-gallery.oss-cn-guangzhou.aliyuncs.com/img/1631718305(1).png)

### 2 halo

halo的使用看官方文档一般就够了，这里需要补充的是其代理配置。
因为halo的在线下载更新主题功能通常需要连接到github，我习惯通过代理访问
这里提供一下配置方法
即在容器启动时添加JVM参数即可

```shell
docker run -it -d --name halo --network host -e JVM_OPTS="-Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=7890 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=7890" -v /opt/halo/workspace:/root/.halo --restart=always halohub/halo
 
```

### 3 markdown图片

markdown图片的存放一直是个麻烦的问题。
最害怕遇到就是图链的失效，而且往往自己还不能发现。
理想状态下就是markdown一张图片支持配置多个图床链接，第一个图床链接超时就使用下一个。
这种服务端的处理思想很明显不适合放到客户端。
退而求其次，配置一个链接，访问这个链接会触发对多个图床的访问，然后那个快用那个。
这个效果技术上不难实现，也有个商业产品（聚合图床）是这样的，缺点是收费。
然后我又在github、gitee上找了各个图床软件，都不怎么样（这个时间成本都够给聚合图床开几年会员了）
最终还是妥协，用云存储吧，选了阿里
七牛、腾讯也都试了，其实都差不多，看个人爱好，没有太特别的理由

如果你用typora写markdown的话很方便，它支持picgo插件

但我习惯在idea里面编写，idea也有一些markdown-image插件，基本都不好用
所以我还是安装了picgo，开了快捷键，复制图片直接快捷键粘贴体验也还是比较舒服的
picgo的特点是插件多，不过插件质量一般，有很多bug

花了两天时间纠结、测试，最后的方案是：idea编辑+阿里云存储+picgo上传

### 4 同步

这才是重点

#### 1 同步的方向

即在哪里写文章，同步到哪里

我还是习惯用idea写markdown文档而不是在网页上。
所以确定是流向为 hexo->halo

#### 2 技术支撑

halo支持导入markdown文件，所以主要问题为hexo的markdown博客源码文件的获取
hexo文章存储路径为 source/_posts ，有多层级文件夹，可以简单地理解成文件IO操作获取文章内容。
但关键是存储在git上，这里可以用JGit进行操作。
同时，JGit支持获取两次commit之间的文件变化情况。
即可以捕获到文章的增删改操作，而不用每次都全量地同步。

#### 3 成果

又处理了一些细节问题，最终还是自己做了个haloSyncServer同步程序，
封装成docker，放服务器上跑，实现同步。
待整理后开源。

2021年11月更新
开源地址为：https://github.com/linshenkx/haloSyncServer
效果
[![img](https://lian-gallery.oss-cn-guangzhou.aliyuncs.com/img/1637394238(1).jpg)](https://lian-gallery.oss-cn-guangzhou.aliyuncs.com/img/1637394238(1).jpg)