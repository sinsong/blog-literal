---
title: PostgreSQL 的 SQL (start)
date: 2018-07-14 22:04:02
tags:
- PostgreSQL
- SQL
---

# 概念

PostgreSQL是一种 关系型数据库管理系统(RDBMS)。它是一种用于管理存储在关系中的数据的系统。
每个表都是一个命名的行的集合。一个给定表的每一行由同一组的命名列组成，而且每一列都有一个特定的数据类型。虽然列在每行里的顺序时固定的，但一定要记住SQL并不对表中的顺序做任何保证（你可以显式的排序）。
表被分组成数据库，一个由单个PostgreSQL服务器实例管理的数据库集合组成一个数据库集簇。

# 创建表

```sql
CREATE TABLE account (
    user    varchar(12),    --用户名
    passwd  varchar(16)     --密码
);
```

psql识别命令到分号才结束。

SQL命令中自由使用空白。随意对齐，两个横线(`--`)引入注释，一行中注释标记后的内容都被忽略。SQL对关键字和标识符大小写不敏感，只有标识符用双引号包围时才能保留他们的大小写！

# 删除表

```sql
DROP TABLE account;
```
这样就删掉了account表。

# 在表中添加行

使用INSERT语句

```sql
INSERT INTO account VALUES('somebody', 'youdontseethat');
```

不是简单数字值的常量通常必须用单引号(`'`)包围。

这个语法要求与表定义中列的顺序匹配。
一个可选的语法允许你明确列出列，并且允许你在该语句自由的列出列。
可以重新组合或者选择性的列出列。
```sql
INSERT INTO account(user, passwd)
    VALUES ('somebody', 'youdontseethat');
```

还可以使用COPY命令从文本文件中装载大量数据。这样通常更快。
```sql
COPY account FROM '文件路径';
```

# 查询表

使用SELECT语句。该语句分为选择列表（列出要返回的列）、表列表（列出从中检索数据的表）以及可选的条件（指定任意的限制）。

```sql
SELECT * FROM account;
```
`*`是所有列的缩写。

选择列表中可以写任意表达式，不仅仅是列的列表。
`SELECT city, (temp_hi+temp_lo)/2 AS temp_avg, date FROM weather;`
AS语句是如何给输出的列重新命名的。

WHERE字句修饰查询，需要指定哪些行。包含一个布尔表达式，以此筛选行。
例如：`SELECT * FROM weather WHERE city = 'San Francisco' AND prcp > 0.0;`

也可以要求排序
`SELECT * FROM weather ORDER BY city;`

也可以要求消除重复的行
`SELECT DISTINCT city FROM weather;`

# 表之间链接

拿weather表每行city列和cities表所有行的name列进行比较，并选取在改值匹配的行对。
```sql
SELECT *
    FROM weather, cities
    WHERE city = name;
```

明确的指出要输出的列
```sql
SELECT city, temp_lo, temp_hi, prcp, date, location
    FROM weather, cities
    WHERE city = name;
```

如果表里有重复的列，需要限定一下：
```sql
SELECT weather.city, weather.temp_lo, weather.temp_hi,
       weather.prcp, weather.date, cities.location
    FROM weather, cities
    WHERE cities.name = weather.city;
```

可以定义缩写。。。
```sql
SELECT *
    FROM weather w, cities c
    WHERE w.city = c.name;
```

# 聚集函数

count计数, sum和, avg均值, max最大值, min最小值。

获取最高温度：
```sql
SELECT max(temp_lo) FROM weather;
```

想知道是那个城市
`SELECT city FROM weather WHERE temp_lo = max(temp_lo);     错误方法`
我们可以使用子查询：
```sql
SELECT city FROM weather
    WHERE temp_lo = (SELECT max(temp_lo) FROM weather);
```

聚集同样也常用于和GROUP BY子句组合。比如，我们可以获取每个城市观测到的最低温度的最高值：
```sql
SELECT city, max(temp_lo)
    FROM weather
    GROUP BY city;
```

# 更新
使用UPDATE更新现有行。
```sql
UPDATE weather
    SET temp_hi = temp_hi - 2,  temp_lo = temp_lo - 2
    WHERE date > '1994-11-28';
```

# 删除
使用DELETE语句
```sql
DELETE FROM weather WHERE city = 'Hayward';
```

这种形式的删除一定要小心
```sql
DELETE FROM tablename;
```
如果没有一个限制，DELETE将从指定表中删除所有行，把它清空。做这些之前系统不会请求你确认！ 

# 视图
假设天气记录和城市为止的组合列表对我们的应用有用，但我们又不想每次需要使用它时都敲入整个查询。我们可以在该查询上创建一个视图，这会给该查询一个名字，我们可以像使用一个普通表一样来使用它： 
```sql
CREATE VIEW myview AS
    SELECT city, temp_lo, temp_hi, prcp, date, location
        FROM weather, cities
        WHERE city = name;

SELECT * FROM myview;
```

对视图的使用是成就一个好的SQL数据库设计的关键方面。视图允许用户通过始终如一的接口封装表的结构细节，这样可以避免表结构随着应用的进化而改变。 
视图几乎可以用在任何可以使用表的地方。在其他视图基础上创建视图也并不少见。 

# 外键

回想第2章中的weather和cities表。考虑以下问题：我们希望确保在cities表中有相应项之前任何人都不能在weather表中插入行。这叫做维持数据的引用完整性。在过分简化的数据库系统中，可以通过先检查cities表中是否有匹配的记录存在，然后决定应该接受还是拒绝即将插入weather表的行。这种方法有一些问题且并不方便，于是PostgreSQL可以为我们来解决： 
新的表定义如下： 
```sql
CREATE TABLE cities (
        city     varchar(80) primary key,
        location point
);

CREATE TABLE weather (
        city      varchar(80) references cities(city),
        temp_lo   int,
        temp_hi   int,
        prcp      real,
        date      date
);
```

# 事务

事务是所有数据库系统的基础概念。事务最重要的一点是它将多个步骤捆绑成了一个单一的、要么全完成要么全不完成的操作。步骤之间的中间状态对于其他并发事务是不可见的，并且如果有某些错误发生导致事务不能完成，则其中任何一个步骤都不会对数据库造成影响。 

事务型数据库的另一个重要性质与原子更新的概念紧密相关：当多个事务并发运行时，每一个都不能看到其他事务未完成的修改。例如，如果一个事务正忙着总计所有支行的余额，它不会只包括Alice的支行的扣款而不包括Bob的支行的存款，或者反之。所以事务的全做或全不做并不只体现在它们对数据库的持久影响，也体现在它们发生时的可见性。一个事务所做的更新在它完成之前对于其他事务是不可见的，而之后所有的更新将同时变得可见。 

在PostgreSQL中，开启一个事务需要将SQL命令用BEGIN和COMMIT命令包围起来。因此我们的银行事务看起来会是这样： 

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100.00
    WHERE name = 'Alice';
-- etc etc
COMMIT;
```

如果，在事务执行中我们并不想提交（或许是我们注意到Alice的余额不足），我们可以发出ROLLBACK命令而不是COMMIT命令，这样所有目前的更新将会被取消。 
PostgreSQL实际上将每一个SQL语句都作为一个事务来执行。如果我们没有发出BEGIN命令，则每个独立的语句都会被加上一个隐式的BEGIN以及（如果成功）COMMIT来包围它。一组被BEGIN和COMMIT包围的语句也被称为一个事务块。 

# 窗口函数

一个窗口函数在一系列与当前行有某种关联的表行上执行一种计算。这与一个聚集函数所完成的计算有可比之处。但是与通常的聚集函数不同的是，使用窗口函数并不会导致行被分组成为一个单独的输出行--行保留它们独立的标识。在这些现象背后，窗口函数可以访问的不仅仅是查询结果的当前行。 
下面是一个例子用于展示如何将每一个员工的薪水与他/她所在部门的平均薪水进行比较： 
```sql
SELECT depname, empno, salary, avg(salary) OVER (PARTITION BY depname) FROM empsalary;
```
