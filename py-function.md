---
title: Python 函数
date: 2018-08-18 16:13:03
tags:
- Python
---

# 调用函数

function_name(args)，这种形式即可。

#函数定义

使用`def`关键字，格式如下：

def function_name(args...):
    stmt

函数体用缩进表示。

例如：
```python
def abs(x):
    if x >= 0:
        return x
    else:
        return -x
```

## 定义空函数

用pass语句，什么也不做，pass只用来做占位符。

```python
def nop():
    pass
```

空着的话，会有语法错误。

# 参数检查

调用函数时，参数个数要匹配。

# 返回多个值

返回tuple即可。

# 参数

## 位置参数

```python
def func(a, b, c):
    stmt

func(a, b, c)
```
调用时参数按照位置顺序传递。

## 默认参数

指定某些参数的默认值，可以不提供该参数。

```python
def func(a, d = value):
    stmt

func(a) # d采用默认参数
```

1. 必选参数在前，默认参数在后，不然报错（二义性）
2. 越稳定的参数放到后面。从左到右，特殊到一般。

## 可变参数

参数个数不确定

```python
def func(*l):
    stmt

func(a,b,c,d,e,...) # 参数可变
```

参数前面加`*`号，可变参数接收为tuple。

已经有list或tuple，直接当作参数。
调用时在list或tuple前加`*`即可。
```python
l = [1,2,3]
func(*l)
```

## 关键字参数

```python
def func(a, b, **kw)
    stmt

func(a, b)
func(a, b, key=value)
```
关键字参数，调用时自动组装为dict。

现成dict
```python
dict = {key:value}
func(**dict)
```

## 命名关键字参数

限制关键字的名字。

```python
def func(a, b, *, key1, key2):
    stmt

func(a, b, key1=value1, key2=value2)
```

命名关键字参数需要一个特殊分隔符`*`，后面的参数被视为命名关键字参数。
只接受这些关键字。。。

如果函数定义中已经有一个可变参数，后面的命名关键字参数可以省略特殊分隔符
```python
def func(a, b, *args, key1, key2):
    stmt
```

命名关键字参数必须传入参数名字，如果没有，就报错。

命名关键字参数可以有默认值。

## 参数组合

参数定义的顺序：
必选参数，默认参数，可变参数，命名关键字参数，关键字参数

```python
def func(a, b, c=0, *args, **kw):
    stmt
```

对于任意函数，都可以通过类似`func(*args, **kw)`的形式调用它，无论它的参数是如何定义的。

