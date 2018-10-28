---
title: PostgreSQL 的 SQL 行安全策略
date: 2018-07-16 12:00:35
tags:
- PostgreSQL
- SQL
---

行安全策略

启用和禁用行安全，以及增加表策略，总是表所有者的特权。 
使用`CREATE POLICY`命令创建策略， 使用`ALTER POLICY`命令修改策略， 使用`DROP POLICY`命令删除策略。 为了启用和禁用特定表的行安全性，使用`ALTER TABLE`命令。