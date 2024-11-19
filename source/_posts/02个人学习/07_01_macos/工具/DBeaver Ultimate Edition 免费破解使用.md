---
title: DBeaver Ultimate Edition 免费破解使用
tags:
  - mac
categories: 工具
abbrlink: 1afe5736
date: 2024-11-20 01:08:00
---

# DBeaver Ultimate Edition 免费破解使用

### 背景

免费版本不支持导出excel格式，所以得想办法使用ue版本。

### 系统环境

- 系统: macos 14.2.1/macos 10.13.6
- SOC: m1 pro/intel

### 安装DBeaver UE

官方安装包下载地址 [https://downloads.dbeaver.net/ultimate/23.3.0/dbeaver-ue-23.3.0-macos-x86_64.dmg](https://downloads.dbeaver.net/ultimate/23.3.0/dbeaver-ue-23.3.0-macos-x86_64.dmg)

### 下载agent

链接: [https://pan.baidu.com/s/1xWo75x9HiWDyQajGY9TUKw?pwd=a16r](https://www.shiqidu.com/url/aHR0cHM6Ly9wYW4uYmFpZHUuY29tL3MvMXhXbzc1eDlIaVdEeVFhakdZOVRVS3c/cHdkPWExNnI=) 提取码: a16r，下载并解压，放到一个专门的目录，下面会用到

### 修改 DBeaver UE

在应用程序里，右键显示包内容，找到dbeaver.ini (/Applications/DBeaverUltimate.app/Contents/Eclipse/dbeaver.ini)

找到 -vm，替换原始java路径，修改如下

```
-vm

# 你的java路径，如果本地没java环境，需要安装java环境。
/Library/Java/JavaVirtualMachines/zulu-20.jdk/Contents/Home/bin/java
```

然后在文件末尾添加如下两行

```
-Dlm.debug.mode=true

# 你下载的agent解压后的dbeaver-agent.jar的路径
-javaagent:/Users/xxx/Downloads/dbeaver-agent/dbeaver-agent.jar
```

### 修改Hosts

```
# dbeaver start
127.0.0.1 dbeaver.com
```

### 打开DBeaver UE

打开安装好的DBeaver UE，如果闪退或提示已损坏，请自行搜索解决办法。打开后，会提示让你输入注册码，输入下面的注册码即可

```
aYhAFjjtp3uQZmeLzF3S4H6eTbOgmru0jxYErPCvgmkhkn0D8N2yY6ULK8oT3fnpoEu7GPny7csN
sXL1g+D+8xR++/L8ePsVLUj4du5AMZORr2xGaGKG2rXa3NEoIiEAHSp4a6cQgMMbIspeOy7dYWX6
99Fhtpnu1YBoTmoJPaHBuwHDiOQQk5nXCPflrhA7lldA8TZ3dSUsj4Sr8CqBQeS+2E32xwSniymK
7fKcVX75qnuxhn7vUY7YL2UY7EKeN/AZ+1NIB6umKUODyOAFIc8q6zZT8b9aXqXVzwLJZxHbEgcO
8lsQfyvqUgqD6clzvFry9+JwuQsXN0wW26KDQA==
```

如无意外，目前应该已经破解成功了。

![破解成功](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2024/11/20/hYadXN.png)