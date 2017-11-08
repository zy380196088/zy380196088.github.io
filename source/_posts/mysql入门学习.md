---
title: mysql入门学习
date: 2016-05-06 10:33:48
tags:
---

## php常用的mysql函数：
### mysql_connect("主机名称/ip","用户名","密码")
```php
$link = @mysql_connect("localhost","root","root")or die("数据库连接错误").mysql_error();
```
### mysql_error()
返回上一个mysql操作的文本错误信息；@错误抑制符号

### mysql_select_db("数据库名称",$link)

### mysql_query()
向数据库发送一条sql命令

### mysql_affected_rows()
取的前一条sql语句，返回受影响的行数

### mysql_cloes()