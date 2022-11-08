---
title: Dockerfile
tags:
  - docker
categories: 技术
abbrlink: 5624fad4
date: 2022-02-28 19:57:47
---
# Docker相关指令

## docker镜像运行以及日志

```bash
❯ docker run --restart=always -d --name holer-client -e PARAMS="www.swimminghao.top 6060 51c45156bf1c4a82b4e6ffff2150b65e" swimminghao/holer-client:latest
❯ docker run -d --name holer-server --restart=always --network=host -p 600:600 -p 6060:6060 -p 6443:6443 -p 11000-11010:11000-11010 swimminghao/holer-server:latest
❯ docker logs -f 1ed9a5d6f0cc
```

## Dockerfile 打包jar成docker镜像

```dockerfile
dockerfile1
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ADD ./holer-client.jar holer-client.jar
RUN echo "Asia/Shanghai" > /etc/timezone
ENV PARAMS=
ENTRYPOINT java -Djava.security.egd=file:/dev/./urandom -Duser.timezone=GMT+8 -jar /holer-client.jar ${PARAMS}
```

```dockerfile
dockerfile2
FROM openjdk:8-jdk-alpine
VOLUME /tmp
ADD ./holer-client.jar holer-client.jar
RUN echo "Asia/Shanghai" > /etc/timezone
ENV PARAMS=
ENV JVM_XMS="256m"
ENV JVM_XMX="256m"
ENTRYPOINT java -Xms${JVM_XMS} -Xmx${JVM_XMX} -Djava.security.egd=file:/dev/./urandom -Duser.timezone=GMT+8 -jar /holer-client.jar ${PARAMS}
```

dockerfile和jar包放同一文件夹，再执行下面指令

```bash
❯ docker build -t swimminghao/holer-client:latest .
```

```dockerfile
FROM openjdk:8-jdk-alpine
VOLUME /tmp

ARG JAR_FILE=./holer-server.jar
#ARG PORT=8090
ARG TIME_ZONE=Asia/Shanghai

ENV TZ=${TIME_ZONE}
ENV JVM_XMS="256m"
ENV JVM_XMX="256m"

COPY ${JAR_FILE} holer-server.jar

EXPOSE 600
EXPOSE 6060
EXPOSE 6443

ENTRYPOINT java -Xms${JVM_XMS} -Xmx${JVM_XMX} -Djava.security.egd=file:/dev/./urandom -server -jar holer-server.jar
```

dockerfile和jar包放同一文件夹，再执行下面指令

```bash
❯ docker build -t swimminghao/holer-server:latest .
```

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/nuW6sf_20221108211749.png)

## docker容器停止以及镜像删除

```bash
❯ docker ps -a
❯ docker pull seanhongxing/holer-server
❯ docker stop f8ca94969ba0 && docker rm f8ca94969ba0
❯ docker image ls -a
❯ docker image rm f8ca94969ba0
```

## docker镜像反推Dockerfile：

### 1、指令

```bash
❯ docker history <83d576a828a8> --format "{{.CreatedBy}}" --no-trunc |tac | awk '{if($3~/nop/){for(i=1;i<=3;i++){$i=""};print substr($0,4)}else{print "RUN",$0}}'
```

### 2、bash脚本

```bash
❯ bash decompile.sh seanhongxing/holer-server
```

```bash
#!/bin/bash
#########################################################################
# File Name: dockerfile.sh
# Author: www.linuxea.com
# Version: 1
# Created Time: Thu 14 Feb 2019 10:52:01 AM CST
#########################################################################
case "$OSTYPE" in
    linux*)
        docker history --no-trunc --format "{{.CreatedBy}}" $1 | # extract information from layers
        tac                                                    | # reverse the file
        sed 's,^\(|3.*\)\?/bin/\(ba\)\?sh -c,RUN,'             | # change /bin/(ba)?sh calls to RUN
        sed 's,^RUN #(nop) *,,'                                | # remove RUN #(nop) calls for ENV,LABEL...
        sed 's,  *&&  *, \\\n \&\& ,g'                           # pretty print multi command lines following Docker best practices
    ;;
    darwin*)
        docker history --no-trunc --format "{{.CreatedBy}}" $1 | # extract information from layers
        tail -r                                                | # reverse the file
        sed -E 's,^(\|3.*)?/bin/(ba)?sh -c,RUN,'               | # change /bin/(ba)?sh calls to RUN
        sed 's,^RUN #(nop) *,,'                                | # remove RUN #(nop) calls for ENV,LABEL...
        sed $'s,  *&&  *, \\\ \\\n \&\& ,g'                      # pretty print multi command lines following Docker best practices
    ;;
    *)
        echo "unknown OSTYPE: $OSTYPE"
    ;;
esac
```

## docker镜像下载jar包

```bash
//镜像导出
❯ docker save -o ./client.tar seanhongxing/holer-client:latest
//容器导出
❯ docker export seanhongxing/holer-client > ./client.tar
```

**注：分层，jar包在某一层**