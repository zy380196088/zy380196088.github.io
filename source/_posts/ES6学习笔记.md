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

# for of 循环

> 最简洁、最直接的遍历数组元素(或其他集合)的语法
> 与 forEach()不同的是，它可以正确响应 break、continue 和 return 语句

```js
  for (var value of myArray ){
    console.log(value);
  }
```

# 模板字符串 - 反撇号(`)

> 如果你需要在模板字符串中引入字符 **$** 和 **{**。无论你要实现什么样的目标， 你都需要用反斜杠转义每一个字符:`\$`和`\{`


# 生成器 Generators

```js
function* GetUp(time){
  yield "现在是" + time + "!";
  yield "赶快洗漱,准备出门!";
  if( (new Date()).getHours() > time){
    yield "快迟到了,没时间刷牙洗脸了,赶紧出门!";
  }else{
    yield "正在刷牙洗脸..."
  }
  yield "OK,出门咯!";
}
var getUp = GetUp(8);
```

> 普通函数使用 function 声明, 而生成器函数使用function * 声明.
> 普通函数和生成器函数之间最大的区别是 普通函数不能自暂停, 生成器函数可以;
> **yield** 关键字用来暂停和继续执行一个生成器函数。当外部调用生成器的 next() 方法时，yield 关键字右侧的表达式才会执行
> 普通函数只可以return 一次, 而生成器函数可以 yield 多次
> 每当生成器执行 yields 语句，生成器的堆栈结构(本地变量、 参数、临时值、生成器内部当前的执行位置)被移出堆栈。然而，生成器对象保留了对 这个堆栈结构的引用(备份)，所以稍后调用.next()可以重新激活堆栈结构并且继续执 行。