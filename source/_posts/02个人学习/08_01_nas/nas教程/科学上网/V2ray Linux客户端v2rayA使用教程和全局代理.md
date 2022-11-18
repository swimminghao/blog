---
title: NAS上搭建SS客户端
tags:
  - nas
categories: 工具
abbrlink: e215f7cb
date: 2022-11-18 11:45:00
---

# V2ray Linux客户端v2rayA使用教程和全局代理

源地址：https://github.com/v2rayA/v2rayA

我这里直接只用docker

```
docker run -d \
-p 2017:2017 \
-p 20170-20172:20170-20172 \
--restart=always \
--name v2raya \
--privileged=true \
-v /etc/v2raya:/etc/v2raya \
mzz2017/v2raya
```

部署完毕后，访问该机器的2017端口即可使用，如http://localhost:2017

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/11/18/rKx6ni.png)

添加完节点后在Linux里添加socks或者http(s)代理

参考：https://zhuanlan.zhihu.com/p/46973701

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/11/18/4OAjT0.png)

## 方法一：（推荐使用）

> 为什么说这个方法推荐使用呢？因为他只作用于当前终端中，不会影响环境，而且命令比较简单

在终端中直接运行：

```
export http_proxy=http://proxyAddress:port
```

如果你是SSR,并且走的http的代理端口是12333，想执行wget或者curl来下载国外的东西，可以使用如下命令：

```
export http_proxy=http://127.0.0.1:20171
```

如果是https那么就经过如下命令：

```
export https_proxy=http://127.0.0.1:20171
```

## 方法二 ：

> 这个办法的好处是把代理服务器永久保存了，下次就可以直接用了

把代理服务器地址写入shell配置文件.bashrc或者.zshrc 直接在.bashrc或者.zshrc添加下面内容

```
export http_proxy="http://localhost:port"
export https_proxy="http://localhost:port"
```

或者走socket5协议（ss,ssr）的话，代理端口是1080

```
export http_proxy="socks5://127.0.0.1:20170"
export https_proxy="socks5://127.0.0.1:20170"
```

或者干脆直接设置ALL_PROXY

```
export ALL_PROXY=socks5://127.0.0.1:20170
```

最后在执行如下命令应用设置

```
source ~/.bashrc
```

或者通过设置alias简写来简化操作，每次要用的时候输入setproxy，不用了就unsetproxy。

```
 alias setproxy="export ALL_PROXY=socks5://127.0.0.1:20170" alias unsetproxy="unset ALL_PROXY"
```

## 方法三:

改相应工具的配置，比如apt的配置

```
sudo vim /etc/apt/apt.conf
```

在文件末尾加入下面这行

```
Acquire::http::Proxy "http://proxyAddress:port"
```

> 重点来了！！如果说经常使用git对于其他方面都不是经常使用，可以直接配置git的命令。

## 使用ss/ssr来加快git的速度

直接输入这个命令就好了

```
git config --global http.proxy 'socks5://127.0.0.1:20170' 
git config --global https.proxy 'socks5://127.0.0.1:20170'
```

如何全局代理

原地址:https://www.xpath.org/blog/0015707906789019d24776f72a14223abc103b4f80fe003000

安装redsocks

```
apt install redsocks
```

安装完成以后,默认配置文件：`/etc/redsocks.conf`
一些常用的命令：

```
sudo service redsocks start     #打开redsocks
sudo service redsocks stop      #停止redsocks
sudo service redsocks restart   #重启redsocks
sudo systemctl enable redsocks  #开机启动redsocks
sudo systemctl disable redsocks  #停止开机启动redsocks
```

### 配置redsocks

编辑redsocks配置文件`/etc/redsocks.conf`
找到redoscks节点

```
redsocks {
        /* `local_ip' defaults to 127.0.0.1 for security reasons,
         * use 0.0.0.0 if you want to listen on every interface.
         * `local_*' are used as port to redirect to.
         */
        local_ip = 127.0.0.1;
        local_port = 12345; //这个端口默认就行,只要跟你以后iptables,重定向的端口一样就ok

        // `ip' and `port' are IP and tcp-port of proxy-server
        // You can also use hostname instead of IP, only one (random)
        // address of multihomed host will be used.
        ip = 127.0.0.1; //本地v2ray客户端,地址就是127.0.0.1
        port = 20170;    //v2ray客户端的端口,默认就是1080


        // known types: socks4, socks5, http-connect, http-relay
        type = socks5; //使用的代理类型

        // login = "foobar";
        // password = "baz";
}
```

修改正确的配置以后,重启redsocks即可

### 配置iptables

大佬写了一个脚本附带注释,方便不太懂iptables的同学们来使用

```bash
#file name iptables.sh

#!/bin/bash
if [ $# -lt 1 ]

#不重定向目的地址为服务器的包
then
    echo -en "\n"

    echo "Iptables redirect script to support global proxy on ss for linux ... "
    echo -en "\n"
    echo "Usage : ${0} action [options]"
    echo "Example:"
    echo -en "\n"
    echo "${0} start server_ip To start global proxy"
    echo "${0} stop To stop global proxy"
    echo -en "\n"

else
    if [ ${1} == 'stop' ]
    then
        echo "stoping the Iptables redirect script ..."
        sudo iptables -t nat -F
   fi
   if [ ${1} == 'start' ]
   then
       if [ $# -lt 2 ]
       then
            echo -e "\033[49;31mPlease input the server_ip ...\033[0m"
       else
           ##不重定向目的地址为服务器的包  
           sudo iptables -t nat -A OUTPUT -d ${2} -j RETURN #请用你的shadowsocks服务器的地址替换$SERVER_IP
           # #不重定向私有地址的流量
           sudo iptables -t nat -A OUTPUT -d 10.0.0.0/8 -j RETURN
           sudo iptables -t nat -A OUTPUT -d 172.16.0.0/12 -j RETURN
           sudo iptables -t nat -A OUTPUT -d 192.168.0.0/16 -j RETURN

           #不重定向保留地址的流量,这一步很重要
           sudo iptables -t nat -A OUTPUT -d 127.0.0.0/8 -j RETURN

            # #重定向所有不满足以上条件的流量到redsocks监听的12345端口
           sudo iptables -t nat -A OUTPUT -p tcp -j REDIRECT --to-ports 12345 #12345是你的redsocks运行的端口,请根据你的情况替换它
     fi
  fi
fi
```

使用方法很简单,就两条命令

```
#ip是你的sha服务器的ip,开启全局代理
./iptables.sh start ip
./iptables.sh stop
#结束全局代理,这句是不用全局代理之后,必须运行的,否则是没有办法上网的
```