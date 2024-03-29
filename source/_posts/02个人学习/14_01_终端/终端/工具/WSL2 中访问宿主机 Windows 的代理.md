---
title: WSL2 中访问宿主机 Windows 的代理
tags:
  - 终端
categories: 工具
abbrlink: 5acbe4b2
date: 2022-06-23 23:45:00
---

# WSL2 中访问宿主机 Windows 的代理

最近疫情期间很多事情都得用代理连到学校内网去做，但是 WSL2 因为是通过虚拟机的方式实现，网络不再像 WSL1 一样与 Windows 共享，变成了一个新的网段，所以想使用宿主机的代理就比较麻烦了。

## WSL 中获取宿主机 IP

WSL 每次启动的时候都会有不同的 IP 地址，所以并不能直接用静态的方式来设置代理。WSL2 会把 IP 写在 `/etc/resolv.conf` 中，因此可以用下面指令获得宿主机 IP 。


```zsh
cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }'
```

WSL2 自己的 IP 可以用下面指令得到。

```zsh
hostname -I | awk '{print $1}'
```

## 设置代理

有了宿主机 IP 之后，就可以通过设置环境变量的方式设置代理了。这里端口需要自己填写，而且别忘了**代理软件中设置允许来自局域网的连接**。

```zsh
export http_proxy='http://<Windows IP>:<Port>' 
export https_proxy='http://<Windows IP>:<Port>' 
```

这种设置方式每次重启终端都得重新设置一遍，而且 IP 还得自己手打，还是挺麻烦的，这种时候就得靠脚本了！

**第 4 行 `<PORT>` 记得换成自己宿主机代理的端口！！！！！！**

```bash
#!/bin/sh 
hostip=$(cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }') wslip=$(hostname -I | awk '{print $1}')
port=<PORT>

PROXY_HTTP="http://${hostip}:${port}" 
set_proxy(){
		export http_proxy="${PROXY_HTTP}"
    export HTTP_PROXY="${PROXY_HTTP}"
    export https_proxy="${PROXY_HTTP}"
    export HTTPS_proxy="${PROXY_HTTP}"
}

unset_proxy(){
		unset http_proxy
    unset HTTP_PROXY    
    unset https_proxy    
    unset HTTPS_PROXY
}

test_setting(){ 
		echo "Host ip:" ${hostip}
    echo "WSL ip:" ${wslip}
    echo "Current proxy:" $https_proxy 
}

if [ "$1" = "set" ]
then    
		set_proxy

elif [ "$1" = "unset" ]
then
		unset_proxy 

elif [ "$1" = "test" ]
then
		test_setting 
else
		echo "Unsupported arguments."
fi 
```



**第 4 行 `<PORT>` 记得换成自己宿主机代理的端口！！！！！！**

如果希望 git 也能通过代理，可以分别在 `set_proxy` 和 `unset_proxy` 函数中加上如下命令

```bash
// 添加代理
git config --global http.proxy "${PROXY_HTTP}" 
git config --global https.proxy "${PROXY_HTTP}" 
// 移除代理 
git config --global --unset http.proxy 
git config --global --unset https.proxy 
```

之后运行 `. ./proxy.sh set` 就可以自动设置代理了。`unset` 可以取消代理，`test` 可以查看代理状态，能够用来检查环境变量是否被正确修改。

> 运行的时候不要忘记之前的 `.`，或者使用 `source ./proxy.sh set`，只有这样才能够修改环境变量
>
> 直接运行例如 `./proxy.sh set` 或者 `sh proxy.sh set`，这样会是运行在一个子 shell 中，对当前 shell 没有效果

另外可以在 `~/.bashrc` 中选择性的加上下面两句话，记得**将里面的路径修改成你放这个脚本的路径**。

```bash
alias proxy="source /path/to/proxy.sh" . /path/to/proxy.sh set 
```

第一句话可以为这个脚本设置别名 `proxy`，这样在任何路径下都可以通过 `proxy` 命令使用这个脚本了，之后在任何路径下，都可以随时都可以通过输入 `proxy unset` 来暂时取消代理。

第二句话就是在每次 shell 启动的时候运行该脚本实现自动设置代理，这样以后不用额外操作就默认设置好代理啦~

## 防火墙设置

如果前面完成后已经可以正常使用了，那么下面就不用管了。如果你代理已经设置正确了，尤其是已经允许来自局域网的访问，但是依旧无法正常访问，代理的软件的确也没收到请求，那么很可能是被 Windows 的防火墙给拦截了。

可以先尝试 ping 宿主机 ip 和 telnet 代理的端口，检查是否连通。如果无法连通，则多半是防火墙的问题。

![img](https://zinglix.xyz/img/in-post/WSL/4.png)

可以尝试在控制面板的防火墙面板左侧“允许应用或功能通过防火墙”，即上述界面中，打上勾允许代理软件通过防火墙。

或者可以尝试在高级设置中，入站规则中新建一个相关规则，如果你不是很了解，可以允许任何程序的任何协议，远程 IP 为 `172.16.0.0/12` 及 `192.168.0.0/16` 的入站请求。