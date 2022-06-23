---
title: hexo同步
tags:
- 博客
- 记录
  categories: 技术
  abbrlink: a07389a8
  date: 2022-03-02 15:10:00
---
# hexo同步

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
```

