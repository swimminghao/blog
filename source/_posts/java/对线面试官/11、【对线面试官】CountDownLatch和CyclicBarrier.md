---
title: 11、【对线面试官】CountDownLatch和CyclicBarrier
tags:
  - 面试
  - 记录
categories: 技术
abbrlink: 58f7b356
date: 2022-02-28 19:57:47
---
# 11、【对线面试官】CountDownLatch和CyclicBarrier

## 我现在有个场景：现在我有50个任务，这50个任务在完成之后，才能执行下一个「函数」，要是你，你怎么设计？

1. 可以用JDK给我们提供的线程工具类，CountDownLatch和CyclicBarrier都可以完成这个需求。
2. 这两个类都可以等到线程完成之后，才去执行某些操作

## 那既然都能实现的话？那CountDownLatch和CyclicBarrier有什么什么区别呢？

- 主要的区别就是CountDownLatch用完了，就结束了，没法复用。而CyclicBarrier不一样，它可以复用。
- 比如说，你得给我解释：CountDownLatch和CyclicBarrier都是线程同步的工具类
  od
- CountDownLatch允许一个或多个线程一直等待，直到这些线程完成它们的操作
- 而CyclicBarrier不一样，它往往是当线程到达某状态后，暂停下来等待其他线程等到所有线程均到达以后，才继续执行
- 可以发现这两者的等待主体是不一样的。
- CountDownLatch调用await（）通常是主线程/调用线程，而CyclicBarrier调用await（）是在任务线程调用的
- 所以，CyclicBarrier中的阻塞的是任务的线程，而主线程是不受影响的
- 简单叙述完这些基本概念后，可以特意抛出这两个类都是基于AQS实现的
- countDownLatch
  1. 前面提到了CountDownLatch也是基于AQS实现的，它的实现机制很简单
  2. 当我们在构建CountDownLatch对象时，传入的值其实就会赋值给AQS的关键变量state
  3. 执行countDown方法时，其实就是利用CAS将state-1
     执行await方法时，其实就是判断state是否为0，不为0则加入到队列中，将该线程阻塞掉（除了头结点）
  4. 因为头节点会一直自旋等待state为0，当state为0时，头节点把剩余的在队列中阻塞的节点也一并唤醒
- CycllicBarrier
  1. 从源码不难发现的是，它没有像CountDo wnLatch和ReentrantLock使用AQS的stat e变量，而CyclicBarrier是直接借助ReentrantLock加上Condition等待唤醒的功能进而实现的
  2. 在构建CyclicBarrier时，传入的值会赋值给CyclicBarrier内部维护count变量，也会赋值给parties变量（这是可以复用的关键）
  3. 每次调用await时，会将count-1，操作count值是直接使用ReentrantLock来保证线程安全性
  4. 如果count不为0，则添加则condition队列中
  5. 如果count等于0时，则把节点从condition队列添加至AQS的队列中进行全部唤醒，并且将parties的值重新赋值为count的值（实现复用）

## 那如果是这样的话，那我多次用CountDownLatch不也可以解决问题吗？

1. 是这样的，我提出了个场景，它确实很像可以用CountDownLatch和CyclicBarrier解决
2. 但是，作为面试者的你可以尝试向我获取更多的信息
3. 我可没说一个任务就用一个线程处理哦
4. 放一步讲，即便我是想考察CountDownLatch和CyclicBarrier的知识，但是过程也是很重要的。我会看你在这个过程中思考的以及沟通

## 总结

1. CountDownlatch基于AQS实现，会将构造CountDownLatch的入参传递至state，countDown（）就是在利用CAS将state减- 1，await（）实际就是让头节点一直在等待s tate为0时，释放所有等待的线程
2. 而CyclicBarrier则利用ReentrantLock和Condition，自身维护了count和parties变量。每次调用await将count-1，并将线程加入到condition队列上。等到count为0时，则将condition队列的节点移交至AQS队列，并全部释放。
3. 