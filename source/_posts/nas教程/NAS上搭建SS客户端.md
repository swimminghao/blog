---
title: NAS上搭建SS客户端
tags:
  - nas
categories: 工具
abbrlink: e215f7cb
date: 2022-02-28 19:57:47
---

# 群辉NAS上搭建SS客户端来连接远程并提供本地HTTP/Socks5代理

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/mxF2RW_20220217094542.png)

[主要参考了这片文章。](https://odcn.top/2018/10/24/854/群晖ss同步dropbox和googledrive/)

> [shadowsocks的Http代理桥接为SOCKS5代理，使群晖SS同步Dropbox和GoogleDrive](https://odcn.top/2018/10/24/854/群晖ss同步dropbox和googledrive/)

<iframe title="《shadowsocks的Http代理桥接为SOCKS5代理，使群晖SS同步Dropbox和GoogleDrive》—oD^Blog" class="wp-embedded-content" sandbox="allow-scripts" security="restricted" src="https://odcn.top/2018/10/24/854/%e7%be%a4%e6%99%96ss%e5%90%8c%e6%ad%a5dropbox%e5%92%8cgoogledrive/embed/#?secret=hiN7sQcs2t" data-secret="hiN7sQcs2t" width="600" height="338" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="box-sizing: border-box; margin: 0px; padding: 0px; border: 0px; max-width: 100%; position: absolute; clip: rect(1px, 1px, 1px, 1px);"></iframe>
**Contents**  [hide](https://1024.ee/index.php/2020/04/06/群辉nas上搭建ss客户端来连接远程并提供本地http-socks5代理/#) 

[1 下载docker – ss-privoxy](https://1024.ee/index.php/2020/04/06/群辉nas上搭建ss客户端来连接远程并提供本地http-socks5代理/#xia_zaidocker_-_ss-privoxy)

[2 高级设置](https://1024.ee/index.php/2020/04/06/群辉nas上搭建ss客户端来连接远程并提供本地http-socks5代理/#gao_ji_she_zhi)

[3 设置端口](https://1024.ee/index.php/2020/04/06/群辉nas上搭建ss客户端来连接远程并提供本地http-socks5代理/#she_zhi_duan_kou)

[4 最后查一查](https://1024.ee/index.php/2020/04/06/群辉nas上搭建ss客户端来连接远程并提供本地http-socks5代理/#zui_hou_cha_yi_cha)

[5 开启后查查日志，一切正常即可](https://1024.ee/index.php/2020/04/06/群辉nas上搭建ss客户端来连接远程并提供本地http-socks5代理/#kai_qi_hou_cha_cha_ri_zhi_yi_qie_zheng_chang_ji_ke)

[6 群辉自己的使用，看这里就行。](https://1024.ee/index.php/2020/04/06/群辉nas上搭建ss客户端来连接远程并提供本地http-socks5代理/#qun_hui_zi_ji_de_shi_yong_kai_zhe_li_jiu_xing)

[7指令](https://1024.ee/index.php/2020/04/06/群辉nas上搭建ss客户端来连接远程并提供本地http-socks5代理/#qun_hui_zi_ji_de_shi_yong_kai_zhe_li_jiu_xing)

## 下载docker – ss-privoxy

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2NcHvz_20220217094617.png)

## 高级设置

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/JTDf8S_20220217094637.png)注意是**文件**，不是文件夹。而且，文件名就是**config**, 不是config.conf之类。

```dockerfile
#配置文件

confdir /etc/privoxy
logdir /var/log/privoxy

actionsfile match-all.action # Actions that are applied to all sites and maybe overruled later on.
actionsfile default.action # Main actions file
actionsfile user.action # User customizations

filterfile default.filter
filterfile user.filter # User customizations

logfile privoxy.log
#下面这行的意思是监听来自任意地址的8118访问
listen-address :8118
toggle 1

enable-remote-toggle 0
enable-remote-http-toggle 0
enable-edit-actions 0
enforce-blocks 0

buffer-limit 4096
enable-proxy-authentication-forwarding 0

#启用这段全局代理模式#####################################
##下面一行表示将所有网址转发给本地7070端口，也就是本地的SS客户端所开放的端口。
#forward-socks5 / 127.0.0.1:7070 .
#启动这段只有部分网址走代理###############################
forward / .
#下面这一段表示需要走代理的规则
forward-socks5 .dropbox*.com 127.0.0.1:7070 .
forward-socks5 .*google*.* 127.0.0.1:7070 .
forward-socks5 .*facebook*.* 127.0.0.1:7070 .
forward-socks5 .*twitter*.* 127.0.0.1:7070 .
#forward-socks5 .*youtube*.* 127.0.0.1:7070 .
##########################################################
forwarded-connect-retries 0

accept-intercepted-requests 0
allow-cgi-request-crunching 0
split-large-forms 0
keep-alive-timeout 300
tolerate-pipelining 1
default-server-timeout 60
socket-timeout 300

#配置文件结束
```

## 设置端口

- 7070 For Socks5 – All Traffic
- 8118 for Http/Https – 部分域名翻墙，改conf文件即可

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/t5PYQv_20220217094721.png)

## 最后查一查

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/iwGoJB_20220217094740.png)

## 开启后查查日志，一切正常即可

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/S5u8G1_20220217094809.png)

## 群辉自己的使用，开这里就行。

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/L0Nwl6_20220217094828.png)
## 指令
```bash
docker run -d --restart=always \
-i -t -e SERVER_ADDR=n24.boom.party \
-e SERVER_PORT=12000 \
-e PASSWORD=Uk92CS \
-e METHOD=aes-256-cfb \
-e PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin \
-e TIME_OUT=300 \
-p 7070:7070 \
-p 8118:8118 \
-v /share/CACHEDEV1_DATA/Container/etc/privoxy/config:/etc/privoxy/config \
oldiy/ss-privoxy
```