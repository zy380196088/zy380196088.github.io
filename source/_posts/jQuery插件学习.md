---
title: jQuery插件学习
date: 2016-09-05 21:12:15
tags: jQuery
---

写 jQuery 插件最核心的有如下两种放方法:
### $.extend(object) 
可以理解为 jQuery 添加一个静态方法
### $.fn.extend(object)
可以理解为 jQuery 实例添加一个方法

```javascript
//$.extend 定义与调用
$.extend({ fun1: function () { alert("执行方法一"); } });
$.fun1();
//$.fn.extend 定义与调用
$.fn.extend({ fun2: function () { alert("执行方法2"); } });
$(this).fun2();
//等同于
$.fn.fun3 = function () { alert("执行方法三"); }
$(this).fun3();
```

### jQuery(function(){}); 与(function(){})(jQuery);的区别


```javascript
jQuery(function(){});
//相当于
$(document).ready(function(){});//是某个DOM元素加载完毕后执行方法里的代码。
(function(){})();
//相当于
var fn = function($){};
fn(jQuery);
//定义了一个匿名函数，其中jQuery代表这个匿名函数的实参。通常用在JQuery插件开发中，起到了定义插件的私有域的作用
```

### 开发 jQuery 插件标准结构


```JavaScript
//step01 定义JQuery的作用域,防止$符号污染的 jQuery 插件
(function ($) {
    //step03-a 插件的默认值属性
    var defaults = {
        prevId: 'prevBtn',
        prevText: 'Previous',
        nextId: 'nextBtn',
        nextText: 'Next'
        //……
    };
    //step02 插件的扩展方法名称
    $.fn.easySlider = function (options) {
        //step03-b 合并用户自定义属性，默认属性
        var options = $.extend(defaults, options);
        //step4 支持JQuery选择器
        //step5 支持链式调用
        return this.each(function () {
        });
    }
})(jQuery);
```

1. 定义作用域;
2. 为 jQuery 扩展一个插件;
3. 设置默认值;
4. 支持jQuery 选择器
5. 支持JQuery的链接调用
6. 插件里的方法