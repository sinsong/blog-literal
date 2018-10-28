---
title: C++ 面向对象的程序设计
date: 2018-08-14 21:39:34
tags:
- C++
---

# OOP:概述

面向对象程序设计(object-oriented programming)的核心思想是数据抽象、继承和动态绑定。
通过使用数据抽象，我们可以将类的接口与实现分离；
使用继承，可以定义相似的类型并对其相似关系建模；
使用动态绑定，可以在一定程度上忽略相似类型的区别，而以统一的方式使用它们的对象。

### 继承

通过继承(inheritance)联系在一起的类构成一种层次关系。通常在层次关系的根部有一个基类(base class)，其他类则直接或间接的从基类继承而来，这些继承得到的类成为派生类(derived class)。基类负责定义在层次关系中所有类共同拥有的成员，而每个派生类定义各自特有的成员。

C++中，基类将类型相关的函数与派生类不做改变直接继承的函数区分对待。
对于某些函数，基类希望它的派生类各自定义适合自身的版本，此时基类就将这些函数声明成虚函数(virtual function)。

```C++
class B
{
public:
    virtual T func(...);
};
```

派生类必须通过使用类派生列表(class derivation list)明确指出他是从哪个（哪些）基类继承而来的。
类派生列表的形式是：首先一个冒号，后面紧跟以逗号分隔的基类列表，其中每个基类前面可以有访问说明符。

```C++
class C : public B //C继承了B
{
    T func() override;
}
```

### 动态绑定

通过使用动态绑定(dynamic binding)，我们能用同一段代码分别处理B和C的对象。

函数的运行版本由实参决定给，即在运行时选择函数的版本，所以动态绑定有时又被称为运行时绑定(run-time binding)。

> 在C++语言中，当我们使用基类的引用（或指针）调用一个虚函数时将发生动态绑定。

# 定义基类和派生类

## 定义基类

继承关系中根节点的类通常都会定义一个虚析构函数。

> 基类通常都应该定义一个虚析构函数，即使该函数不执行任何实际操作也是如此。

### 成员函数与继承

派生类可以继承其基类的成员。
派生类需要对某些操作提供自己的新定义以覆盖(override)从基类继承而来的旧定义。

C++语言中，基类必须将他的两种成员区分开来：
一种是基类希望其派生类进行覆盖的函数；
另一种是基类希望派生类直接继承而不要改变的函数。
第一种，基类通常将它定义为虚函数(virtual)。当我们使用指针或引用调用虚函数时，该调用将被动态绑定。根据引用或指针所绑定的对象类型不同，该调用可能执行基类的版本，也可能执行某个派生类的版本。

基类通过在其成员函数的声明语句之前加上关键字virtual使得该函数执行动态绑定。任何构造函数之外的非静态函数都可以是虚函数。关键字virtual只能出现在类内部的声明语句之前而不能用于类外部的函数定义。如果基类把一个函数声明成虚函数，则该函数在派生类中隐式的也是虚函数。

成员函数如果没有被声明为虚函数，则其解析过程发生在编译时而不是运行时。

### 访问控制与继承

派生类可以继承定义在基类中的成员，但是派生类的成员函数不一定有权访问从基类继承而来的成员。和其他使用基类的代码一样，派生类能访问公有成员，而不能访问私有成员。不过在某些时候基类中还有一种成员，基类希望它的派生类有权访问该成员，同时禁止其他用户访问。
我们用受保护的(protected)访问运算符说明这样的成员。

## 定义派生类

派生类必须通过使用类派生列表(class derivation list)明确指出它是从哪个（哪些）基类继承而来的。
类派生列表的形式是：首先一个冒号，后面紧跟以逗号分隔的基类列表，其中每个基类前面可以有一下三种访问说明符中的一个：`public`, `protected`, `private`。

派生类必须将其继承而来的成员函数中需要覆盖的那些重新声明。

```C++
class C : public B
{
public:
    C() = default;

    ret_t mem(...) override;
}
```

访问说明符的作用是控制派生类从基类继承而来的成员是否对派生类的用户可见。

大多数类都只继承一个类，这种形式的继承被称作“单继承”。

### 派生类中的虚函数

派生类经常（但不总是）覆盖它继承的虚函数。如果派生类没有覆盖其基类中的某个虚函数，则该虚函数的行为类似于其他的普通成员，派生类会直接继承其在基类中的版本。

派生类可以在它覆盖的函数前使用virtual关键字，但不是非得这么做。C++新标准允许派生类显式的注明它使用某个成员函数覆盖了它继承的虚函数。
具体做法是在形参列表后面、或者在从const成员函数的const关键字后面、或者在引用成员函数的引用限定符后面添加一个关键字override。

### 派生类对象即派生类向基类的类型转换

一个派生类对象包含多个组成部分：一个含有派生类自己定义的（非静态）成员的子对象，以及一个与该派生类继承的基类对应的子对象，如果有多个基类，那么这样的子对象也有多个。

C++标准并没有明确规定派生类的对象在内存中如何分布。

因为在派生类对象中含有与其基类对应的组成部分，所以我们能把派生类的对象当成基类对象来使用，而且我们也能将基类的指针或引用绑定到派生类对象中的基类部分上。

```C++
B b; //基类对象
C c; //派生类对象
B *p = &b; //p指向B对象
p = &c; //p指向c的B部分
B &r = c; //r绑定到c的B部分
```

这种转换通常称为派生类到基类的(dervived-to-base)类型转换。
编译器会隐式的执行派生类到基类的转换。

这种隐式特性意味着我们可以把派生类对象或者派生类对象的引用用在需要基类引用的地方；同样的，我们也可以把派生类对象的指针用在需要基类指针的地方。

> 派生类对象中含有与其基类对应的组成部分，这一事实是继承的关键所在。

### 派生类构造函数

尽管派生类对象中含有从基类继承而来的成员，但是派生类并不能直接初始化这些成员。派生类也必须使用基类的构造函数来初始化它的基类部分。

> 每个类控制它自己的成员初始化过程。

派生类对象的基类部分与派生类对象自己的数据成员都是在构造函数的初始化阶段执行初始化操作的。
派生类构造函数同样是通过构造函数初始化列表来将实参传递给基类构造函数的。

```C++
C(...) : B(...), mem(...)
{}
//与之前一致
```

传给基类构造函数相应的参数，执行结束后，然后初始化初始值列表中的其他对象，最后执行派生类的构造函数体。

除非我们特别之处，否则派生类对象的基类部分会像数据成员一样执行默认初始化。如果想使用其他的基类构造函数，我们需要以类名加圆括号内的实参列表的形式为构造函数提供初始值。这些实参将帮助编译器决定到底应该使用哪个构造函数来初始化派生类对象的基类部分。

> 首先初始化基类的部分，然后按声明的顺序依次初始化派生类的成员。

### 派生类使用基类的成员

派生类可以访问基类的公有成员和保护成员。

派生类的作用域潜逃在基类的作用域之内。

> 遵循基类的接口
> 每个类负责定义各自的接口。要想与类的对象交互必须使用该类的接口，即使对这个对象是派生类的基类部分也是如此。
> 因此，派生类对象不能直接初始化基类的成员。尽管从语法上来说我们可以在派生类构造函数体内给它的公有或保护的基类成员赋值，但是最好不要这么做。和使用基类的其他场合一样，派生类应该遵循基类的接口，并且通过调用基类的构造函数来初始化那些从基类中继承而来的成员。

### 继承与静态成员

如果基类定义了一个静态成员，则在整个继承体系中只存在该成员的唯一定义。不论从基类中派生出来多少个派生类，对于每个静态成员来说都只存在唯一的实例。

静态成员遵循通用的访问控制规则。

### 派生类的声明

派生类的声明与其他类差别不大，声明中包含类名但是不包含它的派生列表。

```C++
class C : public B; //错误：派生列表不能出现在这里
class C;            //正确：声明派生类的正确方式
```

一条声明语句的目的是令程序知晓某个名字的存在以及该名字表示一个什么样的实体，如一个类，一个函数或一个变量等。
派生列表以及与定义有关的其他细节必须与类的主题一起出现。

### 被用作基类的类

如果我们想将某个类用作基类，则该类必须已经定义而非仅仅声明。

原因：派生类中包含并且可以使用它从基类继承下来的成员，为了使用这些成员，派生类当然要知道它们是什么。因此该规定还有一层隐含的意思，即一个类不能派生它本身。

一个类是基类，同时它也可以是一个派生类。（派生一个链）。

```C++
class Base{/*...*/};
class D1 : public Base{/*...*/};
class D2 : public D1{/*...*/}
```

在这个继承关系中，Base是D1的直接基类(direct base)，同时是D2的间接基类(indirect base)。直接基类出现在派生列表中，而间接基类由派生类通过其直接基类继承而来。

每个类都会继承直接基类的所有成员。对于一个最终的派生类来说，他会继承其直接基类的成员；该直接基类的成员又含有其基类的成员；依此类推直至继承链的顶端。因此，最终的派生类将包含它的直接基类的子对象以及每个间接基类的子对象。

### 防止继承的发生

有时我们会定义这样一种类，我们不希望其他类继承它，或者不想考虑它是否适合作为一个基类。
C++11新标准提供了一种防止继承发生的方法，即在类名后跟一个关键字final。

```C++
class NoDerived final {/*...*/}
class Last final : Base {/*...*/}
```

## 类型转换与继承

> 理解基类和派生类之间的类型转换是理解C++语言面向对象编程的关键所在。

通常情况下，如果我们想把引用或指针绑定到一个对象上，则引用或指针的类型应与对象的类型一致，或者对象的类型含有一个可接受的const转换规则。存在继承关系的类是一个重要的例外：我们可以将基类的指针或引用绑定到派生类的对象上。

可以将基类的指针或引用绑定到派生类对象上有一层极为重要的含义：当使用基类的引用（或指针）时，实际上我们并不清楚该引用（或指针）所绑定对象的真实类型。该对象可能是基类的对象，也可能时派生类的对象。

> 和内置指针一样，智能指针类也支持派生类向基类的类型转换，这意味着我们可以将一个派生类对象的指针储存在一个基类的智能指针内。

### 静态类型与动态类型

当我们使用存在继承关系的类型时，必须将一个变量或其他表达式的静态类型(static type)与该表达式表示对象的动态类型(dynamic type)区分开来。表达式的静态类型在编译时总是已知的，它是变量声明时的类型或表达式生成的类型；动态类型则是变量或表达式表示的内存中的对象的类型。动态类型直到运行时才可知。

如果表达式既不是引用也不是指针，则它的动态类型永远与静态类型一致。
例如：C变量永远是C变量，我们无论如何都不能改变该变量对应的对象的类型。

> 基类的指针或引用的静态类型可能与其动态类型不一致。

### 不存在从基类向派生类的隐式类型转换

之所以存在派生类向基类的类型转换是因为每个派生类对象都包含一个基类部分，而基类的引用或指针可以绑定到该基类部分上。一个基类的对象既可以以独立的形式存在，也可以作为派生类对象的一部分存在。如果基类对象不是派生类对象的一部分，则它只含有基类定义的成员，而不含有派生类定义的成员。

因为一个基类的对象可能是派生类对象的一部分，也可能不是，所以不存在从基类向派生类的类型转换。

即使一个基类指针或引用绑定在一个派生类对象上，我们也不能执行从基类向派生类的转换。

```C++
C c;
B *p = &c; //正确：动态类型是C
C *p2 = p; //错误：不能将基类转换成派生类
```

编译器在编译时无法确定某个特定的转换在运行时是否安全，这是因为编译器只能通过检查指针或引用的静态类型来推断该转换是否合法。
如果在基类中含有一个或多个虚函数，我们可以使用dynamic_cast请求一个类型转换，该转换的安全检查将在运行时执行。
同样，如果我们已知某个基类向派生类的转换是安全的，则我们可以使用static_cast来强制覆盖掉编译器的检查工作。

### 在对象之间不存在类型转换

派生类向基类的自动类型转换只对指针和引用类型有效，在派生类类型和基类类型之间不存在这样的转换。很多时候，我峨嵋你确实希望将派生类对象转换成它的基类类型，但是这种转换的实际发生过程往往与我们期望的有所差别。

请注意，当我们初始化或赋值一个类类型的对象时，实际上是在调用某个函数。当执行初始化时，我们调用构造函数；而当执行赋值操作时，我们调用赋值运算符。这些成员通常都包含一个参数，该参数的类型是类类型的const版本的引用。

因为这些成员接受引用作为参数，所以派生类向基类的转换允许我们给基类的拷贝/移动操作传递一个派生类的对象。这些操作不是虚函数。当我们给基类的构造函数传递一个派生类的对象时，实际运行的构造函数是基类中定义的那个，显然该构造函数只能处理基类自己的成员。类似的，如果我们将一个派生类对象赋值给一个基类对象，则实际运行的赋值运算符也是基类中定义的那个，该运算符同样只能处理基类自己的成员。

> 当我们用一个派生类对象为一个基类对象初始化或赋值时，只有该派生类对象中的基类部分会被拷贝、移动或赋值，它的派生类部分将被忽略掉。

### 关键概念：存在继承关系的类型之间的转换规则

要想理解在具有继承关系的类之间发生的类型转换，有三点非常重要：
* 从派生类向基类的类型转换只对指针或引用类型有效。
* 基类向派生类不存在隐式类型转换。
* 和任何其他成员一样，派生类向基类的类型转换也可能会由于访问受限而变得不可行。

尽管自动类型转换只对指针或引用有效，但是继承体系中的大多数类仍然（显式或隐式的）定义了拷贝控制成员。因此，我们通常能够将一个派生类对象拷贝、移动或赋值给一个基类对象。不过需要注意的是，这种个操作只能处理派生类对象的基类部分。

# 虚函数

当我们使用基类的引用或指针调用一个虚成员函数时会执行动态绑定。因为我们直到运行时才能知道到底调用了那个版本的虚函数，所以所有虚函数都必须有定义。
通常情况下，如果我们不使用某个函数，则无需为函数提供定义。但是我们必须为每一个虚函数都提供定义，而不管它是否被用到了，这是因为连编译器也无法确定到底会使用哪个虚函数。

### 对虚函数的调用可能在运行时才被解析

当某个虚函数通过指针或引用调用时，编译器产生的代码直到运行时才能确定应该调用哪个版本的函数。被调用的函数是与绑定到指针或引用上的对象的动态类型相匹配的那一个。

动态绑定只有当我们通过指针或引用调用虚函数时才会发生。

当我们通过一个具有普通类型（非引用非指针）的表达式调用虚函数时，在编译时就会将调用的版本确定下来。

### 关键概念：C++的多态性

OOP的核心思想是多态性(polymorphism)。
我们把具有继承关系的多个类型称为多态类型，因为我们能使用这些类型的“多种形式”而无需在意它们的差异。引用或指针的静态类型与动态类型不同的事实正是C++语言支持多态性的根本所在。

当我们使用基类的引用或指针调用基类中定义的一个函数时，我们并不知道该函数真正作用的对象是什么类型，因为它可能是一个基类的对象也可能是一个派生类的对象。如果该函数是虚函数，则直到运行时才会决定到底要执行那个版本，判断的依据是引用或指针所绑定的对象的真实类型。

另一方面，对非虚函数的调用在编译时进行绑定。类似的，通过对象进行的函数（虚函数或非虚函数）调用也在编译时绑定。对象的类型是确定不变的，我们无论如何都不可能令对象的动态类型与静态类型不一致。因此，通过对象进行的函数调用将在编译时绑定到该对象所属类中的函数版本上。

> 当且仅当对通过指针或引用虚函数时，才会在运行时解析该调用，也只有在这种情况下对象的动态类型才有可能与静态类型不同。

### 派生类中的虚函数

当我们在派生类中覆盖了某个虚函数时，可以再一次使用virtual关键字指出该函数的性质。然而这么做并非必须，因为一旦某个函数被声明称虚函数，则在所有派生类中他都是虚函数。

一个派生类的函数如果覆盖了某个继承而来的虚函数，则它的形参类型必须与被覆盖的基类函数完全一致。

同样，派生类中虚函数的返回类型也必须与基类函数匹配。
例外：当类的虚函数返回类型是类本身的指针或引用时，上述规则无效。。。

> 基类中的虚函数在派生类中隐含的也是一个虚函数。当派生类覆盖了某个虚函数时，该函数在基类中的形参必须与派生类中的形参严格匹配。

### final和override说明符

派生类如果定义了一个函数与基类中虚函数的名字相同但是形参列表不同，这仍然是合法的行为。
编译器将认为新定义的这个函数与基类中原有的函数时相互独立的。
这是，派生类的函数并没有覆盖掉基类中的版本。
就实际的编程习惯而言，这种声明往往意味着发生了错误，因为我们可能原本希望派生类额能覆盖掉基类中的虚函数，但是一不小心把形参列表弄错了。

要想调试并发现这样的错误显然非常困难。
在C++11新标准中我们可以使用override关键字来说明派生类中的虚函数。
这么做的好处是在使得程序员的意图更加清晰的同时让编译器可以为我们发现一些错误，后者在编程实践中显得更加重要。
如果我们使用override标记了某个函数，但该函数并没有覆盖已存在的虚函数，此时编译器将报错。

```C++
struct B
{
    virtual void f1(int) const;
    virtual void f2();
    void f3();
};
struct D : B
{
    void f1(int) const override; //正确：f1与基类中的f1匹配
    void f2(it) override;        //错误：B没有形如f2(int)的函数
    void f3() override;          //错误：f3不是虚函数
    void f4() override;          //错误：B没有名为f4的函数
};
```

我们还能把某个函数指定为final，如果我们已经把函数定义成final了，则之后任何尝试覆盖该函数的操作都将引发错误。

```C++
struct D : B
{
    void f(int) const final; //不允许后续的其他类覆盖f1(int)
};
```

final和override说明符出现在形参列表（包括任何const或引用修饰符）以及尾置返回类型之后。

### 虚函数与默认实参

虚函数也可以拥有默认实参。
如果某次函数调用使用了默认实参，则该实参值由本次调用的静态类型决定。

换句话说，如果我们通过基类的引用或指针调用函数，则使用基类中定义的默认实参，即使实际运行的是派生类中的函数版本也是如此。此时，传入派生类函数的将是基类函数定义的默认实参。如果派生类函数依赖不同的实参，则程序结果将于我们的预期不符。

> 如果虚函数使用默认实参，则基类和派生类中定义的默认实参最好一致。

### 回避虚函数的机制

在某些情况下，我们希望对虚函数的调用不要进行动态绑定，而是强迫其执行虚函数的某个特定版本。使用作用域运算符可以实现这一目的。

```C++
// 强行调用基类中定义的函数版本而不管baseP的动态类型到底是什么
double undiscounted = baseP->Quote::net_price(42);
```

该代码强行调用Quote的net_price函数，而不管baseP实际指向的对象类型到底是什么。
该调用将在编译时完成解析。

> 通常情况下，只有成员函数（或友元）中的代码才需要使用作用域运算符来回避虚函数机制。

什么时候我们需要回避虚函数的默认机制呢？
通常是当一个派生类的虚函数调用它覆盖的基类的虚函数版本时。在此情况下，基类的版本通常完成继承层次中所有类型都要做的共同任务，而派生类中定义的版本需要执行一些与派生类本身密切相关的操作。

> 如果一个派生类虚函数需要调用它的基类版本，但是没有使用作用域运算符，则在运行时该调用将被解析为对派生类版本自身的调用，从而导致无限递归。

# 抽象基类

不要创建某种类的实例。
创建方法以供覆盖，不要使用。

### 纯虚函数

将函数定义为纯虚(pure virtual)函数，告诉用户这个函数没有实际意义。
纯虚函数无需定义。我们通过在函数体的位置（即在声明语句的分毫之前）书写=0就可以将一个虚函数说明为纯虚函数。
其中，=0只能出现在类内部的虚函数声明语句处。

```C++
class D : public B
{
public:
    ret_t func(...) = 0;
};
```

我们也可以为纯虚函数提供定义，不过函数体必须定义在类的外部。

### 含有纯虚函数的类是抽象基类

含有（或者未经覆盖直接继承）纯虚函数的类是抽象基类(abstract base class)。抽象基类负责定义接口，而后续的其他类可以覆盖该接口。我们不能（直接）创建一个抽象基类的对象。

> 我们不能创建抽象基类的对象

### 关键概念：重构

重构(refactoring)负责重新设计类的体系以便将操作和/或数据从一个类移动到另一个类中。
对于面向对象的引用程序来说，重构是一种很普遍的现象。

# 访问控制与继承

每个类分别控制自己的成员初始化过程，与之类似，每个类还分别控制着其成员对于派生类来说是否可访问(accessible)。

### 受保护的成员

protected关键字来声明那些它希望与派生类共享但是不想被其他公共访问使用的成员。
protected说明符可以看作是public和private中和后的产物：
* 和私有成员类似，受保护的成员对于类的用户来说是不可访问的。
* 和公有成员类似，受保护的成员对于派生类的成员和友元来说是可访问的。
* （重要性质）派生类的对象或友元只能通过派生类对象来访问基类的受保护的成员。派生类对于一个基类对象中的受保护成员没有任何访问特权。

最后一个规则的例子：

```C++
class Base
{
protected:
    int prot_mem; //protected成员
};
class Sneaky : public Base
{
    friend void clobber(Sneaky&); //能访问Sneaky::prot_mem
    friend void clobber(Base&); //不能访问Base::prot_mem
    int j; //j默认是private
};
//正确： clobber能访问Sneaky对象的private和protected成员
void clobber(Sneaky &s) {s.j = s.prot_mem = 0;}
//错误： clobber不能访问Base的protected成员
void clobber(Base &b) {b.prot_mem = 0;}
```

如果派生类（及其友元）能访问基类对象的受保护成员，则上面的第二个clobber（接受一个Base&）将是合法的。该函数不是Base的友元，但是它仍然能够改变一个Base对象的思路。如果按照这样的思路，则我们只要定义一个形如Sneaky的新类就能非常简单的规避掉protected提供的访问保护了。

要想阻止以上的用法，我们就要做出如下规定，即派生类的成员和友元只能访问派生类对象中基类部分的受保护成员；对于普通的基类对象中的成员不具有特殊的访问权限。

### 公有、私有和受保护继承

某个类对其继承而来的成员的访问权限受到两个因素影响：一是在基类中该成员的访问说明符，二是在派生类的派生列表中的访问说明符。

```C++
class Base
{
public:
    void pub_mem(); //public成员
protected:
    int prot_mem; //protected成员
private:
    char priv_mem; //private成员
};
struct Pub_Derv : public Base
{
    //正确：派生类能访问protected成员
    int f() {return prot_mem;}
    //错误：private成员对于派生类来说是不可访问的
    char g() {return priv_mem;}
};
struct Priv_Derv : private Base
{
    //private不影响派生类的访问权限
    int f1() const {return prot_mem;}
};
```

派生访问说明符对于派生类的成员（及其友元）能否访问其直接基类的成员没什么影响。对基类成员的访问权限只与基类中的访问说明符有关。

派生访问说明符的目的是控制派生类用户（包括派生类的派生类在内）对于基类成员的访问权限。

```C++
Pub_Derv d1; //继承自Base的成员是public的
Priv_Derv d2； //继承字Base的成员是private的
d1.pub_mem(); //正确：pub_mem在派生类中是public的
d2.pub_mem(); //错误：pub_mem在派生类中是private的
```

派生访问说明符还可以控制继承自派生类的新类的访问权限。

```C++
struct Derived_from Public : public Pub_derv
{
    //正确： Base::prot_mem和Pub_Derv中仍然是protected的
    int use_base() {return prot_mem;}
};
struct Dervied_form_Private : public Priv_Derv
{
    //错误： Base::prot_mem在Pub_Derv中是Private的
    int use() {return prot_mem;}
};
```

### 派生类向基类转换的可访问性

派生类向基类的转换是否可访问由使用该转换的代码决定，同时派生类的派生访问说明符也会有影响。假定D继承自B：
* 只有当D公有的继承B时，用户代码才能使用派生类向基类的转换；如果D继承B的方式是受保护的或者私有的，则用户代码不能使用该转换。
* 不论D以什么方式继承B，D的成员函数和友元函数都能使用派生类向基类的转换；派生类向其直接基类的类型转换对于派生类的成员和友元来说永远是可访问的。
* 如果D继承B的方式是公有的或者受保护的，则D的派生类的成员和友元可以使用D向B的类型转换；反之，如果D继承B的方式是私有的，则不能使用。

> 对于代码中的某个给定节点来说，如果基类的公有成员是可访问的，则派生类向基类的类型转换也是可访问的；反之则不行。

### 友元与继承

就像友元关系不能传递一样，友元关系同样也不能继承。
基类的友元在访问派生类成员时不具有特殊性，类似的，派生类的友元也不能随意访问基类的成员。

```C++
class Base
{
    //添加friend声明，其他成员与之前的版本一致
    friend class Pal; //Pal在访问Base的派生类时不具有特殊性
};
class Pal
{
public:
    int f(Base b) {return b.prot_mem;} //正确：Pal时Base的友元
    int f2(Sneaky s) {return s.j;} //错误：Pal不是Sneaky的友元
    //对基类的访问权限由基类本身控制，即使对于派生类的基类部分也是如此
    int f3(Sneaky s) {return s.prot_mem;} //正确：Pal是Base的友元
};
```

每个类负责控制自己成员的访问权限。

当一个类将另一个类声明为友元时，这种友元关系只对作出声明的类有效。对于原来那个类来说，其友元的基类或者派生类不具有特殊的访问能力。

```C++
//D2对Base的protected和private成员不具有特殊的访问能理
class D2 : public Pal
{
public:
    int mem(Base b)
    {
        return b.prot_mem; //错误：友元关系不能继承
    }
};
```

> 不能继承友元关系；每个类负责控制各自成员的访问权限。

### 改变个别成员的可访问性

有时我们需要改变派生类继承的某个名字的访问级别，通过使用using声明可以达到这一目的。

```C++
class Base
{
public:
    std::size_t size() const {return n;}
protected:
    std::size_t n;
};
class Derived : private Base //注意：private继承
{
public:
    //保持对象尺寸相关的成员的访问级别
    using Base::size;
protected:
    using Base::n;
}
```

通过在类的内部使用using声明语句，我们可以将该类的直接或间接基类中的任何可访问成员（例如，非私有成员）标记出来。using声明语句中名字的访问权限由该using声明语句之前的访问说明符来决定。也就是说，如果一条using声明语句出现在类的private部分，则该名字只能被类的成员或友元访问；如果using声明语句位于public部分，则类的所有用户都能访问它；如果using声明语句位于protected部分，则该名字对于成员、友元和派生类是可访问的。

> 派生类只能为那些它可以访问的名字提供using声明。

### 默认的继承保护级别

默认派生运算符也有定义派生类所用的关键字来决定。
默认情况下，使用class关键字定义的派生类是私有继承的；而使用struct关键字定义的派生类是公有继承的。

struct和class唯一的差别就是默认成员访问说明符即默认派生访问说明符；除此之外，再无其他不同之处。

> 一个私有派生的类最好显式的将private声明出来，而不要仅仅依赖与默认的设置。显式声明的好处是可以令私有继承关系清晰明了，不至于产生误会。

# 继承中的类作用域

每个类都有自己的作用域，在这个作用域内我们定义类的成员。当存在继承关系时，派生类的作用域潜逃在其基类的作用域之内。如果一个名字在派生类的作用域内无法正确解析，则编译器将继续在外层的基类作用域中寻找该名字的定义。

### 在编译时进行名字查找

一个对象，引用或指针的静态类型决定了该对象的哪些成员是可见的。即使静态类型与动态类型可能不一致（当使用基类的引用或指针时会发生这种情况），但是我们能使用哪些成员仍然是由静态类型决定的。

### 名字冲突与继承

派生类也能重用定义在其直接基类或间接基类中的名字，此时定义在内层作用于（即派生类）的名字将隐藏定义在外层作用域（即基类）的名字。

> 派生类的成员将隐藏同名的基类成员。

### 通过作用域运算符来使用隐藏的成员

我们可以通过作用域运算符来使用一个被隐藏的基类成员。

作用域运算符将覆盖掉原有的查找规则。

> 除了覆盖继承而来的虚函数之外，派生类最好不要重用其他定义在基类中的名字。

### 关键概念：名字查找与继承

理解函数调用的解析过程对于理解C++的继承至关重要，假定我们调用p->mem()（或者obj.mem()），则依次执行以下4个步骤：
1. 首先确定p（或obj）的静态类型。因为我们调用的是一个成员，所以该类型必然是类类型。
2. 在p（或obj）的静态类型对应的类中查找mem。如果找不到，则依次在直接基类中不断查找直至到达继承链的顶端。如果找遍了该类及其基类仍然找不到，则编译器将报错。
3. 一旦找到了mem，就进行常规的类型检查以确认对于当前找到的mem，本次调用是否合法。
4. 假设调用合法，则编译器将根据调用的是否是虚函数而产生不同的代码。
    * 如果mem是虚函数且我们是通过引用或指针进行的调用，则编译器产生的代码将在运行时确定到底运行该虚函数的哪个版本，依据是对象的动态类型。
    * 反之，如果mem不是虚函数或者我们是通过对象（而非引用或指针）进行的调用，则编译器将产生一个常规函数调用。

### 一如往常，名字查找先于类型检查

声明在内层作用域的函数并不会重载声明在外层作用域的函数。
因此，定义派生类中的函数也不会重载其基类中的成员。
和其他作用域一样，如果派生类（即内层作用域）的成员与基类（即外层作用域）的某个成员同名，则派生类将在其作用域内隐藏该基类成员。即使派生类成员和基类成员的形参列表不一致，基类成员也仍然会被隐藏掉。

### 虚函数与作用域

现在我们可以理解为什么基类与派生类中的虚函数必须有相同的形参列表了。
假如不同，则我们就无法通过基类的引用或指针调用派生类的虚函数了。

### 通过基类调用隐藏的虚函数

（看书吧：P550）

### 覆盖重载的函数

和其他函数一样，成员函数无论是否是虚函数都能被重载。派生类可以覆盖重载函数的0个或多个实例。如果派生类希望所有的重载版本对于它来说都是可见的，那么它就需要覆盖所有的版本，或者一个也不覆盖。

有时一个类仅需覆盖集合中的一些而非全部函数，此时，如果我们不得不覆盖基类中的每一个版本的话，显然操作将及其繁琐。

一种好的解决方案是为重载的成员提供一条using声明语句，这样我们就无需覆盖基类中的每一个重载版本了。
using声明语句指定一个名字而形参列表，所以一条基类成员函数的using声明语句就可以把该函数的所有重载实例添加到派生类作用域中。此时，派生类只需要定义其特有的函数就可以了，而无需为继承而来的其他函数重新定义。

类内using声明的一般规则同样适用于重载函数的而名字；基类函数的每个实例在派生类中都必须是可访问的。对派生类没有重新定义的重载版本的访问实际上是对using声明点的访问。

# 构造函数与拷贝控制

和其他类一样，位于继承体系中的类也需要控制当其对象执行一系列操作时发生什么样的行为这些操作包括创建、拷贝、移动、赋值和销毁。如果一个类（基类或派生类）没有定义拷贝控制操作，则编译器将为它合成一个版本。当然，这个合成的版本也可以定义成被删除的函数。

## 虚析构函数

继承关系对基类拷贝控制最直接的影响是基类通常应该定义一个虚析构函数，这样我们就能动态分配继承体系中的对象了。

当我们delete一个动态分配的对象的指针时将执行析构函数。
如果该指针指向继承体系中的某个类型，则有可能出现指针的静态类型与被删除对象的动态类型不符的情况。
我们通过在基类中将析构函数定义成虚函数以确保执行正确的析构函数版本。

```C++
class Base
{
public:
    //如果我们删除的是一个指向派生类对象的基类指针，则需要虚析构函数
    virtual ~Base() = default; //动态绑定析构函数
};
```

和其他虚函数一样，析构函数的虚属性也会被继承。

> 如果基类的析构函数不是虚函数，则delete一个指向派生类对象的基类指针将产生未定义的行为。

之前我们曾介绍过一条经验准则，即如果一个类需要析构函数，那么它也同样需要拷贝和赋值操作。
基类的析构函数并不遵循上述准则，他是一个重要的例外。
一个基类总是需要析构函数，而且他能将析构函数设定为虚函数。此时，该析构函数为了成为虚函数而令内容为空，我们显然无法由此推断该基类还需要赋值运算符和拷贝构造函数。

### 虚析构函数将阻止合成移动操作

基类需要一个虚析构函数这一事实还会对基类和派生类的定义产生另外一个间接的影响：如果一个类定义了析构函数，即使它通过=default的形式使用了合成的版本，编译器也不会为这个类合成移动操作。

## 合成拷贝控制与继承

基类或派生类的拷贝控制成员的行为与其他合成的构造函数、赋值运算符或析构函数类似：它们对类本身的成员依次进行初始化、赋值或销毁操作。此外，这些合成的成员还负责使用直接基类中对应的操作对一个对象的直接基类部分进行初始化、赋值或销毁的操作。

（后面的看书，P553）

### 派生类中删除的拷贝控制与基类的关系

就像其他任何类的情况一样，基类或派生类也能出于同样的原因将其合成的默认构造函数或者任何一个拷贝控制成员定义成删除的函数。此外，某些定义基类的方式也可能导致有的派生类成员成为被删除的函数：
* 如果基类中的默认构造函数、拷贝构造函数、拷贝赋值运算符或析构函数是被删除的函数或者不可访问，则派生类中对应的成员将是被删除的，原因是编译器不能使用基类成员来执行派生类对象部分的构造、赋值或销毁操作。
* 如果在基类中有一个不可访问或删除掉的析构函数，则派生类中合成的默认和拷贝构造函数是被删除的，因为编译器无法销毁派生类对象的基类部分。
* 编译器将不会合成一个删除掉的移动操作。当我们使用=default请求一个移动操作时，如果基类中对应操作是删除的或不可访问的，那么派生类中该函数将是删除的，原因是派生类对象的基类部分不可移动。同样，如果基类的析构函数是删除的或不可访问的，则派生类的移动构造函数也将是被删除的。

### 移动操作与继承

大多数基类都会定义一个虚析构函数。
因此在默认情况下，基类通常不含有合成的移动操作，而且在它的派生类中也没有合成的移动操作。

因为基类缺少移动操作会阻止派生类拥有自己的合成移动操作，所以当我们确实需要执行移动操作时应该首先在基类中进行定义。

## 派生类的拷贝控制成员

派生类构造函数在其初始化阶段中不但要初始化派生类自己的成员，还负责初始化派生类对象的基类部分。
因此，派生类的拷贝和移动构造函数在拷贝和移动自有成员的同时，也要拷贝和移动基类部分的成员。
类似的，派生类赋值运算符也必须为其基类部分的成员赋值。

和构造函数及赋值运算符不同的是，析构函数只负责销毁派生类自己分配的资源。
如前所述，对象的成员是被隐式销毁的；类似的，派生类对象的基类部分也是自动销毁的。

> 当派生类定义了拷贝和移动操作时，该操作负责拷贝或移动包括基类部分成员在内的整个对象。

### 定义派生类的拷贝或移动构造函数

当为派生类定义拷贝或移动构造函数时，我们通常使用对应的基类构造函数初始化对象的接力部分。

```C++
class D : public Base
{
public:
    //默认情况下，基类的默认构造函数初始化对象的基类部分
    //要想使用拷贝或移动构造函数，我们必须在构造函数初始值列表中
    //显式的调用该构造函数
    D(const D& d) : Base(d) //拷贝基类成员
        /*D的成员的初始值*/ {/*...*/}
    D(D&& d) : Base(std::move(d)) //移动基类成员
        /*D的成员的初始值*/ {/*...*/}
};
```

初始值Base(d)将一个D对象传递给基类构造函数。尽管从道理上来说，Base可以包含一个参数类型为D的构造函数，但是在实际编程过程中通常不会这么做。相反，Base(d)一般会匹配Base的拷贝构造函数。D类型的对象d将被绑定到该构造函数的Base&形参上。Base的拷贝构造函数负责将d的基类部分拷贝给要创建的对象。

假如我们没有提供基类的初始值的话。

```C++
//D的这个拷贝构造函数很可能不是正确的定义
//基类部分被默认初始化，而非拷贝
D(const D& d) /*成员初始值，但是没有提供基类初始值*/
    {/*...*/}
```

> 在默认情况下，基类默认构造函数初始化派生类对象的基类部分。如果我们想拷贝（或移动）基类部分，则必须在派生类的构造函数初始值列表中显式的使用基类的拷贝（或移动）构造函数。

### 派生类赋值运算符

派生类的赋值运算符也必须显式的为其基类部分赋值。

```C++
// Base::operator=(const Base&)不会被自动调用
D &D::operator=(const D &rhs)
{
    Base::operator=(rhs); //为基类部分赋值
    //按照过去的方式为派生类的成员赋值
    //酌情处理自赋值及释放已有资源等情况
    return *this;
}
```

上面的运算符首先显式的调用基类赋值运算符，令其为派生类对象的基类部分赋值。基类的运算符（应该可以）正确的处理自赋值的情况，如果赋值命令是正确的，则基类运算符将释放掉其左侧运算对象的基类部分的旧值，然后利用rhs为其赋一个新值。随后，我们继续进行其他为派生类成员赋值的工作。

值得注意的是，无论基类的构造函数或赋值运算符是自定义的版本还是合成的版本，派生类的对应操作都能使用它们。

### 派生类析构函数

析构函数体执行完成后，对象的成员会被隐式销毁。
类似的，对象的基类部分也是隐式销毁的。
因此，和构造函数及赋值运算符不同的是，派生类析构函数只负责销毁由派生类自己分配的资源。

```C++
class D : public Base
{
public:
    //Base::~Base被自动调用执行
    ~D() {/*此处由用户定义清除派生类成员的操作*/}
};
```

对象销毁的顺序正好与其创建的顺序相反：派生类析构函数首先执行，然后是基类的析构函数，以此类推，沿着继承体系的反方向直至最后。

### 在构造函数和析构函数中调用虚函数

派生类对象的基类部分首先被创建。当执行基类的构造函数时，该对象的派生类部分是未被初始化的状态。类似的，销毁派生类对象的次序正好相反，因此当执行基类的构造函数时，派生类部分已经被销毁掉了。由此可知，当我们执行上述基类成员的时候，该对象出于未完成的状态。

为了能够正确的处理这种未完成的状态，编译器认为对象的类型在构造或析构的过程中仿佛发生了改变一样。也就是说，当我们创建一个对象时，需要把对象的类和构造函数的类看作是同一个；对虚函数的调用绑定正好符合这种把对象的类和构造函数的类看作同一个的要求；对于析构函数也是同样的道理。上述的绑定不但对直接调用虚函数有效，对间接调用也是有效的，这里的间接调用是指通过构造函数（或析构函数）调用另一个函数。

为了理解上述行为，不妨考虑当基类构造函数调用虚函数的派生类版本时会发生什么情况。这个虚函数可能会访问派生类的成员，毕竟，如果它不需要访问派生类成员的话，则派生类直接使用基类的虚函数版本就可以了。然而，当执行基类构造函数时，它要用到的派生类成员尚未初始化，如果我们允许这样的访问，则程序很可能会崩溃。

> 如果构造函数或析构函数调用了某个虚函数，则我们应该执行与构造函数或析构函数所属类型相对应的虚函数版本。

## 继承的构造函数

在C++11新标准中，派生类能够重用其直接基类定义的构造函数。
尽管如我们所知，这些构造函数并非以常规的方式继承而来，但是为了方便，我们不妨姑且称其为“继承”的。
一个类只能初始化它的直接基类，出于同样的原因，一个类也只继承其直接基类的构造函数。
类不能继承默认、拷贝和移动构造函数。
如果派生类没有直接定义这些构造函数，则编译器将为派生类合成它们。

派上类继承基类构造函数的方式是提供一条注明了（直接）基类名的using声明语句。

```C++
class Bulk_quote : public Disc_quote
{
public:
    using Disc_quote::Disc_quote; //继承Disc_quote的构造函数
    double net_price(std::size_t) const;
};
```

通常情况下，using声明语句只是令某个名字在当前作用域内可见。而当作用于构造函数时，using声明语句将令编译器产生代码。对于基类的每个构造函数，编译器都生成一个与之对应的派生类构造函数。
换句话说，对于基类的每个构造函数，编译器都在派生类生成一个形参列表完全相同的构造函数。

```C++
derived(params) : Base(args) {}
```

args将派生类构造函数的形参传递给基类的构造函数。

如果派生类含有自己的数据成员，则这些成员将被默认初始化。

### 继承的构造函数的特点

和普通成员的using声明不一样，一个构造函数的using声明不会改变该构造函数的访问级别。

而且，一个using声明语句不能指定explicit或constexpr。如果基类的构造函数时explicit或者constexpr，则继承的构造函数也拥有相同的属性。

当一个基类构造函数含有默认实参时，这些实参并不会被继承。相反，派生类将获得多个继承的构造函数，其中每个构造函数分别省略掉一个含有默认实参的形参。
例如：如果基类有一个接受两个形参的构造函数，其中第二个形参含有默认实参，则派生类将获得两个构造函数：一个构造函数接受两个形参（没有默认实参），另一个构造函数只接受一个形参，它对应基类中最左侧的没有默认值的哪个形参。

如果基类含有几个构造函数，则除了两个例外情况，大多数时候派生类会继承所有这些构造函数。第一个例外是派生类可以继承一部分构造函数，而为其他构造函数定义自己的版本。如果派生类定义的构造函数与基类的构造函数具有相同的参数列表，则该构造函数将不会被继承。定义在派生类中的构造函数将替换继承而来的构造函数。

第二个例外是默认、拷贝和移动构造函数不会被继承。这些构造函数按照正常规则被合成。继承的构造函数不会被作为用户定义的构造函数来使用，因此，如果一个类只含有继承的构造函数，则它也将拥有一个合成的默认构造函数。
