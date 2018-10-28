---
title: C++ 类型处理
date: 2018-07-17 23:35:52
tags:
- C++
---

# 类型别名

定义一个别名，作为某个类型。

```C++
typedef 声明符列表

typedef long double real, *real_ptr;
//real就是long double ，real_ptr 就是 long double *
```

typedef做的是类型别名定义而不是定义变量！声明符可以包含类型修饰，从而构造出复合类型出来。

新标准给出了新的方法，别名声明(alias declaration)来定义类型别名

```C++
using 别名 = 类型;
using real = long double;
```

类型别名也能用类型修饰符修饰，派生出相应的引用或指针。
而且不能简单的将类型别名替换成他的定义，这么理解是错误的！而应该以定义为基础派生出其他类型。

# auto类型说明符

让编译器推导它的类型，使用auto类型说明符。
某些情况下，不清楚表达式的类型，或者根本做不到。

auto让编译器分析初始值表达式的类型，推导出变量的类型，所以，auto定义的变量必须有初始值！

```C++
auto i = 3.14;
//多半是double
```

auto能在一条语句中声明多个变量。
因为一条声明语句只能有一个基本数据类型，所以该语句中所有变量的初始基本数据类型都必须一样！

```C++
auto i = 0, *p = &i; //正确，auto -> int
auto sz = 0, pi = 3.14; //错误，sz和pi的基本类型不一致。
```

## 复合类型，常量和auto

编译器推导出的auto类型有时候和初始值的类型并不完全一样，编译器会适当的改变结果类型使其更符合初始化规则。

引用作为初始值时，真正参与初始化的其实是引用对象的值。编译器以被引用对象的类型作为auto的类型。
```C++
int i  = 0, &r = i;
auto a = r; //auto -> int (r -> i, i -> int)
```

auto一般会忽略掉顶层const，同时底层const会保留下来
```C++
const int ci = i, &cr = ci;
auto b = ci; //auto -> int (忽略掉ci的顶层const)
auto c = cr; //auto -> int (cr是ci的别名，ci是顶层const)
auto d = &i; //auto -> int * (整数的地址就是指向整数的指针)
auto e = &ci; //auto -> const int * (对常量对象取地址是一种底层const)

如果希望推导出顶层const，需要明确指出：
```C++
const auto f = ci; //auto -> int, f -> const int
```

auto 配合符合类型修饰符，任然适合原来的初始化规则。
auto的引用，初始值中的顶层常量属性仍然保留。？？？
牢记&和*术语声明符！

# decltype类型指示符

有时候，希望从表达式的类型推断出要定义的变量的类型，但是不想用该表达式值初始化变量。
有了decltype，返回表达式的数据类型。编译器分析表达式并得到它的类型，却不实际计算表达式的值：

```C++
decltype(f()) sum = x; //sum 的类型就是函数f的返回类型
```

如果使用decltype的表达式是一个变量，则decltype返回该变量的类型，包括顶层const和引用！

```C++
const int ci = 0, &cj = ci;
decltype(ci) x = 0; //x -> const int
decltype(cj) y = x; //y -> const int &, y -> x
decltype(cj) z; //错误！z是一个引用，必须初始化
```

因为cj是引用，所以decltype(cj)的结果就是引用类型。
引用从来都是作为其所指对象的同义词出现，只有在decltype处是个例外。

## decltype和引用

如果decltype使用的表达式不是一个变量，则decltype返回表达式结果对应的类型。
decltype可能返回引用，意味着该表达式的结果对象能作为左值。

```C++
int i = 42, *p = &i, &r = i;

decltype(r + 0) b; //b -> int
decltype(*p) c; //错误：c -> int& ,必须初始化。
```

因为r是引用，decltype(r) -> int& ，然而r+0是一个具体值，而不是引用。
解引用后返回的是引用！decltype(*p) -> int&。

decltype和auto的重要区别，decltype的结果类型与表达式形式密切相关。
decltype的表达式来说，变量名套上一层括号，会把它当做表达式，变量是一种可以作为左值的特殊表达式，所以会得到引用。

```C++
int i = 42;

decltype(i) a; // a -> int
decltype((i)) b; //错误：b -> int&
```

> 切记：decltype((variable))的结果永远是引用！而decltype(variable)结果只有当variable本身就是一个引用才是引用。

