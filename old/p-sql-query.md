---
title: PostgreSQL 的 SQL 查询
date: 2018-07-16 12:23:01
tags:
- PostgreSQL
- SQL
---

从数据库中检索数据的过程或命令叫做查询。
SQL里用SELECT命令用于查询。

SELECT命令的一般语法是：
```sql
[WITH with_queries] SELECT select_list FROM table_expression[sort_specification]
```

简单查询形式：
```sql
SELECT * FROM table1;
```
从table1中检索所有列。

可以对列之间进行计算
```sql
SELECT a, b + c FROM table1;
```
假设table1拥有列a,b,c。

可以省略整个表达式把SELECT当做计算器。
```sql
SELECT 3 * 4;
```

可以调用函数
```sql
SELECT random();
```
