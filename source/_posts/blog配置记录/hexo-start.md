---
title: hexo-start
top: 10
tags:
    - 博客
    - 记录
categories: 技术
abbrlink: 4a17b156
date: 2022-06-24 01:10:00
---

摘要内容......
<!-- more -->
以下为隐藏内容


# hexo-start

Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/one-command-deployment.html)


## hexo同步

**以下操作在你的第二个平台上进行，并确定已安装 node.js & npm。**

在你想要同步博客的文件夹下执行

```zsh
git clone <远端博客仓库地址>
cd # 进入到主题文件夹
git clone <远端主题仓库地址>
cd # 进入到第三方主题文件夹
git checkout customize #切换到customize分支
# 回到 hexo 根目录，安装依赖
npm install hexo
npm install hexo-deployer-git
npm install hexo-cli -g
npm install
```

执行hexo指令

```zsh
hexo clean && proxy4 hexo d -g