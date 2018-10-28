---
title: PostgreSQL 的 SQL 数据操纵
date: 2018-07-16 12:02:35
tags:
- PostgreSQL
- SQL
---

# 插入数据

```sql
INSERT INTO 表 VALUES (数据1, 数据2, ...);
```

显式的列出表，顺序任意

```sql
INSERT INTO 表 (列1, 列2, ...) VALUES (数据1, 数据2, ...);
```

显式地要求缺省值

```sql
INSERT INTO 表 (列1, 列2, ...) VALUES (DEFAULT, 数据2, ...);
```

一个命令中插入多行

```sql
INSERT INTO 表 (列1, 列2, ...) VALUES 
    (DEFAULT, 数据2, ...),
    (DEFAULT, 数据2, ...),
    (DEFAULT, 数据2, ...);
```

# 更新数据

修改已经存储在数据库中的数据的行为叫做更新。你可以更新单个行，也可以更新表中所有的行，还可以更新其中的一部分行。 我们可以独立地更新每个列，而其他的列则不受影响。 
要更新现有的行，使用UPDATE命令。这需要提供三部分信息： 

1. 表的名字和要更新的列名
2. 列的新值
3. 要更新的是哪（些）行

```sql
UPDATE 表 SET 列 = 新值 WHERE 条件;
```

用于新值的表达式也可以引用行中现有的值。
```sql
UPDATE 表 SET 列 = 列 * 1.10;
```

忽略WHERE子句， 那么就意味着表中的所有行都要被更新。如果出现了WHERE子句， 那么只有匹配它后面的条件的行被更新。

# 删除数据

```sql
DELETE FROM 表 WHERE 条件;
```

```sql
DELETE FROM products;
```
那么表中所有行都会被删除！程序员一定要注意。