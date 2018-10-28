---
title: PostgreSQL 的 SQL 修改表
date: 2018-07-16 11:57:41
tags:
- PostgreSQL
- SQL
---

修改表

对表的结构进行修改。。。

# 增加列

```sql
ALTER TABLE 表名 ADD COLUMN 列 类型;
```

也可以同时为列定义约束
```sql
ALTER TABLE 表名 ADD COLUMN 列 类型 CHECK (表达式);
```

# 移除列

```sql
ALTER TABLE 表名 DROP COLUMN 列;
```

列中的数据将会消失。涉及到该列的表约束也会被移除。然而，如果该列被另一个表的外键所引用，PostgreSQL不会安静地移除该约束。我们可以通过增加CASCADE来授权移除任何依赖于被删除列的所有东西： 
```sql
ALTER TABLE 表名 DROP COLUMN 列 CASCADE;
```

# 增加约束

表约束的语法

```sql
ALTER TABLE 表名 ADD CHECK (表达式);
ALTER TABLE 表名 ADD CONSTRAINT some_name UNIQUE (列);
ALTER TABLE 表名 ADD FOREIGN KEY (product_group_id) REFERENCES product_groups;
```

要增加一个不能写成表约束的非空约束，可使用语法： 
```sql
ALTER TABLE 表 ALTER COLUMN 列 SET NOT NULL;
```
该约束会立即被检查，所以表中的数据必须在约束被增加之前就已经符合约束。 

# 移除约束

为了移除一个约束首先需要知道它的名称。如果在创建时已经给它指定了名称，那么事情就变得很容易。否则约束的名称是由系统生成的，我们必须先找出这个名称。psql的命令\d 表名将会对此有所帮助，其他接口也会提供方法来查看表的细节。因此命令是： 
```sql
ALTER TABLE 表 DROP CONSTRAINT 约束名字;
```

和移除一个列相似，如果需要移除一个被某些别的东西依赖的约束，也需要加上CASCADE。一个例子是一个外键约束依赖于被引用列上的一个唯一或者主键约束。 
这对除了非空约束之外的所有约束类型都一样有效。为了移除一个非空约束可以用：
```sql 
ALTER TABLE 表 ALTER COLUMN 列 DROP NOT NULL;
```
（回忆一下，非空约束是没有名称的，所以不能用第一种方式。） 

# 更改列的默认值

要为一个列设置一个新默认值，使用命令： 
```sql
ALTER TABLE 表 ALTER COLUMN 列 SET DEFAULT 默认值;
```

注意这不会影响任何表中已经存在的行，它只是为未来的INSERT命令改变了默认值。 
要移除任何默认值，使用：
```sql 
ALTER TABLE 表 ALTER COLUMN 列 DROP DEFAULT;
```
这等同于将默认值设置为空值。相应的，试图删除一个未被定义的默认值并不会引发错误，因为默认值已经被隐式地设置为空值。 

# 修改列的数据类型

为了将一个列转换为一种不同的数据类型，使用如下命令：
```sql
ALTER TABLE 表 ALTER COLUMN 列 TYPE 类型;
```
只有当列中的每一个项都能通过一个隐式造型转换为新的类型时该操作才能成功。如果需要一种更复杂的转换，应该加上一个USING子句来指定应该如何把旧值转换为新值。 

# 重命名列
 
```sql
ALTER TABLE 表 RENAME COLUMN 列 TO 新列名字;
```

# 重命名表

```sql
ALTER TABLE 表 RENAME TO 新表名;
```