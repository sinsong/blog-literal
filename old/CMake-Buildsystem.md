---
title: CMake-Buildsystem
date: 2018-05-28 20:23:22
tags:
- CMake
---

CMake buildsystem组织了一系列高层逻辑目标。每个目标相当一个可执行文件或者库，或者一个自定义目标包含自定义的命令。目标之间的依赖关系在构建系统中表达，以确定响应响应的构建顺序和构建规则。 

# 二进制目标

可执行用 `add_executable()`定义，库用`add_library()`定义。生成的二进制文件有适当的前缀，后缀和扩展名，用于目标平台。

```cmake
add_library(archive archive.cpp zip.cpp lzma.cpp)
add_executable(zipapp zipapp.cpp)
target_link_libraries(zipapp archive)
```

archive定义为一个静态库-一个归档包含archive.cpp,zip.cpp和lzma.cpp的目标文件。zipapp定义为一个可执行文件由zipapp.cpp编译和链接形成。当链接zipapp时，archive静态库被链接上。

## 二进制可执行

```cmake
add_executable(mytool mytool.cpp)
```

就像`add_custom_command()`，
TODO!

## 二进制库类型

### 普通库

一般，如果不指定类型，`add_library()`命令定义一个静态库。可以这样指定类型。

```cmake
add_library(archive SHARED archive.cpp zip.cpp lzma.cpp)
```

```cmake
add_library(archive STATIC archive.cpp zip.cpp lzma.cpp)
```

`BUILD_SHARED_LIBS`变量会改变`add_library()`的行为，去默认生成动态库。

TODO！

### 目标库

`OBJECT`库类型不被链接。
TODO！

# 生成规范和使用要求

`target_include_directories()`, `target_compile_definitions()` 和 `target_compile_options()` 命令指定生成规范和二进制目标的使用要求。命令分别填充`INCLUDE_DIRECTORIES`, `COMPILE_DEFINITIONS`和`COMPILE_OPTIONS`目标属性，和`INTERFACE_INCLUDE_DIRECTORIES`, `INTERFACE_COMPILE_DEFINITIONS` 和 `INTERFACE_COMPILE_OPTIONS` 目标属性。

每个命令都有`PRIVATE`,`PUBLIC`和`INTERFACE`模式。

## 目标属性

## 可传递使用要求

## 兼容接口性质

## 属性源调试

## 生成规范带上生成器表达式


