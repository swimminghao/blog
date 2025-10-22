---
title: CentOS7 源码安装redis4
tags:
  - 终端
categories: linux
abbrlink: a0ae5a65
date: 2025-10-22 10:58:39
---

# CentOS7 源码安装redis4

## 1. 安装编译依赖

```
# 安装开发工具和依赖
sudo yum groupinstall "Development Tools" -y
sudo yum install wget tcl -y
```

## 2. 下载并编译 Redis 4

```
# 创建安装目录
sudo mkdir -p /usr/local/redis
cd /usr/local/src

# 下载 Redis 4.0.14（最后一个 4.x 版本）
sudo wget http://download.redis.io/releases/redis-4.0.14.tar.gz

# 解压
sudo tar xzf redis-4.0.14.tar.gz
cd redis-4.0.14

# 编译安装
sudo make
sudo make install

# 创建配置目录
sudo mkdir -p /etc/redis
sudo mkdir -p /var/lib/redis
sudo mkdir -p /var/log/redis
```

## 3. 配置 Redis 服务

#### 复制配置文件

```
# 复制配置文件
sudo cp /usr/local/src/redis-4.0.14/redis.conf /etc/redis/

# 复制服务脚本
sudo cp /usr/local/src/redis-4.0.14/utils/redis_init_script /etc/init.d/redis
```

#### 修改配置文件

```
# 编辑 Redis 配置
sudo vim /etc/redis/redis.conf
```

主要修改以下配置：

```
# 以守护进程方式运行
daemonize yes

# 绑定地址（允许远程连接）
bind 0.0.0.0

# 端口
port 6379

# 数据目录
dir /var/lib/redis

# 日志文件
logfile "/var/log/redis/redis.log"

# 设置密码（可选）
# requirepass yourpassword

# 最大内存
maxmemory 256mb
maxmemory-policy allkeys-lru
```

#### 修改服务脚本

```
sudo vim /etc/init.d/redis
```

修改以下部分：

```
REDISPORT=6379
EXEC=/usr/local/bin/redis-server
CLIEXEC=/usr/local/bin/redis-cli
PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/etc/redis/redis.conf"
```

## 4. 创建系统服务

#### 创建 systemd 服务文件

```
sudo vim /etc/systemd/system/redis.service
```

添加以下内容：

```
[Unit]
Description=Redis persistent key-value database
After=network.target

[Service]
Type=forking
ExecStart=/usr/local/bin/redis-server /etc/redis/redis.conf
ExecStop=/usr/local/bin/redis-cli shutdown
Restart=always
User=root
Group=root
RuntimeDirectory=redis
RuntimeDirectoryMode=0755
PIDFile=/var/run/redis_6379.pid

[Install]
WantedBy=multi-user.target
```

## 5. 设置权限和启动服务

```
# 创建 Redis 用户（可选）
sudo useradd -r -s /bin/false redis

# 设置目录权限
sudo chown -R redis:redis /var/lib/redis
sudo chown -R redis:redis /var/log/redis

# 重新加载 systemd
sudo systemctl daemon-reload

# 启动 Redis 服务
sudo systemctl start redis

# 设置开机自启
sudo systemctl enable redis

# 检查服务状态
sudo systemctl status redis
```

## 6.验证安装

```
# 检查 Redis 版本
redis-cli --version
redis-server --version

# 测试连接
redis-cli ping

# 如果设置了密码
redis-cli -a yourpassword ping

# 查看 Redis 信息
redis-cli info server
```

#### 防火墙配置（如果需要远程访问）

```
# 开放 Redis 端口
sudo firewall-cmd --permanent --add-port=6379/tcp
sudo firewall-cmd --reload
```