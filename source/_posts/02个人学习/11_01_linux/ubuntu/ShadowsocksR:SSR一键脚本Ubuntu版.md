---
title: ShadowsocksR/SSR一键脚本Ubuntu版
tags:
  - 脚本
categories: linux
abbrlink: d16a02f5
date: 2024-01-09 13:57:47
---


# ShadowsocksR/SSR一键脚本Ubuntu版

今天把**Ubuntu**系统的ShadowsocksR/SSR的一键脚本也补齐了，目前已经上传到 [Github](https://github.com/hijkpw/scripts)。CentOS系统请参考： [ShadowsocksR/SSR一键脚本](https://ssrvps.org/archives/1031)

**提示：**这是自行搭建科学上网环境的第三步，请确认已经做了前两步：

1. 购买服务器。想要服务器速度快请参考 [搬瓦工购买服务器详细教程](https://ssrvps.org/archives/3480) 或 [购买AkkoCloud德国、美西CN2 GIA VPS](https://www.akkocloud.com/aff.php?aff=122&gid=7) ，想ip被封后免费换请参考：[购买vultr服务器超详细图文教程](https://ssrvps.org/archives/1288)
2. 连接到服务器，Windows系统请参考 [Bitvise连接Linux服务器教程](https://ssrvps.org/archives/1327)，mac用户请参考 [Mac电脑连接Linux教程](https://ssrvps.org/archives/1579)。

**注意：**

1. 如果有域名，强烈建议使用 [v2ray带伪装一键脚本](https://ssrvps.org/archives/1023)，能有效应付近些天的疯狂封杀，提供稳如狗的体验！
2. **BBR换成魔改BBR/BBR Plus/锐速清参考：[安装魔改BBR/BBR Plus/锐速(Lotserver)](https://ssrvps.org/archives/2770)。**

## 使用教程

登录到购买好的Linux服务器（windows可参考 [Bitvise连接Linux服务器教程](https://ssrvps.org/archives/1327)，mac用户请参考 [Mac电脑连接Linux教程](https://ssrvps.org/archives/1579)），在终端（黑框框）里输入如下命令：

```
bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/ubuntu_install_ssr.sh)
```

按回车键，屏幕出现“**请设置SSR的密码**（不输入则随机生成）” 的提示，按照提示设置密码（SSR的密码。例如1234abcd，**不是服务器网页后台的密码**）、端口（SSR的端口，例如12345，**不能是22和80**）并选择加密方式。

接下来屏幕上开始疯狂出现一堆你看得懂看不懂的东西，**如果安装过程中卡住，请耐心等待几分钟；**安装期间网络断开（windows上表现为黑框框中或者顶部标题出现disconnected字样，mac表现为终端出现“closed by remote host”或”broken pipe”），请重新连接后再次执行命令。安装成功的界面如下：

[![ubuntu ssr成功提示信息](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2024/01/09/brBUN6.png)

ubuntu ssr成功提示信息

**到此服务端配置完毕**，服务器可能会自动重启（**没提示重启则不需要**），windows终端出现“disconnected”，mac出现“closed by remote host”说明服务器重启了。

SSR一键脚本做了如下事情：

1. 更新系统到最新版
2. 安装bbr加速模块
3. 安装SSR并设置开机启动

## 客户端下载

接下来是**科学上网最后一步**：下载客户端，并参考页面中的配置教程进行配置：

[ShadowsocksR/SSR windows客户端下载](https://ssrvps.org/archives/1420)
[ShadowsocksR/SSR安卓客户端下载](https://ssrvps.org/archives/1365)
[ShadowsocksR/SSR mac客户端下载](https://ssrvps.org/archives/1425)
[ShadowsocksR/SSR ios客户端下载](https://ssrvps.org/archives/1373)

下载客户端配置好后，就可以愉快的上外网了！

## 其他

1. 查看ssr运行状态/配置：`bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/ubuntu_install_ssr.sh) info`
2. SSR管理命令：启动：`systemctl start shadowsocksR`；停止：`systemctl stop shadowsocksR`；重启：`systemctl restart shadowsocksR`；
3. 更改密码、端口、混淆参数的最简单方法：重新运行一键脚本；
4. 升级到最新版：重新运行一键脚本；
5. 卸载SSR：`bash <(curl -sL https://raw.githubusercontent.com/hijkpw/scripts/master/ubuntu_install_ssr.sh) uninstall`。