---
title: ZFile Docker 镜像构建过程
tags:
  - docker
categories: 技术
abbrlink: '7347e535'
date: 2024-05-09 23:57:47
---
# ZFile Docker 镜像构建过程

## 前言

下文提供 ZFile 镜像构建过程，供大家参考。

## Dockerfile

```dockerfile
FROM ibm-semeru-runtimes:open-8-jre

WORKDIR /root

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN echo 'Asia/Shanghai' >/etc/timezone

RUN curl -o app.jar https://c.jun6.net/ZFILE/zfile-release.jar

EXPOSE 8080

ENTRYPOINT java $JAVA_OPTS -Xshareclasses -Xquickstart -jar /root/app.jar

```

- `FROM ibm-semeru-runtimes:open-8-jre` 表示使用的基础镜像，这里选中的这个是 `ibm` 的 `openj9` `jdk8` 版本的 `jre`，这个版本的 jdk 具有内存占用小，启动速度快等优势，且针对 `docker`环境做过特殊优化。
- `WORKDIR /root` 表示工作目录在 `/root`，下方所有命令将在这个目录下执行
- `RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime` 和 `RUN echo 'Asia/Shanghai' >/etc/timezone` 表示设置时区为上海时间，这两个设置的区别参考：https://unix.stackexchange.com/questions/384971/whats-the-difference-between-localtime-and-timezone-files
- `RUN curl -o app.jar https://c.jun6.net/ZFILE/zfile-release.jar` 表示下载 `zfile` 最新版 `jar` 包，并命名为 `app.jar`，因为上方工作目录为 `/root`，所以实际下载到了 `/root/app.jar`
- `EXPOSE 8080` 表示容器可能会使用这个端口，这个只是声明或备注式的，不会实际影响端口情况。
- `ENTRYPOINT java $JAVA_OPTS -Xshareclasses -Xquickstart -jar /root/app.jar` 表示启动 `/root/app.jar`，其中 `-Xshareclasses -Xquickstart` 是 `ibm` 的 `openj9 jvm` 的参数，用来优化内存占用。

## build

由于需要同时支持 `arm64` 架构和 `amd64` 架构，所以使用的 `docker` 的 [多平台构建](https://docs.docker.com/desktop/multi-arch/)，即同时 `build` 多个架构的镜像。

1. 由于 `Docker` 默认的 `builder` 实例不支持同时指定多个 `--platform`，我们必须首先创建一个新的 `builder` 实例。

```bash
docker buildx create --name zfile-builder --driver docker-container

```

1. 使用新创建好的 `builder` 实例

```bash
docker buildx use zfile-builder

```

1. 查看已有的 `builder` 支持构建的架构类型

```bash
docker buildx ls

```

1. 安装模拟器（用于模拟其他平台的构建）

```bash
docker run --privileged --rm tonistiigi/binfmt --install all

```

1. 在 `Dockerfile` 所在目录执行构建命令并 `push` 到 `docker hub` (需提前 `docker login` 账号)

```bash
docker buildx build --platform linux/arm64,linux/amd64 -t zhaojun1998/zfile:latest --push .

```

## 结语

然后在 `docker hub` 就可以看到推送的镜像了。

[![docker-hub_images](https://cdn.jun6.net/uPic/2022/05/21/EcI7tT.png)

后续还会更新如何使用 `CI` 工具自动化构建镜像的方式。