---
title: 移动端web开发Meta标签
date: 2016-06-27 20:11:33
tags: [移动端,Meta]
---

## 关闭自动识别数字串为电话链接
关闭iPhone上Safari(一些其他基于webkit Android手机浏览器)自动将数字串识别为电话链接.
```html
<meta name="format-detection" content="telephone=no"/>
```
如果关闭自动识别后,又希望某些电话能够被识别,可以通过如下声明
```html
<a href="tel:18500888800">18500888800</a>
```
##  关闭邮箱识别
加上下面代码将不识别邮箱
```html
<meta name="format-detection" content="email=no"/>
```
## 删除默认的苹果工具栏和菜单
```html
<meta name="apple-mobile-web-app-capable" content="yes"/>
```
如果需要显示时,不加上条meta即可,默认为显示.

```html
<meta name=”apple-mobile-web-app-status-bar-style” content=black” /> 
```
默认值为default（白色），可以定为black（黑色）和black-translucent（灰色半透明）。
注意： 若值为“black-translucent”将会占据页面px位置，浮在页面上方（会覆盖页面20px高度–iphone4和itouch4的Retina屏幕为40px)。

## 在手机HOME界面创建应用程序样式的图标

```html
<!-- for IOS-->
<link rel="apple-touch-icon" href="/static/images/xxx.png">
<!-- for Android-->
<link rel="apple-touch-icon-precomposed" href="/static/images/xxx.png">
```
