---
title: bash
date: 2018-08-26 15:25:38
tags:
- bash
---

# 定义

blank空白
空格或者制表符

word单词
字符序列被shell认为是一个独立的单元

name
一个word只包含字母数字字符和下划线，以字母或者一个下划线开头。也指identifier标识符。

metacharacter元字符
一个字符，未引用的，分离单词。
有：`|` `&` `;` `(` `)` `<` `>` `space` `tab` `newline`

control operator控制操作符
一个词元提供一个控制功能。
有：`||` `&` `&&` `;` `;;` `;&` `;;&` `(` `)` `|` `|&` `<newline>`

# 保留字

保留字是一些在shell里含有特殊意义的words。
有：! case coproc do done elif else esac fi for function if in select then until while { } time [[ ]]]

