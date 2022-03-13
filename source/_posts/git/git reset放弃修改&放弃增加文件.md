---
title: git reset放弃修改&放弃增加文件
date: 2022-02-28 19:57:47
tags: [git]
categories: 技术
---

# git reset放弃修改&放弃增加文件

**1. 本地修改了一堆文件(并没有使用git add到暂存区)，想放弃修改。**
单个文件/文件夹：

```
$ git checkout -- filename
```

所有文件/文件夹：

```
$ git checkout .
```

**2. 本地新增了一堆文件(并没有git add到暂存区)，想放弃修改。**
单个文件/文件夹：

```
$ rm filename / rm dir -rf
```

所有文件/文件夹：

```
$ git clean -xdf
```

// 删除新增的文件，如果文件已经已经git add到暂存区，并不会删除！

**3. 本地修改/新增了一堆文件，已经git add到暂存区，想放弃修改。**
单个文件/文件夹：

```
$ git reset HEAD filename
```

所有文件/文件夹：

```
$ git reset HEAD .
```

**4. 本地通过git add & git commit 之后，想要撤销此次commit**

```
$ git reset commit_id
```

这个id是你想要回到的那个节点，可以通过git log查看，可以只选前6位
**// 撤销之后，你所做的已经commit的修改还在工作区！**

```
$ git reset --hard commit_id
```

这个id是你想要回到的那个节点，可以通过git log查看，可以只选前6位
**// 撤销之后，你所做的已经commit的修改将会清除，仍在工作区/暂存区的代码不会清除！**