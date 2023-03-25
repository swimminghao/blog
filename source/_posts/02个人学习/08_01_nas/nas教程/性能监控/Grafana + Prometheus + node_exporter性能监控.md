---
title: Grafana + Prometheus + node_exporter性能监控
tags:
  - nas
  - 监控
categories: 工具
abbrlink: 5ed33e10
date: 2023-03-25 15:14:00
---
# Grafana + Prometheus + node_exporter性能监控
## 一、背景

在用Jmeter执行性能测试时，为了更好得收集压测数据，并展示性能测试结果数据，需要搭建一套监控平台。有了平台后测试人员可以随时通过查看性能压测数据，对比历史数据，分析性能优化结果。

## 二、服务准备

### 1、MySQL用户授权

需要对用户进行授权才能收集监控数据信息

- 首先，登录mysql服务，这里以docker为例

  

  highlighter- applescript

  ```
  docker exec -it mysqlserver sh
  
  mysql -uroot -p
  ```

- 然后，创建用户并开启远程登录。

  

  highlighter- n1ql

  ```
  #CREATE USER '你的账号'@'%'  IDENTIFIED BY '你的密码';
  CREATE USER 'exporter'@'%' IDENTIFIED BY '123123123';
  ```

- 然后，授予查看主从运行、线程，及所有数据库的权限

  

  highlighter- lasso

  ```
  #GRANT PROCESS, REPLICATION CLIENT ON 库名.* TO '你的账号'@'%';
  GRANT PROCESS, REPLICATION CLIENT ON *.* TO 'exporter'@'%';
  ```

- 然后，授予**监控MySQL server运行过程中的资源消耗、资源**权限

  

  highlighter- lasso

  ```
  #GRANT SELECT ON performance_schema.* TO '你的账号'@'%';
  GRANT SELECT ON performance_schema.* TO 'exporter'@'%';
  ```

- 最后，更新用户权限后，刷新权限表

  

  highlighter- abnf

  ```
  flush privileges;
  ```

  ![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/7KF9sD.png)

### 2、部署服务

docker-compose.yml

```
version: '2'

networks:
    monitor:
        driver: bridge

services:
    prometheus:
        image: prom/prometheus:latest
        container_name: prometheus
        hostname: prometheus
        restart: always
        volumes:
            - ./prometheus.yml:/etc/prometheus/prometheus.yml
        ports:
            - "9090:9090"
        networks:
            - monitor

    grafana:
        image: grafana/grafana:latest
        container_name: grafana
        hostname: grafana
        restart: always
        ports:
            - "3000:3000"
        networks:
            - monitor

    node-exporter:
        image: quay.io/prometheus/node-exporter:latest
        container_name: node-exporter
        hostname: node-exporter
        restart: always
        ports:
            - "9100:9100"
        networks:
            - monitor

    cadvisor:
        image: google/cadvisor:latest
        container_name: cadvisor
        hostname: cadvisor
        restart: always
        volumes:
            - /:/rootfs:ro
            - /var/run:/var/run:rw
            - /sys:/sys:ro
            - /var/lib/docker/:/var/lib/docker:ro
        ports:
            - "8899:8080"
        networks:
            - monitor

    redis-exporter:
        image: oliver006/redis_exporter
        container_name: redis-exporter
        hostname: redis-exporter
        restart: always
        ports:
            - "9121:9121"
        command:
            - "--redis.addr=redis://127.0.0.1:6379"
        networks:
            - monitor

    mysql_exporter:
        image: prom/mysqld-exporter
        container_name: mysql-exporter
        hostname: mysql-exporter
        restart: always
        ports:
            - "9104:9104"
        environment:
            DATA_SOURCE_NAME: 'exporter:123123123@(127.0.0.1:3306)'
        networks:
            - monitor
```

在`docker-compose.yml`当前目录下，创建`prometheus.yml`文件，它将`prometheus`与`exporter`服务关联起来。

prometheus.yml

```
# Prometheus全局配置项
global:
  scrape_interval: 15s # 设定抓取数据的周期，默认为1min
  evaluation_interval: 15s # 设定更新rules文件的周期，默认为1min
  scrape_timeout: 15s # 设定抓取数据的超时时间，默认为10s


# scape配置
scrape_configs:
  # job_name默认写入timeseries的labels中，可以用于查询使用
  - job_name: 'prometheus'
    scrape_interval: 15s # 抓取周期，默认采用global配置
    static_configs:
    - targets: ['192.168.50.218:9090'] # prometheus所要抓取数据的地址，即instance实例项

  - job_name: 'node'
    scrape_interval: 8s
    static_configs:
     - targets: ['192.168.50.218:9100']
       labels:
         instance: node

  - job_name: 'cadvisor'
    static_configs:
     - targets: ['192.168.50.218:8899']
     labels:
         instance: cadvisor

  - job_name: 'redis_exporter'
    static_configs:
      - targets: [ '192.168.50.218:9121' ]
        labels:
          instance: redis

  - job_name: 'mysql_exporter'
    static_configs:
      - targets: [ '192.168.50.218:9104' ]
        labels:
          instance: mysql
```

在`docker-compose.yml`以及`prometheus.yml`文件都准备好后，在当前目录下创建一个`run.sh`的脚本，方便快速启动服务，不用这个脚本，用脚本中的命令也可以。

run.sh

```
#!/usr/bin/env bash
docker-compose down
docker-compose rm
docker-compose up -d --build
```

全部启动后

```
cd ~/data/prometheus

docker-compose ps
```

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/zFCLmS.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220823205240-131178.png)

启动成功，后就可以访问：

- `http://192.168.50.218:9090` :prometheus的原生web-ui
- `http://192.168.50.218:3000` :Grafana开源的监控可视化组件页面，默认用户及密码为admin
- `http://192.168.50.218:9100` :收集服务器（node-exporter）的metrics
- `http://192.168.50.218:8899` :收集docker（cadvisor）的metrics
- `http://192.168.50.218:9100` :收集redis（redis-exporter）的metrics
- `http://192.168.50.218:9104` :收集mysql（mysql-exporter）的metrics

打开 `http://192.168.50.218:9090/targets `，如果State都是UP即代表Prometheus工作正常，如下图所示：

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/XfPd4R.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220823210417-32948.png)

## 三、服务配置

### 1、配置数据源

首先，服务启动后，要让`prometheus`的数据能被`Grafana`访问，那么就需要打开`Grafana`页面（`http://192.168.50.218:3000` ），默认用户及密码均为`admin`，并在`Grafana`页面中配置`prometheus`的数据来源。

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/XtUuHw.png)

配置好数据源后，就可以配置对应监控模板。

### 2、配置服务器监控

本次要导入的模板：https://grafana.com/grafana/dashboards/11074，模板ID：11074

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/deGwTM.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220824163539-912751.png)

开`Grafana`页面（`http://192.168.50.218:3000` ），然后选择`import`导入模板

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/rc7jO1.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220824162319-169624.png)

然后，填写模板的名称（名称随意取），接着，选择数据源为`prometheus`

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/ZQzJcu.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220824162509-230520.png)

最后，点击`import`导入模板，结果如图：

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/7jTvBl.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220824162832-752439.png)

可以看到`node-exporter`把获取的数据信息，展示在了`Grafana`页面。

### 3、配置容器监控

本次要导入的模板：https://grafana.com/grafana/dashboards/893，模板ID：893

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/8BJ3pP.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220824163507-964056.png)

开`Grafana`页面（`http://192.168.50.218:3000` ），然后选择`import`导入模板

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/NpzNmg.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220824163631-394593.png)

然后，填写模板的名称（名称随意取），接着，选择数据源为`prometheus`

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/A8mEix.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220824163715-583039.png)

最后，就可以看到，收集docker（cadvisor）的metrics信息，并展示在grafana页面。

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/9lEXNH.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220824164056-462019.png)

### 4、配置redis监控

本次要导入的模板：https://grafana.com/grafana/dashboards/11835，模板ID：11835

开`Grafana`页面（`http://192.168.50.218:3000` ），然后选择`import`导入模板

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/VT3DPm.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220824164435-339924.png)

### 5、配置mysql监控

本次要导入的模板：https://grafana.com/grafana/dashboards/7362，模板ID：7362

[![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2023/03/25/HhJUJl.png)](https://photo-1301375639.cos.ap-chengdu.myqcloud.com/undefined/20220824164224-275451.png)

## 四、参考

1、docker-compose 搭建 Prometheus+Grafana监控系统 :https://www.cnblogs.com/qdhxhz/p/16325893.html

2、jmeter压测练习：https://github.com/princeqjzh/iJmeter

3、dbeaver下载：https://dbeaver.io/download/

4、mysql监控：https://www.prometheus.wang/exporter/use-promethues-monitor-mysql.html