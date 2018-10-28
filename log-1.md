---
title: log-1
date: 2018-09-01 02:45:17
tags:
- C++
---

C++ 合成拷贝构造函数带来的坑

# 详情

观察该类

```C++
class primitive
{
    primitive(unsigned x, unsigned y) : sx(x), sy(y), size(sx * sy)
    {
        data = new wchar_t[size];
    }

    ~primitive()
    {
        delete[] data;
    }

private:
    const unsigned sx;
    const unsigned sy;
    const unsigned size;

    wchar_t *data;
}
```

在观察一个函数

```C++
void prisent(primitive p)
{
    screen.draw(p);
}
```

# 原因

prisent函数的参数是传值的，所以会发生拷贝。
调用拷贝构造函数。
我们没有定义拷贝构造，编译器会合成一个。
合成的版本简单的拷贝各个成员，将data指针的地址拷贝了。

然后prisent中的副本将data指针释放。
main传入的那个对象将data重复释放。造成错误。

# 反思

做好拷贝控制，认真设计拷贝控制成员。
谨慎使用动态内存。
一般传参用const T&。