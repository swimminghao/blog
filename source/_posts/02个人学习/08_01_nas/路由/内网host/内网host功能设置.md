---
title: 华硕路由器内网host功能设置
tags:
    - asus
categories: 工具
date: 2023-05-16 21:39:00
---

# 华硕路由器修改本地Hosts局域网DNS转发设置

## 引子

联通网络，入户给配了智能光猫（集成光猫和无线路由器的功能），192.168.0.1 是智能光猫的地址。光猫后面就是华硕路由器，华硕路由器自动获取的 IP 地址 192.168.0.2 ，路由器本身的登录地址改为192.168.50.1，路由器的子设备为192.168.50.xxx。
在路由器的子设备中有一个 Synology(群辉) 的 Nas，端口映射到外网，有真实的外网 IP 地址，外网可以正常访问。但从光猫、路由器内的局域网中，都无法直接访问这个外网 IP 地址，Ping 也无法 Ping 通。

### 问题

综上，尴尬的事情就来了，Nas 绑定的域名无法在我的局域网内访问 Nas，因为域名指向的是我 Nas 的外网 Ip 地址，但是这个 Ip 并不能在我家的局域网内访问，所以，一旦连接上我家的 WIFI 后，访问我的 Nas 域名，就要自动指向到我 Nas 的内网 IP 地址。

### 解决

既然只有连接到我家的 Wifi 时需要将 Nas 的域名指向到我家的内网 Ip 地址，那么我就从路由器动手，在路由器作为 DNS 服务器的时候，将域名的请求指向 Nas 内网 IP 。

## 修改路由器 Hosts

华硕路由器本身无法直接修改 Hosts 文件，因为文件修改权限不够，就像 iOS 需要越狱、Android 需要 Root 一样，路由器也需要获取到管理员权限，才能对其核心文件进行修改，所以分两步，首先给路由器刷梅林固件，然后再使用 WinSCP 连接路由器，改 Hosts 文件。

### 华硕路由器刷梅林固件

因为这个不是本篇文章的主要内容，我就简单略过了，梅林固件的安装也非常简单，和安装原版固件基本相同。

#### 升级固件

系统管理 - 固件升级 - 新固件文件，选择梅林固件之后，点击上传，然后按照提示省级，重启就好了。

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/GFCI79_20230517001806.png)

**建议**
如果可以的话，还是进入系统管理 - 恢复/导出/上传设置里面把配置文件先备份一下，以后也方便。

#### 初始化路由器

升级完成进入[https://router.asus.com](https://router.asus.com/)，会出现自动设置向导，点击 跳过设置向导 （因为等下要恢复出厂），进入 系统管理 - 系统设置，勾选 Format JFFS partition at next boot，点击 应用本页面设置。
![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/N1jG4O_20230517001823.png)

#### 恢复路由器出厂设置

进入 系统管理 - 恢复/导出/上传设置，在 原厂默认值 一栏，点击 恢复 按钮，恢复出厂设置。

#### 重新配置路由器

重新进入[https://router.asus.com](https://router.asus.com/)，现在可以根据 设置向导 来配置你的路由器登陆密码和wifi密码，或者你可以稍后自己设置。

#### 配置软件中心

进入系统管理 - 系统设置，勾选 `Enable JFFS custom scripts and configs`，点击 应用本页面设置 （此步关系到软件中心能否正常使用）

这样，华硕路由器的梅林系统就安装完成了，安装梅林系统的同时，路由器的权限也随之获取了，我们下一步就开始干正事。

### 配置 Hosts 文件

因为已经取得了路由器的控制权限，所以我们首先将路由器的SSH功能开启，然后利用 WinSCP 登录到路由器中。

#### 开启路由器 SSH 功能

进入系统管理 - 系统设置 - SSH 连接，按照我的截图进行设置即可。

注意
我截图里面选择的是`只允许有线连接`，如果你是无线连接的记得选择 Wifi 可以使用 SSH ，更改 `LAN only` 那一项即可。



![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/Eilylb_20230517001845.png)

#### 建立 SSH 连接

用浏览器打开下面的链接
[ssh://admin@192.168.50.1](ssh://admin@192.168.50.1)

注意
上面SSH 链接的 admin 是我路由器的管理员帐号。
192.167.50.1 是我路由器的访问地址

如果在 Mac 下可能出现以下错误

复制代码

```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
```

这个错误我没有列全，因为涉及到地址等信息，如果出现以上错误，在 Mac 本地删除 `/Users/你的 mac 用户名/.ssh/known_hosts`中的所有文件即可。
Finder 中按 `shift + cmd + G` 或者直接 点击菜单上的 `前往 - 前往本地文件夹`
![WX20180824-113238](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/qkLEo7_20230517002003.png)
删除该文件夹下的所有内容
![WX20180824-113310](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/cI8Lvs_20230517002026.png)

我们再次连接，输入 `yes` ，进行确认。
![WX20180824-113737](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/xjIiMx_20230517002038.png)

然后输入路由器后台的登录密码，并出现以下画面，表示登录成功。
![WX20180824-113800](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/vbpbzH_20230517002053.png)

### ssh登录到路由器

#### 引入 Hosts

登入完成后，点击最上方的…，直到进入根目录，找到 jffs 目录，进入该目录。然后在 `jffs` 目录下，右键单击，新建一名为`dnsmasq.conf.add`的文件，在文件中输入

复制代码

```
addn-hosts=/jffs/configs/hosts
```

完成后，点击左上角保存按钮保存该文件，然后关闭。

#### 配置 Hosts

然后进入该文件夹下的 configs 文件夹，,右键单击，新建一名为hosts的文件，在文件中输入你需要的 hosts 内容

复制代码

```
192.168.50.218 www.swimminghao.top
192.168.50.1 www.example.com
```

完成后，点击左上角保存按钮保存该文件，然后关闭。

#### 重启 DNS 服务

打开刚才链接的 SSH 命令窗口，输入

复制代码

```
service restart_dnsmasq
```

![WX20180824-113807](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/ZGGkEm_20230517002218.png)
重启dnsmasq服务，来使hosts生效。
重启路由器后依然存在并且有效。这样，就可以了~！