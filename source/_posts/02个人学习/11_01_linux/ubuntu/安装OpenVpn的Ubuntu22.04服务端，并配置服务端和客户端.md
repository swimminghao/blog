---
title: 安装OpenVpn的Ubuntu22.04服务端，并配置服务端和客户端
tags:
  - openvpn
  - roon
categories: linux
abbrlink: 3b65ab0a
date: 2025-11-07 12:12:38
---
# 安装OpenVpn的Ubuntu22.04服务端和Windows10客户端
®
本文将在Ubuntu22.04服务器安装OpenVpn服务端，并在配置OpenVpn客户端ovpn文件。

## 安装OpenVpn和Easy-RSA

Easy-RSA主要用来生成CA证书，服务端证书和key，客户端证书和key。先登录Ubuntu服务器，然后用root身份执行下文的命令。

```
sudo apt update
sudo apt install openvpn easy-rsa

# 安装完可以查看下openvpn版本
openvpn --version
# 我的是OpenVPN 2.4.12版本
```

## 制作所需的证书

执行如下命令

```
cd /usr/share/easy-rsa

# 拷贝一份vars文件
cp vars.example vars

# 编辑vars文件
vim vars

# 在最后一行添加如下内容，这个 KEY_NAME 下文会用到，请记住
export KEY_NAME="myserver"
```

开始制作证书

```
# 执行完这个命令，会在/usr/share/easy-rsa下多出个pki目录
./easyrsa init-pki
chmod 777 pki

# 创建ca证书，此时在pki目录下会多出 ca.crt
./easyrsa build-ca nopass

# 创建服务端证书，命令中的myserver要换成前面vars文件中设置的KEY_NAME
# 此时，pki下的issued和private目录会多出myserver.crt、myserver.key 等文件
./easyrsa build-server-full myserver nopass

# 创建客户端证书
# 再次查看，pki下的issued和private目录会多出myclient.crt、myclient.key 等文件
./easyrsa build-client-full myclient nopass

# 创建DH密钥
./easyrsa gen-dh
```

## 配置OpenVpn服务端

拷贝配置文件

```
cp /usr/share/doc/openvpn/examples/sample-config-files/server.conf /etc/openvpn/
```

拷贝刚才创建的CA证书，服务端证书和key(私钥)，客户端证书和key，以及创建tls的key

```
cd /etc/openvpn/
cp /usr/share/easy-rsa/pki/ca.crt .
cp /usr/share/easy-rsa/pki/dh.pem .
cp /usr/share/easy-rsa/pki/issued/myserver.crt server/
cp /usr/share/easy-rsa/pki/private/myserver.key server/
cp /usr/share/easy-rsa/pki/issued/myclient.crt client/
cp /usr/share/easy-rsa/pki/private/myclient.key client/

# 生成tls需要的key
openvpn --genkey tls-auth ta.key
```

编辑服务端配置文件server.conf，改成如下内容，安全起见我把默认端口1194，改成了11000：

```
# vpn服务端口
port 11000

# 使用tcp协议
proto tcp

# vpn使用的虚拟网卡设备
dev tun

# CA以及证书和私钥
ca /etc/openvpn/ca.crt
cert /etc/openvpn/server/myserver.crt
key /etc/openvpn/server/myserver.key  # This file should be kept secret
dh /etc/openvpn/dh.pem

# vpn服务端要给客户端分配的IP地址（以及网段）
server 10.8.100.0 255.255.255.0

ifconfig-pool-persist /var/log/openvpn/ipp.txt

# vpn服务端要帮客户端转发的网段（即192.168.50.0/255.255.255.254这些地址，才会经过vpn）
# 代表 vpn 转发 192.168.50.1 - 192.168.50.254的请求
push "route 192.168.50.0 255.255.255.0"

# 让客户端使用下面的dns服务器
push "dhcp-option DNS 114.114.114.114"
push "dhcp-option DNS 8.8.8.8"

# 客户端之间可以互相访问
client-to-client

# 支持多个客户端连接服务端
duplicate-cn
keepalive 10 120

# 开启tls安全，上文的ta.key就用在这里
tls-auth ta.key 0 # This file is secret
cipher AES-256-CBC

# 启用lzo压缩
comp-lzo

# 运行最多30个客户端连接
max-clients 30

# 持久key和tun设备
persist-key
persist-tun

status /var/log/openvpn/openvpn-status.log
log    /var/log/openvpn/openvpn.log

# 记录的日期等级为3
verb 3
```

启动vpn服务端

```
systemctl start openvpn@server
systemctl status openvpn@server

# 感兴趣的话，可以查看日志
tail -f /var/log/openvpn/openvpn.log
```

如果你的Ubuntu系统是云服务器，通常管理后台可以设置防火墙，建议在Ubuntu关闭ufw：

```
# 停止ufw
sudo ufw disable
# 禁用ufw
sudo systemctl stop ufw
sudo systemctl disable ufw
```

## 设置IP转发

如果你搭建的VPN服务器所在的内网还有其他云服务器，你想访问其他云服务器就应该设置ip转发。就如上文描述的，我们在VPN服务端配置文件server.conf中配置了 `push "route 192.168.0.0 255.255.255.224"`。

打开内核ip转发：

```
# 编辑文件/etc/sysctl.conf
vim /etc/sysctl.conf

# 打开如下一行，改为：
net.ipv4.ip_forward = 1

# 然修改永久生效
sysctl -p
```

设置防火墙（iptables ）转发规则：

```
# 设置源地址翻译SNAT：把来自10.8.100.0/24的流量在离开网络接口ens3前，修改源ip为192.168.50.14
# enp0s31f6为你自己的网卡名称，可使用`ip addr show`查看。
# 192.168.50.14为安装openvpn 服务端的主机ip
iptables -t nat -A POSTROUTING -s 10.8.100.0/24 -o enp0s31f6 -j SNAT --to-source 192.168.50.14
```

重启下vpn服务端：`systemctl start openvpn@server`。到这里，服务端安装完成。

## 配置客户端ovpn文件

在openvpn官网下载客户端：[这里](https://openvpn.net/client/client-connect-vpn-for-windows/)。下载好以后点击运行，进行安装。

```
# 准备好Ubuntu上的这4个文件
/etc/openvpn/ca.crt
/etc/openvpn/ta.key
/etc/openvpn/client/myclient.crt
/etc/openvpn/client/myclient.key
```

## 创建客户端配置文件

打开终端，复制粘贴以下脚本指令，脚本会在~/openvpn-clients/目录下，创建一个client1.ovpn客户端配置文件，内容如下：

```bash
# 创建客户端的配置
cat > ~/openvpn-clients/client1.ovpn << 'EOF'
client
dev tun
proto tcp
remote 你的公网IP地址 11000
resolv-retry infinite
nobind
persist-key
persist-tun
remote-cert-tls server
verb 3
ca ca.crt
cert myclient.crt
key myclient.key
tls-auth ta.key 1
comp-lzo
route 192.168.50.0 255.255.255.0

<ca>
EOF

sudo cat /etc/openvpn/ca.crt >> ~/openvpn-clients/client.ovpn
cat >> ~/openvpn-clients/client1.ovpn << 'EOF'
</ca>

<cert>
EOF

sudo cat /etc/openvpn/client/myclient.crt >> ~/openvpn-clients/client1.ovpn
cat >> ~/openvpn-clients/client1.ovpn << 'EOF'
</cert>

<key>
EOF

sudo cat /etc/openvpn/client/myclient.key >> ~/openvpn-clients/client1.ovpn
cat >> ~/openvpn-clients/client1.ovpn << 'EOF'
</key>

<tls-auth>
EOF

sudo cat /etc/openvpn/ta.key >> ~/openvpn-clients/client1.ovpn
cat >> ~/openvpn-clients/client1.ovpn << 'EOF'
</tls-auth>
EOF
```

**注意替换，你的公网IP地址和修改路由转发参数**

上面的 `route 192.168.50.0 255.255.255.0` 也是告诉服务端，客户端访问这些 192.168.50.0/255.255.255.0 地址时，才经过VPN。

保存配置文件，然后启动 openvpn gui 客户端。通常，客户端会有opvn文件导入功能，导入客户端，点连接即可。连接成功显示如下：

[![img](https://cdn.jsdelivr.net/gh/jasonz666/imgHost/blogPic/11.png)](https://cdn.jsdelivr.net/gh/jasonz666/imgHost/blogPic/11.png)

然后，在CMD中ping VPN服务端的地址（下面10.8.100.1是vpn分配的网关地址，vpn客户端分配到的是10.8.100.6，192.168.50.28是vpn服务器内网ip，192.168.50.4是vpn服务所在内网的另一条机器的内网ip）：
[![img](https://cdn.jsdelivr.net/gh/jasonz666/imgHost/blogPic/22.png)](https://cdn.jsdelivr.net/gh/jasonz666/imgHost/blogPic/22.png)
至此，vpn客户端和服务端全部安装完成。