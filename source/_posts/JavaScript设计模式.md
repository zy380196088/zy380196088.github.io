---
title: JavaScript设计模式
categories: 学习
date: 2017-03-04 22:24:12
tags:
---

<!--more-->

# 面向过程与面向对象
## 封装
### 创建一个类
首先声明一个函数保存在一个变量里(一般将首字母大写).
然后在这个函数(类)内部通过 this 添加属性或者方法来实现对类添加属性活着方法:
```js
var Book = function(id, bookName, price){
    this.id = id ;
    this.bookName = bookName;
    this.price = price;
}
```
也可以通过在类的原型上添加属性和方法.有如下两种方式:
```js
//方法一:为原型对象属性赋值
Book.prototype.display = function(){
    //...
}

//方法二:将一个对象赋值给类的原型对象
Book.prototype = {
    display:function(){}
}
```

### constructor
constructor 是一个属性,当创建一个函数或者对象时都会为其创建一个原型对象 prototype,
在 prototype 对象中又会像函数中创建 this 一样创建一个 constructor 属性,
那么constructor 属性指向的就是拥有整个源性对象的函数或对象.

### 私有属性 私有方法
由于JavaScript的函数级作用域,生命在函数内部的变量以及方法在外界是访问不到的.
通过此特性即可创建类的私有变量以及私有方法.然而在函数内部通过 this 创建的属性和方法,在类创建对象时,每个对象自身都拥有一份并且可以在外部访问的到.
因此,通过 this 创建的属性可以看做是对象共有的属性和公有方法.
```js
var Book = function(id,name,price){
    //私有属性
    var num = 1;
    //私有方法
    function checkId(){
        //...
    };
    //特权方法
    this.getName = function(){};
    this.getPrice = function(){};
    this.setName = function(){};
    this.setPrice = function(){};
    //对象共有属性
    this.id = id;
    //对象公有方法
    this.copy = function(){};

    //构造器
    this.setName(name);
    this.setPrie(price);
}
```

## 闭包
我们时常将累得静态变量通过闭包来实现:
```js
var Book = (function(){
    //静态私有变量
    var bookNum = 0;
    //静态私有方法
    function checkBook (name){

    }
    //返回构造函数
    return function(newId,newName , newPrice){
        //私有变量
        var name , price;
        //私有方法
        function checkID(id){}
        //特权方法
        this.getName = function(){};
        this.getPrice = function(){};
        this.setName = function(){};
        this.setPrice = function(){};
        //对象共有属性
        this.id = newId;
        //公有方法
        this.copy = function(){}
        //构造器
        this.setName(name);
        this.setPrice(price);
    }
})();

Book.prototype = {
    isJSBook :false,
    display:function(){}
}
```

### 找位监察 - 创建对象的安全模式
```js
//图书安全类
var Book = function(title,time ,type){
    //判断执行过程中 this 是否是当前这个对象(如果是说明是用 new 创建的)
    if(this instanceof Book){
        this.title = title ;
        this.time = time;
        this.type = type;
    }else{
        //否则重新创建这个对象
        return new Book(title,time,type);
    }
}

var book = Book('JavaScript','2014','js');
```

## 传宗接代-继承
### 子类的原型对象 - 类式继承
```js
//声明父类
function SuperClass(){
    this.superValue = true;
}
//为父类添加公有方法.
SuperClass.prototype.getSuperValue = function(){
    return this.superValue;
};
//申明子类
function SubClass(){
    this.subValue = false;
}
//继承父类
SubClass.prototype = new SuperClass();
//为子类添加公有方法
SubClass.prototype.getSuperValue = function
(){
    return this.subValue;
}
```
我们可以通过 instanceof 来检测某个对象是否是某个类的实例,或者说某个对象是否继承了某个类.
instanceof 通过判断对象的 prototype 链来确定这个对象是否是某个类的实例,而不关心对象与类的自身结构.

### 创建即继承 - 构造函数继承
```js
//声明父类
function SuperClass(id){
    //引用类型共有属性
    this.books = ['JavaScript','html','css'];
    //值类型共有属性
    this.id = id;
}
//父类声明原型方法
SuperClass.prototype.showBooks = function(){
    console.log(this.book)
}

//申明子类
function SubClass(id){
    //继承父类
    SuperClass.call(this,id);
}
//创建子类实例
var instance1 = new SubClass(10);
VAR instance2 = new SubClass(11);

instancel.books.push("设计模式"); 

console.log(instance1.books);//['JavaScript','html','css','设计模式']
console.log(instance1.id);//10
console.log(instance2.books)//['JavaScript','html','css']
console.log(instace2.id)//11
```

### 将有点为我所用--组合继承
```js
//组合式继承
//申明父类
function SubClass (name){
    //值类型公有属性
    this.name = name;
    //引用类型公有属性
    this.books = ["html","css","Javascript"];
}
//父类原型公有方法
SuperClass.prototype.getName = function(){
    console.log(this.name)
}
//声明子类
function SubClass(name,time){
    //构造函数式继承父类 name 属性
    SubClass.call(this,name);
    //子类中新增公有属性
    this.time = time;
}
//类式继承 子类原型继承父类
SubClass.prototype = new SuperClass();
//子类原型方法
SubClass.prototype.getTime = function(){
    console.log(this.time)
}
```

### 洁净的继承者-- 原型式继承
```js
//原型是继承
function inheritObject(o){
    //申明一个过渡函数对象
    function F(){}
    //过渡对象的原型继承父对象
    F.prototype = o;
    //返回过渡对象的一个实例,该实例的原型继承了父对象
    return new F();
}
```


### 如虎添翼--寄生式继承
```js
//寄生式继承
//申明对基对象
var book = {
    name : "js book",
    alikeBooks:["css book","html book"]
};
function createBook(obj){
    //通过原型继承方式创建新对象
    var o = new inheritObject(obj);
    //拓展新对象
    o.getName = function(){
        console.log(name);
    };
    //返回拓展后的新对象
    return o;
}
```

寄生式继承就是对原型继承的第二次封装,并且在这第二次封装过程中对继承的对象进行了拓展,这样新创建的对象不仅仅有父类中的属性和方法而且还添加新的属性和方法.

## 多继承
```js
//单继承 属性复制
var extend = function(target,source){
    //遍历源对象中的属性
    for (var property in source){
        //将源对象中的属性复制到目标对象中
        target[property] = source[property];
    }
    //返回目标对象
    return target;
}
```

## 多态
```
function add(){
    //获取参数
    var arg = arguments;
    //获取参数长度
    var len = arg.length;

    switch(len){
        case 0:
            return 'null';
        case 1:
            return arg[0];
        case 2:
            return arg[0]+arg[1];
    }
}
```


# 创建型设计模式
## 简单工厂模式
```
//工厂模式
function createBook(name , time,type){
    //创建一个对象,并对对象拓展属性和方法
    var o  = new Object();

    o.name = name;
    o.time = time;
    o.type = type;
    o.getName = function(){
        console.log(this.name);
    }
    //将对象返回
    return o;
}

function createPop(type,text){
    var o = new Object();
    o.content = text;
    o.show = function(){
        //显示方法
    }
    if(type  == 'alert'){}
    if(type  == 'prompt'){}
    if(type == 'confirm'){}

    //将对象返回
    return o;
}

//创建警示框
var userNameAlert = createPop('alert','用户名只能是26个字母和数字')

```

## 工厂方法模式
工厂方法模式本意是说将实际创建对象工作推迟到子类当中.可以将工厂发发看作是一个实例化对象的工厂类.我们可以采用安全模式类,将创建对象的基类放在工厂方法类的原型中即可.



### 安全模式类
## 抽象工厂模式
## 建造者模式
## 原型模式
## 单例模式





