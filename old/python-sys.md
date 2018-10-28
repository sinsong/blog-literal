---
title: Python sys
date: 2018-08-14 22:39:47
tags:
- Python
---

Module:sys

提供对解释器使用或提供的一些对象的访问。

动态对象：

argv
命令行参数；argv[0]是已知脚本路径

path
模块查找路径；path[0]是脚本目录，或者''

modules
已加载脚本的目录

displayhook
调用，用来显式结果，在一个解释器的会话中。

excepthook
调用，用来处理任何未捕获异常。去自定义打印。。。

stdin
标准输入文件对象；input()使用的

stdout
标准输出文件对象；print()使用的

stderr
标准错误对象；错误信息使用的

last_type
最后一个未捕获异常的类型

last_value
最后一个未捕获异常的值

last_traceback
最后一个未捕获异常的跟踪

静态对象：

builtin_module_names
内置进解释器名字的元组

copyright
这个解释器的版权声明

exec_prefix
寻找指定机器Python库的前缀

executable
Python解释器二进制可执行文件的绝对路径

float_info
一个序列结构，包含浮点实现的信息

float_repr_style
字符串指示repr()输出浮点的风格

hash_info
一个结构序列，包含散列算法的信息

hexversion
版本信息，加密到一个证书

implementation
Python实现信息

int_info
一个结构序列，包含整数实现信息

maxsize
最大支持的容器长度

maxunicode
最大的Unicode代码点的值

platform
平台指示

prefix
查找Python库的前缀

thread_info
一个结构序列，包含线程实现的信息

version
这个解释器的版本，字符串

version_info
解释器的版本，命名的元组

dllhandle
[仅Windows]Python DLL的整数句柄

winver
[仅Windows]Python Dll版本数字

__stdin__
原始stdin；不要动它！

__stdout__
原始stdout；不要动它！

__stderr__
原始stderr；不要动它！

__displayhook__
原始displayhook；不要动它！

__excepthook__
原始excepthook；不要动它！

函数：

displayhook()
打印一个对象到屏幕，然后保存它到builtins。

excepthook()
打印一个异常和它的跟踪到sys.stderr

exc_info()
返回当前异常的线程安全信息

exit()
退出解释器，使用释放SystemExit

getdlopenflags()
返回dlopen()调用使用的标志

getprofile()
获得全局profiling函数

getrefcount()
返回一个对象的引用计数

getrecursionlimit()
返回解释器的最大递归深度

getsizeof()
返回一个对象的尺寸，用字节

gettrace()
获取全局跟踪函数

setcheckinterval()
控制解释器检查事件的频率

setdlopenflags()
设置dlopen()调用要用的标志

setprofile()
设置全局profiling函数

setrecursionlimit()
设置解释器的最大迭代深度

settrace()
设置全局调试跟踪函数