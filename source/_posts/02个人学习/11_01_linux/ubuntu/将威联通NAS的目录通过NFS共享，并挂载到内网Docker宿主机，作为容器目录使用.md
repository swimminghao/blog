---
title: 将威联通NAS的目录通过NFS共享，并挂载到内网Docker宿主机，作为容器目录使用
tags:
  - docker
  - NFS
  - qnap
categories: linux
abbrlink: 9540b2ea
date: 2025-11-09 12:08:02
---
 
# 将威联通NAS的目录通过NFS共享，并挂载到内网Docker宿主机，作为容器目录使用

## 核心思路

先将远程内网主机的共享目录挂载到 Docker 宿主机的一个本地目录上，然后再将这个本地目录作为数据卷（Volume）或绑定挂载（Bind Mount）提供给 Docker 容器使用。

```
[ 内网共享主机 (NAS/Samba Server) ]
              |
              | (网络共享协议，如 NFS/SMB)
              |
[Docker 宿主机 ]
    | (本地文件系统挂载点，如 /mnt/nas/downloads)
    |
    | (Docker 卷或绑定挂载)
    |
[ Docker 容器 (如 Transmission, qBittorrent) ]
```



## 威联通nas开启NFS共享

因为我的内网装有qBittorrent 的docker宿主机是基于ubuntu的服务器，所以推荐使用NFS。NFS 在 Linux 环境下性能好，开销低，是首选方案。如果docker宿主机是 Windows ，或启用了NAS的 Samba服务，这种方法更合适。



### 创建共享目录

威联通依次打开控制台⇨权限⇨共享目录⇨创建⇨共享文件夹

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/09/veh7NI.png)

填写共享目录名称pt，记住文件夹名称，之后会用到。然后手动输入共享文件夹的路径，点击创建。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/09/YqFN0x.png)

### 激活威联通nas的nfs功能

威联通依次打开控制台⇨网络和文件服务⇨Win/Mac/NFS⇨NFS服务

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/09/jV1oWk.png)

激活NFS v4服务和启用manage-gids，关闭写入高速缓存。

- 启用最新的 NFS 主要版本v4：v4 在安全性（强认证）、性能（复合操作）和跨防火墙友好性（固定端口）方面有显著改进。
- 启用manage-gids：对于 Docker 挂载场景，这可以极大地简化容器内用户（如 `PUID=1000`, `PGID=1000`）对挂载目录的读写权限管理，避免出现 "Permission denied" 错误。
- 关闭写入高速缓存：对于 Docker 挂载，如果共享目录中存储的是重要的、不可再生的数据（如文档、数据库、个人文件），为了数据安全，**建议停用**。如果存储的是可以重新下载的缓存、临时文件或可丢弃的数据，可以启用以获得更好性能。

### 设置共享文件夹权限

创建完的共享文件夹，会在这里显示。点击图中按钮，编辑共享文件夹权限。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/09/Heirw8.png)

依次选择⇨选择权限类别：NFS主机访问⇨勾选访问权限⇨设置访问的IP地址或域名。

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2025/11/09/hHvrzU.png)

主机ip：`192.168.60.0/24` 是 **CIDR（无类别域间路由）** 的表示方法，它表示了一个包含256个IP地址的网络段，范围从192.168.60.0 到 192.168.60.255。。

Squash：选择读写。需要docker宿主机能够读写共享目录。

Squash选项：选择不Squash所有用户。

## 内网docker宿主机设置

### 创建挂载目录`/mnt/nas/downloads`

```
sudo mkdir /mnt/nas/downloads
```

### 挂载在目录，192.168.50.218:/pt

```
sudo mount -t nfs4 -o rw,hard,intr,timeo=300,retrans=3 192.168.50.218:/pt /mnt/nas/downloads
```

**参数解释**：

- `hard`：在 NFS 服务器故障时，程序会一直等待直到服务器恢复，保证数据一致性。
- `intr`：允许用户中断正在等待的 NFS 操作。
- `timeo=300`：设置超时时间为十分之300秒（即30秒）。
- `retrans=3`：设置重传次数为3次。

### 开机自动挂载

编辑 `/etc/fstab`文件，`sudo vim /etc/fstab`。

```
# 在 /etc/fstab 末尾添加
192.168.50.218:/pt   /mnt/nas/downloads   nfs4 rw,hard,intr,timeo=300,retrans=3,_netdev,auto   0   0
```

**注意**：`_netdev` 选项非常重要，它告诉系统在网络就绪后再挂载。

**整体含义**

将位于 IP 地址为 `192.168.50.218` 的服务器上的共享目录 `/pt`，通过网络文件系统版本 4 挂载到本地目录 `/mnt/nas/downloads`。

------

**参数解释**：

该配置行由 6 个字段组成，字段间用空格或制表符分隔：

1. **`192.168.50.218:/pt`**
   - **含义**：NFS 服务器和它提供的共享路径。
   - `192.168.50.218`：NFS 服务器的 IP 地址。
   - `:/pt`：服务器上被共享出来的目录路径。
2. **`/mnt/nas/downloads`**
   - **含义**：**挂载点**。这是本地的一个**空目录**，远程共享的内容将出现在这个目录里。
3. **`nfs4`**
   - **含义**：文件系统类型。这里指定使用 **NFS 版本 4**。
4. **`rw,hard,intr,timeo=300,retrans=3,_netdev,auto`**
   - **含义**：挂载选项，多个选项用逗号分隔。
   - `rw`：以**读写**模式挂载（而非只读 `ro`）。
   - `hard`：**硬挂载**。如果服务器无响应，客户端会持续重试。这对于保证数据一致性非常重要，但可能导致进程在访问挂载点时卡住。
   - `intr`：允许**中断**正在等待服务器响应的 I/O 操作。当使用 `hard` 挂载时，如果服务器宕机，可以用这个选项来终止被挂起的进程。
   - `timeo=300`：设置超时时间为 **300 十分之一秒（即 30 秒）**。这是客户端等待服务器响应的时间。
   - `retrans=3`：在放弃并返回错误之前，进行 **3 次** 重传。
   - `_netdev`：这是一个非常重要的选项，它告诉系统这是一个**网络设备**。系统会等待网络就绪后再尝试挂载，避免系统启动时因网络未准备好而挂载失败。
   - `auto`：使用 `mount -a` 命令（通常在系统启动时执行）时会**自动挂载**此文件系统。
5. **`0`**
   - **含义**：供 `dump` 备份工具使用。`0` 表示不需要被 `dump` 备份。
6. **`0`**
   - **含义**：供 `fsck` 磁盘检查工具使用。`0` 表示在启动时不需要检查此文件系统（因为它是网络文件系统，不在本地磁盘上）。

## docker宿主机运行qbittorrent容器，并挂载共享目录

```
docker run -d \
  --name=qbittorrent \
  -e PUID=$(id -u) \
  -e PGID=$(id -g) \
  -e TZ=Asia/Shanghai \
  --net=host \
  -v ~/software/docker/qbittorrent/config:/config \
  -v /mnt/nas/downloads/qbittorrent/downloads:/downloads \
  --restart unless-stopped \
  lscr.io/linuxserver/qbittorrent:latest
```

qbittorrent默认端口8080，可在config目录里的qbittorrent.conf配置文件修改。
