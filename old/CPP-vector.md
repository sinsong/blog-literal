---
title: vector
date: 2018-07-05 00:39:19
tags:
- C++
---

vector
`template < class T, class Alloc = allocator<T> > class vector;`

> 特性：
> 有序，动态数组，可察觉的分配器

模板参数
T
元素的类型

Alloc
分配器对象的类型

成员类型

`value_type`			第一模板参数T
`allocator_type`		第二模板参数Alloc
`reference`			`value_type&`
`const_reference`		`const value_type&`
`pointer`			`allocator_traits<allocator_type>::pointer`
`const_pointer`			`allocator_traits<allocator_type>::const_pointer`
`iterator`			`value_type`的随机访问迭代器
`const_iterator`		`const value_type`的随机访问迭代器
`reverse_iterator`		`reverse_iterator<iterator>`
`const_reverse_iterator`	`reverse_iterator<const_iterator>`
`difference_type`		有符号整数类型，等同`iterator_traits<iterator>::difference_type`
`size_type`			无符号整数类型，可以表示任意非负`difference_type`值

成员函数

(构造函数)	构造vector
(析构函数)	析构vector
operator=	内容赋值

迭代器
begin	首迭代器
end	尾后迭代器
rbegin	首反向迭代器
rend	尾后反向迭代器
迭代器对应的常量版本
cbegin	
cend
crbegin
crend

容量
`size`		尺寸
`max_size`	最大尺寸
`resize`	更改尺寸
`capacity`	已分配储存容量
`empty`		是否为空
`reserve`	请求容量更改
`shrink_to_fit`	压缩尺寸

元素访问

`operator[]`	访问元素
`at`		访问元素
`front`		访问首元素
`back`		访问尾元素
`data`		访问数据

修改器
`assign`	vector内容赋值
`push_back`	添加元素到末端
`pop_back`	删除最后一个元素
`insert`	插入元素
`erase`		擦除元素
`swap`		交换内容
`clear`		清除内容
`emplace`	构造和插入元素
`emplace_back`	构造和插入元素到末端

分配器
`get_allocator`	获取构造器

非成员函数重载
relational operators
swap	交换vecotr的内容

模板特例化
`vector<bool>`


