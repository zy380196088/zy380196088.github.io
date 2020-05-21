---
title: Flex 布局
categories: 学习
date: 2017-04-19 15:39:37
tags:
---

<!--more-->

学习 FLex, 首先我们来了解几个相关概念:

# 容器

容器具有这样的特点：父容器可以统一设置子容器的排列方式，子容器也可以单独设置自身的排列方式，如果两者同时设置，以子容器的设置为准.

## 父容器 container

### justify-content

用于定义如何沿着主轴方向排列子容器.

#### 位置排列

>flex-start 起始端对齐
>flex-end 末尾端对齐
>center 居中对齐

#### 分布排列

>space-between 子容器沿主轴均匀分布，位于首尾两端的子容器与父容器相切。
>space-around 子容器沿主轴均匀分布，位于首尾两端的子容器到父容器的距离是子容器间距的一半。

### align-items

属性用于定义如何沿着交叉轴方向分配子容器的间距

#### 位置排列 flex-start flex-end center

>flex-start
>flex-end
>center

#### 基线排列 baseline

基线对齐,默认指首行文字.

#### 拉伸排列 stretch

子容器沿交叉轴方向的尺寸拉伸至与父容器一致.

### flex-wrap 设置换行方式

#### flex-wrap:nowrap 不换行

#### flex-wrap:wrap 换行

#### flex-wrap:wrap-reverse 逆序换行

逆序换行是指沿着交叉轴的反方向换行。

### flex-flow 轴向与换行组合设置

### flex-content 多行沿交叉轴对齐

## 子容器 item

### flex

子容器是有弹性的（flex 即弹性），它们会自动填充剩余空间，子容器的伸缩比例由 flex 属性确定.
flex 的值可以使无单位数字(1,2,3...),也可以是带单位的(100px,200px...)
还可以为 none,设置为 none 的话不会伸缩.

### align-self

每个子容器也可以单独定义沿交叉轴排列的方式，此属性的可选值与父容器 align-items 属性完全一致，如果两者同时设置则以子容器的 align-self 属性为准。

# 轴

## 主轴 main axis

主轴的起始端由 flex-start 表示，末尾段由 flex-end 表示。不同的主轴方向对应的起始端、末尾段的位置也不相同。

### flex-direction

> flex-direction:row 向右
> flex-direction: column 向下
> flex-direction: row-reverse 向左
> flex-direction: column-reverse 向上

## 交叉轴 cross axis
主轴沿逆时针方向旋转 90° 就得到了交叉轴，交叉轴的起始端和末尾段也由 flex-start 和 flex-end 表示。

# 居中
使用 flex 居中布局首先要设置父容器:
``display:flex;``
设置 ``justify-content:center;``实现水平居中;
再设置 ``align-items:center;``实现垂直居中;

代码示例:
```CSS
#dad{
    display:flex;
    justify-content:center;
    align-items:center;
}
#son{
    width:50%;
    height:50%;
}
```
