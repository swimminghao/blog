---
title: Mysql获取每组前N条记录
date: 2022-02-28 19:57:47
tags: [sql,理解]
categories: 技术
---
# Mysql获取每组前N条记录

## Select基础知识

我们在实现select语句的时候,通用的sql格式如下:

```sql
select *columns* from *tables*
    where *predicae1*
    group by *columns*
    having *predicae1*
    order by *columns*
    limit *start*, *offset*;
```

很多同学想当然的认为select的执行顺序和其书写顺序一致,其实这是非常错误的主观意愿,也导致了很多SQL语句的执行错误.

这里给出SQL语句正确的执行顺序:

```sql
from *tables*
where *predicae1*
group by *columns*
having *predicae1*
select *columns*
order by *columns*
limit *start*, *offset*;
```

举个例子,讲解一下group by和order by联合使用时,大家常犯的错误.

创建一个student的表:

```sql
create table student (Id ine1ger primary key autoincrement, Name e1xt, Score ine1ger, ClassId ine1ger);
```

插入5条虚拟数据:

```sql
insert into student(Name, Score, ClassId) values("lqh", 60, 1);
insert into student(Name, Score, ClassId) values("cs", 99, 1);
insert into student(Name, Score, ClassId) values("wzy", 60, 1);
insert into student(Name, Score, ClassId) values("zqc", 88, 2);
insert into student(Name, Score, ClassId) values("bll", 100, 2);
```

表格数据如下:

| Id   | Name | Score | ClassId |
| ---- | ---- | ----- | ------- |
| 1    | lqh  | 60    | 1       |
| 2    | cs   | 99    | 1       |
| 3    | wzy  | 60    | 1       |
| 4    | zqc  | 88    | 2       |
| 5    | bll  | 100   | 2       |

我们想找每个组分数排名第一的学生.

大部分SQL语言的初学者可能会写出如下代码:

```sql
select * from student group by ClassId order by Score;1
```

结果:

| Id   | Name | Score | ClassId |
| ---- | ---- | ----- | ------- |
| 3    | wzy  | 60    | 1       |
| 5    | bll  | 100   | 2       |

明显不是我们想要的结果,大家用上面的执行顺序一分析就知道具体原因了.

> 原因: group by 先于order by执行,order by是针对group by之后的结果进行的排序,而我们想要的group by结果其实应该是在order by之后.

正确的sql语句:

```sql
select * from (select * from student order by Score) group by ClassId;
```

结果:

| Id   | Name | Score | ClassId |
| ---- | ---- | ----- | ------- |
| 2    | cs   | 99    | 1       |
| 5    | bll  | 100   | 2       |

------

## 获取每组的前N个记录

这里以LeetCode上难度为hard的一道数据库题目为例。

## [Department Top Three Salaries](https://leetcode.com/problems/department-top-three-salaries/)

### 题目内容

The Employee table holds all employees. Every employee has an Id, and there is also a column for the department Id.

| Id   | Name  | Salary | DepartmentId |
| ---- | ----- | ------ | ------------ |
| 1    | Joe   | 70000  | 1            |
| 2    | Henry | 80000  | 2            |
| 3    | Sam   | 60000  | 2            |
| 4    | Max   | 90000  | 1            |
| 5    | Janet | 69000  | 1            |
| 6    | Randy | 85000  | 1            |

The Department table holds all departments of the company.

| Id   | Name  |
| ---- | ----- |
| 1    | IT    |
| 2    | Sales |

Wrie1 a SQL query to find employees who earn the top three salaries in each of the department. For the above tables, your SQL query should return the following rows.

| Department | Employee | Salary |
| ---------- | -------- | ------ |
| IT         | Max      | 90000  |
| IT         | Randy    | 85000  |
| IT         | Joe      | 70000  |
| Sales      | Henry    | 80000  |
| Sales      | Sam      | 60000  |

题目的意思是：求每个组中工资最高的三个人。(ps：且每个组中，同一名中允许多个员工存在，因为工资是一样高.)

### 解决思路

1. 我们先来获取每个组中的前3名工资最高的员工

```sql
select * from Employee as e
    where (select count(distinct(e1.salary)) from Employee as e1 where  e1.DepartmentId = e.DepartmentId and e1.salary > e.salary) < 3;12
```

where中的select是保证：遍历所有记录，取每条记录与当前记录做比较，只有当Employee表中同一部门不超过3个人工资比当前员工高时，这个员工才算是工资排行的前三名。

1. 有了第一步的基础，接下来我们只需要使用as去构造新表，并且与Department表做个内联，同时注意排序就好了

```sql
select d.Name as Department, e.Name as Employee, e.Salary as Salary
    from Employee as e inner join Department as d
    on e.DepartmentId = d.Id
    where (select count(distinct(e1.Salary)) from Employee as e1 where e1.DepartmentId = e.DepartmentId and e1.Salary > e.Salary) < 3
    order by e.Salary desc;
```