---
title: mysql连表操作后字符集不同导致索引失效问题1
date: 2022-02-28 19:57:47
tags: [mysql]
categories: 技术
---
# mysql 连表操作后字符集不同导致索引失效

## 背景

一个表的字符集为utf8mb4
一个表的字符集为utf8

连表查询后可以发现索引失效的情况，查询很慢。这时候可以通过show warnings;查询警告信息。发现了convert(testdb.t1.code using utf8mb4)之后，发现2个表的字符集不一样。

t1为utf8，t2为utf8mb4。但是为什么表字符集不一样（实际是字段字符集不一样）就会导致t1全表扫描呢？下面来做分析。

## 分析原因

（1）首先t2 left join t1决定了t2是驱动表，这一步相当于执行了

```sql
select * from t2 where t2.name = 'dddd'
```

取出code字段的值，这里为’8a77a32a7e0825f7c8634226105c42e5’;

（2）然后拿t2查到的code的值根据join条件去t1里面查找，这一步就相当于执行了

```sql
select * from t1 where t1.code = '8a77a32a7e0825f7c8634226105c42e5';
```

（3）但是由于第（1）步里面t2表取出的code字段是utf8mb4字符集，而t1表里面的code是utf8字符集，这里需要做字符集转换，字符集转换遵循由小到大的原则，因为utf8mb4是utf8的超集，所以这里把utf8转换成utf8mb4，即把t1.code转换成utf8mb4字符集，转换了之后，由于t1.code上面的索引仍然是utf8字符集，所以这个索引就被执行计划忽略了，然后t1表只能选择全表扫描。如果t2筛选出来的记录不止1条，那么t1就会被多次全表扫描，性能之差可想而知。

## 修改字符集

```sql
alter table t1 charset utf8mb4;
```

但这是错的，这只是改了表的默认字符集，即新的字段才会使用utf8mb4，已经存在的字段仍然是utf8。
show create table t1 会发现DEFAULT CHARSET=utf8mb4改变了，但是SHOW FULL COLUMNS FROM pay_log_all_from_mq;会发现字段的字符集没有改变。

```sql
alter table t1 convert to charset utf8mb4;
```

查看 SHOW FULL COLUMNS FROM t1 ;会发现字段的字符已经改变。

## 注意点

- 表字符集不同时，可能导致join的SQL使用不到索引，引起严重的性能问题；