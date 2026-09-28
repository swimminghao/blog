---
title: 在Ubuntu下安装CUPS使惠普打印机HP M1005实现局域网手机电脑Airprint无线打印
tags:
  - hp1005
categories: linux
date: 2026-09-28 11:08:26
abbrlink: 2e3d7c07
---

# 在Ubuntu下安装CUPS使惠普打印机HP M1005实现局域网手机电脑Airprint无线打印

环境：Ubuntu20
设备：HP M1005
局域网设置静态路由表，使192.168.100.x-192.168.127.x实现跨网互访。

以下操作均使用ROOT用户身份并开启SSH登录选项。如未开启，则先手动执行以下命令开启ROOT身份的SSH选项。

```
vim /etc/ssh/sshd_config
找到如下段落修改PermitRootLogin为YES并去除注释
修改完毕后如下图所示，Esc退出：wq!保存。
#LoginGraceTime 2m
PermitRootLogin yes
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10
```

接下来执行安装CUPS软件

```
apt-get install hplip
apt-get install hpijs-ppds -y
apt-get install cups -y
```

手动修改CUPS访问权限

```
cupsctl --remote-admin --remote-any --share-printers
```

此时，你的CUPS已经可以正常访问，在浏览器输入地址https://ip:631回车访问，**一定使用SSH的root账号密码登录**。

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2026/09/28/yxXUrz.png)CUPS界面

安装hp插件

```
hp-plugin
一路YES
```

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2026/09/28/qC9VOq.png)

如下载慢卡在0%，可下载链接内博主已经上传完毕的文件，放入/root根目录，并选择p，手动指定/root路径进行安装。[下载地址](https://cloud.189.cn/t/7R7VneQNZ3A3)（访问码：ay6x）
当提示Done并且无报错时，则表示惠普插件安装完毕。

现在，去浏览器界面找到Administration标签，点击add printert添加新的打印机。如果提示已禁止，则证明此时你使用的登录身份不是root，所以无法添加。

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2026/09/28/GZivgw.png)

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2026/09/28/t4Ian0.png)

接下来在**Local Printers**选择你的打印机，并点击Continue。在下一步的页面中自定义打印机的名称，并勾选Share，否则打印机将无法被他人可见。

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2026/09/28/O7DV7s.png)

然后再最后这个页面选择和你打印机相同的驱动，一定选择带hpijs后缀的驱动。

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2026/09/28/ql2UGE.png)

然后一台打印机就展现在你的打印机列表里了，现在选择Print Test Page，看看打印机是不是响起来了！？

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2026/09/28/3F0RDZ.png)

接下来在电脑和手机添加打印机地址，就可以实现局域网内无线打印啦！
以上就是在Ubuntu下安装CUPS使惠普打印机HP M1005实现局域网手机电脑Airprint无线打印的方法。