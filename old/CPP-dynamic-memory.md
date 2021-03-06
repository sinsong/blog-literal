---
title: C++ 动态内存
date: 2018-08-08 23:46:17
tags:
- C++
---

静态内存用来保存局部static对象，类static数据成员以及定义在任何函数之外的变量。
栈内存用来保存定义再函数内的非static对象。
分配再静态或栈内存的对象由编译器自动创建和销毁。
对于栈对象，仅在其定义的程序块运行时才存在；static对象在使用之前分配，在程序结束时销毁。

除了静态内存和栈内存，每个程序还拥有一个内存池。这部分内存被称作自由空间(free store)或堆(heap)。程序用堆来存储动态分配(dynamically allocate)的对象——即，那些在程序运行时分配的对象。动态对象的生存期由程序来控制，也就是说，当动态对象不再使用时，我们的代码必须显式的销毁它们。

> 虽然使用动态内存有时是必要的，但众所周知，正确的管理动态内存是非常棘手的。

# 动态内存与智能指针

C++中，动态内存的管理是通过一对运算符来完成的：new，在动态内存中为对象分配空间并返回一个指向该对象的指针，我们可以选择对对象进行初始化；delete，接受一个动态对象的指针，销毁该对象，并释放与之关联的内存。

为了更容易（同时也更安全）的使用动态内存，新的标准库提供了两种智能指针(smart pointer)类型来管理动态对象。
智能指针的行为类似常规指针，重要的区别是它负责自动释放所指向的对象。
shared_ptr允许多个指针指向同一个对象；
unique_ptr则“独占”所指向的对象。
还有一个weak_ptr的伴随类，他是一种弱引用，指向shared_ptr所管理的对象。
这三种类型都定义在memory头文件中。

# shared_ptr

shared_ptr和unique_ptr都支持的操作

shared_ptr<T> sp
unique_ptr<T> up
空智能指针，可以指向类型为T的对象

p
将p用作一个条件判断，若p指向一个对象，则为true

*p
解引用p，获得它指向的对象

p->mem
等价于(*p).mem

p.get()
返回p中保存的指针。
要小心使用，若智能指针释放了其对象，返回的指针所指向的对象也就消失了

swqp(p, q)
p.swap(q)
交换p和q中的指针


shared_ptr独有的操作

make_shared<T>(args)
返回一个shared_ptr，指向一个动态分配的类型为T的对象。使用args初始化此对象。

shared_ptr<T> p(q)
p是shared_ptr的拷贝；此操作会递增q中的计数器。q中的指针必须能转换为T*

p = q
p和q都是shared_ptr，所保存的指针必须能相互转换。此操作会递减p的引用计数，递增q的引用计数；若p的引用计数变为0，则将其管理的原内存释放

p.unique()
若p.use_count()为1，返回true；否则返回false

p.use_count()
返回与p共享对象的智能指针数量；可能很慢，主要用于调试

### make_shared 函数

最安全的分配和使用动态内存的方法是调用一个名为mark_shared的标准库函数。此函数在动态内存中分配一个对象并初始化它，返回指向此对象的shared_ptr。与智能指针一样，make_shared也定义在memory中。

要使用mark_shared时，必须指定想要创建的对象的类型。

```C++
shared_ptr<int> p = make_shared<int>(42);
```

# 直接管理内存

C++定义了两个运算符来分配和释放内存。
运算符new分配内存，delete释放new分配的内存。

相对于智能指针，使用这两个运算符管理内存非常容易出错。自己管理内存的类与使用智能指针的类不同，它们不能依赖类对象拷贝、赋值和销毁操作的任何默认定义。因此，使用智能指针的程序更容易编写和调试。

### 使用new动态分配和初始化对象

在自由空间分配的内存是无名的，因此new无法为其分配的对象命名，而是返回一个指向该对象的指针。

```C++
int *pi = new int; //pi指向一个动态分配的、未初始化的无名对象
```

此new表达式在自由空间构造一个int型对象，并返回指向该对象的指针。

默认情况下，动态分配的对象是默认初始化的，这意味着内置类型或组合类型的对象的值将是未定义的，而类类型对象将用默认构造函数进行初始化。

```C++
string *ps = new string; //初始化为空string
int *p1 = new int; //pi指向一个未初始化的int
```

我们可以使用直接初始化方式类初始化一个动态分配的对象。
我们可以使用传统的构造方式，在新标准下，也可以使用列表初始化。

```C++
int *pi = new int(1024); //pi指向的对象的值为1024
string *ps = new string(10, '9'); //*ps为"9999999999"
vector<int> *pv = new vector<int>{0,1,2,3,4,5,6,7,8,9};
```

也可以对动态分配的对象进行值初始化，只要在类型名之后跟一对空括号即可。

```C++
string *ps = new string(); //值初始化为空string
int *pi = new int(); //值初始化为0
```

对于定义了自己的构造函数的类类型来说，要求值初始化是没有意义的；不管采用什么形式，对象都会通过默认构造函数来初始化。
但对于内置类型，两种形式的区别就很大了；值初始化的内置类型对象有着良好定义的值，而默认初始化的对象的值则是未定义的。
类似的，对于类中那些依赖于编译器合成的默认构造函数的内置类型成员，如果它们未在类内被初始化，那么它们的值也是未定义的。

> 出于与变量初始化相同的原因，对动态分配的对象进行初始化通常是个好主意。

如果我们提供了一个括号包围的初始化器，就可以使用auto从此初始化器来推断我们想要分配的类型，只有当括号中仅有单一初始化器时才可以使用auto。

```C++
auto p1 = new auto(obj); //p指向一个与obj类型相同的对象
//该对象用obj进行初始化

auto p2 = new auto{a,b,c}; //错误：括号中只能有单个初始化器
```

p1的类型是一个指针，指向从obj自动推断出的类型。若obj是一个int，那么p1就是int*；新分配的对象用obj的值进行初始化。

### 动态分配的const对象

用new分配一个const对象是合法的

```C++
//分配并初始化一个const int
const int *pci = new const int(1024);
//分配并默认初始化一个const的空string
const string *pcs = new cosnt string;
```

一个动态分配的const对象必须进行初始化。对于一个定义了默认构造函数的类类型，器const动态对象可以隐式初始化，而其他类型的对象就必须显式初始化。由于分配的对象是const的，new返回的指针是一个指向const的指针。

### 内存耗尽
虽然现代计算机通常都配备大容量内存，但是自由空间被耗尽的情况还是有可能发生。
一旦一个程序用光了它所有可用的内存，new表达式就会失败。
默认情况下，如果new不能分配所要求的内存空间，它会抛出一个bad_alloc的异常。我们可以改变使用new的方式来阻止他抛出异常。

```C++
//如果分配失败，new返回一个空指针
int *p1 = new int; //如果分配失败，new抛出std::bad_alloc
int *p2 = new (nothrow) int; //如果分配失败，new返回一个空指针
```

我们称这种形式的new为定位new(placement new)。
定位new表达式允许我们向new传递额外的参数。
此例中，我们传递给它一个由标准库定义的名为nothrow的对象。如果将nothrow传递给new，我们的意图是告诉他不能抛出异常。如果这种形式的new不能分配所需内存，它会返回一个空指针。
bad_alloc和nothow都定义在new中。

### 释放动态内存

为了防止内存耗尽，在动态内存使用完毕后，必须将其归还给系统。
我么通过delete表达式(delete expression)来将动态内存归还给系统。delete表达式接受一个指针，指向我们想要释放的对象。

```C++
delete p; //p必须指向一个动态分配的对象或是一个空指针
```

与new类型类似，delete表达式也执行两个动作：销毁给定的指针指向的对象；释放对应的内存。

### 指针值和delete

我们传递给delete的指针必须指向动态分配的内存，或者是一个空指针。
释放一块并非new分配的内存，或者将相同的指针值释放多次，其行为都是未定义的。

虽然一个const对象的值不能改变，但它本身是可以被销毁的。
释放一个const对象，只要delete指向它的指针即可。

```C++
const int *pci = new const int(1024);
delete pci; //正确：释放一个const对象
```

### 对象的生存期直到被释放时为止

当一个指针离开其作用域时，它所指向的对象什么也不会发生。
如果这个指针指向的是动态内存，那么内存不会被自动释放。

> 由内置指针（而不是智能指针）管理的动态内存在被显式释放前一直都会存在。

小心：动态内存的管理非常容易出错。

1. 忘记delete内存。忘记释放动态内存会导致人们常说的“内存泄漏”问题，因为这种内存永远不可能被归还给自由空间了。查找泄露错误是非常困难的，因为通常应用成语运行很长时间后，真正耗尽内存时，才能检测到这种错误。
2. 使用已经释放的对象。通常在释放内存后将指针置为空，有时可以检测出这种错误。
3. 同一块内存释放两次。当有两个指针指向相同的动态分配对象时，可能发生这种错误。如果对其中一个指针进行了delete操作，对象的内存就被归还给自由空间了。如果我们随后又delete第二个指针，自由空间就可能被破坏。

> 坚持只使用智能指针，就可以避免所有这些问题。对于一块内存，只有在没有任何智能指针指向他的情况下，智能指针才会自动释放它。

### delete之后重置指针值

当我们delete一个指针后，指针值就变为无效了。虽然指针已经无效，但在很多机器上指针仍保存着（已经释放的）动态内存的地址。在delete之后，指针就变成了人们所说的空悬指针(dangling pointer)，即，指向一块曾经保存数据对象但现在已经无效的内存的指针。

未初始化指针的所有缺点空悬指针也都有。
有一种方法可以避免空悬指针的问题：在指针即将要离开其作用域之前释放掉它所关联的内存。
这样，在指针关联的内存被释放掉之后，就没有机会继续使用指针了。如果我们需要保留指针，可以在delete之后将nullptr赋予指针，这样就清楚的指出指针不指向任何对象。

# 删除器

默认情况下，shared_ptr假定它们指向的是动态内存。
所以，当一个shared_ptr被销毁时，它门人的对它管理的指针进行delete操作。
我们可以定义一个函数来代替delete。
这个删除器(deleter)函数必须能够完成对shared_ptr中保存的指针进行释放操作。

```C++
void deleter(T *p)
{
    //定制操作
}

shared_ptr<T> p(&obj, deleter);
```

# unique_ptr

一个unique_ptr“独占”它所指向的对象：某个时刻只能有一个unique_ptr指向一个给定对象。
当unique_ptr被销毁时，它所指向的对象也被销毁。

没有类似make_shared的标准库函数返回一个unique_ptr。
当我们定义一个unique_ptr时，需要将其绑定到一个new返回的指针上。
类似shared_ptr，初始化unique_ptr必须采用直接初始化形式。

```C++
unique_ptr<double> p1; //可以指向一个double的unique_ptr
unique_ptr<int> p2(new int(42)); //p2指向一个值为42的int
```

unique_ptr不支持普通的拷贝或赋值操作。

unique_ptr的操作

unique_ptr<T> u1
unique_ptr<T, D> u2
空unique_ptr，可以指向类型为T的对象。u1会使用delete释放它的指针；u2会使用一个类型为D的可调用对象来释放它的指针

unique_ptr<T, D> u(d)
空unique，指向类型为T的对象,用类型为D的对象d代替delete

u = nullptr
释放u指向的对象，将u置为空

u.release()
u放弃对指针的控制权，返回值这，并将u置为空

u.reset()
释放u指向的对象

u.reset(q)
u.reset(nullptr)
如果提供了内置指针q，令u指向这个对象；否则将u置为空

虽然我们不能拷贝和赋值，但是我们可以转移所有权。

```C++
//将所有权从p1转移给p2
unique_ptr<string> p2(p1.release()); //release将p1置为空
unique_ptr<string> p3(new string("..."));
//将所有权从p3转移给p2
p2.reset(p3.release()); //reset释放了p2原来指向的内存
```

### 向unique_ptr传递删除器

```C++
void deleter(T* obj)
{
    //...
}

unique_ptr<T, decltype(deleter)*> p(&obj, deleter);
```

# weak_ptr

weak_ptr是一种不控制所指向对象生存期的智能指针，它指向由一个shared_ptr管理的对象。

weak_ptr

weak_ptr<T> w
空weak_ptr可以指向类型为T的对象

weak_ptr<T> w(sp)
与shared_ptr sp指向相同对象的weak_ptr。T必须能转换为sp指向的类型

w = p
p可以是一个shared_ptr或一个weak_ptr。赋值后w与p共享对象

w.reset()
将w置为空

w.use_count()
与w共享对象的shared_ptr数量

w.expired()
如果w.use_count()为0，返回true，否则返回false

w.lock()
如果expired为true，返回一个空shared_ptr；否则返回一个指向w的对象的shared_ptr

# 动态数组

new和delete运算符一次分配/释放一个对象，但某些应用需要一次为很多对象分配内存的功能。

C++提供了两种一次分配对象数组的方法。
另一种new表达式语法和allocator的类。

## new和数组

为了让new分配一个对象数组，我们要在类型名之后跟一对方括号，在其中指明要分配的对象的数目。

```C++
new T[count];
```

方括号中的大小必须是整形，但不必是常量。

也可以用数组类型的类型别名来分配一个数组，这样，new表达式中就不需要方括号了。

```C++
typedef int arrT[42]; //arrT表示42个int的数组类型
int *p = new arrT; //分配一个42个int的数组；p指向第一个int
```

这代码中没有方括号，编译器执行这个表达式时还是会用new[]。即：new int[42];

### 分配一个数组会得到一个元素类型的指针

当用new分配一个数组时，我们并未得到一个数组类型的对象，而是得到一个数组元素类型的指针。即使我们使用类型别名定义了一个数组类型，new也不会分配一个数组类型的对象。

由于分配的内存并不是一个数组类型，因此不能对动态数组调用begin或end。这些函数使用数组维度来返回指向首元素和尾后元素的指针。出于相同的原因，也不能用范围for语句来处理（所谓的）动态数组中的元素。

> 要记住我们所说的动态数组并不是数组类型，这是很重要的。

### 初始化动态分配的数组

默认情况下，new分配的对象，不管是单个分配的还是数组中的，都是默认初始化的。
可以对数组中的元素进行值初始化，方法是在大小之后跟一对空括号。

```C++
int *pia = new int[10](); //10个值初始化为0的int
```

新标准中，我们可以使用列表初始化。

```C++
int *pia = new int[10]{0,1,2,3,4,5,6,7,8,9};
```

剩余元素值初始化，如果列表元素个数大于元素数目，则new表达式失败，不会分配任何内存。抛出bad_array_new_length的异常。

虽然我们用空括号对数组元素进行值初始化，但不能在括号中给出初始化器，这意味着不能用auto分配数组。

### 动态分配一个空数组时合法的

```C++
new T[0];
```

当我们用new分配一个大小为0的数组时，new返回一个合法的非空指针。此指针保证与new返回的其他指针都不相同。
对于零长度的数组来说，此指针就像尾后指针一样，我们可以像使用尾后迭代器一样使用这个指针。可以用此指针进行比较操作，可以移动。但是不能解引用——毕竟它不指向任何元素。

### 释放动态数组

为了释放动态数组，我们使用一种特殊形式的delete————在指针前加上一个空方括号对。

```C++
delete p; //p必须指向一个动态分配的对象或为空
delete [] pa; //pa必须指向一个动态分配的数组或为空
```

销毁pa指向的数组中的元素，并释放对应的内存。数组中的元素按逆序销毁。

当我们释放一个指向数组的指针时，空方括号对是必须的：它指示编译器此指针指向一个对象数组的第一个元素。如果我们在delete一个指向数组的指针时忽略了方括号（或者delete[]了一个单一对象的指针），其行为是未定义的。

> delete和delete[]的使用上，如果用错，编译器很可能不会给出警告。我们的程序可能在执行过程中在没有任何警告的情况下行为异常。

### 智能指针和动态数组

标准库提供了一个可以管理new分配的数组的unique_ptr版本。
为了用一个unique_ptr管理动态数组，我们必须在对象类型后面跟一对空方括号。

```C++
unique_ptr<int[]> up(new int[10]);
up.release(); //自动用delete[]销毁其指针
```

当一个unique_ptr指向一个数组时，我们不能使用点和箭头运算符。
毕竟指向的是一个数组而不是单个对象，因此这些运算符是无意义的。

当unique_ptr指向一个数组时，我们可以使用下表运算符来访问数组中的元素。

```C++
up[pos];
```

指向数组的unique_ptr

unique_ptr<T[]> u
u可以指向一个动态分配的数组，数组元素类型为T

unique_ptr<T[]> u(p)
u指向内置指针p所指向的动态分配的数组。
p必须能转换为类型T*

u[i]
返回u拥有的数组中位置i处的对象
u必须指向一个数组

shared_ptr不直接支持管理动态数组。如果希望使用shared_ptr管理一个动态数组，必须提供自己定义的删除器。

```C++
shared_ptr<int> sp(new int[10], [](int *p){ deletep[] p; });
sp.reset(); //使用我们提供的lambda释放数组，它使用delete[]
```

如果未提供删除器，这段代码将是未定义的。

shared_ptr未定义下标运算符，而且智能指针类型不支持指针算术运算。
因此，为了访问数组中的元素，必须用get获取一个内置指针，然后用它来访问数组元素。

