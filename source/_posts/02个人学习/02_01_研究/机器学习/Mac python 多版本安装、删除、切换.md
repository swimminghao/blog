---
title: Mac python 多版本安装、删除、切换
tags:
  - 总结
  - python
categories: 学习
abbrlink: f31abf10
date: 2023-11-21 15:08:00
---

# Mac python 多版本安装、删除、切换

### **一、安装pyenv**
```bash
  brew install pyenv
```

![img](https://pic1.zhimg.com/80/v2-5a2f5e1d54dcec75038053fe93d59ca8_1440w.webp)

### 二、查看当前安装的pyenv的版本

```text
pyenv -v
```

![img](https://pic2.zhimg.com/80/v2-2bfc185eb3484d4807e486261cae36b9_1440w.webp)

### 三、**将pyenv配置到全局环境变量中**

1.打开全局的环境变量配置文件

```text
vim  /etc/profile 
```

2.在profile 文件最下边加上这两行配置

```text
 export PYENV_ROOT=~/.pyenv
 export PATH=$PYENV_ROOT/shims:$PATH
```

![img](https://pic4.zhimg.com/80/v2-68add43e4291c992590fc219759fe5df_1440w.webp)

3.使环境变量配置文件立即生效

```text
 source /etc/profile 
```

![img](https://pic4.zhimg.com/80/v2-400715701c963de2a67ba72dec366027_1440w.webp)

### **四、查看所有的python版本**

```text
 pyenv versions 
```

![img](https://pic2.zhimg.com/80/v2-4b681a4e068ecb9127c900f7e5afaf71_1440w.webp)

*指向的是当前所使用的版本，system是系统安装的python

### **五、查看所有可以安装的python版本**

```text
pyenv install --list
```

![img](https://pic2.zhimg.com/80/v2-9c6d8f212a214ea0a13c017616730c75_1440w.webp)

列表很长，这里截图只是一部分。

### 六、安装指定版本的python

> 命令格式：pyenv install 版本号 ，eg：

```text
pyenv install 3.8.9
pyenv rehash # 在进行安装、删除指定python版本后使用，更新版本管理数据库
```

![img](https://pic1.zhimg.com/80/v2-fc16c1dbbbf98f0b4f94b147014f8124_1440w.webp)

### 七、**查看当前安装的所有版本**

```text
pyenv versions
```

### **八、切换python版本**

1. 全局切换

> **命令格式：**pyenv global 版本号，eg：

```text
pyenv global 3.5.5
```

2. 当前目录及其子目录生效（激活）

> 命令格式： pyenv local 版本号， eg：

```text
 pyenv local 3.5.5
```

> 激活后，在每次进入该目录时会自动切换到指定的版本。如果取消激活则使用--unset参数：

```text
pyenv local --unset
```

3. 验证是否切换成功

```text
python -V 
```

### **九、卸载指定的Python版本**

```text
 pyenv uninstall 3.8.9
 pyenv rehash # 在进行安装、删除指定python版本后使用，更新版本管理数据库
```

### 十、更新版本管理数据库

> 在进行安装、删除指定python版本后使用，更新版本管理数据库

```text
pyenv rehash 
```

注意：

```
export PYENV_ROOT=~/.pyenv
export PATH=$PYENV_ROOT/shims:$PATH 为什么我在/etc/profile 加了这两行不生效，
然后在 ~/.zshrc 加了下面内容就好了
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init --path)" 
```

