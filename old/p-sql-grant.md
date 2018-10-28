---
title: PostgreSQL 的 SQL 权限
date: 2018-07-16 11:59:26
tags:
- PostgreSQL
- SQL
---

权限

一旦一个对象被创建，它会被分配一个所有者。所有者通常是执行创建语句的角色。对于大部分类型的对象，初始状态下只有所有者（或者超级用户）能够对该对象做任何事情。为了允许其他角色使用它，必须分配权限。 

有多种不同的权限：SELECT、INSERT、UPDATE、DELETE、TRUNCATE、REFERENCES、TRIGGER、CREATE、CONNECT、TEMPORARY、EXECUTE以及USAGE。可以应用于一个特定对象的权限随着对象的类型（表、函数等）而不同。PostgreSQL所支持的不同类型的完整权限信息请参考GRANT。下面的章节将简单介绍如何使用这些权限。 
修改或销毁一个对象的权力通常是只有所有者才有的权限。 

要分配权限，可以使用GRANT命令。
ALL权限表示所有权限
```sql
GRANT 权限 ON 表 TO 角色;
```

为了撤销一个权限，使用REVOKE命令： 
```sql
REVOKE ALL ON 表 FROM 角色;
```

对象拥有者的特殊权限（即执行DROP、GRANT、REVOKE等的权力）总是隐式地属于拥有者，并且不能被授予或撤销。但是对象拥有者可以选择撤销他自己的普通权限，例如把一个表变得对他自己和其他人只读。 