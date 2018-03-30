---
title: SVG Notes
date: 2018-03-27 17:20:17
tags: [ SVG ]
categories:
---

# 基本形状
## 线段<line/>
### stroke-width: 线条粗细
### stroke: 线条填充颜色
> rgba()
> hsl()
> hsla()
> transparent == rgba(0,0,0,0)
### stroke-opacity: 线条的不透明度
### stroke-dasharray: 虚线段长度

<!--more-->

## 矩形 <rect/>
```html
<rect x="10" y="50" width="200" height="50" style="fill:#fff;stoke:red;fill-opacity:0.5;"/>
```
## 圆角矩形
```html
<rect x="10" y="10" width="200" height="50" rx="2" ry="2" style="stoke:black;fill:none;"/>
```
> PS:CSS 中 border-radius 可以通过百分比来设置圆角, SVG通过百分比来制定圆角半径的值,并不是相对矩形本身的宽高的百分比,而是相对视口的宽高的百分比;

## 圆 和 椭圆 <circle/>
```html
<circle cx="30" cy="30" r="20" style="stoke:black;fill:none;"/>
```

## 多邊形 <polygon/>
```html
<polygon points="48 16, 16 96, 96 48, 0 48, 80 96"/> 
```
### 填充波阿边线交叉的多边形
```html
<polygon points="48,16 16,96 96,48 0,48 80,96" style="fill-rule:nonzero;fill:yellow;stroke:black;"/>
<polygon points="48,16 16,96 96,48 0,48 80,96" style="fill-rule:evenodd;fill:yellow;stroke:black;"/> 
```
## 折线 <polyline/>
## 线帽和线连接 
### stroke-linecap 
该属性用来指定线的头尾形状(butt/round/square),默认 butt
### stroke-linejoin 
该属性用来指定线段在图形棱角处交叉时的效果(miter尖的/round 圆的/ bevel 平的)

# 文档结构
## SVG样式
### 内联样式
### 内部样式表
### 外部样式表
### 表现属性

## 分组和引用对象
### <g></g>元素
将其所有子元素作为一个组合,附带唯一一个 id 作为名称;
```html
<g id="house" style="fill:none;stroke:black;">
  <desc>House</desc>
  <rect x="6" y="50" width="60" height="60"/>
  <polyline points="6 50,36 9,66 50"/>
  <polyline points="36 110, 36 80, 50 80, 50 110"/>
</g>
```

### <use/>元素
重用 <g>元素内的组合;
```html
<use xlink:href="#house" x="80" y="100"/>
```

### <defs></defs>元素
模板

### <symbol></symbol>元素
#### viewBox 
#### preserveAspectRatio

### <image/>元素
```html
<image xlink:href="xxxx.jpg" x="40" y="100" width="100" height="100"/>
```

# 坐标系统变换

## translate(x,y)
按照指定的 x和 y值移动用户坐标系统,若果没有指定 y的值默认为0.
```html
<use xlink:href="#house" transorm="translate(50,50)"/>
```

## scale(x,y)
使用指定的 x 和 y 乘以所有的用户坐标系统,比例值可以使小数或负数.

## 变换序列
将多个便哈通过空格或逗号分隔,一次放入 trasnform 属性即可;

## 笛卡尔坐标系统转换

## rotate(angle,centerX,centerY) 
按照指定 angele 旋转用户坐标,旋转中心有 centerX 和 centerY指定,无 centerX 和 centerY默认为(0,0)

## 围绕中心点缩放

## skewX(angle) 和 skewY(angle)
根据指定的 angle 倾斜 x 坐标或 y 坐标

## matrix(a b c d e f)
指定一个六个值组成的矩阵变换.

# 路径

## moveto , lineto , closepath ( M , L , Z)
每个路径都必须以 moveto 命令开始,命令字母为大写的 M; 
```html
<path d="M 10 10 L 100 10" />
```
## 相对 moveto 和 lineto ( m , l)
如果使用小写的字母启动路径,它的坐标会被解析为相对位置;
closepath 命令没有坐标,大小写效果相同;

## 路径的快捷方式 
### 水平 lineto ( H , h)
> H 20 => L 20 current_y 
> h 20 => l 20 0
### 垂直 lineto ( V , v)
> V 20 => L current_x 20
> v 20 => l 0 20

## 椭圆弧