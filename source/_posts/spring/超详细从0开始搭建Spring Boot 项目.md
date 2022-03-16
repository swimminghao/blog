---
title: 超详细从0开始搭建 Spring Boot 项目
tags:
  - SpringBoot
categories: 技术
abbrlink: 79603c56
date: 2022-03-14 17:11:00
---
# 超详细从0开始搭建 Spring Boot 项目

这个项目，我是打算作为种子项目的，作为种子项目，必须的“开箱即用”，必须要包含大部分 web 开发的相关功能，后期所有的 Spring Boot 项目都是将这个项目拿来，简单修改一下配置，就可以快速开发了。

# 源码

[Github](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fwqlm%2Fboot)

# 1.0.0 创建项目

我使用的是 idea

创建项目 ![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/14/w8NHXh.png)

选择 Spring initializr ，如果点Next一直在转圈，可能是 [start.spring.io/](https://link.juejin.cn/?target=https%3A%2F%2Fstart.spring.io%2F) 在国外，访问比较慢。可以科学上网或者，使用自定义的 [start.spring.io/](https://link.juejin.cn/?target=http%3A%2F%2Fstart.spring.io%2F) ![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/14/T8zrCm.png)

主要改以下组织名称、项目名称和项目描述就好了 ![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/14/JQ1yll.png)

我创建项目的时候， Spring Boot 最新稳定版是 2.1.9 。要用就用最新的！！！ 依赖先都不勾选，后期一项一项加

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/14/2PU7rC.png)

项目文件夹名称以及存放位置 ![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/14/ejmw40.png)

## 添加maven镜像

添加maven镜像加快依赖下载速度

```xml
<repositories>
    <repository>
        <name>华为maven仓库</name>
        <id>huawei</id>
        <url>https://mirrors.huaweicloud.com/repository/maven/</url>
    </repository>
</repositories>

<pluginRepositories>
    <pluginRepository>
        <name>华为maven插件仓库</name>
        <id>huawei_plugin</id>
        <url>https://mirrors.huaweicloud.com/repository/maven/</url>
    </pluginRepository>
</pluginRepositories>
```

## pom 文件

整体 .pom 文件内容如下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.9.RELEASE</version>
        <relativePath/>
    </parent>

    <groupId>com.wqlm</groupId>
    <artifactId>boot</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>boot</name>
    <description>Spring Boot Demo</description>

    <properties>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <repositories>
        <repository>
            <name>华为maven仓库</name>
            <id>huawei</id>
            <url>https://mirrors.huaweicloud.com/repository/maven/</url>
        </repository>
    </repositories>

    <pluginRepositories>
        <pluginRepository>
            <name>华为maven插件仓库</name>
            <id>huawei_plugin</id>
            <url>https://mirrors.huaweicloud.com/repository/maven/</url>
        </pluginRepository>
    </pluginRepositories>

</project>

```

## 依赖结构图

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/14/123.png)

# 番外

观察仔细的人应该发现了，**spring-boot-starter** 和 **spring-boot-starter-test** 都没有指定版本，那它们是怎么确定版本的？

> 参考 [为什么 maven 依赖可以不指定版本](https://juejin.cn/post/6844903965444816903)

# 1.1.0 添加 web 模块

在 pom 文件中添加 web 模块

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

```

由于 **spring-boot-starter-web** 包含 **spring-boot-starter** ![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/2022/03/14/JFwFNq.png)

建议删掉如下 **spring-boot-starter** 依赖，以保证依赖的干净整洁

```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter</artifactId>
</dependency>

```

## 作用

引入 **spring-boot-starter-web** 后，我们可以

- 编写 web 应用
- 不需要配置容器即可运行 web 应用
- 对请求参数进行校验
- 将业务结果对象转换成 josn 返回

## 依赖结构图

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/155a44e024874585a472be781a6fa524~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp) 从图中我们可以看到 **spring-boot-starter-web** 引入了几个关键的依赖

- spring-boot-starter

- **spring-boot-starter-tomcat**：spring boot 不需要 tomcat 也能启动就是因为它

- spring-webmvc

- spring-web

- spring-boot-starter-json

  ： 有了它，就可以使用 @ResponseBody 返回 json 数据

  - **jackson** ：spring boot 默认的 json 解析工具

- hibernate-validator

  ：提供参数校验的注解，如

   

  @Range、@Length

  - **javax.validation**：提供参数校验的注解，如 **@NotBlank、@NotNull、@Pattern**

关于参数校验请参考 [参数校验 Hibernate-Validator](https://juejin.cn/post/6844903961581846535#heading-4)

# 1.2.0 集成 mysql

Spring Boot 集成 mysql 需要 **JDBC 驱动** 和 **mysql 驱动**

## 引入依赖

```
<!--JDBC-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>

<!--mysql-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>

```

版本号可以不用填，spring boot 配置了默认的版本号。例如 spring boot 2.1.9.RELEASE 对应的 mysql-connector-java 版本为 8.0.17

## 配置 mysql

根据 **mysql-connector-java** 版本不同，配置的内容也有些许差异

```
# mysql-connector-java 6.0.x 以下版本配置
spring.datasource.driverClassName=com.mysql.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/boot?useUnicode=true&characterEncoding=utf-8&useSSL=false
spring.datasource.username=root
spring.datasource.password=123456


# mysql-connector-java 6.0.x 及以上版本配置
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/boot?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=Asia/Shanghai
spring.datasource.username=root
spring.datasource.password=123456

```

如下图，**spring boot 2.1.9.RELEASE** 对应的 **mysql-connector-java** 版本为 8.0.17 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4aa3024083464a419edbece2869fd5d9~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

更多请参考[MySQL JDBC 连接](https://juejin.cn/post/6844903965713252360)

## 创建示例数据库和表

```
-- 创建 boot 数据库
CREATE DATABASE
IF
	NOT EXISTS boot DEFAULT CHARSET utf8 COLLATE utf8_bin;

-- 选择 boot 数据库
USE boot;

-- 创建 user 表
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;
DROP TABLE
IF
	EXISTS `user`;
CREATE TABLE `user` (
	`id` INT ( 11 ) NOT NULL AUTO_INCREMENT,
	`user_name` VARCHAR ( 255 ) COLLATE utf8_bin NOT NULL,
	`pasword` VARCHAR ( 255 ) COLLATE utf8_bin NOT NULL,
	`salt` VARCHAR ( 255 ) COLLATE utf8_bin NOT NULL,
	PRIMARY KEY ( `id` ) 
) ENGINE = INNODB DEFAULT CHARSET = utf8 COLLATE = utf8_bin;
SET FOREIGN_KEY_CHECKS = 1;

```

# 1.3.0 多环境配置

详细参考[spring profile 与 maven profile 多环境管理](https://juejin.cn/post/6844903891763625998)

## spring 多环境配置

**配置一个 dev 环境**

创建 `application-dev.properties` 文件，并将 mysql 相关配置迁移过来 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/44b606788c4f4ee3bfd0815b8ffa3923~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

**使用 dev 环境**

在 `application-dev.properties` 指定要使用的环境

```
spring.profiles.active=dev

```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/585cee84397548a59d26c2b983ae6276~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

> 同理你也可以创建 test、prod 环境，但一般公共配置还是会放在 application.properties 中，只有非公共配置才会放在各自的环境中

## maven 多环境配置

spring boot 多环境配置有两个缺点

1. 每次切换环境要手动修改 `spring.profiles.active` 的值
2. 打包的时候，要手动删除其它环境的配置文件，不然其它环境的敏感信息就都打包进去了

而 maven 的 profile 可以解决这两个问题

### 第一个问题

***"每次切换环境要手动修改`spring.profiles.active`的值"***

这个问题就可以通过配置 profile 解决，在pom的根节点下添加

```
<profiles>
    <profile>
        <id>dev</id>
        <activation>
            <!-- activeByDefault 为 true 表示，默认激活 id为dev 的profile-->
            <activeByDefault>true</activeByDefault>
        </activation>
        <!-- properties 里面可以添加自定义节点，如下添加了一个env节点 -->
        <properties>
            <!-- 这个节点的值可以在maven的其他地方引用，可以简单理解为定义了一个叫env的变量 -->
            <env>dev</env>
        </properties>
    </profile>
    <profile>
        <id>test</id>
        <properties>
            <env>test</env>
        </properties>
    </profile>
    <profile>
        <id>prod</id>
        <properties>
            <env>prod</env>
        </properties>
    </profile>
</profiles>

```

如上，定义了三套环境，其中id为dev的是默认环境，三套环境中定义了叫 env的“变量”

如果你用的是idea编辑器，添加好后，maven控件窗口应该会多出一个 Profiles,其中默认值就是上面配置的dev

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3cf399a73f4145daafafc2283786df2c~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

最小化的 profiles 已经配置好了，通过勾选上图中的Profiles，就可以快速切换 maven的 profile 环境。

现在 maven profile 可以通过 勾选上图中的Profiles 快速切换环境

Spring Profile 还得通过 手动修改`spring.profiles.active`的值来切环境

现在的问题是怎样让 maven profile的环境与Spring Profile一一对应，达到切换maven profile环境时，Spring Profile环境也被切换了

还记得maven profile 中定义的 env "变量"吗,现在只需要把

```
spring.profiles.active=dev

```

改成

```
spring.profiles.active=@env@

```

就将maven profile 与 Spring Profile 环境关联起来了

当maven profile 将 环境切换成 test 时，在pom中定义的id为test的profile环境将被激活，在该环境下env的值是test，maven插件会将 @env@ 替换为 test，这样Spring Profile的环境也随之发生了改变。从上面可以看出，自定义的"变量"env的值还不能乱写，要与Spring Profile的环境相对应。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/056436935c884ec1a42a1516ece63f82~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

**总结**

- 第一步，在`pom`文件中配置 profiles
- 第二步，在`application.properties`配置文件中添加 `spring.profiles.active=@env@`

### 第二个问题

***打包的时候，要手动删除其它环境的配置文件，不然其它环境的敏感信息就都打包进去了***

解决这个问题需要在pom根节点下中配置 build 信息

```
<build>
    <resources>
        <resource>
            <directory>src/main/resources</directory>
            <excludes>
                <!--先排除application开头的配置文件-->
                <exclude>application*.yml</exclude>
            </excludes>
        </resource>
        <resource>
            <directory>src/main/resources</directory>
            <!--filtering 需要设置为 true，这样在include的时候，才会把
            配置文件中的@env@ 这个maven`变量`替换成当前环境的对应值  -->
            <filtering>true</filtering>
            <includes>
                <!--引入所需环境的配置文件-->
                <include>application.yml</include>
                <include>application-${env}.yml</include>
            </includes>
        </resource>
    </resources>
</build>

```

- directory：资源文件所在目录
- includes：需要包含的文件列表
- excludes：需要排除的文件列表

如上，配置了两个 `<resource>`，第一个先排除了src/main/resources目录下所有 application 开头是配置文件，第二个在第一个的基础上添加了所需的配置文件。注意 `application-${env}.yml`,它是一个动态变化的值，随着当前环境的改变而改变，假如当前环境是 id叫 dev的 profile，那么env的值为 dev。

这样配置后，maven在build时，就会根据配置先排除掉指定的配置文件，然后根据当前环境添加所需要的配置文件。

## pom 文件

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.9.RELEASE</version>
        <relativePath/>
    </parent>

    <groupId>com.wqlm</groupId>
    <artifactId>boot</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>boot</name>
    <description>Spring Boot Demo</description>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <!--maven 多环境-->
    <profiles>
        <profile>
            <id>dev</id>
            <activation>
                <activeByDefault>true</activeByDefault> <!-- 为 true 表示，默认激活该 profile-->
            </activation>

            <properties> <!-- properties 里面可以添加自定义节点，如下添加了一个env节点 -->
                <env>dev</env> <!-- 这个节点的值可以在maven的其他地方引用，可以简单理解为定义了一个叫env的变量 -->
            </properties>
        </profile>

        <profile>
            <id>test</id>
            <properties>
                <env>test</env>
            </properties>
        </profile>

        <profile>
            <id>prod</id>
            <properties>
                <env>prod</env>
            </properties>
        </profile>
    </profiles>

    <dependencies>
        <!--web-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!--mysql-->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
        </dependency>

        <!--mybatis-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.0</version>
        </dependency>

        <!--test-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
        <resources>
            <resource>
                <directory>src/main/resources</directory>
                <excludes>
                    <!--先排除application开头的配置文件-->
                    <exclude>application*.yml</exclude>
                </excludes>
            </resource>
            <resource>
                <directory>src/main/resources</directory>
                <!--filtering 需要设置为 true，这样在include的时候，才会把配置文件中的@env@ 这个maven`变量`替换成当前环境的对应值-->
                <filtering>true</filtering>
                <includes>
                    <!--引入所需环境的配置文件-->
                    <include>application.yml</include>
                    <include>application-${env}.yml</include>
                </includes>
            </resource>
        </resources>
    </build>

    <repositories>
        <repository>
            <name>华为maven仓库</name>
            <id>huawei</id>
            <url>https://mirrors.huaweicloud.com/repository/maven/</url>
        </repository>
    </repositories>

    <pluginRepositories>
        <pluginRepository>
            <name>华为maven插件仓库</name>
            <id>huawei_plugin</id>
            <url>https://mirrors.huaweicloud.com/repository/maven/</url>
        </pluginRepository>
    </pluginRepositories>
</project>

```

# 1.4.0 多模块配置

稍微大一点的项目一般都会采用多模块的形式管理项目，将一个大型项目升级成多模块项目，一般需要两大步骤

1. 拆分现有项目
2. 进行 **maven 多模块配置**

> 关于**如何拆分现有项目**和**maven 多模块配置**的详细介绍请参考 [Maven 多模块配置、管理](https://juejin.cn/post/6844903970024980488)

## 拆分现有项目

我是按照功能拆分项目的，但由于该项目本身不大，所以我简单的拆分为 **user 模块和 common 模块**

在boot下创建两个新的模块 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/81c309461c8948ef9467215bbc28c09a~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/261c0ea999b04afc969250c5562520f2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/991ac8c47c6e4c5981e209ec7b1c29cc~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/dc3bc4593dbb42f88a18c552965fc3a8~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

子模块的pom ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8abea00bdaaa482488ed9d01afeddfac~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

此时，父模块的pom内容也发生了改变，添加了如下三行 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/489ee5050e1540098de852758c874f8d~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

再来创建 common 模块，创建过程同上，我就不演示了,创建好之后，pom如下 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f931469d35c246568ed67a1e2a581da7~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

整个项目结构如下 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e7724ad2b2184b81b4ec41ccb0ee1166~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

下一步进行迁移工作，将原先 src 目录下面的内容迁移到对于的子模块中 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fe98b1f292254633b99020927f8d2925~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

这里我放到 user 模块中，迁移过程中注意路径和命名规范，过程就不展示了，迁移之后，结构如下。 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/765a2acb0c8e4e6abb7da46e93dbe8bd~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

## 多模块配置

多模块配置遵守以下原则

- 公共、通用配置一定要配置在父pom中
- 版本号由父 pom 统一管理

如下图，蓝色背景的元素都会被子项目全部继承 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/46e760c955de457b95ff48e8734ebe0e~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

由于目前，只有 user 模块用到了如下依赖项，而 common 模块不需要用到这些依赖，所以，将依赖复制到 user 模块下，后删掉依赖 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/cd2963678d054d22b12394d02a70a1f1~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

**build**中的配置目前也是只有 user 模块用到，也复制到 user 模块下，后删掉 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5435e515d2b44aababbca49c1010be3c~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

父项目已经配置完成了，接下来配置 user 模块，如下 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2351fd240787473fa4acb0babcb4580f~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

common 模块用到的时候在配置

## 多模块管理

**多模块环境管理** 我们在父 pom 中配置了 maven 多环境，子模块会继承这些配置。之后，我们只需要在 maven 插件中切换环境，所有的子模块的 maven 环境都会被切换 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3f88b6a0eba44951ae48935de84c6207~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

**多模块构建管理** 在 maven 插件中，通过 boot 项目对所以子模块进行、编译、测试、打包、清理... ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d49e53ae4ae44c51869e28623b51b430~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

# 1.5.0 集成 mybatis

集成 mybatis 一般需要5步

1. 引入依赖
2. 创建 PO 层，存放我们的数据持久化对象
3. 创建 DAO 层，存放数据库的增删改查方法
4. 创建 mapper.xml 层, 对应增删改查语句
5. 在启动类上配置 @MapperScan
6. 其他。如配置 MyBatis Generator，用来帮我们生成 PO、DAO、Mapper.xml

## 添加依赖

在多模块项目中添加依赖要注意一下几点

- 不要将依赖直接添加到父 pom 中，这样所以子模块都会继承该依赖
- 多个模块引用了统一个依赖，最好保证依赖的版本一致

maven dependencyManagement 可以非常方便的管理多模块的依赖

> 关于 maven dependencyManagement 请参考 [maven 依赖版本管理](https://juejin.cn/post/6844903965444816903)

这里我就不在解释，直接应用了

1. 在 **properties** 定义 mybatis-spring-boot-starter 版本号的变量

   ```
   <mybatis-spring-boot-starter.version>2.1.0</mybatis-spring-boot-starter.version>
   
   ```

   ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5dc3f5d99f00434fa7155f5a33c9dd86~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

2. 在父 pom 的根节点下，**申明** mybatis 依赖

   ```
   <dependencyManagement>
       <dependencies>
           <dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
               <version>${mybatis-spring-boot-starter.version}</version>
           </dependency>
       </dependencies>
   </dependencyManagement>
   
   ```

   ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/12d6847585ce4792b47961bdaa8f6af6~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

3. 在 user 模块的 pom 文件中引入 mybatis 依赖

   ```
   <dependency>
       <groupId>org.mybatis.spring.boot</groupId>
       <artifactId>mybatis-spring-boot-starter</artifactId>
   </dependency>
   
   ```

4. 由于 **mybatis-spring-boot-starter** 包含 **spring-boot-starter-jdbc** ，所以删除**spring-boot-starter-jdbc**依赖，保证依赖的整洁

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ea13788aaac74331b119cdafe19e9352~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

## 依赖结构图

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1280fbe2516d4464acf313c5fd450c90~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

## 创建对应的文件夹

如图

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2a3ef4b41f8e42ec80f713f0de0c3800~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

**com.wqlm.boot**

- **controller** ：控制层
- **po** : 存放与数据库中表相对应的java对象
- **dto** : 存放数据传输对象，比如注册时，注册的信息可以用一个 dto 对象来接收
- **dao** : 存放操作数据库的接口
- **service** : 业务层
- **vo** : 存放业务返回结果对象
- **qo** : 封装了查询参数的对象

**resources**

- **mapper** : 存放mapper.xml 文件

## 配置 mybatis

mybatis 需要知道有那些类是 **mapper**！有两种方式可以告诉 mybatis。

**第一种**

在启动类上配置 **@MapperScan**

```
# 指定你的 mapper接口所在的 package
@MapperScan("com.wqlm.boot.user.dao")

```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6d55502ea0764fe0be3f2350f72b45e9~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

**第二种** 在接口上加 `@Mapper` 注解，如下 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/78d1476fea764113aedaec9a3c918768~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

> 要我选我肯定选第一种配置方式，一劳永逸

除此之外，还要告诉 mybatis ，你的 mapper.xml 文件在哪

```
# mybatis
# mapper.xml文件的位置
mybatis.mapper-locations=classpath*:mapper/*.xml

```

由于这里的配置跟环境无关,所以应该配置在 **application.properties** 中 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5f328cde1eff4915b2cbac0baf0a968c~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

# 1.5.1 配置 MyBatis Generator

MyBatis Generator 是 MyBatis 提供的一个代码生成工具。可以帮我们生成 表对应的持久化对象(po)、操作数据库的接口(dao)、CRUD sql的xml(mapper)。

使用方法主要分为三步

1. 引入并配置 MyBatis Generator 插件
2. 配置 MyBatis Generator Config 文件
3. 使用 MyBatis Generator 插件

详细说明请参考 [MyBatis Generator 超详细配置](https://juejin.im/editor/posts/5db694e3e51d452a2e25ba45)

这里只给出一种最终配置

## 引入并配置 MyBatis Generator 插件

在user项目的pom文件的根节点下添加如下配置

```
<build>
    <plugins>
        <plugin>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.3.7</version>
            <configuration>
                <!--mybatis的代码生成器的配置文件-->
                <configurationFile>src/main/resources/mybatis-generator-config.xml</configurationFile>
                <!--允许覆盖生成的文件-->
                <overwrite>true</overwrite>
                <!--将当前pom的依赖项添加到生成器的类路径中-->
                <!--<includeCompileDependencies>true</includeCompileDependencies>-->
            </configuration>
            <dependencies>
                <!--mybatis-generator插件的依赖包-->
                <!--<dependency>-->
                    <!--<groupId>org.mybatis.generator</groupId>-->
                    <!--<artifactId>mybatis-generator-core</artifactId>-->
                    <!--<version>1.3.7</version>-->
                <!--</dependency>-->
                <!-- mysql的JDBC驱动 -->
                <dependency>
                    <groupId>mysql</groupId>
                    <artifactId>mysql-connector-java</artifactId>
                    <version>8.0.17</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
<build>    

```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f72ad5f395ec4177b874d25c4d37df4a~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

## 配置 MyBatis Generator Config 文件

在在user项目的 **resources** 目录下，创建**mybatis-generator-config.xml**，内容如下

```
<?xml version="1.0" encoding="UTF-8" ?>
<!--mybatis的代码生成器相关配置-->
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!-- 引入配置文件 -->
    <properties resource="application-dev.properties"/>

    <!-- 一个数据库一个context,context的子元素必须按照它给出的顺序
        property*,plugin*,commentGenerator?,jdbcConnection,javaTypeResolver?,
        javaModelGenerator,sqlMapGenerator?,javaClientGenerator?,table+
    -->
    <context id="myContext" targetRuntime="MyBatis3" defaultModelType="flat">

        <!-- 这个插件给生成的Java模型对象增加了equals和hashCode方法 -->
        <!--<plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin"/>-->

        <!-- 注释 -->
        <commentGenerator>
            <!-- 是否不生成注释 -->
            <property name="suppressAllComments" value="true"/>
            <!-- 不希望生成的注释中包含时间戳 -->
            <!--<property name="suppressDate" value="true"/>-->
            <!-- 添加 db 表中字段的注释，只有suppressAllComments为false时才生效-->
            <!--<property name="addRemarkComments" value="true"/>-->
        </commentGenerator>


        <!-- jdbc连接 -->
        <jdbcConnection driverClass="${spring.datasource.driverClassName}"
                        connectionURL="${spring.datasource.url}"
                        userId="${spring.datasource.username}"
                        password="${spring.datasource.password}">
            <!--高版本的 mysql-connector-java 需要设置 nullCatalogMeansCurrent=true-->
            <property name="nullCatalogMeansCurrent" value="true"/>
        </jdbcConnection>

        <!-- 类型转换 -->
        <javaTypeResolver>
            <!--是否使用bigDecimal，默认false。
                false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer
                true，把JDBC DECIMAL 和 NUMERIC 类型解析为java.math.BigDecimal-->
            <property name="forceBigDecimals" value="true"/>
            <!--默认false
                false，将所有 JDBC 的时间类型解析为 java.util.Date
                true，将 JDBC 的时间类型按如下规则解析
                    DATE	                -> java.time.LocalDate
                    TIME	                -> java.time.LocalTime
                    TIMESTAMP               -> java.time.LocalDateTime
                    TIME_WITH_TIMEZONE  	-> java.time.OffsetTime
                    TIMESTAMP_WITH_TIMEZONE	-> java.time.OffsetDateTime
                -->
            <!--<property name="useJSR310Types" value="false"/>-->
        </javaTypeResolver>

        <!-- 生成实体类地址 -->
        <javaModelGenerator targetPackage="com.wqlm.boot.user.po" targetProject="src/main/java">
            <!-- 是否让 schema 作为包的后缀，默认为false -->
            <!--<property name="enableSubPackages" value="false"/>-->
            <!-- 是否针对string类型的字段在set方法中进行修剪，默认false -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>


        <!-- 生成Mapper.xml文件 -->
        <sqlMapGenerator targetPackage="mapper" targetProject="src/main/resources">
            <!--<property name="enableSubPackages" value="false"/>-->
        </sqlMapGenerator>

        <!-- 生成 XxxMapper.java 接口-->
        <javaClientGenerator targetPackage="com.wqlm.boot.user.dao" targetProject="src/main/java" type="XMLMAPPER">
            <!--<property name="enableSubPackages" value="false"/>-->
        </javaClientGenerator>


        <!-- schema为数据库名，oracle需要配置，mysql不需要配置。
             tableName为对应的数据库表名
             domainObjectName 是要生成的实体类名(可以不指定，默认按帕斯卡命名法将表名转换成类名)
             enableXXXByExample 默认为 true， 为 true 会生成一个对应Example帮助类，帮助你进行条件查询，不想要可以设为false
             -->
        <table schema="" tableName="user" domainObjectName="User"
               enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false"
               enableUpdateByExample="false" selectByExampleQueryId="false">
            <!--是否使用实际列名,默认为false-->
            <!--<property name="useActualColumnNames" value="false" />-->
        </table>
    </context>
</generatorConfiguration>

```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f3f82557db6e4a48a305dd09f93727b4~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

### application-dev.properties 的配置

MyBatis Generator Config 引用的外部配置文件内容如下

```
# mysql
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/boot?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=Asia/Shanghai
spring.datasource.username=root
spring.datasource.password=123456

```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/43c69d3120b44ef289dbc1efa228efdf~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

## 使用 MyBatis Generator 插件

配置好后，双击 maven 中的 MyBatis Generator 运行 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8c7a4e30710344cb8f939b071fedb5e4~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

# 1.5.2 集成 tk.mybatis (通用mapper)

上一节中，MyBatis Generator 为我们生成了一些常用的操作数据库的方法。其实我们也可以通过 集成 tk.mybatis (通用mapper) 来实现，集成之后，会有更多的通用方法，并且这些通用方法是不用配置mapper.xml 的。

springboot 集成 tk.mybatis (通用mapper) 一般需要3步

1. 引入依赖
2. 配置 tk.mybatis 的 MyBatis Generator 插件
3. 启动类上配置要扫描的 dao 路径
4. 配置通用 Mapper

> [tk.mybatis 的 GitHub](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fabel533%2FMapper%2Fwiki%2F1.3-spring-boot)

## 引入依赖

老规矩，还是在父 pom 中配置 tk.mybatis 的版本并申明 tk.mybatis 的依赖 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2aeb2b75dfe546448320fe1dfc7c46a6~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

```
<properties>
    <tk.mybatis.mapper-spring-boot-starter.version>2.1.5</tk.mybatis.mapper-spring-boot-starter.version>
</properties>

<!--申明依赖-->
<dependencyManagement>
    <dependencies>
        <!--tk.mybatis 通用mapper-->
        <dependency>
            <groupId>tk.mybatis</groupId>
            <artifactId>mapper-spring-boot-starter</artifactId>
            <version>${tk.mybatis.mapper-spring-boot-starter.version}</version>
        </dependency>
    </dependencies>
</dependencyManagement>

```

然后在 user 模块的 pom 中引入依赖

```
<dependencies>
    <!--tk.mybatis 通用mapper-->
    <dependency>
        <groupId>tk.mybatis</groupId>
        <artifactId>mapper-spring-boot-starter</artifactId>
    </dependency>
</dependencies>

```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5d27ea172ba747e29cdc7b9676eda880~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

## 配置 tk.mybatis 的 MyBatis Generator 插件

tk.mybatis 为 MyBatis Generator 开发了一个插件，用于改变 MyBatis Generator 的原始生成策略，配置好之后，生成出来的文件更加精练、注释更加有意义。

整个配置过程分两步

1. 引入插件依赖
2. 修改 MyBatis Generator Config

### 引入插件依赖

在原来的 MyBatis Generator 插件的 **dependencies** 里面添加如下依赖 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8fca2326005047f5b7e5e4d80132b258~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

```
<!--4.15 是目前最新的版本-->
<dependency>
    <groupId>tk.mybatis</groupId>
    <artifactId>mapper</artifactId>
    <version>4.1.5</version>
</dependency>

```

### 修改 MyBatis Generator Config

主要有一下几点改动

首先是 **targetRuntime** 的值改为 **MyBatis3Simple**，**defaultModelType** 设置为 **flat** ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/831bd748f5bc46899b77bd906ca1866f~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

如果 `targetRuntime="MyBatis3"` 的话，生成出来的 mapper.xml 会多出一段无用代码，如下 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2468cb27156247119bfb4cdb09f65873~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

然后添加 tk.mybatis 插件

```
<plugin type="tk.mybatis.mapper.generator.MapperPlugin">
    <!--dao 要继承的接口-->
    <property name="mappers" value="tk.mybatis.mapper.common.Mapper"/>
    <!--是否区分大小写，默认false-->
    <property name="caseSensitive" value="true"/>
</plugin>

```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a3c3f4e72aae4b288c63319ba1c7a7e7~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

其他地方都不需要改动，配置好之后，运行 MyBatis Generator 插件，生成出来的文件如下

**po** ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/91a8042b8225421c989fe1f000bfeb09~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp) 可以看到，相比与 MyBatis Generator 生成的注释，tk.mybatis 生成的注解跟简洁易懂。除此之外，它还多了几个注解

- **@Table(name = "user")**：意思是该po对应数据库的user表
- **@Id**：表示该属性对应user表的主键
- **@Column(name = "user_name")**：表示该属性对应user表的 **user_name** 字段

**dao** ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/4417e101249d48058e876e1d430821a6~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp) 相比于 MyBatis Generator 生成的代码，少了很多接口，但多继承了一个类，这个类就是你在 tk.mybatis 插件里面配置的类 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/18a291ad402b425a83c4cbdc3062056a~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp) 你可能猜到了，少的那些接口，都在继承的这个`tk.mybatis.mapper.common.Mapper`类中有，如下图，userMapper 继承了这么多方法，而且这些方法都是可以直接使用的 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/23db588546ca4ce6897f7da04b19d745~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

**mapper.xml** ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/668ebea3b426459688f92b7653b7b5cf~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp) 相比于 MyBatis Generator 少了很多代码

## 启动类上配置要扫描的 dao 路径

这一步我们在集成 mybatis 时已经配置过了 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e65f69f01ea7496687a069a309033173~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

但是集成 tk.mybatis 后，需要使用 tk.mybatis 包下的 `@MapperScan` ,因此需要修改一下 `@MapperScan` 的包路径 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c8fc718173d3452a90180c7c8fa119aa~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

```
import tk.mybatis.spring.annotation.MapperScan;

```

## 配置通用 Mapper

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6492443f60a74ae3a74d6f5fd518693b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

```
# 通用 mapper
# 主键自增回写方法,默认值MYSQL
mapper.identity=MYSQL
# 设置 insert 和 update 中，字符串类型!=""才插入/更新,默认false
#mapper.not-empty=true

```

tk.mybatis 至此就全部集成完了

# 1.5.3 集成 pagehelper 分页插件

分页查询是web开发中一个很常见的功能，mybatis 虽然也可以实现分页，但那是基于内存的分页，即把数据一次性全查出来，然后放在内存中分多次返回。

而 pagehelper 分页插件是物理分页，是通过 sql 关键字来实现的。例如mysql中的**limit**，oracle中的**rownum**等。

> pagehelper 分页插件和 tk.mybatis 是通一个作者写的[pagehelper项目地址](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fpagehelper%2Fpagehelper-spring-boot)

集成 pagehelper 需要两步

- 引入 pagehelper 依赖
- 配置 pagehelper

## 引入 pagehelper 依赖

老规矩，父pom中定义依赖版本，并申明依赖 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0d8e2b9027f64bf0aececb71a875dcb7~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

```
<properties>
    <pagehelper-spring-boot-starter.version>1.2.12</pagehelper-spring-boot-starter.version>
</properties>

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>${pagehelper-spring-boot-starter.version}</version>
        </dependency>
    </dependencies>
</dependencyManagement>

```

子模块中引入依赖 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d2cf920c43384863b9e3936f5dcbe623~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

```
<dependencies>
    <dependency>
        <groupId>com.github.pagehelper</groupId>
        <artifactId>pagehelper-spring-boot-starter</artifactId>
    </dependency>
</dependencies>

```

## 配置 pagehelper

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ca62bb2b7b0e44bc9fab70a3cd92b659~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

```
# 使用的sql方言
pagehelper.helperDialect=mysql
# 是否启用合理化，默认false，启用合理化时，如果 pageNum<1会查询第一页，如果pageNum>pages会查询最后一页
pagehelper.reasonable=true
# 是否支持通过Mapper接口参数来传递分页参数，默认false
#pagehelper.supportMethodsArguments=true
pagehelper.params=count=countSql

```

更多配置请参考[官网](https://link.juejin.cn/?target=https%3A%2F%2Fgithub.com%2Fpagehelper%2FMybatis-PageHelper%2Fblob%2Fmaster%2Fwikis%2Fzh%2FHowToUse.md)

# 1.6.0 引入 lombok 插件

lombok 是一个简化代码的插件，能通过一个注解帮你生成 get、set、tostring、hash... 方法。

集成 lombok 十分简单

1. 引入 lombok 依赖
2. 安装 ide 对应的 lombok 插件

lombok 使用方法参考这篇文章 [lombok 插件](https://juejin.cn/post/6844903961602818061)

## 引入 lombok 依赖

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fa134f2abc73462b958f899ec9a52b7c~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

由于 `spring-boot-starter-parent` 中已经申明了lombok 依赖，我们只需要在子模块中引入就好了

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/fcb29fbe5f4a4d8da39a3202605f0351~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

```
<!--lombok-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>

```

## 安装 ide 对应的 lombok 插件

我用的是 idea ，所以以idea为例

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2b935739e2914058afa74ec1c71be74a~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

# 1.7.0 集成 redis

作为最常用的 nosql 数据库，Spring Boot 对 redis 做了非常好对支持，集成起来也很简单，只需要4步

1. 引入依赖
2. 配置 redis
3. 自定义 RedisTemplate (推荐)
4. 自定义 redis 操作类 (推荐)

详细的集成步骤我单独提出来写了篇文章，地址如下

> [Spring Boot 2.0 集成 redis](https://juejin.cn/post/6844903990090694663)

# 1.7.1 集成 spring cache

Spring Cache 是 Spring 为缓存场景提供的一套解决方案。通过使用 @CachePut、@CacheEvict、@Cacheable等注解实现对缓存的，存储、查询、删除等操作

由于我们已经引入 **redis** ，所以只需要简单配置一下，就可以使用 **spring cache**

详细的集成步骤我单独提出来写了篇文章，地址如下(在 Spring Cache 那一节)

> [Spring Boot 2.0 集成 redis](https://juejin.cn/post/6844903990090694663)

> 当然，不用也可以选择不去集成

# 1.8.0 开发用户模块

不同的系统对应不同的用户设计，掘金的用户系统和淘宝的用户系统肯定是不一样的，所以，该项目虽然是一个种子项目，但很难给出通用的用户模块设计。

因此，我转变思路，并不去追求什么通用的用户模块，而是力求在该模块中，将上面集成的技术，全部都应用起来。

这样，在以后使用到上面的某项技术的时候，也有一个参照。

## 用户表

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/61a4e8f922b74455ba86448448f0af39~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp) 建表语句在 **配置 mysql** 那一章

## 添加 controller 、service 、dto

先创建 controller 、service 、dto 目录 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d47b591850574690baf26e4072ab09bd~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

先从注册接口开始写

注意校验参数一定要加 `@Valid` 注解 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7ccd503a23814d3db7b8d55cbca11383~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

其中 `@Data` 用到了 lombok 插件 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/97f89badb1cd4594b1abf45f8c3296f7~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

数据库存的是密码加盐后的hash，也就是说就连我们自己也看不到用户的密码 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/096e0f86347c43808da882a0821be054~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

# 1.8.1 自定义全局状态码和业务结果类

虽然向上面这样直接返回 `注册成功`、`注册失败` 也没有什么问题，但却不太优雅，因为其他接口返回的可能不是简单的字符串。

我们其实可以自定义一个业务结果类，所以的接口，都返回该业务结果对象，这个业务结果类，除了有业务结果外，还有业务执行状态、业务消息等。

除了业务结果类，我还建议创建全局状态码类，就想蚂蚁金服的api接口一样，调用失败会返回一个状态码，方便错误排查

## 自定义全局状态码

新建 enums 目录 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7fb8ce4af04f4bf0b458fec4b212eacc~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

创建 ApplicationEnum 全局状态码类，我这里只写了几个，之后可以往里面加 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a444154ead1c4b74946dccea2e777560~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

## 创建业务结果类

如下，在 vo 目录下下创建 result 目录，并在里面创建业务结果类 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/7faf67f7466c4c948220c9b69f6e4588~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c46474f1eb9843f9876ee774fa721193~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

为了方便使用，再创建一个 SuccessResult 和一个 FailResult ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5de1b9231c1e4ea7839581ce56750b91~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2c2f496b9a8b4751b7c759d6709890d6~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

## 改造注册接口

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b491253f8a9e4d1ca6d2821d5577e4de~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/32bdeeeb82df4d4b90c8dfd7b0dc8303~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

# 1.8.2 统一异常处理

业务执行过程中会产生的各种异常，对其进行统一处理是所有web项目的通用需求。Spring 提供了 @RestControllerAdvice、@ExceptionHandler 注解来帮助我们处理异常。

具体的异常处理该如何设计没有统一的标准，下面是我给出的设计，仅供参考。

我的做法是自定义一个异常类，然后在业务出错时抛出，最后在自定义异常处理类中处理。

> 更多内容请参考 [基于spring 的统一异常处理](https://juejin.cn/post/6844903822545027080)

## 自定义异常类

每一种异常都对于一种ApplicationEnum ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e70902848c174399ba3dbf8fd96b9e3b~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

## 自定义异常处理类

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/2403a153b16f41a685a5cb6cfa705a43~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

如果一个异常能匹配多个 @ExceptionHandler 时，选择匹配深度最小的Exception(即最匹配的Exception)

## 使用自定义异常

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ac30c98615a84a12967336cd826c7857~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/aa490c58276c42ca94768eff409eab5d~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

# 1.8.3 参数校验及异常处理

## 参数校验

首先在要校验的对象前加上 `@valid` 注解 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/56c3a4492b364e2e9efe503c3d629c03~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

然后在要校验的对象中使用适当的注解 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/19885a85b8be42c7b3e6bd00f17347b3~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

详细内容请参考

> [参数校验 Hibernate-Validator](https://juejin.cn/post/6844903961581846535)
> [Spring 参数校验详解](https://juejin.cn/post/6844904003310977031)

## 异常处理

如果参数绑定不成功或者校验不通过，就会抛出异常！但是默认抛出的异常包含很多敏感信息，如下： ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/684975ea0b154c07929f5bfd38f6029f~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp) 因此我们应该对常见的异常进行捕获后再封装。

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5bc05caa7634401c971a87a657b4e6d8~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

```
extends ResponseEntityExceptionHandler` 是为了重写几个常见异常的默认处理方式。当然，你也可以直接通过 `@ExceptionHandler()` 拦截，这样就不用`extends ResponseEntityExceptionHandler
```

![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/30980ec4c5f44c49931eae5ae8d7daf2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp) 一般只需要处理这三个异常就可以覆盖大部分需要手动处理参数异常的场景

- org.springframework.validation.BindException
- org.springframework.web.bind.MethodArgumentNotValidException
- javax.validation.ConstraintViolationException

详细内容请参考

> [Spring 参数校验的异常处理](https://juejin.cn/post/6844904003684302861)

# 1.8.4 添加登陆、修改密码、获取用户信息的接口

主要是些业务逻辑，没有什么指的说的，具体代码参考项目源码！唯一指的提一嘴的是 使用 `@Value("${}")` 注解获取配置文件中的属性 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/68f70a0f8a944d89b55298a7756792a2~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp) ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/eb6e1e8b7aad4f2481c65c0625fc85ee~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

# 1.8.5 添加认证拦截器

有些接口我们希望只有登陆的用户能访问，解决方案一般是添加一个登陆拦截器。

方法也很简单，主要分为3步

- 定义哪些接口需要认证(登陆后才能访问)，或者哪些接口不需要认证
- 自定义 `HandlerInterceptor` , 在访问接口前进行拦截处理
- 自定义 `WebMvcConfigurer` ，定义哪些接口需要拦截

1.确定免认证url ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/5a7db56697fe4988abbe5326afcf9cb0~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

2.自定义 `HandlerInterceptor` ，对拦截到的请求进行token有效性校验 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/3fb6a645c8b14c9ead98c56f18badfe4~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

3.自定义 `WebMvcConfigurer` ，拦截除免认证url列表之外的所有请求 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/45b9aa8bb09e4ae38aa1af440afee777~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

# 1.8.6 统一映射自定义配置

之前我们都是哪里需要用到 `properties` 中的配置，就在那里使用 `@Value("${}")` 来获取。如下 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/8793d77e92d14d5999b0f72033d22942~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

我们也可以创建一个自定义配置类，将所有 `properties` 中的自定义属性全部映射到对应的属性上，如下 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/65b9a532adc34d6ebe880f1d166d8a9f~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

然后使用时，直接访问该类 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/17e6f7fe628a4b25a3d01804fcc2464e~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp) ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a48b4f8711054d129434ec5593d13a13~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)

# 1.9.0 配置日志

spring boot 已经对日志系统进行了默认的配置，但是如果你想显示 sql 或将日志输出到文件就需要进行进一步配置。

详细内容请参考

> [java 生态下的日志框架](https://juejin.cn/post/6844904031228264456)
> [Java 日志实现框架 Logback](https://juejin.cn/post/6844904031224070157)
> [Logback 配置样例](https://juejin.cn/post/6844904036580196359)
> [Spring Boot Logging](https://juejin.cn/post/6844904037301616647)

## 显示 sql

在 `application.properties` 中配置

```
# dao(com.wqlm.boot.user.dao) 层设置成 debug 级别以显示sql
logging.level.com.wqlm.boot.user.dao=debug

```

## 输出日志到文件

在 `application.properties` 中配置

```
# 当前活动的日志文件名
logging.file.name=logs/user/user.log
# 最多保留多少天的日志
logging.file.max-history=30
# 单个日志文件最大容量
logging.file.max-size=10MB

```

## 精细化配置

在 `application.properties` 中只能进行有限的配置，如果想进一步配置，就需要使用对应日志框架的配置文件了！

spring boot 使用 `logback` 作为日志实现框架，spring boot 推荐使用 `logback-spring.xml` 作为配置文件的名称。

以下是一个参考的配置样例

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- scan ：开启"热更新" scanPeriod："热更新"扫描周期，默认 60 seconds(60秒)-->
<configuration scan="true" scanPeriod="300 seconds">

    <!-- 引入颜色转换器 -->
    <conversionRule conversionWord="clr" converterClass="org.springframework.boot.logging.logback.ColorConverter"/>

    <!-- 自定义变量  name ：变量名   scope ： 在哪个环境中查找 source ： 使用哪个属性 defaultValue ：没找到时的默认值-->
    <springProperty name="env" scope="context" source="spring.profiles.active" defaultValue="env"/>

    <!-- 应用名称 -->
    <property name="APP_NAME" value="user"/>

    <!-- 自定义变量，用于配置日志输出格式，这个格式是尽量偏向 spring boot 默认的输出风格
    %date：日期，默认格式 yyyy-MM-dd hhh:mm:ss,SSS 默认使用本机时区，通过 %d{yyyy-MM-dd hhh:mm:ss,SSS} 来自定义
    %-5level：5个占位符的日志级别，例如" info"、"error"
    %thread : 输出日志的线程
    %class : 输出日志的类的完全限定名，效率低
    %method : 输出日志的方法名
    %line : 输出日志的行号，效率低
    %msg : 日志消息内容
    %n : 换行
    -->
    <property name="LOG_PATTERN" value="%date %-5level ${PID:- } --- [%thread] %class.%method/%line : %msg%n"/>

    <!-- 彩色日志格式 -->
    <property name="LOG_PATTERN_COLOUR"
              value="${env} %date %clr(%-5level) %magenta(${PID:- }) --- [%thread] %cyan(%class.%method/%line) : %msg%n"/>


    <!--日志输出器. ch.qos.logback.core.ConsoleAppender : 输出到控制台-->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <!-- 配置日志输出格式 -->
            <pattern>${LOG_PATTERN_COLOUR}</pattern>
            <!-- 使用的字符集 -->
            <charset>UTF-8</charset>
        </encoder>
    </appender>

    <!-- 日志输出器。ch.qos.logback.core.rolling.RollingFileAppender : 滚动输出到文件 -->
    <appender name="ROLLING" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <!-- 活动中的日志文件名(支持绝对和相对路径) -->
        <file>logs/${APP_NAME}/${APP_NAME}.log</file>
        <!-- 滚动策略. ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy : 按照大小和时间滚动-->
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <!-- 何时触发滚动，如何滚动，以及滚动文件的命名格式
            %d : 日期，默认格式 yyyy-MM-dd，通过 %d{yyyy-MM-dd hhh:mm:ss} 来自定义格式。logback 就是通过 %d 知道了触发滚动的时机
            %i : 单个滚动周期内的日志文件的序列号
            .zip : 将日志文件压缩成zip。不想压缩，可以使用.log 结尾
            如下每天0点以后的第一日志请求触发滚动，将前一天的日志打成 zip 压缩包存放在 logs/app1/backup 下，并命名为 app1_%d_%i.zip
            -->
            <fileNamePattern>logs/${APP_NAME}/backup/${APP_NAME}_%d{yyyy-MM-dd}_%i.zip</fileNamePattern>

            <!--单个日志文件的最大大小-->
            <maxFileSize>10MB</maxFileSize>

            <!--删除n个滚动周期之前的日志文件(最多保留前n个滚动周期的历史记录)-->
            <maxHistory>30</maxHistory>
            <!-- 在有 maxHistory 的限制下，进一步限制所有日志文件大小之和的上限，超过则从最旧的日志开始删除-->
            <totalSizeCap>1GB</totalSizeCap>
        </rollingPolicy>
        <encoder>
            <!-- 日志输出格式 -->
            <pattern>${LOG_PATTERN}</pattern>
            <!-- 使用的字符集 -->
            <charset>UTF-8</charset>
        </encoder>
    </appender>


    <!-- 非 prod 环境下使用以下配置 -->
    <springProfile name="!prod">
        <!-- 记录器 name : 包名或类名， level : 要记录的日志的起始级别， additivity : 是否追加父类的 appender -->
        <logger name="com.wqlm.boot.dao" level="debug" additivity="false">
            <appender-ref ref="STDOUT"/>
            <appender-ref ref="ROLLING"/>
        </logger>
    </springProfile>


    <!-- 根记录器 -->
    <root level="info">
        <!-- 使用 STDOUT、ROLLING 输出记录的日志-->
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="ROLLING"/>
    </root>
</configuration>

```

相对路径的位置 ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f3135ca16dfa4a5c9448485fc7da0adb~tplv-k3u1fbpfcp-zoom-in-crop-mark:1304:0:0:0.awebp)