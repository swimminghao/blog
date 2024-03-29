---
title: 20、【对线面试官】使用kafka会考虑什么问题
tags:
  - 面试
  - 记录
categories: 技术
abbrlink: c5b7e17d
date: 2022-02-28 19:57:47
---
# 20、【对线面试官】使用kafka会考虑什么问题
## 你提到了你这边会从交易的消息报获取到订单的数据，然后做业务的处理；也提到了你用的是Kafka，我想问下，Kafka会丢数据吗？

1. 嗯，使用Kafkal时，有可能会有以下场景会丢消息
2. 比如说，我们用Producer发消息至Broke的时候，就有可能会丢消息
3. 如果你不想丢消息，那在发送消息的时候，需要选择带有callBack的api进行发送
4. 其实就意味着，如果你发送成功了，会回调告诉你已经发送成功了。如果失败了，那收到回调之后自己在业务上做重试就好了。
5. 等到把消息发送到Brokerl以后，也有可能丢消息
6. 一般我们的线上环境都是集群环境下嘛，但可能你发送的消息后broker就挂了，这时挂掉的broker还没来得及把数据同步给别的broker，数据就自然就丢了
7. 发送到Broker之后，也不能保证数据就一定不丢了，毕竟Broker会把数据存储到磁盘之前，走的是操作系统缓存
8. 也就是异步刷盘这个过程还有可能导致数据会丢



1. 嗯，到这里其实我已经说了三个场景了，分别是：producer-》broker，broker-》broker之间同步，以及broker-》磁盘
2. 要解决上面所讲的问题也比较简单，这块也没什么好说的…
3. 不想丢数据，那就使用带有callback的api设置acks、retries、factor等等些参数来保证Producer发送的消息不会丢就好啦。

## 一般来说，还是client消费broker丢消息的场景比较多；那你们在消费数据的时候是怎么保证数据的可靠性的呢？

1. 首先，要想client端消费数据不能丢，肯定是不能使用autoCommit的，所以必须是手动提交的。

2. 我们这边是这样实现的：

   一、从Kafka拉取消息、（一次批量拉取500条，这里主要看配置）
   二、为每条拉取的消息分配一个msgld（递增）
   三、将msgld存入内存队列（sortSet）中
   四、使用Map存储msgld.与msg（有offset相关的信息）的映射关系

   五、当业务处理完消息后，ack时，获取当前处理的消息nsgld，然后从sortSet删除该msgld（此时代表已经处理过了）
   六、接着与sortSet队列的首部第一个ld比较（其实就是最小的msgld），如果当前msgld<=sort Set第一个ID，则提交当前offset

   七、系统即便挂了，在下次重启时就会从sortSet队首的消息开始拉取，实现至少处理一次语义
   八、会有少量的消息重复，但只要下游做好幂等就OK了

## 嗯，你也提到了幂等，你们是怎么实现幂等性的呢？

1. 嗯，还是以处理订单消息为例好了。
2. 幂等Key我们由订单编号+订单状态所组成（一笔订单的状态只会处理一次）
3. 在处理之前，我们首先会去查Redis：是否存在该Key，如果存在，则说明我们已经处理过了，直接丢掉
4. 如果Redis没处理过，则继续往下处理，最终的逻辑是将处理过的数据插入到业务DB上，再到最后把幂等Key插入到Redis上
5. 显然，单纯通过Redis是无法保证幂等的
6. 所以，Redis其实只是一个「前置」处理，最终的幂等性是依赖数据库的唯一Key来保证的（唯一Key实际上也是订单编号+状态）
7. 而插入DB是依赖事务的，所以是没问题的
8. 总的来说，就是通过Redis做前置处理，DB唯一索引做最终保证来实现幂等性的

## 你们那边遇到过顺序消费的问题吗？

1. 嗯，也是有的，我举个例子

2. 订单的状态比如有支付、确认收货、完成等等，而订单下还有计费、退款的消息报

3. 理论上来说，支付的消息报肯定要比退款消息报先到嘛，但程序处理的过程中可不一定的嘛

4. 所以在这边也是有消费顺序的问题（先处理了支付，才能退款啊）

5. 但在广告场景下不是「强顺序」的，只要保证最终一致性就好了。

6. 所以我们这边处理「乱序」消息的实现是这样的：

   1）宽表：将每一个订单状态，单独分出一个或多个独立的字段。消息来时只更新对应的字段就好，消息只会存在短暂的状态不一致问题，但是状态最终是一致的

   2）消息补偿机制：另一个进行消费相同topicl的数据，消息落盘，延迟处理。将消息与DB进行对比，如果发现数据不一致，再重新发送消息至主进程处理

   3）还有部分场景，可能我们只需要把相同userld/orderld.发送到相同的partition（因为一个partition由一个Consumer消费），又能解决大部分消费顺序的问题了呢。

   