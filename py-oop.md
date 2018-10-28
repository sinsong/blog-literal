---
title: Python 面向对象
date: 2018-08-18 18:57:24
tags:
- Python
---

# 定义类

使用class关键字定义类。

```python
class Classname(object):
    
    def  __init__(self, ...)
        self.mem = ...
        ...
```

括号里是要继承的类，如果没有，就写object，这是所有类的基类。

然后用类名()就能创建实例。

```python
obj = Classname(...)
```

__init__方法是特殊的，用来做构造函数。
将我们认为必要的属性强制绑定上去。

成员函数包括__init__的第一个参数必须是self。表示实例本身。
调用成员函数时，除了self不用传递，其他参数正常传入。

# 访问限制

属性的名字前加上两个下划线`__`，就变成了一个私有变量。
只有内部可以访问，外部不能访问。

机制是：将__name改成了_Classname__name。
并没有任何机制阻止你访问。。。
一切全靠自觉。。。

# 继承和多态

isinstance(变量,类)
判断变量是否是某个类型的实例。
基类也会判断为是。

成员调用，只要有那个成员就行。。。
鸭子类型。

可以重写基类的方法。

# 工具

type()返回对象的类型
dir()返回所有成员的list

# 类属性

类本身需要一个绑定

```python
class Classname(object):
    name = value # 这就是一个类属性
```

同名实例属性会屏蔽掉类属性。

