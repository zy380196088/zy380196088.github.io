---
title: 学习笔记-Javascript高级程序设计
date: 2016-05-09 18:22:00
tags: [学习笔记,JavaScript]
---

## JavaScript模块化编程的重要性

1. 出现大量的全局变量
JS在每个地方都可以定义全局变量,容易造成变量污染,程序将难以维护.

2. JS 加载顺序要按照代码的依赖顺序
3. html一次性加载过多的 JS 脚本页面容易出现假死


## window.location.href的用法(动态输出跳转)

### self.location.href = "/url"
当前页面打开 URL 页面

### location.href = "/url"
当前页面打开 URL 页面
 
### windows.location.href = "/url"
当前页面打开 URL 页面

### this.location.href="/url"
当前页面打开URL页面

### parent.location.href="/url" 
在父页面打开新页面

### top.location.href="/url"
在顶层页面打开新页面

## JavaScript 数据类型


### 5种简单数据

## JS中的内部属性与delete操作符介绍
在讲解Configurable之前，我们首先来看一道面试题：
```JS
a = 1;
console.log( window.a ); // 1
console.log( delete window.a ); // true
console.log( window.a ); // undefined

var b = 2;
console.log( window.b ); // 2
console.log( delete window.b ); // false
console.log( window.b ); // 2
```
从上面的这道题可以看出两个的区别：在没有使用var声明变量时，使用delete关键词是可以进行删除的，再次获取时值就是undefined了；在使用var声明的变量，使用delete是不能删除的，再获取时值依然是2。

### delete操作符
使用delete删除变量或属性时，删除成功返回true，否则返回false。如上面的例子中，delete无法删除变量a时，则返回false；而delete能成功删除变量b，则返回true。

除了上述的两种情况，还有其他的各种常用变量也有能被delete删除的，也有不能被删除的。我们先不管delete这些变量时，为什么会产生这样的结果，这里只看他的返回值：

删除delete数组中其中的一个元素：
```JS
// 使用for~in是循环不到的，直接忽略到该元素
// 使用for()可以得到该元素，但是值是undefined
var arr = [1, 2, 3, 4];
console.log( arr );         // [1, 2, 3, 4]
console.log( delete arr[2] );   // true，删除成功
console.log( arr );         // [1, 2, undefined, 4]
```
删除function类型的变量：
```JS
// chrome 不能删除；火狐可以删除
function func(){
}
console.log( func );
console.log( delete func );
console.log( func );
```
删除function.length，该length是获取形参的个数：
```JS
function func1(a, b){
}
console.log( func1.length );    // 2
console.log( delete func1.length ); // true，删除成功
console.log( func1.length );    // 0
```
删除常用变量：
```js
console.log( delete NaN );      // false，删除失败
console.log( delete undefined );// false
console.log( delete Infinity ); // false
console.log( delete null );     // true，删除成功
```
删除prototype，而不是删除prototype上的属性：
```js
function Person(){
}
Person.prototype.name = "蚊子";
console.log( delete Person.prototype ); // false，无法删除
console.log( delete Object.prototype ); // false
```
删除数组和字符串的length时：
```js
var arr = [1, 2, 3, 4];
console.log( arr.length );      // 4
console.log( delete arr.length );   // false，删除失败
console.log( arr.length );      // 4

var str = 'abcdefg';
console.log( str.length );      // 7
console.log( delete str.length );   // false，删除失败
console.log( str.length );      // 7
```
删除obj中的属性时：
```js
var obj = {name:'wenzi', age:25};
console.log( obj.name );    // wenzi
console.log( delete obj.name ); // true，删除成功
console.log( obj.name );    // undefined
console.log( obj );         // { age:25 }
```
删除实例对象中的属性时，从以下的输出结果可以看出，使用delete删除属性时，删除的仅仅是实例对象本身的属性，而不能删除prototype上的属性，即使再删一次也是删除掉不的；若要删除prototype上的属性的属性或方法，只能是：delete Person.prototype.name：
```js
function Person(){
  this.name = 'wenzi';
}
Person.prototype.name = '蚊子';
var student = new Person();
console.log( student.name );    // wenzi
console.log( delete student.name ); // true，删除成功
console.log( student.name );    // 蚊子
console.log( delete student.name ); // true
console.log( student.name );    // 蚊子
console.log( delete Person.prototype.name );// true，删除成功
console.log( student.name );    // undefined
```

### js的内部属性
在上面的例子中，有的变量或属性能够删除成功，而有的变量或属性则无法进行删除，那是什么决定这个变量或属性能不能被删除呢。

ECMA-262第5版定义了JS对象属性中特征（用于JS引擎，外部无法直接访问）。ECMAScript中有两种属性：数据属性和访问器属性。

#### 数据属性

数据属性指包含一个数据值的位置，可在该位置读取或写入值，该属性有4个供述其行为的特性：

[[configurable]]:表示能否使用delete操作符删除从而重新定义，或能否修改为访问器属性。默认为true;
[[Enumberable]]:表示是否可通过for-in循环返回属性。默认true;
[[Writable]]:表示是否可修改属性的值。默认true;
[[Value]]:包含该属性的数据值。读取/写入都是该值。默认为undefined；如上面实例对象Person中定义了name属性，其值为'wenzi',对该值的修改都反正在这个位置
要修改对象属性的默认特征（默认都为true)，可调用Object.defineProperty()方法，它接收三个参数：属性所在对象，属性名和一个描述符对象（必须是：configurable、enumberable、writable和value，可设置一个或多个值）。

如下：
```js
var person = {};
Object.defineProperty(person, 'name', {
  configurable: false,  // 不可删除，且不能修改为访问器属性
  writable: false,      // 不可修改
  value: 'wenzi'            // name的值为wenzi
});
console.log( person.name);          // wenzi
console.log( delete person.name );  // false，无法删除
person.name = 'lily';
console.log( person.name );         // wenzi
```
可以看出，delete及重置person.name的值都没有生效，这就是因为调用defineProperty函数修改了对象属性的特征；值得注意的是一旦将configurable设置为false，则无法再使用defineProperty将其修改为true（执行会报错：Uncaught TypeError: Cannot redefine property: name）;

#### 访问器属性

它主要包括一对getter和setter函数，在读取访问器属性时，会调用getter返回有效值；写入访问器属性时，调用setter，写入新值；该属性有以下4个特征：

[[Configurable]]:是否可通过delete操作符删除重新定义属性；
[[Numberable]]:是否可通过for-in循环查找该属性；
[[Get]]:读取属性时自动调用，默认：undefined;
[[Set]]:写入属性时自动调用，默认：undefined;
访问器属性不能直接定义，必须使用defineProperty()来定义，如下：
```js
var person = {
  _age: 18
};
Object.defineProperty(person, 'isAdult', {
    Configurable : false,
  get: function () {
    if (this._age >= 18) {
      return true;
    } else {
      return false;
    }
  }
});
console.log( person.isAdult ); // true
```
不过还是有一点需要额外注意一下，Object.defineProperty()方法设置属性时，不能同时声明访问器属性（set和get）和数据属性（writable或者value）。意思就是，某个属性设置了writable或者value属性，那么这个属性就不能声明get和set了，反之亦然。

如若像下面的方式进行定义，访问器属性和数据属性同时存在：
```js
var o = {};
Object.defineProperty(o, 'name', {
  value: 'wenzi',
  set: function(name) {
    myName = name;
  },
  get: function() {
    return myName;
  }
});
```
上面的代码看起来貌似是没有什么问题，但是真正执行时会报错，报错如下：
```
Uncaught TypeError: Invalid property. A property cannot both have accessors and be writable or have a value
```
对于数据属性，可以取得：configurable,enumberable,writable和value；

对于访问器属性，可以取得：configurable,enumberable,get和set。

由此我们可知：一个变量或属性是否可以被删除，是由其内部属性Configurable进行控制的，若Configurable为true，则该变量或属性可以被删除，否则不能被删除。

可是我们应该怎么获取这个Configurable值呢，总不能用delete试试能不能删除吧。有办法滴！！

#### 获取内部属性

ES5为我们提供了Object.getOwnPropertyDescriptor(object, property)来获取内部属性。

如：
```js
var person = {name:'wenzi'};
var desp = Object.getOwnPropertyDescriptor(person, 'name'); // person中的name属性
console.log( desp );    // {value: "wenzi", writable: true, enumerable: true, configurable: true}
```
通过Object.getOwnPropertyDescriptor(object, property)我们能够获取到4个内部属性，configurable控制着变量或属性是否可被删除。这个例子中，person.name的configurable是true，则说明是可以被删除的：
```js
console.log( person.name );         // wenzi
console.log( delete person.name );  // true，删除成功
console.log( person.name );         // undefined
```
我们再回到最开始的那个面试题：
```js
a = 1;
var desp = Object.getOwnPropertyDescriptor(window, 'a');
console.log( desp.configurable );   // true，可以删除

var b = 2;
var desp = Object.getOwnPropertyDescriptor(window, 'b');
console.log( desp.configurable );   // false，不能删除
```
跟我们使用delete操作删除变量时产生的结果是一样的。



