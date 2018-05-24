---
title: JS Tips
date: 2016-08-24 12:26:52
tags: [javascript]
---

## URL (http开头)地址正则

```
var re=/(http:\/\/)?[A-Za-z0-9]+\.[A-Za-z0-9]+[\/=\?%\-&_~`@[\]\':+!]*([^<>\"\"])*/gi;
```

## 检查某对象是否具有某属性

当你需要检查某属性是否存在于一个对象,你可能会这样做:
```javascript
var myObject = {
    name: 'tips'
};
if(myObject.name){...}
```

这是可行的,但是需要知道还有两种原生方法可以解决该问题: in操作符 和 hasOwnProperty,任何继承自Object的对象都可以使用这两种方法.

但是这两种方法也有区别:

```javascript
var myFunc = function() {
  this.name = 'tips';
};
myFunc.prototype.age = '10 days';
var user = new myFunc();
user.hasOwnProperty('name'); // true
user.hasOwnProperty('age'); // false, 因为age来自于原型链
```

两者检查属性的深度不同，换言之hasOwnProperty只在本身有此属性时返回true,而in操作符不区分属性来自于本身或继承自原型链。

## 将Node List转换为数组(Array)

querySelectorAll 方法返回一个"类数组"对象为node list.看似数组却没有类似map,foreach这样的数组方法.
我们需要将其转换为DOM元素的数组:

```js
    var nodelist = document.querySelectorAll('div');
    var nodelistToArray = Array.apply(null , nodelist);
    //这样就可以进行map ,foreach等操作了
    nodelistToArray.forEach(...);
    nodelistToArray.map(...);
    nodelistToArray.slice(...);
```

## "快速排序"
"快速排序"的思想步骤:
1.在数组中,找一个基点
2.建立两个数组,分别存储左边和右边的数组
3.利用递归进行下次比较

```javascript
    function quickSort(arr){
        if(arr.length <=1){
            return arr;//如果数组只有一个数则直接返回
        }
        var num =  Math.floor(arr.length/2);//找到中间数的索引值
        var numValue = arr.splice(num,1);//取中间数的值
        var left = [];
        var right = [];
        for(var i = 0 ,i < arr.length; i++){
            if(arr[i]<numValue){
                left.push(arr[i]);//小于基准数则添加到左边数组中
            }
            else{
                right.pusth(arr[i]);
            }
        }
        //递归
        return quickSort(left).concat([numValue],quickSort(right));
    }
    //调用
     alert(quickSort([2,35,15,44,8]));//弹出"2 8 15 35 44"
```

## 返回对象,使方法可以链式调用

在面向对象的Javascript中为对象建立一个方法时，返回当前对象可以让你在一条链上调用方法。

```js
function Person(name) {
  this.name = name;
  this.sayName = function() {
    console.log("Hello my name is: ", this.name);
    return this;
  };
  this.changeName = function(name) {
    this.name = name;
    return this;
  };
}
	var person = new Person("John");
	person.sayName().changeName("Timmy").sayName();
```

## JavaScript 中 !! 的作用
经常看到这样的例子：﻿﻿
```js
var a；
var b=!!a;
```
a默认是undefined。!a是true，!!a则是false，所以b的值是false，而不再是undefined，也非其它值，主要是为后续判断提供便利。

!!一般用来将后面的表达式强制转换为布尔类型的数据（boolean），也就是只能是true或者false;
因为javascript是弱类型的语言（变量没有固定的数据类型）所以有时需要强制转换为相应的类型，类似的如:
```js
a=parseInt(“1234″)
a=”1234″-0 //转换为数字
b=1234+”” //转换为字符串
c=someObject.toString() //将对象转换为字符串
```
其中第1种、第4种为显式转换，2、3为隐式转换

布尔型的转换，javascript约定规则为:
**false、undefinded、null、0、”” 为 false**
**true、1、”somestring”、[Object] 为 true**

对null与undefined等其他用隐式转换的值，用!操作符时都会产生true的结果，所以用两个感叹号的作用就在于将这些值转换为“等价”的布尔值； 
再来看看：
```js
var foo;
alert(!foo);//undifined情况下，一个感叹号返回的是true;
alert(!goo);//null情况下，一个感叹号返回的也是true;
var o={flag:true};
var test=!!o.flag;//等效于var test=o.flag||false;
alert(test);
```

这段例子，演示了在undifined和null时，用一个感叹号返回的都是true,用两个感叹号返回的就是false,所以两个感叹号的作用就在于，如果明确设置了变量的值(非null/undifined/0/”“等值),结果就会根据变量的实际值来返回，如果没有设置，结果就会返回false。

## 对象转换为数组

```javascript
//注意对象必须是以下格式的才可以通过此方式转化为数组
//获取的DOM集合，以及函数的arguments也可以通过此方式转化为数组
var obj={
	0:'qian',
	1:'long',
	2:'chu',
	3:'tian',
	length:4
}
var _slice=[].slice;
var objArr=_slice.call(obj);
```

## 验证是否为数组
```javascript
 function isArray(obj){
    return  Object.prototype.toString.call(obj) === '[object Array]' ;
}
```

## 清空数组
```javascript
//方式一 通过将长度设置为0
var arr=[1,2,3,4,5];
arr.length=0;
//方式二 通过splice方法
 var arr=[1,2,3,4,5];
arr.splice(0,arr.length);
//方式三 通过将空数组 [] 赋值给数组(严格意义来说这只是将ary重新赋值为空数组，之前的数组如果没有引用在指向它将等待垃圾回收。)
var arr=[1,2,3,4,5];
arr=[];
```

## 保留指定小数位
```javascript
var num =4.345678;
num = num.toFixed(4);  // 4.3457 第四位小数位以四舍五入计算
```

## 生成指定长度的随机字母数字字符串
```javascript
function getRandomStr(len) {
    var str = "";
    for( ; str.length < len; str  += Math.random().toString(36).substr(2));
    return  str.substr(0, len);
}
```

## 找出数组中出现次数最的元素，并给出其出现过的位置
```javascript
function getMaxAndIndex( arr ){
        var obj = {};
        arr.forEach(function(item,index){
            if(!obj[item]){
                obj[item]= {indexs: [index]}
            }else{
                obj[item]['indexs'].push(index);
            }
        });
        var num=0;//记录出现次数最大值
        var str='';//记录出现次数最多的字符
        var reArr;//返回最大值的位置数组
        for(var attr in obj){
            var temp=obj[attr]['indexs'];
            if(temp.length>num){
                num=temp.length;
                str=attr;
                reArr=temp;
            }
        }
        return {
            maxStr:str,
            indexs:reArr
        }
    }
```

## 字符串的split函数的特殊值情况

1. 参数不传，返回包含原字符串对象，长度为1的数组.
```js
"".split()//[""]
"xxcanghai".split()//["xxcanghai"]
```
2. 参数传空字符串，返回将原字符串每个字符分隔的数组,若原字符串为空字符串则返回空数组.
```js
"".split("")//[]
"xxcanghai".split("")//["x", "x", "c", "a", "n", "g", "h", "a", "i"]
```
3. 原字符串为空字符串，参数不为空时，会返回包含一个空字符串的数组.
```js
"".split(",")//[""]，错误，应为[]
"".split("xxcanghai")//[""]，错误，应为[]
```

## 类型判断:
```javascript
//判断undefined
var tmp = undifined;
if(typeof (tmp) == "undifined"){
    console.log("undefined");
}

//判断null
var tmp = null;
if(!tmp && typeof(tmp)!== "undifined" && tmp!= 0){
    console.log("null");
}

//判断NaN
var tmp = 0/0;
if(isNan(tmp)){
    console.log("NaN");
}

//判断undefined和null
var tmp = undefined;
if(tmp == undefined){
    console.log("null or undifined");
}

var tmp = undefined;
if(tmp == null){
    console.log("null or undifined");
}

var tmp =null;
if(!tmp){
   console.log("null or undefined or NaN");
}
```

## jsonp方式跨域
```javascript
function jsonp(config){
    var options = config || {}; // 需要配置url, success, time, fail四个属性.
    var callbackName = ('jsonp_'+ Math.random()).replace(".","");
    var oHead = document.getElementsByTagName('head')[0];
    var oScript = document.getElementsByTagName('script');

    oHead.appendChild(oScript);
    window[callbackName] = function(json){
        //创建jsonp回调函数
        //先删除script标签，实际上执行的是success函数
        oHead.removeChild(oScript);
        clearTimeout(oScript.timer);
        window[callbackName]=null;
        options.success && options.success(json);
    };

    oScripts.src = options.url + '?' + callbackName;//发送请求
    if(options.time){
        oScripts.timer = setTimeout(function(){
            window[callbackName]=null;
            oHead.removeChild(oScript);
            options.fail && options.fail({message:"错误:超时;"});
        },options.time);
    }
};

//使用方法:
jsonp({
    url:'b.com/b.json',
    success:function(d){
        //数据处理
    },
    time:5000,
    fail:function(){
        //错误处理
    }
    })
```

## JS生成随机字符串
```javascript
//这样生成一个32位的随机字符串，相同的概率很低。
var random_str = function(){
    var len = 32;
    var chars = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ1234567890';
    var max = charts.length;
    var str = '';
    for(var i =0; i<len;i++){
        str += charts.chartAt(Math.floor(Math.random()*max));
    }
    return str;
}
```

## 常用正则表达式
### JavaScript过滤Emoji表情:
```javascript
name = name.replace(/\uD83C[\uDF00-\uDFFF]|\uD83D[\uDC00-\uDE4F]/g, "");
```

### 手机号验证:
```javascript
var validate_phoneNumber =function(num){
    var reg = /^1[3-9]\d{9}$/;
    return reg.test(num);
}
```

### 身份证号码验证:
```javascript
var reg = /^[1-9]{1}[0-9]{14}$|^[1-9]{1}[0-9]{16}([0-9]|[xX])$/;
```

### ip验证:
```javascript
var reg = /^(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])(\.(\d{1,2}|1\d\d|2[0-4]\d|25[0-5])){3}$/;
```

### 判断是否有中文：
```javascript
var reg = /.*[\u4e00-\u9fa5]+.*$/;
reg.test('123792739测试')  //true
```

## 常用的JS函数

### 获取浏览器url中的参数值：
```javascript
var getURLParam = function(name) {
    return decodeURIComponent((new RegExp('[?|&]' + name + '=' + '([^&;]+?)(&|#|;|$)', "ig").exec(location.search) || [, ""])[1].replace(/\+/g, '%20')) || null;
};
```

### 操作cookie：
```javascript
own.setCookie = function(cname, cvalue, exdays){
    var d = new Date();
    d.setTime(d.getTime() + (exdays*24*60*60*1000));
    var expires = 'expires='+d.toUTCString();
    document.cookie = cname + '=' + cvalue + '; ' + expires;
};
own.getCookie = function(cname) {
    var name = cname + '=';
    var ca = document.cookie.split(';');
    for(var i=0; i< ca.length; i++) {
        var c = ca[i];
        while (c.charAt(0) == ' ') c = c.substring(1);
        if (c.indexOf(name) != -1) return c.substring(name.length, c.length);
    }
    return '';
};
```

### 数组排序sort函数：
```javascript
var arr = [11,36,6,27,80,32];
arr.srot(function(a,b){
    return a-b;                 //从小到大排序
    return b-a;                 //从大到小排序
    return Math.random() - 0.5; //随机排序(数组洗牌)
});

var arr = [{   //对象数组
    num: 1,
    text: 'num1'
}, {
    num: 5,
    text: 'num2'
}, {
    num: 6,
    text: 'num3'
}, {
    num: 3,
    text: 'num4'
}];
arr.sort(function(a, b) {
    return a.num - b.num;   //从小到大排
    return b.num - a.num;   //从大到小排
});


```

### 判断是对象还是数组：
```javascript
function isArray = function(o) {
    return toString.apply(o) === '[object Array]';
}
function isObject = function(o) {
    return toString.apply(o) === '[object Object]';
}
```


## 浅拷贝 和 深拷贝
```javascript
//浅拷贝
  var obj = {
    name: 'name'
  }
  var a = obj;
  a.name = 'new name';
  console.log(a.name); // 'new name'
  console.log(obj.name); // 'new name'
  //a只是通过赋值符号得到了obj的引用。

//深拷贝
  function object(parent, child) {
    var i,
        tostring = Object.prototype.toString,
        aStr = "[object Array]";
    child = child || {};
    for(i in parent) {
      if(parent.hasOwnProperty(i)) {
        //这时候还要判断它的值是不是对象
        if(typeof parent[i] === 'object') {
          child[i] = tostring.call(parent[i]) === aStr ? [] : {};
          object(parent[i], child[i]);
        }
        else {
          child[i] = parent[i];
        }
      }
    }
    return child;
  }

  var obj = {
    tags: ['js','css'],
    s1: {
      name: 'dai',
      age: 21
    },
    flag: true
  }
  var some = object(obj);
  some.tags = [1,2];
  console.log(some.tags); //[1, 2]
  console.log(obj.tags); //['js', 'css']

  //深度拷贝对象
  function cloneObj(obj) {
    var o = obj.constructor == Object ? new obj.constructor() : new obj.constructor(obj.valueOf());
    for(var key in obj){
        if(o[key] != obj[key] ){
            if(typeof(obj[key]) == 'object' ){
                o[key] = mods.cloneObj(obj[key]);
            }else{
                o[key] = obj[key];
            }
        }
    }
    return o;
}
```

## 原生JS实现千位分隔符
目的:每隔3位数字,加一个','
思考:实现的方法有哪些? (字符串数组分割,正则表达式等)
```js
//正则表达式实现
function thousandBitSeparator(num) {
  return num && (num
    .toString().indexOf('.') != -1 ? num.toString().replace(/(\d)(?=(\d{3})+\.)/g, function($0, $1) {
      return $1 + ",";
    }) : num.toString().replace(/(\d)(?=(\d{3}))/g, function($0, $1) {
      return $1 + ",";
    }));
}
console.log(thousandBitSeparator(1000));
//1,000
```

## 银行卡号每隔4位插入空格(只支持 IE9+)
```js
$(function() {

    $('#kahao').on('keyup', function(e) {
        //只对输入数字时进行处理
       if((e.which >= 48 && e.which <= 57) ||(e.which >= 96 && e.which <= 105 )){
            //获取当前光标的位置
            var caret = this.selectionStart
            //获取当前的value
            var value = this.value
            //从左边沿到坐标之间的空格数
            var sp =  (value.slice(0, caret).match(/\s/g) || []).length
            //去掉所有空格
            var nospace = value.replace(/\s/g, '')
            //重新插入空格
            var curVal = this.value = nospace.replace(/(\d{4})/g, "$1 ").trim()
            //从左边沿到原坐标之间的空格数
            var curSp = (curVal.slice(0, caret).match(/\s/g) || []).length
            //修正光标位置
            this.selectionEnd = this.selectionStart = caret + curSp - sp
        }
    })
})
```
## 获取页面的DOM节点数量
诊断内存泄漏的一个重要步骤是判断页面的DOM数量的增长情况，因此我们需要持续获取页面的DOM数量.
```js
//  递归函数
function countNodes(node) {
    //  计算自身
    var count = 1;
    //  判断是否存在子节点
    if(node.hasChildNodes()) {
        //  获取子节点
        var cnodes = node.childNodes;
        //  对子节点进行递归统计
        for(var i=0; i<cnodes.length; i++) {
            count = count + countNodes(cnodes.item(i))
        }
    }
    return count;
}
//  统计body的节点数量
countNodes(document.body)
```
