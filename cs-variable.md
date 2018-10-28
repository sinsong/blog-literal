---
title: C# 变量和数据类型
date: 2018-10-08 19:42:17
tags:
- C#
---

# 类型系统

## 值类型

### 简单类型

有符号整型：
sbyte
short
int
long

无符号整型：
byte
ushort
uing
ulong

### 枚举

enum E
{
    ...
}

### 自定义结构类型

struct S
{
    ...
}

### 可空类型

具有null值的扩展

int? i = null; //int?可以为空的int

## 引用类型

### 类

所有其他类型的最终基类: object
Unicode 字符串: string

用户自定义的类型
class C
{
    ...
}

### 接口

interface I
{
    ...
}

### 数组

一维或多维数组
int[]
int[,]

### 委托

delegate int D(...)

# 值类型和引用类型的区别

值类型保存的是变量的值
引用类型保存的是对象的引用地址

# 常量与变量

常量用 const 关键字

var 关键字自动推导类型

# 表达式

??
如果类不为空值时，返回自身，为控制返回之后的操作。
int j = i ?? 0; //i不为null，j为i，否则j为0

typeof
获取类型的System.Type对象
System.Type type = typeof(int);

is
检查对象是否与给定类型兼容
x is T 如果x为T类型，true；否则false

as
x as T 返回类型为T的x，如果x不是T，则返回null。

# 简单类型

## 整型

有符号
Sbyte 8
Short 16
Int 32
Long 64

无符号
Byte 8
ushort 16
Uint 32
Ulong 64

## 浮点型

float 4
double 8
decimal 16

# 字符串

string s = @"string";
原生字符串

string 是 Unicode，中英文都占两个字节。

## 字符串比较

string.Compare(string s1, string s2)
字符串比较
s1 > s2  -> 1
s1 == s2 -> 0
s1 < s2  -> -1

s1 == s2
string.Equals(string s)
相等判断

## 字符串查找

string.Contaions(string s)
是否包含子串


string.StartsWith()
string.EndsWith()
从字符串的首或尾开始查找指定的子字符串

string.IndexOf()
求子串再字符串中出现的位置

## 子串

string.Substring

## 插入删除替换

Insert
Remove
Replace

## 移除首位字符

TrimStart   首
TrimEnd     尾
Trim        首尾

## 与数组

string.Split(char[] separator)
按分隔符将字符串划分为数组。
string -> string[]

## StringBuilder

大量连接字符串，使用StringBuilder

System.Text.StringBuilder

# 数组

普通数组
T[] id;

多维数组
T[,] id;

交错数组
T[][] id;


int[] a = new int[10];

int[,] b = new int[3,5] {{}, {}, {}};

int[][] n1 = new int[2][]
{
    new int[] {,,,},
    new int[] {,,,}
}

## 复制、排序、查找

Copy Sort Reverse

# 显示转换

int i = (int)r;

# 值类型和引用类型互相转换

Object类，所有类型的基类。

## 装箱

值类型隐式转换为Object类型。
装箱一个数值，会为其分配一个对象实例，并把该数值复制到新对象中。

```cs
int i = 123;
object o = i; //装箱
```

## 拆箱

拆箱操作是指显式的把Object类型转换为值类型。
步骤：
1.检查对象实例，确认它是否包装了值类型的数。
2.把实例中的值复制到值类型的变量中。

