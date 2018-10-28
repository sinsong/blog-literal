---
title: C++ string
date: 2018-08-27 13:50:00
tags:
- C++
---

```C++
typedef basic_string<char> string;
```

string是提供字符序列的对象。

```C++

// 构造

string(); //空字符串构造（默认构造函数）
string(str); //拷贝构造 拷贝str
string(str, pos, npos); //字串构造 构造为str的从pos开始，npos为长度的字串
string(c_str); //从C风格字符串构造 拷贝C风格字符串
string(c_str, n); //从缓冲区构造，c_str是指向字符数组的指针，拷贝n个字符
string(n, c); //填充构造 填充n个字符c
string(begin, end); //范围构造 拷贝字符序列从范围[begin, end)
string(il); //初始化列表 拷贝il中的每个字符，以相同顺序，il是initializer_list
string(str); //移动构造 str是std::string&&

// 拷贝
operator=

// 迭代器
begin
end
rbegin
rend
cbegin
cend
crbegin
crend

// 容量

str.size(); //字符串长度
str.length(); //字符串长度
str.max_size(); //字符串最大长度

str.resize(n); //改变长度到n个字符
str.resize(n, c); //改变长度到n个字符，指定c为不足的填充，不然就不足的值初始化

str.capacity(); //返回已分配储存空间大小
str.reserve(n); //请求改变capacity为n

str.clear(); //清除字符串
str.empty(); //是否为空
str.shrink_to_fit(); //请求字符串削减capacity到size

// 成员访问

operator[]
str.at(pos); //下标为pos的字符
str.front(); //第一个字符的引用
str.back(); //最后一个字符的引用

// 修改器

operator+= (str); //附加字符串，str是const string&
operator+= (s); //附加C风格字符串，s是const char*
operator+= (c); //附加字符，c是char
operator+= (il); //附加初始化列表，il是initializer_list<char>

str.append(str); //附加字符串，str是const string&
str.append(str, subpos, sublen); //附加str的字串，str是const string&
str.append(s); //附加C风格字符串，s是const char*
str.append(s, n); //附加缓冲区。。。s是const char*，n是个数
str.append(n, c); //附加一个填充的字符串。n是个数，c是一个字符。
str.append(begin, end); //附加一个区间，[begin, end)
str.append(il); //附加初始化列表，il是initializer_list<char>

str.push_back(c); //从尾部添加一个字符c

str.assign(str); //赋值，用str来赋值
//TODO ：这个坑先占着
http://www.cplusplus.com/reference/string/string/assign/

http://www.cplusplus.com/reference/string/string/

// 字符串操作

str.data(); //获取字符串数据，返回一个指针，是C风格字符串

str.substr(pos, len); //生成字串，pos开始，长度为len

```