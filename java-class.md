---
title: Java 类
date: 2018-08-18 21:56:24
tags:
- Java
---

# 类

```java
public class Classname
{
    int i; //成员变量

    static int si; //类变量（静态变量）

    void method()
    {
        //方法
    }
}
```

构造方法与类名同名。

# 创建对象

1. 声明
2. 实例化
3. 初始化

```java
Classname variable = new Classname(...);
```

# 访问成员

```java
variable.mem;
```

# 源文件声明规则

* 一个源文件中只能由一个public类。
* 一个源文件可以有多个非public类。
* 源文件的名称应该和public类的类名一致。
* 如果一个类定义在某个包中，那么package语句应该在源文件的首行。
* 如果源文件要import，放在package和类定义之间，如果没有package语句，放首行。
* import语句和package语句对源文件中定义的所有类都有效。同一源文件中不能给不同的类不同的包声明。

# 包

包用来对类和接口进行分类。

import java.io.*; //通配符导入包。。。

