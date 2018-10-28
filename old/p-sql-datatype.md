---
title: PostgreSQL 的 SQL 数据类型
date: 2018-07-16 15:28:14
tags:
- PostgreSQL
- SQL
---

常用数据类型

|名字|别名|描述|
|:---|:---|:---|
|boolean        |bool       |布尔值|
|smallint       |int2       |有符号2字节整数|
|integer        |int, int4  |有符号4字节整数|
|bigint         |int8       |有符号8字节整数|
|smallserial    |serial2    |自动增长的2字节整数|
|serial         |serial4    |自动增长的4字节整数|
|bigserial      |serial8    |自动增长的8字节整数|
|real               |float4         |单精度浮点数（4字节）|
|double precision   |float8         |双精度浮点数（8字节）|
|numeric[(p,s)]     |decimal[(p,s)] |可选精度的精确数字|
|bit[(n)]         |   |定长位串|
|bit varying[(n)] |   |变长位串|
|bytea            |   |二进制数据（字节数组）|
|character[(n)]         |char[(n)]      |定长字符串|
|character varying[(n)] |varchar[(n)]   |变长字符串|
|text                   |               |变长字符串|
|date       |   |日历日期|
|money      |   |货币数量|
|inet       |   |IPv4或IPv6主机地址|
|macaddr    |   |MAC地址|
|cidr       |   |IPv4或IPv6网络地址|
|uuid       |   |通用唯一识别码|
|json       |   |文本JSON数据|
|jsonb      |   |二进制JSON数据|
|xml        |   |XML数据|
|pg_lsn||PostgreSQL日志序列号|
|txid_snapshot||用户级别事物ID快照|
|tsquery||文本搜索查询|
|tsvector||文本搜索文档|
|time[(p)]||一天中的时间|
|time [ (p) ] [ without time zone ]||一天中的时间（无时区）|
|time [ (p) ] with time zone|timetz|一天中的时间，包括时区|
|timestamp [ (p) ] [ without time zone ]||日期和时间（无时区）|
|timestamp [ (p) ] with time zone|timestamptz|日期和时间，包括时区|
|interval\[fields\][(p)]||时间段|
|box||平面方框|
|line||平面直线|
|circle||平面圆|
|lseg||平面线段|
|path||平面几何路径|
|point||平面集合点|
|polygon||平面封闭几何路径|