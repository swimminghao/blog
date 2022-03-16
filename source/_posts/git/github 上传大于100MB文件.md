---
title: github 上传大文件100MB姿势
tags:
  - git
categories: 技术
abbrlink: 473fac3a
date: 2022-02-28 19:57:47
---
# github 上传大文件100MB姿势

具体就是安装[git-lfs](https://link.jianshu.com/?t=https%3A%2F%2Fgit-lfs.github.com)，先下载，然后就是一顿操作：

1. 先在web建立一个空仓库

2. 然后建立跟仓库名一样的文件夹，并执行初始化命令：`git init`

3. 然后执行`git lfs install`

4. 然后添加你要上传的文件名或后缀名：`git lfs track '*.zip'`

5. 然后就把生成的  `.gitattributes`文件，先传到远程仓库

   - `git add .gitattributes`
- `git commit -m 'large - init file'`
   - `git push -u origin master:master` # 第一次要这样执行，后面再传就`git push`就行。
   
6. 然后就可以正常添加上传大文件了！

   - `git add bigfile.zip`
   - `git commit -m 'upload Big file.'`
   - `git push` # 第一次要这样执行，后面再传就`git push`就行。

------

- 删除远程仓库文件，但本地文件不删除，如

  ```
  bigfile.zip
  ```

  - `git rm bigfile.zip`
  - `git commit -m 'rm bigfile.zip'`
  - `git push`