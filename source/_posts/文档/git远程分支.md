---
title: git远程分支
tags:
  - git
categories: 技术
abbrlink: 53bfd3b2
date: 2022-02-28 19:57:47
---
# 从远程仓库获取最新代码合并到本地分支

![ApJp5Nk24a0](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/ApJp5Nk24a0_20201209094424.jpg)

这里共展示两类三种方式。

## 1.git pull：获取最新代码到本地，并自动合并到当前分支

命令展示

```console
//查询当前远程的版本
$ git remote -v
//直接拉取并合并最新代码
$ git pull origin master [示例1：拉取远端origin/master分支并合并到当前分支]
$ git pull origin dev [示例2：拉取远端origin/dev分支并合并到当前分支]12345
```

分析：不推荐这种方式，因为是直接合并，无法提前处理冲突。

## 2.git fetch + merge: 获取最新代码到本地，然后手动合并分支

### 2.1.额外建立本地分支

代码展示

```console
//查看当前远程的版本
$ git remote -v 
//获取最新代码到本地临时分支(本地当前分支为[branch]，获取的远端的分支为[origin/branch])
$ git fetch origin master:master1  [示例1：在本地建立master1分支，并下载远端的origin/master分支到master1分支中]
$ git fetch origin dev:dev1[示例1：在本地建立dev1分支，并下载远端的origin/dev分支到dev1分支中]
//查看版本差异
$ git diff master1 [示例1：查看本地master1分支与当前分支的版本差异]
$ git diff dev1    [示例2：查看本地dev1分支与当前分支的版本差异]
//合并最新分支到本地分支
$ git merge master1    [示例1：合并本地分支master1到当前分支]
$ git merge dev1   [示例2：合并本地分支dev1到当前分支]
//删除本地临时分支
$ git branch -D master1    [示例1：删除本地分支master1]
$ git branch -D dev1 [示例1：删除本地分支dev1]1234567891011121314
```

备注：不推荐这种方式，还需要额外对临时分支进行处理。

### 2.2.不额外建立本地分支

代码展示

```console
//查询当前远程的版本
$ git remote -v
//获取最新代码到本地(本地当前分支为[branch]，获取的远端的分支为[origin/branch])
$ git fetch origin master  [示例1：获取远端的origin/master分支]
$ git fetch origin dev [示例2：获取远端的origin/dev分支]
//查看版本差异
$ git log -p master..origin/master [示例1：查看本地master与远端origin/master的版本差异]
$ git log -p dev..origin/dev   [示例2：查看本地dev与远端origin/dev的版本差异]
//合并最新代码到本地分支
$ git merge origin/master  [示例1：合并远端分支origin/master到当前分支]
$ git merge origin/dev [示例2：合并远端分支origin/dev到当前分支]1234567891011
```

备注：推荐这种方