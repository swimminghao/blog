---
title: sql难点记录
date: 2022-02-28 19:57:47
tags: [编程,感悟]
categories: 技术
---


# sql难点记录

## 1.

The expression **subject IN ('Chemistry','Physics')** can be used as a value - it will be **0** or **1**.

Show the 1984 winners and subject ordered by subject and winner name; but list Chemistry and Physics last.

这个题目没有中文，翻译的大概意思是 按照获奖的科学领域跟获奖者的名字来排序，但是 化学和物理要被排在最后

```sql
SELECT winner, subject FROM nobel where yr=1984 ORDER BY subject IN ('Physics','Chemistry'),subject asc,winner asc
```
相当于
```sql
SELECT winner, subject FROM nobel where yr=1984 
ORDER BY  CASE WHEN subject IN ('Physics','Chemistry') THEN 1 
								ELSE 0 END,
                subject asc,winner asc
```
相当于
```sql
SELECT winner, subject FROM nobel where yr=1984 
ORDER BY ( case subject
           when 'Chemistry' then 1
           when 'Physics'   then 1
           else 0
           end),subject asc,winner asc
```

这里分析一下，以后也用得上，关键在order by subject IN ('Physics','Chemistry') ,subject asc,winner asc

后两个比较容易理解 字段名加上asc表示按正常排序，难点在 subject in (xxx)这个表达式，

排除后两个表达式，这是一个分组排序，

subject in(xxx)为0的分成一组 排序

subject in(xxx)为1的分成一组 排序

得到结果连接起来就是新的排序表

subject in(xxx) desc ：新的排序表 就在前面

subject in(xxx) asc ：新的排序表 就在后面 （默认asc）