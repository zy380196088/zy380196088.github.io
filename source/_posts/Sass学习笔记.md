---
title: Sass学习笔记
date: 2016-05-09 23:00:04
tags: [Sass]
---

## Sass语法格式
### 变量
SASS允许使用变量，所有变量以$开头。
如果变量需要镶嵌在字符串之中，就必须需要写在#{}之中。
例如:
```scss
$side : left;
.rounded {
	border-#{$side}-radius: 5px;
}
```
<!--more-->
CSS代码:
```css
body {
  font: 100% Helvetica, sans-serif;
  color: #333;
}
```
Sass语法格式来编写:
```scss
$font-stack: Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

SCSS语法格式来编写:
```scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

### 计算
SASS允许在代码中使算式:
```scss
body {
　　　　margin: (14px/2);
　　　　top: 50px + 100px;
　　　　right: $var * 10%;
　　}
```
### 嵌套
下面css代码:
```css
div a{
	color:red;
}
```
可以写成:
```scss
div{
	hi{
	color:red;
	}
}
```
还支持属性嵌套(**属性后必须加上冒号**):
```scss
div{
	border: {
		color:red;
	}
}
```

## 代码重用
### 继承@extend
ASS允许一个选择器，继承另一个选择器。比如，现有box1:
```scss
.box1{
	border:1px sold #fff;
}
```
box2要继承box1,需要使用@extend命令:
.box2{
	@extend .box1;	
	background-color: red;
}

### Mixin@include
Mixin有点像C语言的宏（macro），是可以重用的代码块。
```scss
@mixin left{
	float: left;
	margin-left:10px;
}
```
使用@include命令调用
```scss
div{
	@include left;
}
```
更强大的用法，可以指定参数和缺省值.
```scss
@mixin left($valuse:10px){
	float:left;
	margin-right:$value;
}
//使用的时候加上参数即可
div{
	@include left(20px);
}
```
一个有用的mixin实例，用来生成浏览器前缀:
```scss
@mixin rounded($vert, $horz, $radius: 10px) {
　　　　border-#{$vert}-#{$horz}-radius: $radius;
　　　　-moz-border-radius-#{$vert}#{$horz}: $radius;
　　　　-webkit-border-#{$vert}-#{$horz}-radius: $radius;
　　}
```
### 插入文件@import
```scss
@import "path/filename.scss"
```

## 高级用法
### 条件语句@if
还可以配合使用@else

### 循环语句
#### @for
```scss
@for $i from 1 to 10 {
　　　　.border-#{$i} {
　　　　　　border: #{$i}px solid blue;
　　　　}
　　}
```

#### @while
```scss
$i: 6;
　　@while $i > 0 {
　　　　.item-#{$i} { width: 2em * $i; }
　　　　$i: $i - 2;
　　}
```

#### @each
```scss
@each $member in a, b, c, d {
　　　　.#{$member} {
　　　　　　background-image: url("/image/#{$member}.jpg");
　　　　}
　　}
```

### 自定义函数
```scss
@function double($n) {
　　　　@return $n * 2;
　　}
　　#sidebar {
　　　　width: double(5px);
　　}
```