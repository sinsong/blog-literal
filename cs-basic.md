---
title: C# 基础
date: 2018-10-07 18:55:20
tags:
- C#
---

# using 关键字

## 1.作为引用指令

```cs
using System;
```

## 2.作为别名

```cs
using WinForm = System.Windows.Form;
```

## 3.using 语句

```cs
using (TextWriter w = File.CreateText("test.txt"))
{
    w.WriteLine("Line");
}
```

大括号范围内，using创建的对象有效。

# Main方法

```cs
public static int Main();

public static void Main();
```

默认放在Program.cs的Program类中。

# region注释

```cs
#region 注释。。。

...
...
...

#endregion
```

# 命名规范

* 类名，方法名，属性名，使用Pascal命名法。每个单词第一个字母大写。 GetData...
* 变量名，一般对象名，方法的参数名，使用Camel命名法。第一个单词首字母小写，其余大写。userName...
* 私有字段，可以用下划线开头。_age...

# 控制台应用程序

```cs
//输出
Console.Write()         //输出
Console.WriteLine()     //输出一行

//输入
Console.ReadLine()      //读取一行
Console.ReadKey()       //读取一个按键 void -> ConsoleKeyInfo
```

```cs
// ConsoleKeyInfo

ConsoleKeyInfo key = Console.ReadKey();
            Console.WriteLine("Key:           {0}", key.Key);
            Console.WriteLine("KeyChar:       {0}", key.KeyChar);
            Console.WriteLine("key.Modifiers: {0}", key.Modifiers);
```

例如小写c

Key:       C //大写，描述的是哪个按键
KeyChar:   c //小写，描述的是输入
Modifiers: 0 //描述的是修饰按键

# 格式化输出

## 1.格式化的一般形式

用{}表示格式化。

```
{N[, M][: 格式码]}
```
中括号表示可选项

N    ：输出序号，从0开始，表示后面的传参
M    ：参数输出的最小长度，少的补齐空格，多了就按实际长度输出；如果M为负，左对齐，M为正，右对齐。未指定，默认为0.
格式码：可选的格式化代码的字符串


## 2.格式化的函数

string.Format

```cs
string.Format("格式串", 参数) -> string
```

如果是一个变量，用ToString更简单

```cs
obj.ToString("格式串");
```

```cs
C       金额形式输出
D或者d  十进制整数。后接数字表示输出位数，不够补0
F或者f  小数点后固定位数，F后接数字指定位数，默认为2
N或者n  整数部分每3位用逗号分隔；小数点后固定位数（四舍五入）N后面默认位两位
P或者p  百分比形式输出。整数部分每3位用逗号分隔；小数点后固定位数（四舍五入），P后接数字默认两位
X或者x  十六进制格式，X后接数字表示输出位数，不够补0
0       0占位符，如果数字位数不够指定的占位符位数，左边补0，超过原样输出。小数部分超过的四舍五入。
#       #占位符。整数部分，去掉数字左边的无效0；小数部分，四舍五入后，去掉右边的无效0.
```

特殊用法：如果格式中要用到大括号，用两个连续的大括号表示一个大括号。{{,}}

## 3.其他类型与字符串

type_class.Parse() //从字符串解析
obj.ToString()     //格式化为字符串

# Windows 窗体应用程序

Main方法使用Application类提供的静态方法来启动、退出应用程序。
Run方法：当前线程上启动应用程序消息循环，并显示窗体。
Exit方法：用于停止应用程序消息循环，即退出应用程序。

```cs
static class Program
{
    static void Main()
    {
        Application.Run(new Form1());
    }
}
```

Show()将窗体显示出来后立即返回，不会阻止用户与应用程序中的其他窗口交互。称为无模式窗口。
ShowDialog()阻止用户与其他窗口交互，并且仅在窗体关闭后，该函数才退出，称为模式窗口。

Hide()隐藏打开的窗口。隐藏后可调用Show方法重新显示窗口。

Close()关闭窗口。

Application.Exit()退出应用程序。

# 消息框

```cs
MessageBox.Show(string text, string caption, MessageBoxButtons buttons, MessageBoxIcon, icon);
text:消息框中显示的文本
caption:消息框的标题栏中显示的文本
buttons:消息框中的按钮
icon:消息框中的图标
```
