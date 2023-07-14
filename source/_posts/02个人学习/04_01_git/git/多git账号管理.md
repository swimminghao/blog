---
title: 如何在同一台电脑上使用多个GitHub账号
tags:
  - git
categories: 技术
abbrlink: 76ba13a7
date: 2023-07-14 09:12:00
---

# 如何在同一台电脑上使用多个GitHub账号

我们知道，如果使用ssh key去绑定GitHub账号，那么每次从同一台电脑上push代码的时候就不用输入密码，账号这些信息了。那如果有多个GitHub账号呢？比如我就有两个GitHub账号，一个是工作用的，一个是个人的。下面给大家介绍如何在同一台电脑上使用多个GitHub账号，并且每个账号都对应各自的ssh key。

## 1. 创建SSH key

我们先创建对应公钥和私钥：

```
ssh-keygen -t rsa -b 2048 -C "nas" -f ~/.ssh/id_rsa.github
```

这个指令会创建一个公钥`~/.ssh/id_rsa.github.pub`和一个私钥在`~/.ssh`的目录下。我们可以将这对秘钥当成默认的，用于个人账户。

用于工作的账号，使用下面的命令把公钥绑定到工作账号的邮箱`email@work.com`，并且，生成一个名为`id_rsa_work_user.pub`。

```
ssh-keygen -t rsa -C "email@work.com" -f "id_rsa_work_user"
```

现在我们就有了两组不同的秘钥，~/.ssh/id_rsa.github和~/.ssh/id_rsa_work_user。

## 2. 将ssh key添加到不同GitHub账号

先来处理个人账号。我们用`pbcopy < ~/.ssh/id_rsa.github.pub`拷贝公钥，然后登陆到GitHub。

- 进入settings
- 点击左边栏的SSH and GPG keys进入到SSH keys的编辑块
- 点击New SSH key
- 黏贴刚刚拷贝的内容到Key的编辑框里，并且在title的编辑框内给它取个名字

然后登入到工作的GitHub账号，重复以上步骤，只不过这次是`pbcopy < ~/.ssh/id_rsa_work_user.pub`。

## 3.  使用ssh配置文件配置不同账号使用的ssh key

打开ssh配置文件（**~/.ssh/config**），按照下面的样式去编辑：

```
# Personal account, - the default config
Host github.com
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa.github
    
# Work account
Host github1.com
   HostName github.com
   User git
   IdentityFile ~/.ssh/id_rsa_work_user
```

Host是用来区分不同的Git账号，比如“github.com-work_user”你也可以取另外一个名字，但是使用ssh去clone仓库的时候，记得把拷贝过来的clone命令的时候，**把命令中的github.com替换成对应的Git账号下的Host**。比如，你需要在你的工作账号下克隆这个仓库git@github.com:personal_account_name/repo_name.git，这个就需要把‘github.com’替换成‘github.com-work_user’。

## 4. 本地仓库

我们可以通过git remote -v看看本地仓库对应的远程仓库URL，检查该URL是否与要使用的GitHub主机匹配，否则更新远程原始URL。如果不匹配，可以通过下面这个命令更新：

```
git remote set-url origin git@github.com-worker_user:worker_user/repo_name.git
```

注意替换@和冒号中间的字符串，它们对应你在ssh配置文件中配置的host。

如果你要在本地创建一个全新的仓库。我们都知道使用git init，然后你在GitHub上创建一个仓库，同样需要在本地配置远程仓库的URL，像这样：

```
git remote add origin git@github.com-work_user:work_user/repo_name.git 
```

同样需要注意对应的host。

好了。关于本地使用多个GitHub账号如何配置ssh key和host就介绍这么多。