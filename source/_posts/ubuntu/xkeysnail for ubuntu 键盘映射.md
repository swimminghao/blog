---
title: xkeysnail for ubuntu 键盘映射
tags:
  - 键盘
categories: 设备
abbrlink: 30ffdeaa
date: 2022-03-14 13:57:47![img.png](img.png)
---
# xkeysnail for ubuntu 键盘映射

```bash
sudo xkeysnail xkeysnail.py --device /dev/input/event4    'AT Translated Set 2 keyboard' 
xhost +SI:localuser:root
sudo xkeysnail --watch xkeysnail-gte60.py --device /dev/input/event24    'GT BLE60 0AEBCB Keyboard'
```

