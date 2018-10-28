---
title: PostgreSQL 的 SQL 约束
date: 2018-07-16 11:56:30
tags:
- PostgreSQL
- SQL
---

约束

# 检查约束

SQL允许我们在列和表上定义约束。约束让我们能够根据我们的愿望来控制表中的数据。如果一个用户试图在一个列中保存违反一个约束的数据，一个错误会被抛出。即便是这个值来自于默认值定义，这个规则也同样适用。 

创建表时指定约束

检查约束
它允许我们指定一个特定列中的值必须要满足一个布尔表达式。
```sql
CREATE TABLE 表名(
    列  类型    CHECK (表达式)
);
```

我们也可以给与约束一个独立的名称。
要指定一个命名的约束，请在约束名称标识符前使用关键词CONSTRAINT，然后把约束定义放在标识符之后（如果没有以这种方式指定一个约束名称，系统将会为我们选择一个）。 
```sql
CREATE TABLE 表名(
    列  类型    CONSTRAINT 约束名 CHECK (表达式)
);
```

以上是列约束，而且列约束只能引用它所依附的那一个列。

一个检查约束也可以引用多个列。
```sql
CREATE TABLE 表名(
    列定义,
    CHECK (表达式) -- 表约束
);
```
表约束也能起名成。

# 非空约束

非空约束仅仅指定一个列中不会有空值。
```sql
CREATE TABLE 表名(
    列  类型 NOT NULL
);
```

一个列可以有多个约束，只需一个接一个的写出
```sql
CREATE TABLE 表名(
    列  类型 NOT NULL CHECK (表达式)
);
```
写约束顺序没要求。。。

NULL约束。。。奇怪的要求。必须为空，一般不会使用。
`列 类型    NULL`

# 唯一约束

唯一约束保证表中所有行在一列中或者一组列中保存的数据是唯一的。

```sql
CREATE TABLE 表名(
    列  类型 UNIQUE
);
```

写成表约束就是

```sql
CREATE TABLE products (
    列定义,
    UNIQUE (列1,列2...)
);
```

我们可以通常的方式为一个唯一索引命名：
```sql 
CREATE TABLE products (
    product_no integer CONSTRAINT must_be_different UNIQUE,
    name text,
    price numeric
);
```

# 主键

主键约束表示列，或者列组，可以作为表中行唯一标识符使用。需要该值是 唯一的而且非空。

```sql
CREATE TABLE 表名(
    列  类型    PRIMARY KEY
);
```

主键也可以包含多于一个列，其语法和唯一约束相似： 

```sql
CREATE TABLE products (
    列定义,
    PRIMARY KEY (列1,列2...)
);
```

添加主键将在主键列出的列上或者列组中自动创建唯一B树索引， 并且强制列被标记为非空。
一个表最多只能有一个主键（可以有任意数量的唯一和非空约束，它们可以达到和主键一样的功能，但只能有一个被标识为主键）。
关系数据库理论要求每一个表都要有一个主键。但PostgreSQL中并未强制要求这一点，但是最好能够遵循它。
主键对于建立文档的目的和客户端应用都有用。

# 外键

一个外键约束指定一列（或一组列）中的值必须匹配出现在另一个表中某些行的值。我们说这维持了两个关联表之间的引用完整性。

```sql
CREATE TABLE 表名(
    列  类型    REFERENCES 表(列)
);
```

简写
缺少列的列表，则被引用表的逐渐将被用作被引用列。

```sql
CREATE TABLE 表名(
    列  类型    REFERENCES 表
);
```

`列  类型    REFERENCES 表 ON DELETE RESTRICT`
RESTRICT阻止删除一个被引用的行。
`列  类型    REFERENCES 表 ON DELETE CASCADE`
CASCADE指定当一个被引用行被删除后，引用它的行也应该被自动删除。还有其他两种选项：SET NULL和SET DEFAULT。这些将导致在被引用行被删除后，引用行中的引用列被置为空值或它们的默认值。

与ON DELETE相似，同样有ON UPDATE可以用在一个被引用列被修改（更新）的情况，可选的动作相同。在这种情况下，CASCADE意味着被引用列的更新值应该被复制到引用行中。 

# 排他约束

排他约束保证如果将任何两行的指定列或表达式使用指定操作符进行比较，至少其中一个操作符比较将会返回否或空值。