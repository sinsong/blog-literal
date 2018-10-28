---
title: C++ <vector>
date: 2018-08-27 13:29:59
tags:
- C++
---

# std::vector | class
```C++
template < class T, class Alloc = allocator<T> >
class vector; // generic template
```

vector是顺序容器，提供一个可以改变尺寸的数组。

## 容器性质

* 顺序
* 动态数组
* 储存分配

## 模板参数

T
    元素的类型
Alloc
    内存分配器类型

## 成员类型

value_type
第一模板参数T。

allocator_type
第二模板参数Alloc。

reference
value_type&

const_reference
const value_type&

pointer
allocator_traits<allocator_type>::pointer

const_pointer
allocator_traits<allocator_type>::const_pointer

iterator
value_type的随机访问迭代器

const_iterator
const value_type的随机访问迭代器

reverse_iterator
reverse_iterator<iterator>

const_reverse_iterator
reverse_iterator<const_iterator>

difference_type
带符号整数类型，等价于iterator_traits<iterator>::difference_type

size_type
无符号整数类型，可以表示任意非负 difference_type 的值。

## 成员函数

(constructor)
(destructor)
operator=

### 迭代器

begin
end
rbegin
rend
cbegin
cend
crbegin
crend

### 容量

size 尺寸
max_size 最大尺寸
resize 改变尺寸
capacity 已分配储存容量
empty 是否为空
reserve 请求改变capacity
shrink_to_fit

### 成员访问

operator[]
at
front 首元素
back 尾元素
data

### 修改器

assign 赋值vector内容
push_back 在尾部添加元素
pop_back 删除尾元素
insert 插入元素
erase 清除元素
swap 交换内容
clear 删除内容
emplace 构造和插入元素
emplace_back 构造并在尾部插入元素

### 分配器

get_allocator 获取分配器

## 非成员函数重载

operator:
    == != < <= > >=

swap

## 模板特例化

vector<bool>
