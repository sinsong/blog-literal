---
title: css
date: 2018-06-27 20:16:03
tags:
- Web
- CSS
---

# 样式

## 背景

`background`            背景-简写属性
`background-color`      背景颜色
=#xxxxxx | 颜色名字 | rgb(xxx,xxx,xxx)
=transparent(默认) 透明
=inherit 继承
`background-image`      背景图像
=url('图像url') 声明背景图像
=none(默认) 没有
=inherit 继承
`background-position`   背景图像位置
={top|center|bottom} {left|center(默认)|right}
=x% y% 默认值就是0% 0%
=?px ?px
x%和?px可以混合使用
`background-repeat`     背景图像如何重复
=repeat(默认) 水平，垂直都重复
=repeat-x 水平重复
=repeat-y 垂直重复
=no-repeat 不重复
=inherit 继承
`background-size`       背景图像尺寸
=宽高值，百分比，auto
=cover 覆盖背景区域
=contain 适应内容
`background-origin`     背景图像固定区域
=padding-box 背景图像相对于内边距定位
=border-box 背景图像相对于边框盒定位
=content-box 背景图像相对于内容框定位
`background-clip`       背景图像绘制区域
=border-box 裁剪到边框
=padding-box 才见到内边距框
=content-box 裁剪到内容框
`background-attachment` 背景图像如何固定
=scroll(默认) 随着页面滚动而移动
=fixed 页面滚动，背景图像不会移动
=inherit 继承

## 边框

`border` 4个边框的样式
= 宽度 风格 颜色

`border-{top|bottom|left|right}` 边框上下左右，单独设置
= 宽度 风格 颜色

`botder-{top|bottom|left|right}-{color|style|width}` 上下左右某个边框的某个属性

`outline` 轮廓 位于边框边缘的外围
= 宽度 风格 颜色

`border-radius` 边框圆角
=半径（长度）

`border-{top|bottom}-{left|right}-radius` 单独设置四个圆角

`box-shadow` 边框阴影
=水平位置（必须-允许负值） 垂直位置（必须-允许负值） 模糊距离（可选） 阴影颜色（可选） inset（特值，改为内部阴影）

`border-image` 边框图片-简写属性

`border-image-outset`
=长度|数字 数字对应border-width的倍数。

`border-image-repeat` 如何重复边框
=stretch 拉伸图像来填充
=repeat 平铺，重复图像来填充
=round 缩放

`border-image-slice` 向内偏移
=数字|百分比|fill（特值）

`border-image-source` 边框图像源
=url('图像地址')
=none 不使用图像

### 边框风格 style属性

`none`      无边框
`solid`     实线
`hidden`    隐藏
`dotted`    点状
`dashed`    虚线
`double`    双线
`groove`    3D凹槽
`ridge`     3D垄状
`inset`     3Dinset
`outset`    3Doutset
`inherit`   继承

# 样式次序

1. 浏览器默认设置
2. 外部样式表 `<link rel="stylesheet" src="xxx.css">`
3. 内部样式表（位于`<head>`标签内部`<style>`）
4. 内联样式 `style="xxx"`

# 语法

## css规则

选择器 -> {属性:值;...}

> 值中含有空格时，记得加引号！

## css选择器

### 元素选择器

```css
tag {...}
```

选择某种元素，对所有的这种元素生效。

```css
p{...}
```
对所有段落生效的声明

### 分组 共享

多个选择器共享相同的声明

```css
selector1, selector2, ...
{

}
```

### 类选择器

```css
.class {}
```

具有某个类的任意元素

### 多类选择器

```css
.class1.class2 {}
```

同时具有两个类。

### 元素+类

```css
tag.clsss {}
```

### ID 选择器

```css
#id {}
```

某个id的元素。

> html中id属性必须是唯一的。

### 属性选择器

```css
[attr] {}
```

具有某个属性的所有元素

```css
tag[attr][attr] {}
```
并用，同时具有多个属性的某种标签。

### 后代选择器

```css
tag1 tag2 {}
```

tag1中所有的tag2，包括直接后代和间接后代。

```css
tag1.class1 tag2.class2 {}
```
后代选择器+类约束

### 子元素选择器

```css
tag1 > tag2 {}
```

tag1中所有tag2的子元素，直接后代。

### 相邻兄弟选择器

```css
tag1 + tag2 {}
```

所有的，同一个父元素的，紧跟tag1的tag2。

## 伪类

`:active`   被激活
`:focus`    拥有键盘输入焦点
`:hover`    鼠标悬浮
`:link`     未被访问的链接
`:visited`  已访问的链接
`:first-child` 元素的第一个子元素
`:lang`     带有指定的lang属性(`:lang(zh)`)

## 伪元素

`:before`   元素之前
`:after`    元素之后
`:first-letter` 文本的第一个字母
`:first-line`   文本的第一行