---
title: 10、【对线面试官】TreadLocal
tags:
  - 面试
  - 记录
categories: 技术
abbrlink: 2d4446ad
date: 2022-02-28 19:57:47
---
# 10、【对线面试官】TreadLocal
## 今天要不来聊聊ThreadLocal吧？

1. 我个人对ThreadLocal理解就是
2. 它能够提供了线程的局部变量让每个线程都可以通过set/get来对这个局部变量进行操作
3. 不会和其他线程的局部变量进行冲突，实现了线程的数据隔离

## 你在工作中有用到过ThreadLocal吗？

1. 这块是真不多，不过还是有一处的。就是我们项目有个的DateUtils工具类
2. 这个工具类主要是对时间进行格式化
3. 格式化/转化的实现是用的SimpleDateFormat
4. 但众所周知SimpleDateFormat不是线程安全的 ，所以我们就用ThreadLocal来让每个线程装载着自己的SimpleDateFormat对象
5. 以达到在格式化时间时，线程安全的目的
6. 在方法上创建SimpleDateFormat对象也没问题，但每调用一次就创建一次有点不优雅
7. 在工作中ThreadLocal的应用场景确实不多，但要不我给你讲讲Spring是怎么用的？

## spring中的应用

1. Spring提供了事务相关的操作，而我们知道事务是得保证一组操作同时成功或失败的
2. 这意味着我们一次事务的所有操作需要在同一个数据库连接上
3. 但是在我们日常写代码的时候是不需要关注这点的
4. Spring就是用的ThreadLocal来实现，Th readLocal存储的类型是一个Map
5. Map中的key是DataSource，value是C onnection（为了应对多数据源的情况，所以是一个Map）
6. 用了ThreadLocal保证了同一个线程获取一个Connection对象，从而保证一次事务的所有操作需要在同一个数据库连接上

## 你知道ThreadLocal内存泄露这个知识点吗？

1. 了解的，要不我先来讲讲ThreadLocal的原理？
   - ThreadLocal是一个壳子，真正的存储结构是ThreadLocal里有ThreadLocalMap这么个内部类
   - 而有趣的是，ThreadLocalMap的引用是在Thread上定义的
   - ThreadLocal本身并不存储值，它只是作为key来让线程从ThreadLocalMap获取value
   - 所以，得出的结论就是ThreadLocalMap该结构本身就在Thread下定义，而ThreadLocal只是作为key，存储set到ThreadLocalMap的变量当然是线程私有的咯

![](https://cdn.jsdelivr.net/gh/swimminghao/picture@main/img/NxSnnq_20211029171330.png)

