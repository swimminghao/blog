---
title: shell知识点
date: 2022-02-28 19:57:47
tags: [shell]
categories: 工具
---

# $1易混淆点

## 一、标准输入和参数的区别

这个问题一定是最容易让人迷惑的，具体来说，就是搞不清什么时候用管道 符| 和文件重定向> ，< ，什么时候用变量$ 。

比如说，我现在有个自动连接宽带的 shell 脚本 connect.sh ，存在我的家目录:

```bash
$ where connect.sh 
/home/fdl/bin/connect.sh
```

如果我想删除这个脚本，而且想少敲几次键盘，应该怎么操作呢?我曾经这
样尝试过:

```bash
$ where connect.sh | rm
```

实际上，这样操作是错误的，正确的做法应该是这样的:

```bash
$ rm $(where connect.sh)
```

前者试图将 where 的结果连接到 rm 的标准输入，后者试图将结果作为命令行参数传入。

**标准输入就是编程语言中诸如** **scanf** **或者** **readline** **这种命令;而参数是指 程序的** **main** **函数传入的** **args** **字符数组**。

前文「Linux文件描述符」说过，管道符和重定向符是将数据作为程序的标 准输入，而 $(cmd) 是读取 cmd 命令输出的数据作为参数。

用刚才的例子说， rm 命令源代码中肯定不接受标准输入，而是接收命令行 参数，删除相应的文件。作为对比， cat 命令是既接受标准输入，又接受 命令行参数:

```bash
$ cat filename 
...file text...
$ cat < filename
...file text...
$ echo 'hello world' | cat
hello world
```

**如果命令能够让终端阻塞，说明该命令接收标准输入，反之就是不接受**，比 如你只运行 cat 命令不加任何参数，终端就会阻塞，等待你输入字符串并 回显相同的字符串。

## **二、后台运行程序**

比如说你远程登录到服务器上，运行一个 Django web 程序:

```bash
$ python manager.py runserver 0.0.0.0 Listening on 0.0.0.0:8080...
```

现在你可以通过服务器的 IP 地址测试 Django 服务，但是终端此时就阻塞 了，你输入什么都不响应，除非输入 Ctrl-C 或者 Ctrl-/ 终止 python 进程。

可以在命令之后加一个 & 符号，这样命令行不会阻塞，可以响应你后续输 入的命令，但是如果你退出服务器的登录，就不能访问该网⻚了。

如果你想在退出服务器之后仍然能够访问web服务，应该这样写命令 (cmd &) :

```bash
$ (python manager.py runserver 0.0.0.0 &) Listening on 0.0.0.0:8080...

$ logout
```

**底层原理是这样的**:

每一个命令行终端都是一个 shell 进程，你在这个终端里执行的程序实际上 都是这个 shell 进程分出来的子进程。正常情况下，shell 进程会阻塞，等待 子进程退出才重新接收你输入的新的命令。加上 & 号，只是让 shell 进程不 再阻塞，可以继续响应你的新命令。但是无论如何，你如果关掉了这个 shell 命令行端口，依附于它的所有子进程都会退出。

**而 (cmd &) 这样运行命令，则是将 cmd 命令挂到一个 systemd 系统守护进程名下，认 systemd 做爸爸，这样当你退出当前终端时，对于刚才的 cmd 命令就完全没有影响了。**

类似的，还有一种后台运行常用的做法是这样:

```bash
$ nohub some_cmd &
```

nohub 命令也是类似的原理，不过通过我的测试，**还是 (cmd &) 这种形式 更加稳定**。

## 三、单引号和双引号的区别

不同的 shell 行为会有细微区别，但有一点是确定的，**对于** **$** **，** **(** **，** **)** **这 几个符号，单引号包围的字符串不会做任何转义，双引号包围的字符串会转 义**。

shell 的行为可以测试，使用 set -x 命令，会开启 shell 的命令回显，你可 以通过回显观察 shell 到底在执行什么命令:

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/acjc30_20210910112344.png)

可⻅ echo $(cmd) 和 echo "$(cmd)" ，结果差不多，但是仍然有区别。注 意观察，双引号转义完成的结果会自动增加单引号，而前者不会。

**也就是说，如果** **$** **读取出的参数字符串包含空格，应该用双引号括起来， 否则就会出错**。

## **四、****sudo** **找不到命令**

有时候我们普通用户可以用的命令，用 sudo 加权限之后却报错 command not found:

```bash
$ connect.sh
 network-manager: Permission denied

$ sudo connect.sh
 sudo: command not found
```

原因在于， connect.sh 这个脚本仅存在于该用户的环境变量中:

**当使用** **sudo** **时，系统会使用** **/etc/sudoers** **这个文件中规定的该用户的权 限和环境变量**，而这个脚本在 /etc/sudoers 环境变量目录中当然是找不到 的。

解决方法是使用脚本文件的路径，而不是仅仅通过脚本名称:

```bash
$ sudo /home/fdl/bin/connect.sh
```
