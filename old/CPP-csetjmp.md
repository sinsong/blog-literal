---
title: csetjmp setjmp.h
date: 2018-07-04 23:45:44
tags:
- C++
---

非本地跳转

longjmp
`void longjmp (jmp_buf env, int val);`
跳转

setjmp
`int setjmp (jmp_buf env);`
为跳转保存调用环境

类型
`jmp_buf`	保存调用环境
