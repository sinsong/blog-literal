---
title: cfenv fenv.h
date: 2018-07-04 22:35:42
tags:
- C++
---

浮点环境

访问浮点环境
`#pragma STDC FENV_ACCESS on`	开启
`#pragma STDC FENV_ACCESS off`	关闭

浮点异常

fegetexceptflag
`int fegetexceptflag (fexcept_t* flagp, int excepts);`
获取浮点异常标志

fesetexceptflag
`int fesetexceptflag (const fexcept_t* flagp, int excepts);`
设置浮点异常标志

feclearexcept
`int feclearexcept (int excepts);`
清除浮点异常

feraiseexcept
`int feraiseexcept (int excepts);`
生成浮点异常

浮点异常
`FE_DIVBYZERO`	极值异常
`FE_INEXACT`	不精确的结果异常
`FE_INVALID`	无效参数异常
`FE_OVERFLOW`	上溢范围错误异常
`FE_UNDERFLOW`	下溢范围错误异常
`FE_ALL_EXCEPT`	所有异常

舍入方向

fegetround
`int fegetround (void);`
获取舍入方向模式

fesetround
`int fesetround (int rdir);`
设置舍入方向模式

舍入方向
`FE_DOWNWARD`	向下舍入
`FE_TONEAREST`	向最近舍入
`FE_TOWARDZERO`	向零舍入
`FE_UPWARD`	向上舍入

整个环境

`FE_DFL_ENV`	默认环境

fegetenv
`int fegetenv (fenv_t* envp);`
获取浮点环境

fesetenv
`int fesetenv (const fenv_t* envp);`
设置浮点环境

feholdexcept
`int feholdexcept (fenv_t* envp);`
保持浮点异常

feupdateenv
`int feupdateenv (const fenv_t* envp);`
更新浮点环境

fetestexcept
`int fetestexcept (int excepts);`
测试浮点异常

类型

`fenv_t`	浮点环境类型
`fexcept_t`	浮点异常类型
