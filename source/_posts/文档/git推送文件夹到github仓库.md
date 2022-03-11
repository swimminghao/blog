---
title: git推送文件夹到github仓库
date: 2022-02-28 19:57:47
tags: [git]
categories: 技术
---
# Git推送文件夹到github仓库

有时候我们可能会遇到当文件累积到了一定程度的时候，想使用 git 进行版本管理，或者推送到 Github 等远程仓库上。本文介绍如何将一个本地文件夹中已经存在的内容使用 git 进行管理，并推送至远程仓库，以及对其中可能出现的错误进行分析。

## 创建 git 仓库

在该文件夹下初始化仓库：

```css
git init
```

此时将会在此文件夹下创建一个空的仓库，产生一个 `.git`文件，会看到以下提示：

```css
Initialized empty Git repository in FOLDERPATH/.git/
```

## 将文件添加到暂存区

使用以下命令：

```css
git add .	
```

此操作会将当前文件夹中所有文件添加到 git 仓库暂存区。

## 将文件提交到仓库

`git add` 命令仅仅将文件暂存，但实际上还没有提交，实际上仓库中并没有这些文件，使用以下命令：

```css
git commit
```

此时将会打开一个文件，用于记录提交说明，输入提交说明即可，若说明较为简短，也可以使用以下命令：

```css
git commit -m "YOUR COMMENT"
```

## 添加远程仓库

使用以下命令添加添加一个远程仓库：

```css
git remote add origin YOUR_REMOTE_REPOSITORY_URL
```

or

```css
git remote set-url origin git@github.com:ppreyer/first_app.git
```

其中 origin 相当于给远程仓库的名称，也就是相当于一个标识符。

## 推送至远程仓库

使用以下命令将会将本地仓库中的内容推送至远程仓库的 master 分支：



```css
git push -u origin master
```

or

```css
git push origin dev:master
```

**注意**：如果之前忘记了`git commit` 的步骤，这里将会出现一个错误提示：

```css
error: src refspec master does not match any.
```

为什么会有这个报错呢？原因其实很简单，在没有使用 `git commit` 之前，由于这是一个新创建的`git`仓库，没有`master brench`，也就是并没有一个工作树可供推送至远程仓库，所以自然也就出错啦。