---
title: Ajax学习笔记
date: 2016-04-21 13:46:44
tags: Ajax
---


## Ajax的作用
不重新加载整个网页的情况下，对网页的某部分进行更新。
<!--more-->
## Ajax的原理
### XMLHttpRequest对象
通过在后台与服务器进行少量数据交换，使网页实现异步更新。
### 创建XMLHttpRequest对象
```javascript
var xmlhttp;
if (window.XMLHttpRequest){
	//code for IE7+,Firefox , Chrome, Opera, Safari
	xmlhttp = new XMLHttpRequest();
}else{//code for IE6,IE5
	xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
}
```
### XMLHttpRequest发送请求
两种方法
#### open(method,url,async)
规定请求的类型、url以及是否异步处理请求
method: 请求的类型;GET 或者 POST
url: 文件在服务器上的位置
async:true(异步)、false(同步)
#### send(string)
将请求发送到服务器
string:仅用于POST请求，GET请求不需填写