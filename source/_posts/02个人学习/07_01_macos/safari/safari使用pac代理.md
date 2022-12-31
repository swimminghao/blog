---
title: Safari实现SwitchyOmega的PAC代理功能
tags:
- mac
  categories: 工具
date: 2022-12-31 20:30:00
---

# Safari实现SwitchyOmega的PAC代理功能

Safari 没有开放代理 API，因为 macOS 可以很方便地设置系统级的全局代理，但是全局代理则会导致所有流量走代理服务器。

相比类似于 Chrome + SwitchyOmega (auto switch) 那种代理上网效果（国内正常，国外走代理），Safari 在代理上网方面的扩展程序实在乏善可陈，这篇文章所解决的需求就是在 macOS Safari 平台上实现 Chrome 的这种代理上网方式。

### 方法：

#### 1）导出Chrome的PAC 文件

从 Chrome 的 SwitchyOmega 扩展程序中将 PAC 文件导出，记得选择自动切换模式下的 PAC 文件。

#### 2）启动一个http server承载PAC文件

在本机存放 PAC 的目录下用 python 直接起HTTP 服务：

```
python -m http.server 11111
```

#### 3）配置代理

系统偏好设置 👉🏻 网络 👉🏻 高级 👉🏻 代理 👉🏻 选择「自动代理配置」，URL填入刚才的PAC文件的下载地址:

```
http://127.0.0.1:11111/OmegaProfile_auto_switch.pac
```

Safari.pac就是刚才导出的PAC文件，目的是让系统能够找到PAC文件

### 4）进程服务自启动

可以用docker/supervisor/systemd类似的工具去管理进程，如果不想让本地维护http服务的话，可以在github中建个项目，把PAC文件上传，通过Github生成调用链接也是没问题的。

