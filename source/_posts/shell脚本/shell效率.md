---
title: shell效率
date: 2022-02-28 19:57:47
tags: [shell]
categories: 工具
---

# §1效率

## **输入相似文件名太麻烦**

用花括号括起来的字符串用逗号连接，可以自动扩展，非常有用，直接看例子:

```bash
$ echo {one,two,three}file
onefile twofile threefile
$ echo {one,two,three}{1,2,3}
one1 one2 one3 two1 two2 two3 three1 three2 three3
```

你看，花括号中的每个字符都可以和之后(或之前)的字符串进行组合拼 接，**注意花括号和其中的逗号不可以用空格分隔，否则会被认为是普通的字 符串对待**。

这个技巧有什么实际用处呢?最简单有用的就是给 cp , mv , rm 等命令扩 展参数:

```bash
$ cp /very/long/path/file{,.bak}
==> cp /very/long/path/file /very/long/path/file.bak
# 给 file 复制一个叫做 file.bak 的副本

$ rm file{1,3,5}.txt
# 删除 file1.txt file3.txt file5.txt

$ mv *.{c,cpp} src/
# 将所有 .c 和 .cpp 为后缀的文件移入 src 文件夹
```

## **输入路径名称太麻烦**

- **用** **cd -** **返回刚才呆的目录**，直接看例子吧:

```bash
$ pwd
 /very/long/path
 $ cd # 回到家目录瞅瞅
 $ pwd
 /home/labuladong
 $ cd - # 再返回刚才那个目录 $ pwd
 /very/long/path
```

- **特殊命令** **!$** **会替换成上一次输入的命令最后的路径**，直接看例子:

```bash
#没有加可执行权限
$ /usr/bin/script.sh
zsh: permission denied: /usr/bin/script.sh

$ chmod +x !$
chmod +x /usr/bin/script.sh
export lessCharset-uft8
```

**特殊命令** **!\*** **会替换成上一次命令输入的所有文件路径**，直接看例子:

```BASH
# 创建了三个脚本文件
$ file script1.sh script2.sh script3.sh

# 给它们全部加上可执行权限
$ chmod +x !*
chmod +x script1.sh script2.sh script3.sh
```

**可以在环境变量** **CDPATH** **中加入你常用的工作目录**，当 cd 命令在当前目 录中找不到你指定的文件/目录时，会自动到 CDPATH 中的目录中寻找。

比如说我常去 /var/log 目录找日志，可以执行如下命令:

```bash
$ export CDPATH='~:/var/log'
# cd 命令将会在 〜 目录和 /var/log 目录扩展搜索$ pwd
/home/labuladong/musics
$ cd mysql
cd /var/log/mysql
$ pwd
/var/log/mysql
$ cd my_pictures
cd /home/labuladong/my_pictures
```

这个技巧是十分好用的，这样就免了经常写完整的路径名称，节约不少时间。

需要注意的是，以上操作是 bash 支持的，其他主流 shell 解释器当然都支持 扩展 cd 命令的搜索目录，但可能不是修改 CDPATH 这个变量，具体的设 置方法可以自行搜索。

## 输入重复命令太麻烦

** **使用特殊命令** **!!** **，可以自动替换成上一次使用的命令**:

```bash
$ apt install net-tools
 E: Could not open lock file - open (13: Permission denied)

$ sudo !!
 sudo apt install net-tools [sudo] password for fdl:
```

有的命令很⻓，一时间想不起来具体参数了怎么办?

**对于** **bash** **终端，可以使用** **Ctrl+R** **快捷键反向搜索历史命令**，之所以说是 反向搜索，就是搜索最近一次输入的命令。

比如按下 Ctrl+R 之后，输入 sudo ，bash 就会搜索出最近一次包含 sudo 的命令，你回⻋之后就可以运行该命令了:

```bash
(reverse-i-search)`sudo': sudo apt install git
```

但是这个方法有缺点:首先，该功能似乎只有 bash 支持，我用的 zsh 作为 shell 终端，就用不了;第二，只能查找出一个(最近的)命令，如果我想 找以前的某个命令，就没办法了。

对于这种情况，**我们最常用的方法是使用** **history** **命令配合管道符和** **grep** **命令来寻找某个历史命令**:

```bash
# 过滤出所有包含 config 字段的历史命令
$ history | grep 'config'
	7352 ./configure
	7434 git config --global --unset https.proxy 9609 ifconfig
	9985 clip -o | sed -z 's/\n/,\n/g' | clip
	10433 cd ~/.config
```

你使用的所有 shell 命令都会被记录，前面的数字就表示这是第几个命令， 找到你想重复使用的命令后，也不需要复制粘贴该命令，**只要使用** **!** **+** **你 想重用的命令编号即可运行该命令**。
拿上面的例子，我想重新运行 git config 那条命令，就可以这样:

```bash
$ !7434
git config --global --unset https.proxy # 运行完成
```

我觉得 history 加管道加 grep 这样打的字还是太多，可以在 你的 shell 配置文件中( .bashrc ， .zshrc 等) 中写这样一个函数:

```bash
his() {
	history | grep "$@"
}
```

这样就不需要写那么多，只需要 his 'some_keyword' 即可搜索历史命令。

我一般不使用 bash 作为终端，我给大家推荐一款很好用的 shell 终端叫做 zsh，这也是我自己使用的 shell。这款终端还可以扩展各种插件，非常好 用，具体配置方法可自行搜索。

## 其他小技巧

- **yes **命令自动输入字符**y **进行确认

  我们安装某些软件的时候，可能有交互式的提问:

```bash
$ sudo apt install XXX
...
XXX will use 996 MB disk space, continue? [y/n]
```

一般情况下我们都是一路 y 到底，但如果我们想自动化一些软件的安装就很 烦，遇到这种交互式提问就卡住了，还得手动处理。

yes 命令可以帮助我们:

```bash
 $ yes | your_cmd
```

这样就会一路自动 y 下去，不会停下让我们输入了。 如果你读过前文Linux 文件描述符，就知道其原理很简单:

你单独运行一下 yes 命令，发现它就是打印出一大堆字符 y，通过管道把 输出和 your_cmd 的标准输入相连接，如果 your_cmd 又提出无聊的问题， 就会从标准输入读取数据，也就会读取到一个 y 和换行符，和你手动输入 y 确认是一个效果。

- **特殊变量** **$?** **记录上一次命令的返回值**。

在 Linux shell 中，遵循 C 语言的习惯，返回值为 0 的话就是程序正常退 出，非 0 值就是异常退出出。读取上一次命令的返回值在平时使用命令行时 感觉没什么用，但是如果你想编写一些 shell 脚本，知道返回值非常有用。

**举个实际的例子**，比如我的 Github 仓库 fucking-algorithm ，我需要给其中 所有 markdown 文件最下方添加上一篇、下一篇、目录三个⻚脚链接，有的 文章已经有了⻚脚，大部分都没有。

为了防止重复添加，我必须知道一个 md 文件下方是否已添加，这时候就可 以使用 $? 变量配合 grep 命令做到:

```bash
#!/bin/bash
filename=$1
# 查看文件尾部是否包含关键词
tail | grep '下一篇' $filename
# grep 查找到匹配会返回 0，找不到则返回非 0 值
[ $? -ne 0 ] && { 添加⻚脚; }
```

- **特殊变量** **$$** **记录当前进程的** **PID**。

这个功能可能在平时使用时也不怎么用，但是在写 shell 脚本时也非常有 用，比如说你要在 /tmp 创建临时文件，给文件起名字一直都是非常让人 费脑子 的，这时候可以使用 $$ 变量扩展出当前进程的 PID 作为临时文件的名字。

