---
title: PostgreSQL 的 SQL 组合查询
date: 2018-07-16 15:13:17
tags:
- PostgreSQL
- SQL
---

两个查询的结果可以用集合操作并、交、差进行组合。语法是 
```sql
query1 UNION [ALL] query2
query1 INTERSECT [ALL] query2
query1 EXCEPT [ALL] query2
```
query1和query2都是可以使用以上所有特性的查询。集合操作也可以嵌套和级连，例如 
`query1 UNION query2 UNION query3`
实际执行的是： 
`(query1 UNION query2) UNION query3`

UNION有效地把query2的结果附加到query1的结果上（不过我们不能保证这就是这些行实际被返回的顺序）。此外，它将删除结果中所有重复的行， 就象DISTINCT做的那样，除非你使用了UNION ALL。 
INTERSECT返回那些同时存在于query1和query2的结果中的行，除非声明了INTERSECT ALL， 否则所有重复行都被消除。 
EXCEPT返回所有在query1的结果中但是不在query2的结果中的行（有时侯这叫做两个查询的差）。同样的，除非声明了EXCEPT ALL，否则所有重复行都被消除。 
