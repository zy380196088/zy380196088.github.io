---
title: Promise和Ajax结合进行数据交互
date: 2017-12-01 15:59:56
tags:
categories:
---

<!--more-->

## Ajax问题
当处理复杂页面和很多ajax请求的时候,经常会遇到某个ajax请求依赖其他ajax请求结果的情况.当依赖比较少和简单的时候,估计大多前端小伙伴将有依赖的ajax写在依赖的ajax回调函数里;

当ajax很多,而且还设计到很复杂的事件异步操作时候,这种方法就不太合适了.

## 解决方案
### Promise

现代浏览器大都已经内置支持了Promise，连第三方库都不需要了，只有IE不行，放弃了(鄙视IE一波)

1. 改造ajax封装函数,在成功的时候调用resolve(),失败的时候调用reject(),并且返回Promise对象;
```JS
funciton _ajax(url,type,data,successFn,errorFn){
  var p = new Promise(funciton(resolve,reject){
    $.ajax({
      url:url,
      type:type==null?'POST':type,
      dataType:'json',
      data: data==null?'':JSON.stringify(data),
      async:true,
      contentType:"application/json",
      success:function(res){
        successFn(res);
        resolve();
      },
      error:function(XMLHttpRequest,textStatus,errorThrown){
          errorFn(XMLHttpRequest,textStatus,errorThrown);
          reject();
      }
    });
  });
  return p;
}
```

2. 调用示例
```JS
_ajax('/rest/commom/login','post',{"userName":'joy',"passWord":'xxxxoooo'},
loginOK(res),loginFail(errorCode,"登录失败",backLoginPage)).then(_ajax('/rest/commom/userInfo','post',{"userId":1},renderUserInfo(res),UserInfoFail(errorCode,"获取用户信息失败",backLoginPage))).then(initUserPage);
```
