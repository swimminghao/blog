---
title: Hadoop基础
tags:
  - hadoop
  - 基础
categories: 大数据
abbrlink: f38c9a79
date: 2026-04-24 15:21:59
---
#Hadoop基础

## 1. Hadoop 由哪些核心组件组成，每个组件有什么特性？

Hadoop 的核心组件包括 **HDFS**、**MapReduce**、**YARN** 以及 **Hadoop Common**。

- **HDFS（Hadoop Distributed File System）**
  - **特性**：
    - 分布式存储，将大文件切分为固定大小的块（默认 128MB）分散存储在多台机器上。
    - **高容错**：数据块自动复制多份（默认 3 份），副本分布在不同节点，坏块可自动恢复。
    - **主/从架构**：一个 NameNode（管理命名空间和块映射）和多个 DataNode（实际存储数据块）。
    - **一次写入、多次读取**（不支持随机修改），适合批处理场景。
    - 适合存储超大文件（TB/PB 级）。
- **MapReduce**
  - **特性**：
    - 分布式计算框架，将任务分为 Map（映射）和 Reduce（规约）两个阶段。
    - **数据本地性**：尽量将计算移动到数据所在节点，减少网络传输。
    - **容错**：单个任务失败会自动在其他节点重试。
    - 编程模型简单，但中间结果落盘，不适合低延迟交互式查询。
- **YARN（Yet Another Resource Negotiator）**
  - **特性**：
    - 资源管理和作业调度平台（Hadoop 2.x 引入）。
    - 将资源管理与作业监控分离：
      - **ResourceManager**：全局资源调度。
      - **NodeManager**：每节点资源与任务管理。
      - **ApplicationMaster**：每个作业一个，负责申请资源、监控任务执行。
    - 支持多种计算框架（MapReduce、Spark、Flink 等）运行在同一集群。
- **Hadoop Common**
  - **特性**：
    - 提供其他模块公用的工具类、文件系统抽象、RPC 框架、序列化库等。

------

## 2. Hadoop 2.x 和 Hadoop 1.x 的架构有什么不同？

| 方面         | Hadoop 1.x                                                   | Hadoop 2.x                                                   |
| :----------- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| **资源管理** | JobTracker 同时负责资源管理和作业调度，存在单点故障和扩展性瓶颈 | YARN 将资源管理与作业监控分离：ResourceManager + NodeManager + ApplicationMaster |
| **作业跟踪** | MapReduce 作业由 JobTracker 统一跟踪，任务过多时压力巨大     | ApplicationMaster 负责单个作业的生命周期，ResourceManager 只负责分配资源 |
| **支持框架** | 仅支持 MapReduce                                             | 支持 MapReduce、Spark、Flink、Tez 等多种框架                 |
| **扩展性**   | 集群规模受限（JobTracker 内存压力），通常不超过 4000 节点    | 可扩展至上万节点，ResourceManager 可水平扩展（HA）           |
| **容错**     | JobTracker 故障导致所有作业失败                              | ResourceManager 和 ApplicationMaster 均有 HA 方案，单点故障影响小 |

**简单记忆**：
**1.x** = HDFS + MapReduce（紧耦合，资源管理在 MapReduce 内）。
**2.x** = HDFS + YARN + MapReduce（YARN 独立管理资源，MapReduce 降为运行在 YARN 上的应用）。

------

## 3. Hadoop 可以解决什么问题？

Hadoop 主要用于解决**海量数据（TB/PB 级）的存储与处理问题**，适合以下场景：

1. **海量日志分析**：如网站点击流、服务器日志，通过 MapReduce 进行清洗、聚合、统计。
2. **数据仓库 ETL**：将非结构化/半结构化数据（JSON、CSV、文本）转换加载到数据仓库。
3. **离线批处理**：大型排序、倒排索引、机器学习训练数据预处理等。
4. **低成本存储**：使用普通服务器组成廉价集群，相比 NAS/SAN 成本大幅降低。
5. **数据归档与备份**：利用多副本机制提供高可靠性。
6. **大规模图计算**（配合 Giraph 等）或 **NoSQL 数据库**（HBase on HDFS）。

**不适合**：低延迟随机读写（改用 HBase 或 Kudu）、实时流处理（改用 Spark Streaming/Flink）、事务性要求高的场景。

------

## 4. 使用一台 Linux 机器实现 Hadoop 伪分布集群安装

**伪分布模式**：所有守护进程（NameNode、DataNode、ResourceManager、NodeManager）运行在同一台机器上，但每个进程独立 JVM，模拟分布式行为。

### 步骤概览（以 Ubuntu/CentOS 为例，Hadoop 3.x）：

1. **安装 JDK**（1.8 或 11）

   bash

   ```
   sudo apt install openjdk-8-jdk   # Ubuntu
   java -version
   ```

   

2. **设置 SSH 免密登录**（本机）

   bash

   ```
   ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
   cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
   chmod 600 ~/.ssh/authorized_keys
   ssh localhost   # 测试免密
   ```

   

3. **下载并解压 Hadoop**

   bash

   ```
   wget https://archive.apache.org/dist/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
   tar -xzf hadoop-3.3.6.tar.gz -C /usr/local/
   cd /usr/local/hadoop-3.3.6
   ```

   

4. **修改环境变量**（`~/.bashrc` 或 `/etc/profile`）

   bash

   ```
   export HADOOP_HOME=/usr/local/hadoop-3.3.6
   export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
   export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64   # 根据实际修改
   source ~/.bashrc
   ```

   

5. **修改 Hadoop 配置文件**（位于 `$HADOOP_HOME/etc/hadoop`）

   - **`core-site.xml`**

     xml

     ```
     <configuration>
         <property>
             <name>fs.defaultFS</name>
             <value>hdfs://localhost:9000</value>
         </property>
     </configuration>
     ```

     

   - **`hdfs-site.xml`**

     xml

     ```
     <configuration>
         <property>
             <name>dfs.replication</name>
             <value>1</value>   <!-- 伪分布只需1个副本 -->
         </property>
         <property>
             <name>dfs.namenode.name.dir</name>
             <value>/usr/local/hadoop-3.3.6/data/namenode</value>
         </property>
         <property>
             <name>dfs.datanode.data.dir</name>
             <value>/usr/local/hadoop-3.3.6/data/datanode</value>
         </property>
     </configuration>
     ```

     

   - **`mapred-site.xml`**

     xml

     ```
     <configuration>
         <property>
             <name>mapreduce.framework.name</name>
             <value>yarn</value>
         </property>
     </configuration>
     ```

     

   - **`yarn-site.xml`**

     xml

     ```
     <configuration>
         <property>
             <name>yarn.nodemanager.aux-services</name>
             <value>mapreduce_shuffle</value>
         </property>
         <property>
             <name>yarn.resourcemanager.hostname</name>
             <value>localhost</value>
         </property>
     </configuration>
     ```

     

6. **设置 Hadoop 环境变量文件**（可选 `hadoop-env.sh` 中指定 JAVA_HOME）

7. **格式化 NameNode**（仅首次执行）

   bash

   ```
   hdfs namenode -format
   ```

   

8. **启动 Hadoop 服务**

   bash

   ```
   start-dfs.sh   # 启动 HDFS (NameNode + DataNode)
   start-yarn.sh  # 启动 YARN (ResourceManager + NodeManager)
   ```

   

   或一键启动 `start-all.sh`（已弃用，建议分开）。

9. **验证进程**

   bash

   ```
   jps
   ```

   

   应看到：`NameNode`、`DataNode`、`SecondaryNameNode`、`ResourceManager`、`NodeManager`。

10. **访问 Web 界面**

    - HDFS 管理：`http://localhost:9870`（Hadoop 3.x）
    - YARN 资源管理：`http://localhost:8088`

11. **测试运行**（例如执行自带 wordcount）

    bash

    ```
    hdfs dfs -mkdir /input
    hdfs dfs -put $HADOOP_HOME/etc/hadoop/*.xml /input
    hadoop jar $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-3.3.6.jar wordcount /input /output
    hdfs dfs -cat /output/part-r-00000
    ```

    

------

## 5. 安装 Hadoop 集群需要修改哪几个核心 xml 配置文件？

在一台或多台机器的集群安装中，通常需要修改以下 **4 个核心 XML 文件**（位于 `$HADOOP_HOME/etc/hadoop/`）：

| 文件                  | 作用                | 关键配置示例                                                 |
| :-------------------- | :------------------ | :----------------------------------------------------------- |
| **`core-site.xml`**   | Hadoop 核心通用配置 | `fs.defaultFS`（HDFS 访问地址）                              |
| **`hdfs-site.xml`**   | HDFS 专属配置       | `dfs.replication`（副本数）、`dfs.namenode.name.dir`、`dfs.datanode.data.dir` |
| **`mapred-site.xml`** | MapReduce 配置      | `mapreduce.framework.name`（设为 `yarn`）                    |
| **`yarn-site.xml`**   | YARN 资源管理配置   | `yarn.resourcemanager.hostname`、`yarn.nodemanager.aux-services` |

此外，有时还需要修改：

- **`workers`**（旧名 `slaves`）：列出所有 DataNode 和 NodeManager 的主机名（每行一个）。
- **`hadoop-env.sh`**：设置 `JAVA_HOME` 环境变量。
- **`yarn-env.sh`**：可选的 YARN 环境变量。

> 对于伪分布模式，`workers` 文件里只写 `localhost` 即可。

------

如果你需要更详细的安装教程（比如完全分布式集群的配置）或对每个配置项的解释，我可以进一步展开说明。