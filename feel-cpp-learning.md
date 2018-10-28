---
title: 学习C++的心路历程
date: 2018-08-23 01:10:33
tags:
- feelings
---

# 前言

只学一种语言，那是不可能的咯。尽管JavaScript进军后端，各种语言也有包揽整个开发解决方案的趋势。可是我们得开拓眼界啊！光是往眼睛上塞阀门是不够的，大多语言有自己的理念，自己的语法糖，自己的体系。选择最适合的，往往比牵强来的更有效。
语言有它的特点，也有它们的共性。
这里，试图抽象出学习一种编程语言需要的历程。目前只能那我最熟悉的C++当作例子。
刨析一下书是怎么让我一步步了解这个语言的。
编程语言的各个元素耦合的太强了（大概吧），如何解开一部分耦合，使得理解过程变得简单？
怎样一步步的介绍一堆耦合在一起的概念？

怎么去了解其他语言？从哪里下手呢？
就让我观察下作者是怎么设计学习过程的吧。

# 整体结构

1. 给出最简单的例子，通用的`hello, world!`。（将最小化的入口点代码框架给读者当作形式，给读者亲自实践的能力，并且告诉读者给出的代码框架将在以后解释。介绍简单的调试，输出方法，这样读者可以“看到”程序运行结果，并获得简单的调试技巧。）
2. 讲解注释。（注释的语法是最简单的，注释的作用也是比较重要的，这样使得读者可以在代码内加上自己的话，做自己的标记，同时获得一项调试代码的技巧（把代码注释掉））
3. 基本类型。（类型决定了变量中存储着什么样的数据。类型也将影响一个变量的行为。类型也属于变量定义的一部分。类型是变量的内部结构，结构会决定性质！）
4. 字面值常量。（语法中凭空产生语法对象的一种基本的语法。赋值语句中值的来源，所谓汇编的“潜入指令中的数据”，几乎所有变量的值来自于字面值常量，可见这个语法的重要程度。）
5. 变量。（有了基本类型和字面值常量的铺垫。读者可以安全的定义变量并且将其初始化。变量就是程序中最基本的一个组成元素。变量代表这一段内存，使得程序拥有自己的状态，可以演化，使得编程变得简单。隐式类型转换也是个巨坑，理解其规则是个问题。）（嘴贱：Haskell那种的信奉的是引用透明性(referential transparency)，函数好像没有内部状态，对于一个函数，传入同样的参数，函数的行为完全相同，没有副作用。）
6. 复合类型。（以类型为基础构造出的类型。比如你有一堆整数，为了统一管理，定义了整数的数组，该数组就是以整数为基础构造的类型。抽象一下，称为一个容器，保存，并管理着一些变量。类似的，引用和指针也是以某个类型为基础而构造出的类型。（尽管引用是语法糖，它的类型是用来帮助静态类型检查的，而指针是通过类型获取类型尺寸参数的）。复合类型也可以作为另一个复合类型的基础，从而构造出其他复合类型。）
7. 常量。（语法上生成一种不可改变的量。其一是真正的需要，防止无意中的改变，或者是概念上的常量，不允许发生变化。其二是为了进行优化，常量能进行更多的优化，而不改变程序的语义。然而C++扯那么多是复合类型挖的坑，与复合类型产生的概念问题（指针本身是常量和指针（或引用）指向的类型是常量是两个概念，说人话就是顶层const和底层const）还波及函数传参，类成员，this指针，函数匹配等）
8. 类型处理。（给与程序员定义自己的类型的能力。以及自动类型推导这个工具，简化对类型的处理。而且自定义类型参与类型检查，有着积极作用。）
9. 内置数组。（。。。）
10. C风格字符串。（历史遗留。。。）
11. 表达式。（基于上面的了解，全面介绍表达式。）
12. 语句。（基于上面的了解，全面介绍语句。）
13. 函数。（基于上面的了解，全面介绍函数。）
14. 类。（让程序员可以自定义类型。类类型是类型系统的重要部分，同时类提供了面向对象思想的支持）
15. lambda表达式。（函数式支持。传入函数，定制函数的行为。）
16. 动态内存。（内存管理）
17. 拷贝控制。（详细介绍对象的初始化，拷贝，移动，赋值，销毁。控制对象生命周期，类成员的管理。）
18. 操作符重载和类型转换。
19. 面向对象的程序设计。

# 学习过程

## hello, world! | 最简可运行代码框架 | 初步介绍

hello, world!
```C++
# include <iostream>
int main()
{
    std::cout<<"hello, world!"<<std::endl;
    return 0;
}
```

hello, world! 和一些解释
```C++
// 我是注释，这些代码你就当作是个形式，当作一个框架好了，后面我们会做详细说明
# include <iostream> // 使得你可以使用输入输出功能，现在你只要写上就行
int main() // 程序入口点，这不重要
{
    //把代码放在这里就好
    std::cout<<"hello, world!"<<std::endl; //这条代码输出hello, world!至于其他的东西，现在不重要。
    //类似的，将你要输出的话放在双引号之内就行。但是有些特殊字符不可以哟，其中有着深刻的原因
    std::cout<<"将你的话放在这里哟！"<<std::endl;

    return 0; //这个语句让程序结束，目前你只需要这道这些就行。
} //这些符号都不重要，你只需要将你的代码放到指定位置就行，后面会告诉你为什么。
```

输出方法，看到你的程序的结果。
（看到结果，才能判断程序是否按照预期执行，更多的信息还能判断程序的执行过程是否符合设计和预期。同时让读者感受到自己程序的运行。）

```C++
std::cout << /*变量的名字*/ << std::endl;
// 这样就能将变量的内容“打印”到屏幕上，然后观察变量的内容。
// 还会换行，这样防止下次“打印”的结果和上次结果揉在一起难以区分。
```

给程序加注释
```C++
// 该符号之后，直到这一行的结束，都算作注释，编译器会忽略这些内容。
// 你可以在代码旁边解释你的意图，你的思路！

/* 用这两边的两个符号划分出注释区域。可以跨越多行，也可以截取一行内的一小段部分。 */
```

简单的介绍下函数。

```C++
return_type function_name(args...)
{
    function_body
}
```

程序的编译，运行。

小结：
1. hello, world!
2. 最简可运行代码框架
3. 输出（可能会有输入）
4. 简单介绍编译运行相关知识。
5. 注释
6. 简单介绍变量

## 类型 | 字面值 | 变量

类型决定变量如何储存值，参与哪些运算，保证变量的正确使用（静态类型检查）。

```C++
int i = 42; //整数类型
float f = 3.14f; //浮点数类型
double d = 3.14; //浮点数类型
bool b = true; //布尔类型
```

变量能赋值

```C++
i = 0; //i赋值为0
```

变量能读取

```C++
std::cout<<i<<std::endl;//直接输出i的内容，并换行
```

参与运算

```C++
std::cout<< i + i <<std::endl;
```

初始化

```C++
int i = 42; //定义时给定初始值不是赋值，是初始化！
```

列表初始化

```C++
int i = {0};
int i{0};
```

# 控制流

控制流语句也是很重要的哟。

。。。


# 告一段落

到了这里，能编译，能运行，能检查变量的值，看书自学应该是没什么问题了。
看你咯。