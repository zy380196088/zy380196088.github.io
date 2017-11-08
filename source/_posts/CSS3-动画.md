---
title: CSS3动画
date: 2016-04-22 11:48:56
tags: css3
---

CSS3动画的属性主要分为三类：transform,transition,animation.

<!--more-->

## transform
### rotate()
设置元素顺时针旋转的角度，参数x必须是以deg结尾的角度数或0,可为负数表示反向：
```css
transform:rotate(x);
```

### scale()
设置元素放大或者缩小的倍数，用法：
```css
transform: scale(a);      /*元素x和y方向均缩放a倍*/
transform: scale(a, b);   /*元素x方向缩放a倍，y方向缩放b倍*/
transform: scaleX(a);     /*元素x方向缩放a倍，y方向不变*/
transform: scaleY(b);     /*元素y方向缩放b倍，x方向不变*/
```
### translate()
设置元素的位移，用法：
```css
transform: translate(a, b);  /*元素x方向位移a，y方向位移b*/
transform: translateX(a);    /*元素x方向位移a，y方向不变*/
transform: translateY(b);    /*元素y方向位移b，x方向不变*/
```
### skew()
设置元素倾斜的角度，参数均必须是以deg结尾的角度数或0，可为负数表示反向
：
```css
transform: skew(a,b);  /*元素x方向逆时针倾斜角度a，y方向顺时针倾斜角度b*/
transform: skewX(a);   /*元素x方向逆时针倾斜角度a，y方向不变*/
transform: skewY(b);   /*元素y方向顺时针倾斜角度b，想方向不变*/
```
### origin
设置元素的悬挂点，元素的悬挂点即为它旋转和倾斜时的中心点。取值中的a、b可以是长度值、以%结尾的百分比或者left、top、right、bottom四个值。用法：
```css
transform-origin: a b;  /*元素的悬挂点为(a, b)*/
```
### matrix
设置元素的变形矩阵,有点复杂，详情参考：
[理解CSS3 transform中的Matrix(矩阵)](http://www.zhangxinxu.com/wordpress/2012/06/css3-transform-matrix-矩阵/comment-page-2/)

## transition
### transition-property
指定transition效果作用的CSS属性

### transition-duration
动画效果持续的时间，值为以s结尾的秒数

### transition-timing-function
指定元素状态的变化速率函数，取值基于贝赛尔曲线函数：
![](/images/css3-transition-timing-function.gif)

### transition-delay
动画效果推迟开始执行的时间，其值为以s结尾的秒数。
CSS3动画的生命周期如下图所示，从中可以清楚的看出duration和delay之间的关系：
![](/images/css3-transition-delay.png)

## animation
CSS3中真正的动画属性是animation，而前面的transform和transition都只是对DOM元素的变形或者是状态的过渡。实际上，CSS3所支持的动画效果只是填充动画，也就是说先设定整个动画生命周期中的几个关键状态（key  frame，关键帧），然后动画将自行计算并模拟关键帧之间的过渡。那么在设置animation的属性之前就必须先设定好关键帧了。
关键帧@keyframes的语法结构如下：
```css
@keyframesNAME {
         a% {
         /*CSS属性*/
         }
         b% {
        /*CSS属性*/
         }
         ...
}
```

NAME表示动画的名字；a%、b%表示以百分号结尾的百分数，用于设定该关键帧在动画生命周期中的位置；百分数后面的{ } 中则需要写成该关键帧状态下CSS属性的值。另外，如果同一个百分数值在@keyframes中出现多次，那么后出现的将覆盖先出现的；并且关键帧在@keyframes中时无序的。
设置完关键帧后就可以继续设定animation了。
### animation-name

### animation-duration

### animation-timing-function

### animation-delay

### animation-iteration-count
设定动画执行的次数，其值可以是数字也可以是infinite（循环执行）。

### animation-direction
设定动画执行的方向，其值可以是normal（正常顺序播放）或alternate（反向播放）。

