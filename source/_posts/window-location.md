---
title: window.location
categories: 学习
date: 2017-05-19 16:41:50
tags: [BOM]
---

# window.location
属性                  描述
hash                设置或获取 href 属性中在井号“#”后面的分段。
host                 设置或获取 location 或 URL 的 hostname 和 port 号码。
hostname      设置或获取 location 或 URL 的主机名称部分。
href                  设置或获取整个 URL 为字符串。
pathname      设置或获取对象指定的文件名或路径。
port                  设置或获取与 URL 关联的端口号码。
protocol          设置或获取 URL 的协议部分。
search            设置或获取 href 属性中跟在问号后面的部分。

<!--more-->


## window.location.pathname
设置或获取对象指定的文件名或路径。
```
例：http://localhost:8086/topic/index?topicId=361
alert(window.location.pathname); 则输出：/topic/index
```

## window.location.href
设置或获取整个 URL 为字符串。
```
例：http://localhost:8086/topic/index?topicId=361
alert(window.location.href); 则输出：http://localhost:8086/topic/index?topicId=361
```

## window.location.port
设置或获取与 URL 关联的端口号码。
```
例：http://localhost:8086/topic/index?topicId=361
alert(window.location.port); 则输出：8086
```

## window.location.protocol
设置或获取 URL 的协议部分。
```
例：http://localhost:8086/topic/index?topicId=361
console.log(window.location.protocol); 则输出：http:
```
## window.location.hash
设置或获取 href 属性中在井号“#”后面的分段。

## window.location.host
设置或获取 location 或 URL 的 hostname 和 port 号码。
```
例：http://localhost:8086/topic/index?topicId=361
console.log(window.location.host); 则输出：http:localhost:8086
```

## window.location.search
设置或获取 href 属性中跟在问号后面的部分。
```
例：http://localhost:8086/topic/index?topicId=361
console.log(window.location.search); 则输出：?topicId=361
```

