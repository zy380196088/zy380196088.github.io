---
title: JavaScript - 面向对象编程
date: 2016-05-08 16:50:10
tags: [JavaScript, OOP]
---

## 创建对象
### 构造函数
```javascript
function Student(name){
	this.name = name;
	this.hello = function(){
		alert('hello,'+ this.name + '!');
	}	
}

var xiaoming = new Student('小明');
var xiaohong = new Student('小红');
```
让创建的对象共享一个hello函数，这样可以节省内存
```javascript
function Student(name){
	this.name = name;
} 

Student.rototype.hello = function(){
	alert('hello,'+ this.name + '!');
};
```
还可以写一个createStudent()函数，在内部封装所有的new操作。
```javascript
function Student(props){
	thi.name = props.name || '匿名'; //默认值为'匿名'
	this.grade = props.grade || 1;  // 默认值为1
}

Student.prototype.hello = function(){
	alert('Hello, ' + this.name + '!');
};

function createStudent(props) {
    return new Student(props || {})
}
```
## 原型继承
请参考[原型继承－廖雪峰](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/0014344997013405abfb7f0e1904a04ba6898a384b1e925000)

## class继承
新的关键字class从ES6开始正式被引入到JavaScript中。class的目的就是让定义类更简单。
如果用新的class关键字来编写Student，可以这样写：
```javascript
class Student {
    constructor(name) {
        this.name = name;
    }

    hello() {
        alert('Hello, ' + this.name + '!');
    }
}
```
用class定义对象的另一个巨大的好处是继承更方便了。想一想我们从Student派生一个PrimaryStudent需要编写的代码量。现在，原型继承的中间对象，原型对象的构造函数等等都不需要考虑了，直接通过extends来实现：
```javascript
class PrimaryStudent extends Student {
    constructor(name, grade) {
        super(name); // 记得用super调用父类的构造方法!
        this.grade = grade;
    }

    myGrade() {
        alert('I am at grade ' + this.grade);
    }
}
```
ES6引入的class和原有的JavaScript原型继承有什么区别呢？实际上它们没有任何区别，class的作用就是让JavaScript引擎去实现原来需要我们自己编写的原型链代码。简而言之，用class的好处就是极大地简化了原型链代码。

## 公有属性和公有方法
```javascript
function User(name,age){
    this.name = name;//公有属性
    this.age = age;
}
User.prototype.getName = function(){
    //公有方法
    return this.name;
}

var user = new User('zoey',24);
console.log(user.getName());//zoey
```

## 私有属性和方法
```javascript
function User(name,age){
    //私有属性
    var name = name;
    var age = age;
    function alertAge(){
        //私有方法
        alert(age)
    }
    alertAge(age)//弹出24
}
var user = new User('zoey',24);
```

## 静态属性和方法
```javascript
function User(){}
User.age = 24;
User.myname = 'zoey';
User.getName = function(){
    //静态方法
    return this.myname;
}
console.log(User.getName())//zoey
```

## 特权方法
```javascript
function User(name,age){
    var name = name;
    var age = age;
    this.getName = function(){
        //特权方法
        return name;//私有属性和方法不能使用this调用
    }
}
var user = new User('zoey',24);
console.log(user.getName());//zoey
```

## 静态类
```javascript
var user = {
    init : function(name,age){
        this.name = name;
        this.age = age;
    },
    getName : function(){
        return this.name;
    }
}
user.init('zoey',24);
console.log(user.getName());//zoey
```

## 公有方法调用规则
调用共有方法必须先实例化对象.
```javascript
function User(){
    this.myname = 'zoey';//公有属性
    this.age = 24;
    this.do = function(){
        //特权方法
        return this.myname + '学习JavaScript';
    }
}

User.eat = function(food){
    return '吃了'+food;
}

User.prototype.alertAge = function(){
    alert(this.age)
}
User.prototype.alertDo = function(){
    alert(this.do());//调用特权方法
}

User.prototype.elartEat = function(food){
    alert(User.eat(food));//只能通过对象本身调用静态方法
    alert(this.eat(food));//报错
}

var user = new User();
user.alertAge();//24
user.alertDo();//zoey学习JavaScript
user.alertEat('方便面')//吃了方便面
```

## 静态方法的调用规则
使用静态方法时，无需实例化对象，便可以调用，对象实例不能调用对象的静态方法，只能调用实例自身的静态属性和方法
```javascript
function User() {}
User.age = 26; //静态属性
User.myname = 'fire子海';
User.getName = function() { //静态方法

  return this.myname;
}
var user = new User();
console.log(user.getName); //TypeError: user.getName is not a function
user.supper = '方便面';
user.eat = function() {
  return '晚餐只有' + this.supper;
}
user.eat(); //晚餐只有方便面

```

静态方法无法调用公有属性、公有方法、私有方法、私有属性、特权方法和原型属性.
```javascript
function User() {
  this.myname = 'fire子海'; //公有属性
  this.age = 26;
  this.do = function() { //特权方法
    return this.myname + '学习js';
  }
}
User.prototype.alertAge = function() { //公共方法，也叫原型方法
  alert(this.age);
}
User.prototype.sex = '男'; //原型属性
User.getName = function() { //静态方法
  return this.myname;
}
User.getAge = function() {
  this.alertAge();
}
User.getDo = function() {
    return this.do();
  }
//console.log(User.getName())//undefined
//console.log(User.getDo());//TypeError: this.do is not a function
//console.log(User.getAge())//TypeError: this.alertAge is not a function

```

## 特权方法的调用规则
特权方法通过this调用公有方法、公有属性，通过对象本身调用静态方法和属性，在方法体内直接调用私有属性和私有方法.
```javascript
function User(girlfriend) {
  var girlfriend = girlfriend;

  function getGirlFriend() {
    return '我女朋友' + girlfriend + '是美女！';
  }
  this.myname = 'fire子海'; //公有属性
  this.age = 26;
  this.do = function() { //特权方法
    return this.myname + '学习js';
  }
  this.alertAge = function() {
    this.changeAge(); //特权方法调用公有方法
    alert(this.age);
  }
  this.alertGirlFriend = function() {
    alert(getGirlFriend()); //调用私有方法
  }
}
User.prototype.changeAge = function() {
  this.age = 29;
}
var user = new User('某某');
user.alertAge(); //alert:29
user.alertGirlFriend(); //alert:我的女朋友某某是美女！

```

## 私有方法
对象的私有方法和属性,外部是不可以访问的,在方法的内部不是能this调用对象的公有方法、公有属性、特权方法的.
```javascript
function User(girlfriend) {
  var girlfriend = girlfriend;
  this.myname = 'fire子海'; //公有属性
  this.age = 26;

  function getGirlFriend() {
    //this.myname ;//此时的this指向的window对象，并非User对象，
    // this.myname = 'fire子海',此时的this指向的是getGirFriend对象了。
    //如果通过this调用了getGirFriend中不存在的方法呀属性，this便会指向window 对象，只有this调用了getGirlFriend存在的方法和属性，this才会指定getGirlFriend;
    alert(User.eat('泡面')); //alert：晚餐只有方便面
  }
  this.do = function() { //特权方法
    return this.myname + '学习js';
  }
  this.alertAge = function() {
    this.changeAge(); //特权方法调用公有方法
    alert(this.age);
  }
  this.alertGirlFriend = function() {
    getGirlFriend(); //调用私有方法
  }
}
User.eat = function(supper) {
  return '晚餐只有' + supper;
}
var user = new User('某某');
user.alertGirlFriend();

```







