---
title: C++ I/O
date: 2018-06-09 00:41:13
tags:
- C++
---

C++ 语言没有定义输入输出(I/O)语句，包含一个全面的标准库(standard library)来提供IO机制（当然标准库里头东西多了去了）。
这里我们简单了解一下，打下一个基础。以后想随便输出点什么，想看看变量的值，就要用到。

Hello, world!示例使用的是iostream库。字面意思是：输入输出流。
iostream包含两个基础类型：istream和ostream，分别表示输入和输出流。
一个流就是字符序列，是从IO设备读出或写入IO设备。
流(stream)向表达的是，随着时间的推移，字符时顺序生成和消耗的。

标准库定义了4个IO对象。

* cin		标准输入	处理输入
* cout		标准输出	进行输出
* cerr		标准错误	特殊输出（警告和错误等）
* clog		标准日志	一般性信息

系统会将程序和界面联系起来。
比如你在一个窗口中运行程序，这个程序的cout,cerr,clog将输出到这个窗口中，而且你在这个窗口中的输入或被这个程序的cin处理。

双倍(double)
让用户输入一个整数，然后输出这个数字的二倍。

```cpp
#include <iostream>

int main()
{
	std::cout<<"Please enter integer number:"<<std::endl;
	int i = 0;
	std::cin >> i;
	std::cout<< (i * 2) << std::endl;
	return 0;
}
```

效果应该是这样

```
Please enter integer number:
2
4
```


`#include <iostream>`表示我们要使用iostream库。尖括号中是头文件(header)的名字。使用标准库设施一般是包含相应的头文件。
`#include`指令和头文件的名字必须在同一行！通常，#include执行必须出现在所有函数之外。我们一般将#include指令放在源文件的开始位置。

`std`是命名空间。
`::`是作用域运算符，表示我们要访问std中的名字。
`cout`就是刚才说的标准输出。
`<<`是输出运算符，左边是ostream对象，右边就是要输出的值。
`>>`是输入运算符，左边是istream对象，右边是保存输入的对象。

`<<`是可以连接的，`<<`会返回左侧的对象，而且满足左结合律。所以可以连接起来简写。

```cpp
std::cout << "Please enter integer number:" << std::endl;
( std::cout << "Please enter integer number:" ) << std::endl;
//这两语句是等价的
```

`"Please enter integer number:"` 是字符串字面值常量(string literal)，用一对双引号包围的字符序列。字面值是指这个对象的值就是字面所表示的。
字面值，类比立即数，内嵌到指令中的数据。语法上用来凭空产生一个对象。（声明一个变量，用凭空产生的字面值赋值，编译时把字面值对象优化掉，完美。）

`endl`是操纵符(manipulator)。写入`endl`的效果是换行，并刷新缓冲区。将与设备关联缓冲区(buffer)中的内容刷到设备中。缓冲区刷新操作可以保证到目前为止程序所产生的所有输出都真正写入输出流中，而不是仅仅停留在内存中等待写入流。（缓冲区的存在是用来减少IO操作，提高运行效率。

> 程序员在调试时尝尝使用输出（打印点什么东西）。那么应该保证“一直”刷新流。否则，如果程序崩溃，输出可能还留在缓冲区中，从而影响判断。


