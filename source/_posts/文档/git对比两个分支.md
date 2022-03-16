---
title: git对比两个分支
tags:
  - git
categories: 技术
abbrlink: 1ed54b1d
date: 2022-02-28 19:57:47
---
 # git 对比两个分支差异

  

  ### 1. 显示出branch1和branch2中差异的部分

  ```
  git diff branch1 branch2 --stat
  ```

  ### 2. 显示指定文件的详细差异

  ```
  git diff branch1 branch2 具体文件路径
  ```

  ### 3. 显示出所有有差异的文件的详细差异

  ```
  git diff branch1 branch2
  ```

  ### 4. 查看branch1分支有，而branch2中没有的log

  ```
  git log branch1 ^branch2
  ```

  ### 5. 查看branch2中比branch1中多提交了哪些内容

  ```
  git log branch1..branch2`
  ```
  **注意，列出来的是两个点后边（此处即dev）多提交的内容。**

  ### 6. 不知道谁提交的多谁提交的少，单纯想知道有是吗不一样

  ```
  git log branch1...branch2
  ```

  ### 7. 在上述情况下，在显示出没个提交是在哪个分支上

  ```
  git log --lefg-right branch1...branch2
  ```
  **注意 commit 后面的箭头，根据我们在 –left-right branch1…branch2 的顺序，左箭头 < 表示是 branch1 的，右箭头 > 表示是branch2的。**