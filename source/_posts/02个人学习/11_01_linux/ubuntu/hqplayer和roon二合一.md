---
title: hqplayer和roon二合一
tags:
  - hqplayer
  - roon
categories: linux
abbrlink: f3a01220
date: 2025-11-02 17:16:23
---

# hqplayer和roon二合一

## 第1步：安装并配置宿主机（Debian）

1.安装Debian：从Debian官网下载网络安装镜像，进行最小化安装。在软件选择时，只勾选“SSH server”和“Standard system utilities”。

2.更新系统：

```
sudo apt update && sudo apt upgrade -y
```

3.安装必要工具：

```
sudo apt install -y p7zip-full squashfs-tools systemd-container
```

p7zip-full用于解压7z文件，systemd-container提供了更现代的容器化管理工具（但我们仍会使用传统chroot）

## 第2步：准备HQPlayer OS的chroot环境

1.创建工作目录：

```
sudo mkdir -p /opt/hqplayer-chroot
```
2.解压HQPlayer Embedded：
```
sudo 7z x hqplayer-embedded-5.8.1-x64sse42.7z -o/opt/hqplayer-chroot
```
这会在/opt/hqplayer-chroot下得到hqplayer-embedded-5.8.1/文件夹，里面包含bin, lib, usr等目录。这就是我们chroot的基础。
我们需要把它挂载到一个临时目录，然后复制其中的所有文件。
```
# 创建挂载点
sudo mkdir -p /mnt/hqplayer-image
```
3.挂载镜像中的分区
您需要先找到镜像中的分区偏移量，然后挂载正确的分区。
第1步：查看镜像的分区信息
```
sudo fdisk -l hqplayer-embedded-5.8.1-x64sse42.img
```
fdisk 输出显示镜像中有两个分区：
分区1：Start=2048, Size=27.8M, Type=”Microsoft basic data” (这很可能是EFI系统分区)
分区2：Start=59392, Size=3G, Type=”Linux filesystem” (这是我们要的根文件系统分区)
挂载分区（分区2）

```
# 计算分区2的偏移量 (59392 sectors * 512 bytes/sector)
OFFSET=$((59392 * 512))

# 挂载分区2到挂载点
sudo mount -o loop,offset=$OFFSET hqplayer-embedded-5.8.1-x64sse42.img /mnt/hqplayer-image
```

复制文件到chroot目录

```
# 查看挂载的内容
ls -la /mnt/hqplayer-image/

# 复制所有文件到chroot目录（保留权限和属性）
sudo cp -a /mnt/hqplayer-image/* /opt/hqplayer-chroot/

# 卸载镜像
sudo umount /mnt/hqplayer-image
```

4.创建必要的系统目录：

```
sudo mkdir -p /opt/hqplayer-chroot/{proc,sys,dev,run,var/lib,etc,tmp}
sudo chmod 1777 /opt/hqplayer-chroot/tmp
```

5.复制DNS解析配置：

```
sudo cp /etc/resolv.conf /opt/hqplayer-chroot/etc/
```

6.让 chroot 环境访问主机音频设备
首先识别音频设备

```
# 在宿主机上查看所有音频设备
aplay -l

# 或者使用更详细的方式
cat /proc/asound/cards

# 查看 USB 音频设备详情
lsusb | grep -i audio
```

可以看到您的音频设备配置：

card 0: AB13X USB Audio (USB DAC) - 这是您的外部USB音频设备

card 1: Ensoniq AudioPCI - 这是主板内置声卡

现在让我们确保chroot环境中的HQPlayer能够访问这些设备。

配置chroot环境访问USB DAC, 修改启动脚本确保正确映射

## 第3步：配置chroot环境

我们需要一个脚本来自动挂载必要的文件系统并进入chroot。

## 创建挂载和进入chroot的脚本：

```
sudo nano /usr/local/bin/start-hqplayer-chroot.sh
#!/bin/bash
HQPLAYER_ROOT="/opt/hqplayer-chroot"

# 挂载必要的文件系统
mount --bind /proc "$HQPLAYER_ROOT/proc"
mount --bind /sys "$HQPLAYER_ROOT/sys"
mount --bind /dev "$HQPLAYER_ROOT/dev"
mount --bind /dev/pts "$HQPLAYER_ROOT/dev/pts"
mount --bind /dev/snd "$HQPLAYER_ROOT/dev/snd"
mount --bind /run "$HQPLAYER_ROOT/run"

# 确保USB设备可访问（重要！）
mount --bind /dev/bus/usb "$HQPLAYER_ROOT/dev/bus/usb" 2>/dev/null || true

# 确保使用最新的DNS配置
cp /etc/resolv.conf "$HQPLAYER_ROOT/etc/resolv.conf"

# 运行HQPlayer
exec chroot "$HQPLAYER_ROOT" /usr/bin/hqplayerd
```

## 停止脚本

```
sudo nano /usr/local/bin/stop-hqplayer-chroot.sh
#!/bin/bash
HQPLAYER_ROOT="/opt/hqplayer-chroot"

# 杀死所有hqplayerd进程
pkill -9 -f "hqplayerd" 2>/dev/null

# 卸载文件系统（确保正确的顺序）
umount -f "$HQPLAYER_ROOT/dev/bus/usb" 2>/dev/null
umount -f "$HQPLAYER_ROOT/dev/snd" 2>/dev/null
umount -f "$HQPLAYER_ROOT/dev/pts" 2>/dev/null
umount -f "$HQPLAYER_ROOT/dev" 2>/dev/null
umount -f "$HQPLAYER_ROOT/sys" 2>/dev/null
umount -f "$HQPLAYER_ROOT/proc" 2>/dev/null
umount -f "$HQPLAYER_ROOT/run" 2>/dev/null

sleep 2
```

设置正确的权限

```
sudo chmod +x /usr/local/bin/start-hqplayer-chroot.sh
sudo chmod +x /usr/local/bin/stop-hqplayer-chroot.sh

sudo systemctl daemon-reload
```

重新加载并测试服务

```
# 重新加载systemd配置
sudo systemctl daemon-reload

# 手动测试启动脚本
sudo /usr/local/bin/start-hqplayer-chroot.sh

# 检查hqplayerd是否运行
ps aux | grep hqplayerd

# 检查端口监听
sudo ss -tlnp | grep :8088

# 如果运行正常，停止它
sudo /usr/local/bin/stop-hqplayer-chroot.sh
```

系统使用的是 cgroup2（统一层次结构），而不是传统的cgroup1。这可能是导致服务启动失败的原因，因为一些旧的应用或脚本可能不完全兼容cgroup2。
第1步：检查系统是否支持cgroup1

```
# 检查是否启用了cgroup1兼容性
cat /proc/filesystems | grep cgroup

# 检查内核启动参数
cat /proc/cmdline
```

## 修改systemd服务文件以兼容cgroup2

```
sudo nano /etc/systemd/system/hqplayer-chroot.service
```

使用以下配置：

```
[Unit]
Description=HQPlayer Embedded Daemon in Chroot
After=network.target
Requires=network.target

[Service]
Type=simple
ExecStart=/usr/local/bin/start-hqplayer-chroot.sh
ExecStop=/usr/local/bin/stop-hqplayer-chroot.sh
Restart=on-failure
RestartSec=5
TimeoutStartSec=300
TimeoutStopSec=30

# 关键配置：禁用所有cgroup相关功能
Delegate=no
CPUAccounting=no
MemoryAccounting=no
IOAccounting=no
TasksAccounting=no
IPAccounting=no
BlockIOAccounting=no

# 避免cgroup2相关问题
#Slice=
#ControlGroup=
[Install]
WantedBy=multi-user.target
```

## 启动服务并检查

```
sudo systemctl start hqplayer-chroot.service
systemctl status hqplayer-chroot.service
```

dns或者删除符号链接并创建实际文件

```
# 删除符号链接
sudo rm /opt/hqplayer-chroot/etc/resolv.conf

# 创建实际的 resolv.conf 文件
sudo cp /etc/resolv.conf /opt/hqplayer-chroot/etc/resolv.conf

# 确认现在是普通文件
ls -la /opt/hqplayer-chroot/etc/resolv.conf
```

## 第4步：执行精简

首先查看当前大小

```
sudo du -sh /opt/hqplayer-chroot/ --exclude=proc --exclude=sys --exclude=dev --exclude=run
```

删除最大的文件（镜像文件）

```
# 删除原始镜像文件（节省最多空间）
sudo rm /opt/hqplayer-chroot/hqplayer-embedded-5.8.1-x64sse42.img
sudo rm /opt/hqplayer-chroot/hqplayer-embedded-5.8.1-x64sse42.7z

# 删除备份文件（如果有）
sudo rm -rf /opt/hqplayer-chroot-backup 2>/dev/null || true
```

删除firmware文件

```
# 删除firmware（通常不需要）
sudo rm -rf /opt/hqplayer-chroot/lib/firmware
```

查看精简效果

```
# 查看当前大小
sudo du -sh /opt/hqplayer-chroot/ --exclude=proc --exclude=sys --exclude=dev --exclude=run

# 查看各目录大小
sudo du -sh /opt/hqplayer-chroot/* --exclude=proc --exclude=sys --exclude=dev --exclude=run | sort -hr

# 查看usr目录大小
sudo du -sh /opt/hqplayer-chroot/usr/*
```

验证功能正常

```
# 重启HQPlayer服务测试
sudo systemctl restart hqplayer-chroot.service

# 检查服务状态
systemctl status hqplayer-chroot.service

# 测试音频功能
sudo chroot /opt/hqplayer-chroot aplay -l

# 测试网络功能
sudo chroot /opt/hqplayer-chroot /bin/bash -c "ping -c 2 8.8.8.8"
```