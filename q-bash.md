---
title: 快速参考：bash
date: 2018-08-25 21:12:30
tags:
- bash
---

# 注释和运行

`#!`后面跟该文件需要的shell或解释器。
`#`后面是注释

```bash
#!/bin/bash
echo "Hello, world!"
```

直接当作可执行程序

```bash
chmod +x b.sh #增加执行权限
./b.sh #执行脚本
```

用参数传给解释器

```bash
/bin/bash b.sh
```

# shell变量

```bash
varname="value"
```

* 变量名和等号之间不能有空格（不然变量名就当作程序名字了）
* 名字只能用英文字母，数字和下划线，不以数字开头。
* 不能使用标点符号。
* 不能使用关键字

## 使用变量

$紧跟变量名即可。
${varname}就行，而且大括号是可选的。
加花括号是好习惯。

## 只读变量

用readonly命令

```bash
readonly varname
```

## 删除变量

用unset命令

```bash
unset varname
```

注意：不能删除只读变量

## 变量类型

1. 局部变量 脚本或命令中定义，仅在当前shell实例中有效。
2. 环境变量 所有的程序，包括shell启动的程序，都能访问环境变量。
3. shell变量 shell程序设置的特殊变量。有局部也有环境的。

# 字符串

可以双引号，可以单引号，也可以不用引号。

## 单引号

```bash
s='string'
```

限制：
* 单引号里的任何字符都会原样输出，其中的变量无效。
* 单引号字符串中不能出现单独一个单引号（使用转义符也不行），但可成对出现，作为字符串拼接使用。

## 双引号

```bash
s="string"
```

优点：
* 双引号里可以有变量
* 双引号里可以出现转义字符

## 拼接字符串

```bash
s="hello, "$varname" !"
s="hello, ${varname} !"

s='hello, '$varname' !'
```

## 获取字符串长度

```bash
str="hehe"
echo ${#str}
```

## 提取子字符串

```bash
str="hehe haha"
echo ${str:1:4}
```

从第二个字符开始截取四个字符

# shell数组

array_name=(value0 value1 value2 ...)

array_name[0]=value0
...

## 读取数组

```bash
${array_name[0]}
```

使用`@`符号可以获取数组中的所有元素

## 获取长度

```bash
# 获取数组元素的个数
l=${#array_name[@]}
l=${#array_name[*]}

# 获取数组中单个元素的长度
l=${#array_name[n]}
```

# 多行注释

```bash
:<<EOF
注释。。。
。。。
。。。
EOF
```

EOF可以使用其他符号：

```bash
:<<'
注释...
...
...
'

:<<!
注释。。。
。。。
。。。
!
```

# Shell 参数

$n n是数字。
$n 就是第n个参数。
$0 是执行文件名字。

./b.sh 1 2 3
$0="./b.sh"
$1="1"
$2="2"
$3="3"

$#
参数的个数

$*
以一个单字符串显式所有的参数。"$1 $2 ... $n"

$$
脚本运行的当前进程ID。PID

$!
后台运行的最后一个进程的ID。

$@
"$1" "$2" ... "$n"

$-
shell使用的当前选项，与set命令功能相同

$?
显式最后命令的退出状态

