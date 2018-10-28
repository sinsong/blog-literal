---
title: Python 异常处理
date: 2018-08-18 20:52:47
tags:
- Python
---

# try

```python
try:
    stmt
except ExceptionClass as e:
    stmt # 使用e
finally:
    stmt
```

我们觉得可能发生异常，写进try块。
若有异常，执行except捕获异常。
except执行完以后，如果有finally，则执行。

所有的错误类型都继承自BaseException。
要注意，子类也会被捕获。

# 抛出错误

```python
raise ExceptionClass(...)
```

except中可以写raise重新抛出。

```python
try:
    stmt
except ExceptClass as e:
    stmt
    raise # 重抛
except ExceptClass as e:
    raise ExceptionClass(...) # 重抛为别的异常。
```

