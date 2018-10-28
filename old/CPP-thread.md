---
title: thread
date: 2018-07-05 01:04:13
tags:
- C++
---

thread
`class thread`
表示线程

成员类型
`id`			线程号
`native_handle_type`	原生句柄类型

成员函数
`(构造函数)`	构造线程
`(析构函数)`	析构线程
`operator=`	移动赋值线程
`get_id`	获取线程号
`joinable`	检查joinable
`join`		join线程
`detach`	detach线程
`swap`		交换线程
`native_handle`	获取原生句柄
`hardware_concurrency [static]`
