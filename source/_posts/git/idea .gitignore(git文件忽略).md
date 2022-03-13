---
title: idea .gitignore(git文件忽略)
date: 2022-02-28 19:57:47
tags: [git]
categories: 技术
---

# idea .gitignore(git文件忽略)



idea使用git通常需要忽略一些临时文件，需要配置.gitignore插件

1. 安装插件
   File -> Settings -> Plugins 搜索框搜索.ignore，点击安装

2. 生成初始.ignore文件

   ![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/svvoLk_20210619221210.png)

出现如下弹框，会默认生成所选语言的常用忽略项，我这里选java，



![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/0XlItZ_20210619221230.png)



生成如下文件

```java
# Compiled class file
*.class

# Log file
*.log

# BlueJ files
*.ctxt

# Mobile Tools for Java (J2ME)
.mtj.tmp/

# Package Files #
*.jar
*.war
*.ear
*.zip
*.tar.gz
*.rar

# virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
```

idea项目一般需要自己增加如下两项

```undefined
.idea/
target/
```

即忽略这两个文件夹即文件夹下的所有文件

现在可以使用了，提交一次测试下

发现.idea文件夹下的文件还有变更被提交，这是因为在使用gitignore之前，此文件就以及被跟踪了，这样的话需要移除跟踪，如下命令:

移除指定文件夹即文件夹下所有文件：

```java
git rm --cached --force -r .idea 
rm '.idea/artifacts/xxx_api_war.xml'
rm '.idea/artifacts/xxx_api_war_exploded.xml'
rm '.idea/artifacts/xxx_task_war.xml'
rm '.idea/artifacts/xxx_task_war_exploded.xml'
rm '.idea/libraries/Maven__ch_qos_logback_logback_classic_1_1_7.xml'
rm '.idea/libraries/Maven__ch_qos_logback_logback_core_1_1_7.xml'
rm '.idea/libraries/Maven__com_alibaba_druid_1_0_22.xml'
rm '.idea/libraries/Maven__com_alibaba_fastjson_1_2_14.xml'
rm '.idea/libraries/Maven__com_aliyun_openservices_ons_client_1_2_4.xml'
rm '.idea/libraries/Maven__com_fasterxml_aalto_xml_0_9_11.xml'
rm '.idea/libraries/Maven__com_fasterxml_jackson_core_jackson_annotations_2_5_2.xml'
rm '.idea/libraries/Maven__com_fasterxml_jackson_core_jackson_core_2_5_2.xml'
rm '.idea/libraries/Maven__com_fasterxml_jackson_core_jackson_databind_2_5_2.xml'
rm '.idea/libraries/Maven__com_fasterxml_jackson_dataformat_jackson_dataformat_xml_2_5_2.xml'
rm '.idea/libraries/Maven__com_fasterxml_jackson_module_jackson_module_jaxb_annotations_2_5_2.xml'
rm '.idea/libraries/Maven__com_google_guava_guava_19_0.xml'
rm '.idea/libraries/Maven__commons_codec_commons_codec_1_9.xml'
rm '.idea/libraries/Maven__commons_httpclient_commons_httpclient_3_1.xml'
rm '.idea/libraries/Maven__commons_logging_commons_logging_1_0_4.xml'
rm '.idea/libraries/Maven__commons_logging_commons_logging_1_2.xml'
rm '.idea/libraries/Maven__javax_servlet_javax_servlet_api_3_1_0.xml'
rm '.idea/libraries/Maven__mysql_mysql_connector_java_5_1_39.xml'
rm '.idea/libraries/Maven__org_apache_commons_commons_lang3_3_3_2.xml'
rm '.idea/libraries/Maven__org_apache_commons_commons_pool2_2_0.xml'
rm '.idea/libraries/Maven__org_apache_httpcomponents_httpclient_4_4.xml'
rm '.idea/libraries/Maven__org_apache_httpcomponents_httpcore_4_4.xml'
rm '.idea/libraries/Maven__org_aspectj_aspectjweaver_1_8_9.xml'
rm '.idea/libraries/Maven__org_codehaus_woodstox_stax2_api_3_1_4.xml'
rm '.idea/libraries/Maven__org_mongodb_mongo_java_driver_2_14_0.xml'
rm '.idea/libraries/Maven__org_mybatis_mybatis_3_4_1.xml'
rm '.idea/libraries/Maven__org_mybatis_mybatis_spring_1_3_0.xml'
rm '.idea/libraries/Maven__org_quartz_scheduler_quartz_2_2_2.xml'
rm '.idea/libraries/Maven__org_slf4j_jcl_over_slf4j_1_7_21.xml'
rm '.idea/libraries/Maven__org_slf4j_slf4j_api_1_7_21.xml'
rm '.idea/libraries/Maven__org_springframework_data_spring_data_commons_1_12_2_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_data_spring_data_mongodb_1_9_2_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_spring_aop_4_3_1_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_spring_aspects_4_3_1_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_spring_beans_4_3_1_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_spring_context_4_3_1_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_spring_context_support_4_3_1_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_spring_core_4_3_1_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_spring_expression_4_3_1_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_spring_jdbc_4_3_1_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_spring_tx_4_3_1_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_spring_web_4_3_1_RELEASE.xml'
rm '.idea/libraries/Maven__org_springframework_spring_webmvc_4_3_1_RELEASE.xml'
rm '.idea/libraries/Maven__redis_clients_jedis_2_6_2.xml'
```

移除指定文件：

```java
git rm --cached --force ydq-api/ydq-api.iml
rm 'ydq-api/ydq-api.iml'
```

这样，表示移除成功。

现在，上面的操作进行提交：

![img](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/wa03I1_20210619221316.png)

移交移除文件.png

以后再做一些变更，当再次提交时，只有未被忽略的（被忽略的文件的变更再也不会被提交了）修改的文件了。

现在，idea下配置.gitignore结束。

注：
**.gitignore只能忽略那些原来没有被track的文件，如果某些文件已经被纳入了版本管理中，则修改.gitignore是无效的。那么解决方法就是先把本地缓存删除（改变成未track状态），然后再提交：**
**输入：**
**git rm -r –cached filePath**
**git commit -m “remove xx”**
**或者：**
**git rm -r –cached .**
**git add .**
**git commit -m “update .gitignore”**

**来解释下几个参数 -r 是删除文件夹及其子目录 –cached 是删除暂存区里的文件而不删除工作区里的文件，第一种是删除某个文件，第二种方法就把所有暂存区里的文件删了，再加一遍，相当于更新了一遍。**
