---
title: glance内存分析工具使用
tags:
    - 终端
categories: 工具
date: 2022-04-21 11:29:00
---

# glance内存分析工具使用

[glances ](http://www.ttlsa.com/html/1952.html)是一款用于 [Linux](http://www.ttlsa.com/linux/)、BSD 的开源命令行系统监视工具，它使用 [Python](http://www.ttlsa.com/python/) 语言开发，能够监视 CPU、负载、内存、磁盘 I/O、网络流量、文件系统、系统温度等信息。本文介绍 glances 的使用方法和技巧，帮助 Linux 系统管理员了解掌握服务器性能。

# 前言

glances 可以为 Unix 和 Linux 性能专家提供监视和分析性能数据的功能，其中包括：

- CPU 使用率
- 内存使用情况
- 内核统计信息和运行队列信息
- 磁盘 I/O 速度、传输和读/写比率
- 文件系统中的可用空间
- 磁盘适配器
- 网络 I/O 速度、传输和读/写比率
- 页面空间和页面速度
- 消耗资源最多的进程
- 计算机信息和系统资源

glances 工具可以在用户的终端上实时显示重要的系统信息，并动态地对其进行更新。这个高效的工具可以工作于任何终端屏幕。另外它并不会消耗大量的 CPU 资源，通常低于百分之二。glances 在屏幕上对数据进行显示，并且每隔两秒钟对其进行更新。您也可以自己将这个时间间隔更改为更长或更短的数值。glances 工具还可以将相同的数据捕获到一个文件，便于以后对报告进行分析和绘制图形。输出文件可以是电子表格的格式 (.csv) 或者 html 格式。

## 两种方法安装 glances

通 常可以有两种方法安装 glances。第一种是通过编译源代码的方式，这种方法比较复杂另外可能会遇到软件包依赖性问题。还有一种是使用特定的软件包管理工具来安装 glances，这种方法比较简单。本文使用后者，需要说明的是在 CentOS 特定的软件包管理工具来安装。glances 要首先配置 EPEL repo，然后使用 pip 工具安装 glances。

### pip 软件包简介

通 常 Linux 系统管理员有两种方式来安装一个 Python 的软件包：一种是通过系统的包管理工具（如 apt-get）从系统的软件仓库里安装，一种是通过 Python 自己的包管理工具（如 easy_install 或者 pip）从 Python Cheese Shop 中下载安装。笔者推荐使用 pip。pip 是一个可以代替 easy_install 的安装和管理 Python 软件包的工具，是一个安装 Python 库很方便的工具，功能类似 YUM。注意 CentOS 和 Fedora 下安装 Python-pip 后，关键字不是 pip 而是 pip-Python。

### 首先配置 EPEL repo

如 果既想获得 RHEL 的高质量、高性能、高可靠性，又需要方便易用（关键是免费）的软件包更新功能，那么 Fedora Project 推出的 EPEL（Extra Packages for Enterprise Linux ，http://fedoraproject.org/wiki/EPEL）正好适合你。它是由 Fedora 社区打造，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目。装上了 EPEL，就像在 Fedora 上一样，可以通过 yum install package-name，随意安装软件。安装使用 EPEL 非常简单：

```sh
#wget http://ftp.riken.jp/Linux/fedora/epel/RPM-GPG-KEY-EPEL-6
#rpm --import RPM-GPG-KEY-EPEL-6
#rm -f RPM-GPG-KEY-EPEL-6
#vi /etc/yum.repos.d/epel.repo
# create new
[epel]
name=EPEL RPM Repository for Red Hat Enterprise Linux
baseurl=http://ftp.riken.jp/Linux/fedora/epel/6/$basearch/
gpgcheck=1
enabled=0
```

### 使用 pip 安装 glances

这里介绍一下安装过程：首先使用 YUM 安装 pip 工具，然后使用 pip 工具安装 glances 和用来显示系统温度的相关软件。

```bash
#yum --enablerepo=epel install Python Python-pip Python-devel gcc
# pip-Python install glances
```

### 安装 lm_sensors 软件

lm_sensors 的软件可以帮助我们来监控主板、CPU 的工作电压、风扇转速、温度等数据。这些数据我们通常在主板的 BIOS 也可以看到。当我们可以在机器运行的时候通过 lm_sensors 随时来监测着 CPU 的温度变化，可以预防呵保护因为 CPU 过热而会烧掉。lm_sensors 软件监测到的数据可以被 glances 调用并且显示 。

```bash
#yum install lm_sensor
# pip-Python install PySensors
```

## glances 使用方法

```bash
glances 是一个命令行工具包括如下命令选项：
-b：显示网络连接速度 Byte/ 秒
-B @IP|host ：绑定服务器端 IP 地址或者主机名称
-c @IP|host：连接 glances 服务器端
-C file：设置配置文件默认是 /etc/glances/glances.conf
-d：关闭磁盘 I/O 模块
-e：显示传感器温度
-f file：设置输出文件（格式是 HTML 或者 CSV）
-m：关闭挂载的磁盘模块
-n：关闭网络模块
-p PORT：设置运行端口默认是 61209
-P password：设置客户端 / 服务器密码
-s：设置 glances 运行模式为服务器
-t sec：设置屏幕刷新的时间间隔，单位为秒，默认值为 2 秒，数值许可范围：1~32767
-h : 显示帮助信息
-v : 显示版本信息
```

glances 工作界面如图 1
**图 1.glances 工作界面**

[![glances](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/04/21/SqJsHc.png)](http://www.ttlsa.com/wp-content/uploads/2013/08/1.jpg)

### glances 工作界面的说明 :

在图 1 的上部是 CPU 、Load（负载）、Mem（内存使用）、 Swap（交换分区）的使用情况。在图 1 的中上部是网络接口、Processes（进程）的使用情况。通常包括如下字段：

```bash
VIRT: 虚拟内存大小
RES: 进程占用的物理内存值
%CPU：该进程占用的 CPU 使用率

%MEM：该进程占用的物理内存和总内存的百分比

PID: 进程 ID 号
USER: 进程所有者的用户名
TIME+: 该进程启动后占用的总的 CPU 时间
IO_R 和 IO_W: 进程的读写 I/O 速率
NAME: 进程名称
NI: 进程优先级
S: 进程状态，其中 S 表示休眠，R 表示正在运行，Z 表示僵死状态。
```

在图 1 的中下部是传感器检测到的 CPU 温度。 在图 1 的下部是磁盘 I/O 的使用情况。 另外 glances 可以使用交互式的方式运行该工具，用户可以使用如下快捷键：

```bash
h ： 显示帮助信息
q ： 离开程序退出
c ：按照 CPU 实时负载对系统进程进行排序
m ：按照内存使用状况对系统进程排序
i：按照 I/O 使用状况对系统进程排序
p： 按照进程名称排序
d ： 显示磁盘读写状况
w ： 删除日志文件
l ：显示日志
s： 显示传感器信息
f ： 显示系统信息
1 ：轮流显示每个 CPU 内核的使用情况（次选项仅仅使用在多核 CPU 系统）
```

## glances 的高级应用

### glances 的结果输出方法

让 glances 输出 HTML 格式文件，首先安装相关软件包

```bash
# pip-Python install Jinja2
# glances -o HTML -f /var/www/html
```

下面可以使用 Firefox 浏览器输入网址: http://localhost/glances.html，结果如图 2。
图 2.输出 HTML 格式文件

[![glances](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/04/21/avd6Oo.png)

输出 csv 格式

该文件采用逗号分隔值（CSV）的格式，并且可以将其直接导入到电子表格中。

```bash
# glances -o CSV -f /home/cjh/glances.csv
```

下面使用 libreoffice 的 calc 工具打开 csv 格式文件（如图 3）

```bash
#libreoffice --calc %U /tmp/glances.csv
```

图 3.使用 libreoffice 的 calc 工具打开 csv 格式文件

[![glances](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/04/21/BQpsTJ.png)](http://www.ttlsa.com/wp-content/uploads/2013/08/3.jpg)

### glances 服务器 / 客户端工作方式

glances 支持服务器/客户端工作方式，可以实现远程监控。首先假设

服务器 IP 地址：10.0.2.14

客户端 IP 地址：10.0.2.15

确保二者都已经安装好 glances 软件包。

首先在服务器端启动；

```bash
# glances -s -B 10.0.2.15
glances server is running on 10.0.2.15:61209
```

可以看到 glances 使用的端口号是 61209，所以用户需要确保防火墙打开这个端口。

下面在客户端使用如下命令连接服务器如图 4：

```bash
# glances -c 10.0.2.15
```

图 4.客户端连接服务器

[![glances](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/04/21/bCppDU.png)

注意图 4 的左下角显示“Connected to 10.0.2.15”>表示客户端已经连接服务器成功。

通过 glances 输出颜色了解系统性能
图 5.是 glances 的一个输出界面

![glances](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/04/21/rd5V5J.png)



绿色表示性能良好，无需做任何额外工作；（此时 CPU 使用率、磁盘空间使用率和内存使用率低于 50%，系统负载低于 0.7）。

蓝色表示系统性能有一些小问题，用户应当开始关注系统性能；（此时 CPU 使用率、磁盘空间使用率和内存使用率在 50%-70% 之间，系统负载在 0.7-1 之间）。

品红表示性能报警，应当采取措施比如备份数据；（此时 CPU 使用率、磁盘空间使用率和内存使用率在 70%-90% 之间，，系统负载在 1-5 之间）。

红色表示性能问题严重，可能宕机；（此时 CPU 使用率、磁盘空间使用率和内存使用率在大于 90%，系统负载大于 5）。
