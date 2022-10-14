---
title: linux 中为 cp 和 mv 命令添加进度条
tags:
  - 终端
categories: 工具
abbrlink: 5f1a845d
date: 2022-06-23 23:31:00
---

# linux 中为 cp 和 mv 命令添加进度条

> `GNU cp` 和 `GNU mv` 工具用于复制和移动文件和目录在GNU / Linux的操作系统。这两个应用程序中缺少的一个功能是它们不显示任何进度条。如果你复制一个大文件或目录，你真的不知道复制过程需要多长时间才能完成，或者复制的数据百分比。你不会看到当前正在复制哪个文件，或者已经复制了多少文件。感谢`Advanced Copy`，一个补丁`Gnu Coreutils`，我们现在可以在 Linux 中添加进度条`cp`和`mv`命令，并在复制和/或移动大文件和目录时显示进度条。

Advanced Copy 是`GNU cp` 和 `GNU mv`序的 mod  。它添加了一个进度条，并提供有关复制或移动文件和文件夹时发生的情况的一些信息。不仅是进度条，它还显示数据传输速率、估计剩余时间和当前正在复制的文件名。

## **安装高级复制补丁以在 Linux 中向 cp 和 mv 命令添加进度条**

cp 和 mv 命令是`GNU coreutils`. 所以你需要`GNU coreutils`从`这里`下载最新的。

```zsh
> wget http://ftp.gnu.org/gnu/coreutils/coreutils-9.0.tar.xz
> tar xvJf coreutils-9.0.tar.xz
> cd coreutils-9.0/
> wget https://raw.githubusercontent.com/jarun/advcpmv/master/advcpmv-0.9-9.0.patch
> patch -p1 -i advcpmv-0.9-9.0.patch
> export FORCE_UNSAFE_CONFIGURE=1
> ./configure
> make
```

现在两个新的补丁的二进制文件即`cp`与`mv`将在中创建`coreutils-9.0/src`的文件夹。只需将它们复制到你的 $PATH 中，如下所示：

```zsh
$ cp ./src/cp /usr/local/bin/cpg
$ cp ./src/mv /usr/local/bin/mvg
```

该`cpg`和`mvg`命令有现在进度条的功能。每当你在复制或移动文件和目录时需要进度条时，只需添加`-g`如下标志：

```zsh
$ cpg -g ../coreutils-9.0.tar.xz ./
```

或使用`--progress-bar`标志：

```zsh
$ cpg --progress-bar ../coreutils-9.0.tar.xz ./
```

示例输出:

```zsh
[root@rumenz.com ~]# cpg -g nifi-1.14.0-bin.tar.gz test/
Copying at 119.3 MiB/s (about 0h 0m 7s remaining)
nifi-1.14.0-bin.tar.gz                                                               959.5 MiB /   1.3 GiB
[============================================>                            ] 71.0 %
```

在复制过程结束时，你将看到复制了多少文件、复制文件所用的时间以及每秒的数据传输速率。

```zsh
1 files (  1.3 GiB) copied in 25.5 seconds ( 53.0 MiB/s).
```

要递归复制目录及其子目录，只需添加`-R`标志：

```zsh
$ cpg -gR directory1/ directory2/
```

同样，要使用`mv`命令移动文件，请运行：

```zsh
$ mvg -g nifi-1.14.0-bin.tar.gz test/
```

或者，使用`--progress-bar`标志：

```zsh
$ mvg --progress-bar nifi-1.14.0-bin.tar.gz test/
```

要使用`mv`命令移动目录，请使用：

```zsh
$ mvg -g directory1/ directory2/
```

你还可以创建别名。编辑`~/.bashrc`文件：

在最后添加以下几行：

```zsh
alias cp='/usr/local/bin/cpg -gR'
alias mv='/usr/local/bin/mvg -g'
```

现在运行以下命令使更改生效：

```zsh
$ source ~/.bashrc
```

从现在开始，你可以只使用没有（或）标志的`cp`或`mv`命令。`-g` `--progress-bar`请注意，原始程序不会被覆盖。你仍然可以随时通过`/usr/bin/cp` 或 呼叫他们 `/usr/bin/mv`。

如果你经常复制或移动大量大文件和目录，推荐向`cp`和`mv`命令添加进度条功能。