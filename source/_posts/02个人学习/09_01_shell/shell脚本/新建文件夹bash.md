---
title: 新建文件夹bash
tags:
  - shell
categories: 工具
abbrlink: 64463bad
date: 2022-10-14 15:17:10
---



```bash
#!/bin/bash
for line in `cat apple.txt`
# 将 cat apple.txt的输出作为字符串,for逐个读取赋值line变量
#``为反引号
#也可以使用取值符号$(cat apple.txt)替代`cat apple.txt`
do
		mkdir $line
#取line值，执行新建文件夹命令
done

```

