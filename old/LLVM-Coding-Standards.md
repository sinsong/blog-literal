---
title: LLVM Coding Standards
date: 2018-06-14 22:11:57
tags:
- LLVM
---

翻译自：http://releases.llvm.org/5.0.0/docs/CodingStandards.html#llvm-coding-standards

# 介绍

这个文档尝试描述一些LLVM源码树使用的代码标准。
TODO!!!

# 语言，库，和标准

LLVM和其他LLVM项目大部分源代码使用C++代码的代码标准。
TODO!!!

## C++ 标准版本



## C++ 标准库



## 支持的 C++ 语言和标准库特性
## 其他语言
# Mechanical Source Issues
## 源代码格式化
### 使用注释

注释是可读性和可维护性的一个关键的部分。每个人都知道给他们的代码加注释，你也应该。当你编写注释时，写得像英语散文，表示他们应该使用适当的资本，标点，等。旨在描述代码在尝试做什么和为什么，而不是它如何工作在微观层面上。这是一些文档中关键的东西：
#### 文件头

每个源代码文件都应该有一个头用来描述文件的基本目的。如果一个文件没有头，它不应该被签入源码树。标准的头看起来像这样：

```cpp
//===-- llvm/Instruction.h - Instruction class definition -------*- C++ -*-===//
//
//                     The LLVM Compiler Infrastructure
//
// This file is distributed under the University of Illinois Open Source
// License. See LICENSE.TXT for details.
// 这个文件在伊利诺伊大学开放源代码协议下发布。
//
//===----------------------------------------------------------------------===//
///
/// \file
/// This file contains the declaration of the Instruction class, which is the
/// base class for all of the VM instructions.
/// 这个文件包含 Instruction 类的声明，是所有 VM 指令的基类。
///
//===----------------------------------------------------------------------===//
```

一些需要关注这个特别的格式：`-*- C++ -*-`字符串在首行，告诉 Emacs 这个源文件是 C++ 源文件，不是 C 源文件（Emacs 默认假定 .h 文件是 C 源文件）。

> 在 .cpp 文件中，表示不是必要的。文件名也在首行，连同一个非常短的文件目的的描述。在打印代码和翻转很多页时这样很重要。

TODO!!!

#### 类概览
#### 方法信息
### 注释格式化
### 在文档注释中使用 Doxygen
### #include 风格

在文件头注释后，立即跟上文件要求的可以列出的最小化 `#include` 列表。我们宁可让 `#inlucde`以下面的顺序列出：

1. 主要模块头文件
2. 本地/私有 有文件
3. LLVM项目/子项目头文件(clang/...,lldb/...,llvm/...,等)
4. 系统 `#include`s

TODO!!!
### 源代码宽度

写代码时，限制在80列文本以内（每行80个字符以内）。这帮主我们那些喜欢打印出来代码的人和在 xterm 中看你的代码的人不用重新调整它的大小。
### 使用空格替代制表符
### 代码缩进一致
#### 格式化 lambda 表达式到像代码块
#### Braced Initializer Lists
## 语言和编译器问题
### 编译器警告视为错误
### 编写可移植的代码
### 不要使用 RTTI（运行时类型信息） 或者 异常

努力减小代码和可执行文件的大小，LLVM不使用 RTTI（例如`dynamic_cast<>;`）或者异常。这两个语言特性违反了一般的C++原则"你只用花销在在你使用的东西上"，导致可执行文件膨胀，哪怕异常在源码基础上从来没有被使用，或者如果RTTI从来没有在一个类上使用。因为这个，我们在代码中全局的关闭了他们。

即便如此，LLVM确实制作了可扩展的RTTI手卷？？？使用模板像`isa<>, cast<>, dyn_cast<>`。这个形式的RTTI是opt-in和可以被加入到任何类。还有大体上比`dynamic_cast<>`更有效率。
### 不要使用静态构造函数
### class 和 struct 关键字的使用

C++中，class和struct关键字几乎可以交替的使用。
TODO!!!
### Do not use Braced Initializer Lists to Call a Constructor
### 使用 auto 类型推导使代码更可读
### 小心不必要的拷贝
# 风格问题
## 高层问题
### 公共的头文件是一个模块
### #include 尽可能少
### 保持“内部”头文件私有
### 使用更早的退出和 continue 关键字来简化代码
### 不要在 return 后使用 else
### 将断定的循环变为断定的函数
## 底层问题
### 适合的名字类型，函数，变量，和枚举
### 大方的断言
### 不要使用 `using namespace std`
### 为头文件中的类提供一个虚方法锚
### 用枚举完全覆盖的 switch 语句中不要使用 default 标签
### 在一个循环中不要每次都对 end() 求值
### 禁止使用 `#include <iostream>`

使用 `#include <iostream>`在库文件中是特别的禁止的，因为许多公共实现
TODO!!!
### 使用 `raw_ostream`
### 避免 std::endl

`std::endl`操纵符，和 iostream 一起使用，输出一个新行到指定的输出流。除了这样做，然而，它还刷新输出流。换句话说，一下是等价的：

```cpp
std::cout << std::endl;
std::cout << '\n' << std::flush;
```
大多情况下，你可能没理由去刷新输出流，所以使用一个字面值`\n`更好。

### 在一个类的定义中定义一个函数时不要使用 inline

定义在类定义中的一个成员函数是隐式内敛的，所以不要使用inline关键字在这个情况下。

不要这样：
```cpp
class Foo {
public:
  inline void bar() {
    // ...
  }
};
```
这样做：
```cpp
class Foo {
public:
  void bar() {
    // ...
  }
};
```
## 微小的细节
### 括号前的空格
### 尽量用前置递增
### 命名空间缩进
#### 未命名的命名空间
# 亲参阅
