---
title: ES6学习笔记
categories: 学习
date: 2016-12-06 17:52:31
tags:
---

<!--more-->
# let 和 const

let 是 ES6新增的用来声明局部变量的,只在let命令所在的代码块内有效。

for循环的计数器，就很合适使用let命令。代码如下:
```es6
for (let i = 0; i < 10; i++) {
  // ...
}
console.log(i)
// ReferenceError: i is not defined
```

let命令改变了语法行为,不会发生变量提升,它所声明的变量一定要在声明后使用,否则报错。
