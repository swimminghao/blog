---
title: git命令
tags:
  - git
categories: 技术
abbrlink: 341ecd43
date: 2022-02-28 19:57:47
---
# git指令

# **个人开发**

## 一般来说，日常使用只要记住下图6个命令，就可以了。但是熟练使用，恐怕要记住60～100个命令。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/QTmUTc_20220214115250.png)

`workspace：工作区`

`Index/Stage：暂存区`

`Repository：仓库区（或本地仓库）`

`Remote：远程仓库`

1. ## 新建代码库

   ```bash
   # 下载一个项目和它的整个代码历史
   $ git clone [url]
   ```

2. ## 配置

   Git的设置文件为`.gitconfig`，它可以在用户主目录下（全局配置），也可以在项目目录下（项目配置）。

   ```bash
   # 显示当前的Git配置
   $ git config --list
   
   # 编辑Git配置文件
   $ git config -e [--global]
   
   # 设置提交代码时的用户信息
   $ git config [--global] user.name "[name]"
   $ git config [--global] user.email "[email address]"
   ```

3. ## 增加/删除文件

   ```bash
   # 添加指定文件到暂存区
   $ git add [file1] [file2] ...
   
   # 添加指定目录到暂存区，包括子目录
   $ git add [dir]
   
   # 添加当前目录的所有文件到暂存区
   $ git add .
   
   # 添加每个变化前，都会要求确认
   # 对于同一个文件的多处变化，可以实现分次提交
   $ git add -p
   
   # 删除工作区文件，并且将这次删除放入暂存区
   $ git rm [file1] [file2] ...
   ```
   
4. ## 代码提交

   ```bash
   # 提交暂存区到仓库区
   $ git commit -m [message]
   
   # 提交暂存区的指定文件到仓库区
   $ git commit [file1] [file2] ... -m [message]
   
   # 提交时显示所有diff信息
   $ git commit -v
   ```
   
5. ## 分支

   ```bash
   # 列出所有本地分支
   $ git branch
   
   # 列出所有远程分支
   $ git branch -r
   
   # 列出所有本地分支和远程分支
   $ git branch -a
   
   # 新建一个分支，但依然停留在当前分支
   $ git branch [branch-name]
   
   # 新建一个分支，并切换到该分支
   $ git checkout -b [branch]
   
   # 新建一个分支，指向指定commit
   $ git branch [branch] [commit]
   
   # 新建一个分支，与指定的远程分支建立追踪关系
   $ git branch --track [branch] [remote-branch]
   
   # 切换到指定分支，并更新工作区
   $ git checkout [branch-name]
   
   # 切换到上一个分支
   $ git checkout -
   
   # 建立追踪关系，在现有分支与指定的远程分支之间
   $ git branch --set-upstream [branch] [remote-branch]
   
   # 合并指定分支到当前分支
   $ git merge [branch]
   
   # 删除分支
   $ git branch -d [branch-name]
   
   # 删除远程分支
   $ git push origin --delete [branch-name]
   $ git branch -dr [remote/branch]
   ```
   
6. ## 查看信息

   > ```bash
   > # 显示有变更的文件
   > $ git status
   > 
   > # 显示当前分支的版本历史
   > $ git log
   > 
   > # 显示commit历史，以及每次commit发生变更的文件
   > $ git log --stat
   > 
   > # 搜索提交历史，根据关键词
   > $ git log -S [keyword]
   > 
   > # 显示某个文件的版本历史，包括文件改名
   > $ git log --follow [file]
   > $ git whatchanged [file]
   > 
   > # 显示指定文件相关的每一次diff
   > $ git log -p [file]
   > 
   > # 显示过去5次提交
   > $ git log -5 --pretty --oneline
   > 
   > # 显示所有提交过的用户，按提交次数排序
   > $ git shortlog -sn
   > 
   > # 显示指定文件是什么人在什么时间修改过
   > $ git blame [file]
   > 
   > # 显示暂存区和工作区的差异
   > $ git diff
   > 
   > # 显示暂存区和上一个commit的差异
   > $ git diff --cached [file]
   > 
   > # 显示工作区与当前分支最新commit之间的差异
   > $ git diff HEAD
   > 
   > # 显示两次提交之间的差异
   > $ git diff [first-branch]...[second-branch]
   > 
   > # 显示某次提交的元数据和内容变化
   > $ git show [commit]
   > 
   > # 显示某次提交发生变化的文件
   > $ git show --name-only [commit]
   > 
   > # 显示某次提交时，某个文件的内容
   > $ git show [commit]:[filename]
   > 
   > # 显示当前分支的最近几次提交
   > $ git reflog
   > ```
   
7. ## 远程同步

   ```bash
   # 下载远程仓库的所有变动
   $ git fetch [remote]
   
   # 显示所有远程仓库
   $ git remote -v
   
   # 显示某个远程仓库的信息
   $ git remote show [remote]
   
   # 增加一个新的远程仓库，并命名
   $ git remote add [shortname] [url]
   
   # 取回远程仓库的变化，并与本地分支合并
   $ git pull [remote] [branch]
   
   # 上传本地指定分支到远程仓库
   $ git push [remote] [branch]
   
   # 推送所有分支到远程仓库
   $ git push [remote] --all
   ```
   
8. ## 撤销

   ```bash
   # 恢复暂存区的指定文件到工作区
   $ git checkout [file]
   
   # 恢复某个commit的指定文件到暂存区和工作区
   $ git checkout [commit] [file]
   
   # 恢复暂存区的所有文件到工作区
   $ git checkout .
   
   # 重置暂存区的指定文件，与上一次commit保持一致，但工作区不变
   $ git reset [file]
   
   # 重置暂存区与工作区，与上一次commit保持一致
   $ git reset --hard
   
   # 重置当前分支的指针为指定commit，同时重置暂存区，但工作区不变
   $ git reset [commit]
   
   # 重置当前分支的HEAD为指定commit，同时重置暂存区和工作区，与指定commit一致
   $ git reset --hard [commit]
   
   # 重置当前HEAD为指定commit，但保持暂存区和工作区不变
   $ git reset --keep [commit]
   
   # 新建一个commit，用来撤销指定commit
   # 后者的所有变化都将被前者抵消，并且应用到当前分支
   $ git revert [commit]
   
   # 暂时将未提交的变化移除，稍后再移入
   $ git stash
   $ git stash pop
   ```

   # **多人合作开发**

   1. ## 源仓库
   
      项目开始的时候，项目发起者构建起一个项目的最原始的仓库，称之为源仓库（origin）。
   
      - 源仓库有两个作用
        1. 汇总参与该项目的各个开发者的代码
        2. 存放趋于稳定和可发布的代码
      - 源仓库受到保护，开发者不直接对其进行开发工作。只有项目管理者能对其进行较高权限的操作。
   
   2. ## 开发者仓库
   
      如上所说，任何开发者都不会对源仓库进行直接的操作，源仓库建立以后，每个开发者需要fork一份源仓库，作为自己日常开发的仓库。
   
      每个开发者所fork的仓库是完全独立的，互不干扰。每个开发者仓库相当于一个源仓库实体的镜像，开发者在这个镜像中进行编码，提交到自己的仓库中，这样就可以轻易地实现团队成员之间的并行开发工作。而开发工作完成以后，开发者可以向源仓库发送pull request，请求管理员把自己的代码合并到源仓库中，这样就实现了分布式开发工作，和最后的集中式的管理。
   
   3. ## 分支 （Branch）
   
      git分支有两类，5种：
   
      ```bash
      永久性分支
      master branch：主分支
      develop branch：开发分支
      临时性分支
      feature branch：功能分支
      release branch：预发布分支
      hotfix branch：bug修复分支
      ```
   
      
   
   4. ## 多人合作具体步骤
   
      1）clone远程代码
   
      ```bash
   git clone [url]
      ```
   
      2）切换到develop分支，将本地新项目提交到本地develop分支，再将本地develp分支上的新建项目将上传到远程develop分支。
   
      ```bash
   git checkout develop
      git add .
   git  commit -m "new branch commit"
      git push origin new branch commit
      ```
      
   
   3)开发
   
   - 切换到develop分支
   
     ```bash
     git checkout develop
     ```
   
   - 分出一个功能性分支
   
     ```bash
        git checkout -b function-branch
     ```
   
   - 在功能性分支上进行开发工作，多次commit，测试以后
   
   - 把做好的功能合并到develop中
   
     ```bash
      git checkout develop
      # 回到develop分支
         
      git merge --no-ff function-branch
      # 把做好的功能合并到develop中
      
      git branch -d functio-branch
       # 删除功能性分支
      
      git push origin develop
      # 把develop提交到自己的远程仓库中
     ```
   
     
   
   4)合并远程master和develop分支
   
   切换到master分支，从远程pull代码，将develop分支合并到本地master分支（此时本地master分支是与远程同步的），有冲突解决，没有则罢。最后push到远程master仓库。
   
   **（注）保证多人协作的时候尽量少出现merge conflict和污染主分支,做到以下几点：**
   
    - 做好分工，尽量避免出现多人修改同一个文件。
      
    - 每个人的所有开发工作都只在自己的分支开发。
      
    - 每个人只允许在自己的分支直接push远程分支。
      
    - 合并的时候必须遵循以下条件.
      
      1）首先本地切换到develop分支
      
      ```bash
         git checkout develop
      ```
      
      2）git pull
      
      3）那么在pull到远程的develop最新的内容之后，
      
         ```bash
         git merge  [branch]
         ```
      
      4）如果出现confict那么清除conflict之后，commit。然后把本地develop push到远程develop。
      
      5）没完成一个功能就提交一次。不要累计代码。   



   
git checkout dev

git push origin --delete release-1.00.00
