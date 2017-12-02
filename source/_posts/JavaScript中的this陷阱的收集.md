---
title: JavaScript中的this陷阱的收集
date: 2016-04-15 18:01:03
tags: [css,html]
---
本文摘阅自[JavaScript中的this陷阱的最全收集--没有之一](https://segmentfault.com/a/1190000002640298)
<!--more-->
## global this
在浏览器里，在全局范围内，this等价于window对象。
```javascript
<script type="text/javascript">
console.log(this === window); //true
</script>
```

在浏览器里，在全局范围内，用var声明一个变量和给this或者window添加属性是等价的。
```javascript
<script type="text/javascript">
     var foo = "bar";
     console.log(this.foo); //logs "bar"
     console.log(window.foo); //logs "bar"
</script>
```

如果你在声明一个变量的时候没有使用var或者let(ECMAScript 6),你就是在给全局的this添加或者改变属性值。
```javascript
<script type="text/javascript">
      foo = "bar";

      function testThis() {
        foo = "foo";
      }
     console.log(this.foo); //logs "bar"
     testThis();
     console.log(this.foo); //logs "foo"
</script>
```

在node环境里，如果使用REPL(Read-Eval-Print Loop，简称REPL:读取-求值-输出,是一个简单的，交互式的编程环境)来执行程序,this并不是最高级的命名空间，最高级的是global.
```
> this
{ ArrayBuffer: [Function: ArrayBuffer],
  Int8Array: { [Function: Int8Array] BYTES_PER_ELEMENT: 1 },
  Uint8Array: { [Function: Uint8Array] BYTES_PER_ELEMENT: 1 },
  ...
> global === this
true

```
在node环境里，如果执行一个js脚本，在全局范围内，this以一个空对象开始作为最高级的命名空间，这个时候，它和global不是等价的。
```
test.js脚本内容：
console.log(this);
console.log(this === global);

REPL运行脚本：
$ node test.js
{}
false
```
在node环境里，在全局范围内，如果你用REPL执行一个脚本文件，用var声明一个变量并不会和在浏览器里面一样将这个变量添加给this。
```
test.js:

var foo = "bar";
console.log(this.foo);

$ node test.js
undefined
```
但是如果你不是用REPL执行脚本文件，而是直接执行代码，结果和在浏览器里面是一样的(神坑)
```
> var foo = "bar";
> this.foo
  bar
> global.foo
  bar
```

在node环境里，用REPL运行脚本文件的时候，如果在声明变量的时候没有使用var或者let，这个变量会自动添加到global对象，但是不会自动添加给this对象。如果是直接执行代码，则会同时添加给global和this.
```
test.js
 foo = "bar";
 console.log(this.foo);
 console.log(global.foo);

$ node test.js
  undefined
  bar
```

上面的八种情况可能大家已经绕晕了，总结起来就是：在**浏览器**里面**this**是老大，它等价于window对象，如果你声明一些全局变量(不管在任何地方)，这些变量都会作为this的属性。在node里面，有两种执行JavaScript代码的方式，一种是直接执行写好的**JavaScript**文件，另外一种是直接在里面执行**一行行代码**。对于直接运行一行行JavaScript代码的方式，global才是老大，this和它是等价的。在这种情况下，和浏览器比较相似，也就是声明一些全局变量会自动添加给老大global，顺带也会添加给this。但是在node里面直接脚本文件就不一样了，你声明的全局变量不会自动添加到this，但是会添加到global对象。所以相同点是，在全局范围内，全局变量终究是属于老大的。

## function this
无论是在浏览器环境还是node环境， 除了在DOM事件处理程序里或者给出了thisArg(接下来会讲到)外，如果不是用new调用，在函数里面使用this都是指代全局范围的this。
```javascript
<script type="text/javascript">
    foo = "bar";
    function testThis() {
    	this.foo = "foo";
    }
    console.log(this.foo); //logs "bar"
    testThis();
    console.log(this.foo); //logs "foo"
</script>
```
除非你使用严格模式，这时候this就会变成undefined。
```javascript
<script type="text/javascript">
    foo = "bar"; 
    function testThis() {
    	"use strict";
    	this.foo = "foo";
    }

   	console.log(this.foo); //logs "bar"
    testThis();  //Uncaught TypeError: Cannot set property 'foo' of undefined 
</script>
```

如果你在调用函数的时候在前面使用了new，this就会变成一个新的值，和global的this脱离干系。
```javascript
<script type="text/javascript">
    foo = "bar";

    function testThis() {
    	this.foo = "foo";
    }

    console.log(this.foo); //logs "bar"
    new testThis();
    console.log(this.foo); //logs "bar"

    console.log(new testThis().foo); //logs "foo"
</script>

```

## prototype this
你创建的每一个函数都是函数对象。它们会自动获得一个特殊的属性prototype，你可以给这个属性赋值。当你用new的方式调用一个函数的时候，你就能通过this访问你给prototype赋的值了。
```javascript
function Thing() {
       console.log(this.foo);
}

Thing.prototype.foo = "bar";
var thing = new Thing(); //logs "bar"
console.log(thing.foo);  //logs "bar"
```

当你使用new为你的函数创建多个实例的时候，这些实例会共享你给prototype设定的值。对于下面的例子，当你调用this.foo的时候，都会返回相同的值，除非你在某个实例里面重写了自己的this.foo
```javascript
function Thing() {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () {
    console.log(this.foo);
}
Thing.prototype.setFoo = function (newFoo) {
    this.foo = newFoo;
}

var thing1 = new Thing();
var thing2 = new Thing();

thing1.logFoo(); //logs "bar"
thing2.logFoo(); //logs "bar"
thing1.setFoo("foo");
thing1.logFoo(); //logs "foo";
thing2.logFoo(); //logs "bar";

thing2.foo = "foobar";
thing1.logFoo(); //logs "foo";
thing2.logFoo(); //logs "foobar";

```
实例里面的this是一个特殊的对象。你可以把this想成一种获取prototype的值的一种方式。当你在一个实例里面直接给this添加属性的时候，会隐藏prototype中与之同名的属性。如果你想访问prototype中的这个属性值而不是你自己设定的属性值，你可以通过在实例里面删除你自己添加的属性的方式来实现。
```javascript
function Thing() {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () {
    console.log(this.foo);
}
Thing.prototype.setFoo = function (newFoo) {
    this.foo = newFoo;
}
Thing.prototype.deleteFoo = function () {
    delete this.foo;
}
var thing = new Thing();
thing.setFoo("foo");
thing.logFoo(); //logs "foo";
thing.deleteFoo();
thing.logFoo(); //logs "bar";
thing.foo = "foobar";
thing.logFoo(); //logs "foobar";
delete thing.foo;
thing.logFoo(); //logs "bar";
```
或者你也能直接通过引用函数对象的prototype 来获得你需要的值。
```javascript
function Thing() {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () {
    console.log(this.foo, Thing.prototype.foo);
}

var thing = new Thing();
thing.foo = "foo";
thing.logFoo(); //logs "foo bar";
```
通过一个函数创建的实例会共享这个函数的prototype属性的值，如果你给这个函数的prototype赋值一个Array，那么所有的实例都会共享这个Array，除非你在实例里面重写了这个Array，这种情况下，函数的prototype的Array就会被隐藏掉。
```javascript
function Thing() {
}
Thing.prototype.things = [];
var thing1 = new Thing();
var thing2 = new Thing();
thing1.things.push("foo");
console.log(thing2.things); //logs ["foo"]
```

给一个函数的prototype赋值一个Array通常是一个错误的做法。如果你想每一个实例有他们专属的Array，你应该在函数里面创建而不是在prototype里面创建。
```javascript
function Thing() {
    this.things = [];
}

var thing1 = new Thing();
var thing2 = new Thing();
thing1.things.push("foo");
console.log(thing1.things); //logs ["foo"]
console.log(thing2.things); //logs []
```

实你可以通过把多个函数的prototype链接起来的从而形成一个原型链，因此this就会魔法般地沿着这条原型链往上查找直到找你你需要引用的值。
```javascript
function Thing1() {
}
Thing1.prototype.foo = "bar";

function Thing2() {
}
Thing2.prototype = new Thing1();

var thing = new Thing2();
console.log(thing.foo); //logs "bar"
```

一些人利用原型链的特性来在JavaScript模仿经典的面向对象的继承方式。任何给用于构建原型链的函数的this的赋值的语句都会隐藏原型链上游的相同的属性。
```javascript
function Thing1() {
}
Thing1.prototype.foo = "bar";

function Thing2() {
    this.foo = "foo";
}
Thing2.prototype = new Thing1();

function Thing3() {
}
Thing3.prototype = new Thing2();

var thing = new Thing3();
console.log(thing.foo); //logs "foo"
```
在prototype里面定义的方法里面使用this会影响到当前实例的原型链的上游的this。这意味着你直接给this赋值的时候，隐藏了原型链上游的相同的属性值。这个实例的任何方法都会使用这个最新的值而不是原型里面定义的这个相同的值。
```javascript
function Thing1() {
}
Thing1.prototype.foo = "bar";
Thing1.prototype.logFoo = function () {
    console.log(this.foo);
}

function Thing2() {
    this.foo = "foo";
}
Thing2.prototype = new Thing1();
var thing = new Thing2();
thing.logFoo(); //logs "foo";
```
在JavaScript里面你可以嵌套函数，也就是你可以在函数里面定义函数。嵌套函数可以通过闭包捕获父函数的变量，但是这个函数没有继承this.
```javascript
function Thing() {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () {
    var info = "attempting to log this.foo:";
    function doIt() {
        console.log(info, this.foo);
    }
    doIt();
}

var thing = new Thing();
thing.logFoo();  //logs "attempting to log this.foo: undefined"
```
在doIt里面的this是global对象或者在严格模式下面是undefined。这是造成很多不熟悉JavaScript的人深陷 this陷阱的根源。在这种情况下事情变得非常糟糕，就像你把一个实例的方法当作一个值，把这个值当作函数参数传递给另外一个函数但是却不把这个实例传递给这个函数一样。在这种情况下，一个方法里面的环境变成了全局范围，或者在严格模式下面的undefined。
```javascript
function Thing() {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () {  
    console.log(this.foo);   
}

function doIt(method) {
    method();
}

var thing = new Thing();
thing.logFoo(); //logs "bar"
doIt(thing.logFoo); //logs undefined
```
我喜欢先把this捕获到一个变量里面，通常这个变量叫做self，来避免上面这种情况的发生。
```javascript
function Thing() {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () {
    var self = this;
    var info = "attempting to log this.foo:";
    function doIt() {
        console.log(info, self.foo);
    }
    doIt();
}
var thing = new Thing();
thing.logFoo();  //logs "attempting to log this.foo: bar"
```
但是当需要把一个方法作为一个值传递给一个函数的时候并不管用。
```javascript
function Thing() {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () { 
    var self = this;
    function doIt() {
        console.log(self.foo);
    }
    doIt();
}

function doItIndirectly(method) {
    method();
}
var thing = new Thing();
thing.logFoo(); //logs "bar"
doItIndirectly(thing.logFoo); //logs undefined
```

可以通过bind将实例和方法一切传递给函数来解决这个问题，bind是一个函数定义在所有函数和方法的函数对象上面.
```javascript
function Thing() {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () { 
    console.log(this.foo);
}

function doIt(method) {
    method();
}

var thing = new Thing();
doIt(thing.logFoo.bind(thing)); //logs bar
```
同样可以使用apply和call来在新的上下文中调用方法或函数。
```javascript
function Thing() {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () { 
    function doIt() {
        console.log(this.foo);
    }
    doIt.apply(this);
}
function doItIndirectly(method) {
    method();
}
var thing = new Thing();
doItIndirectly(thing.logFoo.bind(thing)); //logs bar
```

可以用bind来代替任何一个函数或者方法的this，即便它没有赋值给实例的初始prototype。
```javascript
function Thing() {
}
Thing.prototype.foo = "bar";
function logFoo(aStr) {
    console.log(aStr, this.foo);
}

var thing = new Thing();
logFoo.bind(thing)("using bind"); //logs "using bind bar"
logFoo.apply(thing, ["using apply"]); //logs "using apply bar"
logFoo.call(thing, "using call"); //logs "using call bar"
logFoo("using nothing"); //logs "using nothing undefined"
```
应该避免在构造函数里面返回任何东西，因为这可能代替本来应该返回的实例。
```javascript
function Thing() {
   return {};
}
Thing.prototype.foo = "bar";

Thing.prototype.logFoo = function () {
    console.log(this.foo);
}
var thing = new Thing();
thing.logFoo(); //Uncaught TypeError: undefined is not a function
```
但如果你在构造函数里面返回了一个原始值，上面所述的情况并不会发生并且返回语句被忽略了。最好不要在你将通过new调用的构造函数里面返回任何类型的数据，即便你知道自己正在做什么。如果你想创建一个工厂模式，通过一个函数来创建一个实例，这个时候不要使用new来调用函数。当然这个建议是可选的。

可以通过使用Object.create来避免使用new，这样同样能够创建一个实例。
```javascript
function Thing() {
}
Thing.prototype.foo = "bar";

Thing.prototype.logFoo = function () {
    console.log(this.foo);
}
var thing =  Object.create(Thing.prototype);
thing.logFoo(); //logs "bar"
```
在这种情况下并不会调用构造函数
```javascript
function Thing() {
    this.foo = "foo";
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () {
    console.log(this.foo);
}
var thing =  Object.create(Thing.prototype);
thing.logFoo(); //logs "bar"
```
因为Object.create不会调用构造函数的特性在你继承模式下你想通过原型链重写构造函数的时候非常有用.
```javascript
function Thing1() {
    this.foo = "foo";
}
Thing1.prototype.foo = "bar";

function Thing2() {
    this.logFoo(); //logs "bar"
    Thing1.apply(this);
    this.logFoo(); //logs "foo"
}
Thing2.prototype = Object.create(Thing1.prototype);
Thing2.prototype.logFoo = function () {
    console.log(this.foo);
}
var thing = new Thing2();
```

## object this
在一个对象的一个函数里，你可以通过this来引用这个对象的其他属性。这个用new来新建一个实例是不一样的。
```javascript
var obj = {
    foo: "bar",
    logFoo: function () {
            console.log(this.foo);
   }
};
obj.logFoo(); //logs "bar"
```
注意，没有使用new，没有使用Object.create，也没有使用函数调用创建一个对象。你也可以将对象当作一个实例将函数绑定到上面。
```javascript
var obj = {
    foo: "bar"
};
function logFoo() {
    console.log(this.foo);
}
logFoo.apply(obj); //logs "bar"
```
当你用这种方式使用this的时候，并不会越出当前的对象。只有有相同直接父元素的属性才能通过this共享变量.
```javascript
var obj = {
    foo: "bar",
    deeper: {
        logFoo: function () {
            console.log(this.foo);
        }
    }
};
obj.deeper.logFoo(); //logs undefined
```
可以直接通过对象引用你需要的属性.
```javascript
var obj = {
    foo: "bar",
    deeper: {
        logFoo: function () {
            console.log(obj.foo);
        }
    }
};
obj.deeper.logFoo(); //logs "bar"
```

## DOM event this
在一个HTML DOM事件处理程序里面，this始终指向这个处理程序被所绑定到的HTML DOM节点
```javascript
function Listener() {
    document.getElementById("foo").addEventListener("click",
      this.handleClick);
}
Listener.prototype.handleClick = function (event) {
    console.log(this); //logs "<div id="foo"></div>"
}
var listener = new Listener();
document.getElementById("foo").click();
```
除非你自己通过bind切换了上下文.
```javascript
function Listener() {
    document.getElementById("foo").addEventListener("click", 
        this.handleClick.bind(this));
}
Listener.prototype.handleClick = function (event) {
    console.log(this); //logs Listener {handleClick: function}
}

var listener = new Listener();
document.getElementById("foo").click();
```

## HTML this
在HTML节点的属性里面，你可以放置JavaScript代码，this指向了这个元素.
```html
<div id="foo" onclick="console.log(this);"></div>
<script type="text/javascript">
document.getElementById("foo").click(); //logs <div id="foo"...
</script>
```
## override this
不能重写this，因为它是保留字。
```javascript
function test () {
   var this = {};  // Uncaught SyntaxError: Unexpected token this 
}
eval this
```
可以通过eval来访问this,但会造成一个安全问题，除非不用eval，没有其他方式来避免这个问题。
```javascript
function Thing () {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () {
    eval("console.log(this.foo)"); //logs "bar"
}

var thing = new Thing();
thing.logFoo();
```
在通过Function来创建一个函数的时候，同样能够访问this。
```javascript
function Thing () {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = new Function("console.log(this.foo);");

var thing = new Thing();
thing.logFoo(); //logs "bar"
```

## with this
可以通过with来将this添加到当前的执行环境，并且读写this的属性的时候不需要通过this.但许多人认为这样使用是不好的因为with本身就饱受争议。
```javascript
function Thing () {
}
Thing.prototype.foo = "bar";
Thing.prototype.logFoo = function () {
    with (this) {
        console.log(foo);
        foo = "foo";
    }
}
var thing = new Thing();
thing.logFoo(); // logs "bar"
console.log(thing.foo); // logs "foo"
```

## jQuery this
和HTML DOM元素节点的事件处理程序一样，在许多情况下JQuery的this都指向HTML元素节点。这在事件处理程序和一些方便的方法中都是管用的，比如$.each.
```html
<div class="foo bar1"></div>
<div class="foo bar2"></div>
<script type="text/javascript">
$(".foo").each(function () {
    console.log(this); //logs <div class="foo...
});
$(".foo").on("click", function () {
    console.log(this); //logs <div class="foo...
});
$(".foo").each(function () {
    this.click();
});
</script>
```
## thisArg this
如果你用过underscore.js 或者 lo-dash 你可能知道许多类库的方法可以通过一个叫做thisArg 的函数参数来传递实例，这个函数参数会作为this的上下文。举个例子，这适用于_.each。原生的JavaScript在ECMAScript 5的时候也允许函数传递一个thisArg参数了，比如forEach。事实上，之前阐述的bind，apply和call的使用已经给你创造了传递thisArg参数给函数的机会。这个参数将this绑定为你所传递的对象。
```javascript
function Thing(type) {
    this.type = type;
}
Thing.prototype.log = function (thing) {
    console.log(this.type, thing);
}
Thing.prototype.logThings = function (arr) {
   arr.forEach(this.log, this); // logs "fruit apples..."
   _.each(arr, this.log, this); //logs "fruit apples..."
}
var thing = new Thing("fruit");
thing.logThings(["apples", "oranges", "strawberries", "bananas"]);
```
这使得代码变得更加简介，因为避免了一大堆bind语句、函数嵌套和this暂存的使用。
