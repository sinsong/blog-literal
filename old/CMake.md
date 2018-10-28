---
title: CMake
date: 2018-05-27 20:01:23
tags:
- CMake
---

# CMake

```bash
cmake [<options>] (<path-to-source> | <path-to-existing-build>)
cmake [(-D <var>=<value>)...] -P <cmake-script-file>
cmake --build <dir> [<options>...] [-- <build-tool-options>...]
cmake --open <dir>
cmake -E <command> [<options>...]
cmake --find-package <options>...
```

`-D <var>:<type>=<value>, -D <var>=<value>`
	创建cmake cache记录。

`-G <generator-name>`
	指定生成器



# CMake构建工具模式

```bash CMake构建工具模式
cmake --build <dir> [<options>...] [-- <build-tool-options>...]
```

`--build <dir>` 
	构建目录。要求必须是第一个参数。

`--target <tgt>`
	构建tgt目标(target)。只能指定一个。

`--clean-first`
	先执行clean目标，然后构建。(只执行clean,使用`--target clean`)

`--`
	剩下的选项传递给原生工具。

一般：
```bash
cmake --build .
```

# CMake 命令行工具模式

```bash
cmake -E <command> [<options>...]
```


