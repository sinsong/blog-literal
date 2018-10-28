---
title: CMake-Language
date: 2018-05-28 20:42:23
tags:
- CMake
---

# 组织

CMake输入文件使用 "CMake Language"编写在源文件中，命名为`CMakeLists.txt`或者以`.cmake`结尾的文件扩展名。

CMake Language源文件在一个项目里这么组织:

* 目录 -> `CMakeLists.txt`
* 脚本 -> `<script>.cmake`
* 模块 -> `<module>.cmake`

# 语法

## 结尾

## 源文件

一个CMake Language源文件包含零个或更多命令调用，用新行或者可选的空格或者注释分隔。

```
file         ::=  file_element*
file_element ::=  command_invocation line_ending |
                  (bracket_comment|space)* line_ending
line_ending  ::=  line_comment? newline
space        ::=  <match '[ \t]+'>
newline      ::=  <match '\n'>
```
