---
title: Python 基础
date: 2018-08-18 15:00:13
tags:
- Python
---

# 缩进

Python 用缩进表示语法结构。。。

约定的话，应该坚持使用 4个空格 的缩进。
千万不要混用tab和空格
设置你的编辑器：tab -> space * 4

优点就是强迫所有人格式化代码，从而达到干净，明晰的效果。
强迫你写出缩进较小的代码，把很长的代码拆分成很多函数。
缺点就是游标卡尺了解一下。
复制粘贴代码的时候会很爽（防止抄代码吗，笑）

# 注释

`#`是注释的符号，该符号后直到行末，都算作注释。

# 数据类型

## 整数 int
和数学上一样的表示法。
十六进制数字用`0x`开头。
```python
42    # 整数。一切的答案。
0xc1  # 十六进制数字。
```
## 浮点数 float

数学写法，或者科学记数法。
`有效数字e指数`表示，e只是符号而已，底数是10。
3e8 就是3x10^8，也可以是30e7。
0.1就是1e-1，指数带上负号。

浮点数运算会有误差哟！

## 字符串 str

字符串是由单引号`'`或者双引号`"`括起来的文本。
引号要成对使用，用来表示字符串，所以不算做字符串的一部分。
`'abc'`，`"abc"`这样，内容就是abc。

使用`\`来转义。
字符串中这么表示引号。
`\'` -> `'`
`\"` -> `"`
表示`\`符号本身。
`\\` -> `\`
特殊字符
`\n` -> 换行符
`\t` -> 制表符
等

### 原生字符串
字符串前缀r，内部的字符不转义。
`r'\something\'`

### 多行字符串

用`'''...'''`格式表示多行内容。

## 布尔值 bool

布尔代数。
布尔值只有：`True`和`False`。

布尔代数算符有：
`and` 与
val and val
`or` 或
val or val
`not` 非
not val

## 空值 NoneType

特殊的值，用None表示。

# 变量

变量直接使用。。。
变量名 = 值
变量名必须是英文字母，数字和下划线`_`的组合，不能以数字开头。

```python
a = 1 # 变量a，值为1
b_b = 'wow' # 变量b_b，值为wow
bb = True # 变量bb，值为True
```

=是赋值，和数学里的等号不一样。
赋值是指擦除原值，用新的值覆盖。

变量接受任意数据类型，同一个变量可以反复赋值，而且类型可以不同。

```python
a = 42
a = True
```

变量类型不固定，所以这是个动态语言。
更加灵活，不过有时候会搞不清变量的类型。

# 常量

Python通常用全部大写的变量名表示常量。
然而这个只是约定。。。不会阻止你改值。。。

# 除法

Python有两种除法

1. `/`精确除法，产生浮点数
2. `//`整数除法，产生整数，缩进过

# 格式化

将变量变成字符串。

字符串 % 值
字符串 % (值,值,...)

语法与C语言一致。

还有就是字符串的format方法
'str'.format(...)
这又是另一套语法

# list

内置数据类型，列表(list)，一种有序集合。

```python
l = [mem, mem, mem]
```

用`len()`函数可以获取元素个数

用索引访问列表中的元素，索引从`0`开始。

```python
l[0]
l[1]
```

索引超出范围时，会报错`IndexError`。

元素操作：
```python
l.append(mem) # 增加到末尾

l.insert(index, mem) # 添加到索引为index的地方

l.pop() # 删除末尾元素

l.pop(index) # 删除指定元素，索引为index
```

list里面的元素的数据类型可以不同

```python
l = [123, True]
```

list可以嵌套

```python
l = [1, [1,2,3], 3]
```

空列表

```python
l = []
len(l) # 0
```

# tuple

元组，一旦初始化就不能修改。

```python
t = (1, 2, 3)
```

tuple不可变，所以更安全。。。

空的tuple

```python
t = ()
```

只有一个元素的tuple，元素后面要加上`,`，不然会歧义。

```python
t = (1,)
```

# 控制流

if cond:
    stmt

if cond:
    stmt
else:
    stmt

if cond:
    stmt
elif cond:
    stmt
else:
    stmt

for mem in container:
    stmt

range函数生成序列

for mem in range(b, e, s):
    stmt

while cond:
    stmt

break退出循环

continue跳过当前迭代

# dict

字典

d = {key:value, key:value}

索引就是key，而且类型任意。

直接索引，如果索引不存在会报错。

判断索引是否存在
```python
key in d
# True or False
```

# set

key的集合，不能重复。

创建set需要输入集合。

```python
s = set([1,2,3])
```

重复元素在set中自动被过滤。

`s.add(key)`添加元素到set中。
`s.remove(key)`删除元素。

