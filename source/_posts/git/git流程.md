---
title: git流程
tags:
  - git
categories: 技术
abbrlink: f44eb3a2
date: 2022-02-28 19:57:47
---

# git 流程
```java
正常上线
git clone

git checkout dev

git checkout -b feature-function

git add .
git commit -m '结束功能开发'

git checkout dev
git merge feature-function
git push origin dev

git checkout -b release-2019-09-29
git push origin release-2019-09-29

线上发现bug 紧急上线

git checkout -b hotfix-2019-09-29 origin/master
git checkout -b hotfix-2019-09-29 
git add .
git commit -m 'message'
git push origin hotfix-2019-09-29

git checkout dev
git merge hotfix-2019-09-29
git push origin dev


git branch -d feature-function
```