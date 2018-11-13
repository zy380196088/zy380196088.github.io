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

## for of 循环

> 最简洁、最直接的遍历数组元素(或其他集合)的语法
> 与 forEach()不同的是，它可以正确响应 break、continue 和 return 语句

```js
  for (var value of myArray ){
    console.log(value);
  }
```