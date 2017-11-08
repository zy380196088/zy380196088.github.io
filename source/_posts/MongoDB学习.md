---
title: MongoDB学习
categories: 学习
date: 2017-03-13 14:11:50
tags: [MongoDB]
---

MongoDB 是一个基于分布式文件存储的数据库。由 C++ 语言编写。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案。

MongoDB 是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。
<!--more-->

# 简单了解下 MongoDB
MongoDB 将数据存储为一个文档，数据结构由键值(key=>value)对组成。MongoDB 文档类似于 JSON 对象。字段值可以包含其他文档，数组及文档数组。
```JS
{
    name:"Zoey",
    age:"24",
    hobby: ["music","reading","violin"]
}
```

## MongoDB 的主要特点

1. 可以在MongoDB记录中设置任何属性的索引 (如：FirstName="Sameer",Address="8 Gandhi Road")来实现更快的排序。
2. 可以通过本地或者网络创建数据镜像，这使得MongoDB有更强的扩展性。
3. 如果负载的增加（需要更多的存储空间和更强的处理能力） ，它可以分布在计算机网络中的其他节点上这就是所谓的分片。
4. 支持丰富的查询表达式。查询指令使用JSON形式的标记，可轻易查询文档中内嵌的对象及数组。
5. 使用update()命令可以实现替换完成的文档（数据）或者一些指定的数据字段 。
6. Mongodb中的Map/reduce主要是用来对数据进行批量处理和聚合操作。Map函数调用emit(key,value)遍历集合中所有的记录，将key与value传给Reduce函数进行处理.
7. GridFS是MongoDB中的一个内置功能，可以用于存放大量小文件。
8. MongoDB允许在服务端执行脚本，可以用Javascript编写某个函数，直接在服务端执行，也可以把函数的定义存储在服务端，下次直接调用即可。

# MongoDB 常用的数据类型
| 数据类型 | 描述 |
| ----|----|----|
| String | 字符串。存储数据常用的数据类型。在 MongoDB 中，UTF-8 编码的字符串才是合法的。|
| Integer | 整型数值。用于存储数值。根据你所采用的服务器，可分为 32 位或 64 位。
| Boolean | 布尔值。用于存储布尔值（真/假）。
| Double  | 双精度浮点值。用于存储浮点值。
| Min/Max keys | 将一个值与 BSON（二进制的 JSON）元素的最低值和最高值相对比。
| Arrays | 用于将数组或列表或多个值存储为一个键。
| Timestamp | 时间戳。记录文档修改或添加的具体时间。
| Object | 用于内嵌文档。
| Null  | 用于创建空值。
| Symbol |符号。该数据类型基本上等同于字符串类型，但不同的是，它一般用于采用特殊符号类型的语言。
| Date  |  日期时间。用 UNIX 时间格式来存储当前日期或时间。你可以指定自己的日期时间：创建 Date 对象，传入年月日信息。
| Object ID |  对象 ID。用于创建文档的 ID。
| Binary Data 二进制数据。用于存储二进制数据。
| Code | 代码类型。用于在文档中存储 JavaScript 代码。
| Regular expression | 正则表达式类型。用于存储正则表达式。

# 连接

## 启动 MongoDB服务
在MongoDB安装目录的bin目录下执行'mongod'即可。

执行启动操作后，mongodb在输出一些必要信息后不会输出任何信息，之后就等待连接的建立，当连接被建立后，就会开始打印日志信息。

你可以使用 MongoDB shell 来连接 MongoDB 服务器。你也可以使用 PHP 来连接 MongoDB。本教程我们会使用 MongoDB shell 来连接 Mongodb 服务，之后的章节我们将会介绍如何通过php 来连接MongoDB服务。


## 通过shell连接MongoDB服务
你可以通过执行以下命令来连接MongoDB的服务。

注意：localhost为主机名，这个选项是必须的：
``mongodb://localhost``
当你执行以上命令时，你可以看到以下输出结果：
```
$ ./mongo
MongoDB shell version: 3.0.6
connecting to: test
> mongodb://localhostmongodb://localhost
...
```
这时候你返回查看运行 ./mongod 命令的窗口，可以看到是从哪里连接到MongoDB的服务器，您可以看到如下信息：
```
……省略信息……
2015-09-25T17:22:27.336+0800 I CONTROL  [initandlisten] allocator: tcmalloc
2015-09-25T17:22:27.336+0800 I CONTROL  [initandlisten] options: { storage: { dbPath: "/data/db" } }
2015-09-25T17:22:27.350+0800 I NETWORK  [initandlisten] waiting for connections on port 27017
2015-09-25T17:22:36.012+0800 I NETWORK  [initandlisten] connection accepted from 127.0.0.1:37310 #1 (1 connection now open)  # 该行表明一个来自本机的连接
……省略信息……
```

## MongoDB连接命令格式
使用用户名和密码连接到MongoDB服务器，你必须使用 'username:password@hostname/dbname' 格式，'username'为用户名，'password' 为密码。

使用用户名和密码连接登陆到默认数据库：
```
$ ./mongo
MongoDB shell version: 3.0.6
connecting to: test
mongodb://admin:123456@localhost/
```
以上命令中，用户 admin 使用密码 123456 连接到本地的 MongoDB 服务上。输出结果如下所示：
```
> mongodb://admin:123456@localhost/
...
```
使用用户名和密码连接登陆到指定数据库：

连接到指定数据库的格式如下：

mongodb://admin:123456@localhost/test

## 更多连接实例
连接本地数据库服务器，端口是默认的。

mongodb://localhost
使用用户名fred，密码foobar登录localhost的admin数据库。

mongodb://fred:foobar@localhost
使用用户名fred，密码foobar登录localhost的baz数据库。

mongodb://fred:foobar@localhost/baz
连接 replica pair, 服务器1为example1.com服务器2为example2。

mongodb://example1.com:27017,example2.com:27017
连接 replica set 三台服务器 (端口 27017, 27018, 和27019):

mongodb://localhost,localhost:27018,localhost:27019
连接 replica set 三台服务器, 写入操作应用在主服务器 并且分布查询到从服务器。

mongodb://host1,host2,host3/?slaveOk=true
直接连接第一个服务器，无论是replica set一部分或者主服务器或者从服务器。

mongodb://host1,host2,host3/?connect=direct;slaveOk=true
当你的连接服务器有优先级，还需要列出所有服务器，你可以使用上述连接方式。

安全模式连接到localhost:

mongodb://localhost/?safe=true
以安全模式连接到replica set，并且等待至少两个复制服务器成功写入，超时时间设置为2秒。

mongodb://host1,host2,host3/?safe=true;w=2;wtimeoutMS=2000

## 参数选项说明
标准格式：

mongodb://[username:password@]host1[:port1][,host2[:port2],...[,hostN[:portN]]][/[database][?options]]
标准的连接格式包含了多个选项(options)，如下所示：

|选项 | 描述 |
|----|----|
| replicaSet=name | 验证replica set的名称。 Impliesconnect=replicaSet.
| slaveOk=true/false | true在connect=direct模式下，驱动会连接第一台机器，即使这台服务器不是主。在connect=replicaSet模式下，驱动会发送所有的写请求到主并且把读取操作分布在其他从服务器。false 在 connect=direct模式下，驱动会自动找寻主服务器. 在connect=replicaSet 模式下，驱动仅仅连接主服务器，并且所有的读写命令都连接到主服务器。|
| safe=true/false |true:在执行更新操作之后，驱动都会发送getLastError命令来确保更新成功。(还要参考 wtimeoutMS).false: 在每次更新之后，驱动不会发送getLastError来确保更新成功。|
| w=n | 驱动添加 { w : n } 到getLastError命令. 应用于safe=true。|
| wtimeoutMS=ms | 驱动添加 { wtimeout : ms } 到 getlasterror 命令. 应用于 safe=true. true: 驱动添加 { fsync : true } 到 getlasterror 命令.应用于 safe=true.false: 驱动不会添加到getLastError命令中。|
| journal=true/false | 如果设置为 true, 同步到 journal (在提交到数据库前写入到实体中). 应用于 safe=true |
| connectTimeoutMS=ms | 可以打开连接的时间。
| socketTimeoutMS=ms | 发送和接受sockets的时间。

# 创建数据库
## 语法
MongoDB 创建数据库的语法格式如下：
``use DATABASE_NAME``
如果数据库不存在，则创建数据库，否则切换到指定数据库。

## 实例
以下实例我们创建了数据库 runoob:
```
> use runoob
switched to db runoob
> db
runoob
> 
```
如果你想查看所有数据库，可以使用 show dbs 命令：
```
> show dbs
local  0.078GB
test   0.078GB
> 
```
可以看到，我们刚创建的数据库 runoob 并不在数据库的列表中， 要显示它，我们需要向 runoob 数据库插入一些数据。
```
> db.runoob.insert({"name":"菜鸟教程"})
WriteResult({ "nInserted" : 1 })
> show dbs
local   0.078GB
runoob  0.078GB
test    0.078GB
> 
```
MongoDB 中默认的数据库为 test，如果你没有创建新的数据库，集合将存放在 test 数据库中

# 删除数据库
## 语法
MongoDB 删除数据库的语法格式如下：
``db.dropDatabase()``
删除当前数据库，默认为 test，你可以使用 db 命令查看当前数据库名。

## 实例
以下实例我们删除了数据库 runoob。

首先，查看所有数据库：
```
> show dbs
local   0.078GB
runoob  0.078GB
test    0.078GB
```
接下来我们切换到数据库 runoob：
```
> use runoob
switched to db runoob
> 
```
执行删除命令：
```
> db.dropDatabase()
{ "dropped" : "runoob", "ok" : 1 }
```
最后，我们再通过 show dbs 命令数据库是否删除成功：
```
> show dbs
local  0.078GB
test   0.078GB
> 
```

# 插入文档

文档的数据结构和JSON基本一样。
所有存储在集合中的数据都是BSON格式。

BSON是一种类json的一种二进制形式的存储格式,简称Binary JSON。

## 插入文档
MongoDB 使用 insert() 或 save() 方法向集合中插入文档，语法如下：
``db.COLLECTION_NAME.insert(document)``
## 实例
以下文档可以存储在 MongoDB 的 runoob 数据库 的 col集合中：
```
>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```
以上实例中 col 是我们的集合名，前一章节我们已经创建过了，如果该集合不在该数据库中， MongoDB 会自动创建该集合比插入文档。

查看已插入文档：
```
> db.col.find()
{ "_id" : ObjectId("56064886ade2f21f36b03134"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }
> 
```
我们也可以将数据定义为一个变量，如下所示：
```
> document=({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
});
```
执行后显示结果如下：
```
{
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "菜鸟教程",
        "url" : "http://www.runoob.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```
执行插入操作：
```
> db.col.insert(document)
WriteResult({ "nInserted" : 1 })
> 
```
插入文档你也可以使用 db.col.save(document) 命令。如果不指定 _id 字段 save() 方法类似于 insert() 方法。如果指定 _id 字段，则会更新该 _id 的数据。

# 更新文档

## update() 方法
update() 方法用于更新已存在的文档。语法格式如下：
```
db.collection.update(
   <query>,
   <update>,
   {
     upsert: <boolean>,
     multi: <boolean>,
     writeConcern: <document>
   }
)
```
参数说明：
query : update的查询条件，类似sql update查询内where后面的。
update : update的对象和一些更新的操作符（如$,$inc...）等，也可以理解为sql update查询内set后面的
upsert : 可选，这个参数的意思是，如果不存在update的记录，是否插入objNew,true为插入，默认是false，不插入。
multi : 可选，mongodb 默认是false,只更新找到的第一条记录，如果这个参数为true,就把按条件查出来多条记录全部更新。
writeConcern :可选，抛出异常的级别。
## 实例
我们在集合 col 中插入如下数据：
```
>db.col.insert({
    title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```
接着我们通过 update() 方法来更新标题(title):
```
>db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}})
WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })   # 输出信息
> db.col.find().pretty()
{
        "_id" : ObjectId("56064f89ade2f21f36b03136"),
        "title" : "MongoDB",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "菜鸟教程",
        "url" : "http://www.runoob.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
>
```
可以看到标题(title)由原来的 "MongoDB 教程" 更新为了 "MongoDB"。
以上语句只会修改第一条发现的文档，如果你要修改多条相同的文档，则需要设置 multi 参数为 true。
```
>db.col.update({'title':'MongoDB 教程'},{$set:{'title':'MongoDB'}},{multi:true})
save() 方法
save() 方法通过传入的文档来替换已有文档。语法格式如下：

db.collection.save(
   <document>,
   {
     writeConcern: <document>
   }
)
```
参数说明：
document : 文档数据。
writeConcern :可选，抛出异常的级别。

## 实例
以下实例中我们替换了 _id 为 56064f89ade2f21f36b03136 的文档数据：
```
>db.col.save({
    "_id" : ObjectId("56064f89ade2f21f36b03136"),
    "title" : "MongoDB",
    "description" : "MongoDB 是一个 Nosql 数据库",
    "by" : "Runoob",
    "url" : "http://www.runoob.com",
    "tags" : [
            "mongodb",
            "NoSQL"
    ],
    "likes" : 110
})
```
替换成功后，我们可以通过 find() 命令来查看替换后的数据
```
>db.col.find().pretty()
{
        "_id" : ObjectId("56064f89ade2f21f36b03136"),
        "title" : "MongoDB",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "Runoob",
        "url" : "http://www.runoob.com",
        "tags" : [
                "mongodb",
                "NoSQL"
        ],
        "likes" : 110
}
> 
```

## 更多实例
只更新第一条记录：
``db.col.update( { "count" : { $gt : 1 } } , { $set : { "test2" : "OK"} } );``

全部更新：
``db.col.update( { "count" : { $gt : 3 } } , { $set : { "test2" : "OK"} },false,true );``

只添加第一条：
``db.col.update( { "count" : { $gt : 4 } } , { $set : { "test5" : "OK"} },true,false );``

全部添加加进去:
``db.col.update( { "count" : { $gt : 5 } } , { $set : { "test5" : "OK"} },true,true );``

全部更新：
``db.col.update( { "count" : { $gt : 15 } } , { $inc : { "count" : 1} },false,true );``

只更新第一条记录：
``db.col.update( { "count" : { $gt : 10 } } , { $inc : { "count" : 1} },false,false );``

# 删除文档

## 语法
remove() 方法的基本语法格式如下所示：
```
db.collection.remove(
   <query>,
   <justOne>
)
```
如果你的 MongoDB 是 2.6 版本以后的，语法格式如下：
```
db.collection.remove(
   <query>,
   {
     justOne: <boolean>,
     writeConcern: <document>
   }
)
```
参数说明：
query :（可选）删除的文档的条件。
justOne : （可选）如果设为 true 或 1，则只删除一个文档。
writeConcern :（可选）抛出异常的级别。
## 实例
以下文档我们执行两次插入操作：
```
>db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb', 'database', 'NoSQL'],
    likes: 100
})
```
使用 find() 函数查询数据：
```
> db.col.find()
{ "_id" : ObjectId("56066169ade2f21f36b03137"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }
{ "_id" : ObjectId("5606616dade2f21f36b03138"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb", "database", "NoSQL" ], "likes" : 100 }
```
接下来我们移除 title 为 'MongoDB 教程' 的文档：
```
>db.col.remove({'title':'MongoDB 教程'})
WriteResult({ "nRemoved" : 2 })           # 删除了两条数据
>db.col.find()
……                                        # 没有数据
```
如果你只想删除第一条找到的记录可以设置 justOne 为 1，如下所示：
```
>db.COLLECTION_NAME.remove(DELETION_CRITERIA,1)
```
如果你想删除所有数据，可以使用以下方式（类似常规 SQL 的 truncate 命令）：
```
>db.col.remove({})
>db.col.find()
>
```

# 查询文档

## 语法
MongoDB 查询数据的语法格式如下：
```
>db.COLLECTION_NAME.find()
```
find() 方法以非结构化的方式来显示所有文档。
如果你需要以易读的方式来读取数据，可以使用 pretty() 方法，语法格式如下：
```
>db.col.find().pretty()
```
pretty() 方法以格式化的方式来显示所有文档。

## 实例
以下实例我们查询了集合 col 中的数据：
```
> db.col.find().pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "菜鸟教程",
        "url" : "http://www.runoob.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```
除了 find() 方法之外，还有一个 findOne() 方法，它只返回一个文档。

## MongoDB 与 RDBMS Where 语句比较
如果你熟悉常规的 SQL 数据，通过下表可以更好的理解 MongoDB 的条件语句查询：

|操作 | 格式 | 范例 | RDBMS中的类似语句|
|----|----|----|----|
|等于 | {<key>:<value>} | db.col.find({"by":"菜鸟教程"}).pretty() | where by = '菜鸟教程'|
|小于 | {<key>:{$lt:<value>}} | db.col.find({"likes":{$lt:50}}).pretty() | where likes < 50
| 小于或等于 |{<key>:{$lte:<value>}} |db.col.find({"likes":{$lte:50}}).pretty() | where likes <= 50
| 大于 | {<key>:{$gt:<value>}} |db.col.find({"likes":{$gt:50}}).pretty() |   where likes > 50
| 大于或等于 | {<key>:{$gte:<value>}} |db.col.find({"likes":{$gte:50}}).pretty()| where likes >= 50
| 不等于 |{<key>:{$ne:<value>}} |  db.col.find({"likes":{$ne:50}}).pretty() |where likes != 50

## MongoDB AND 条件
MongoDB 的 find() 方法可以传入多个键(key)，每个键(key)以逗号隔开，及常规 SQL 的 AND 条件。

语法格式如下：
```
>db.col.find({key1:value1, key2:value2}).pretty()
```
## 实例
以下实例通过 by 和 title 键来查询 菜鸟教程 中 MongoDB 教程 的数据
```
> db.col.find({"by":"菜鸟教程", "title":"MongoDB 教程"}).pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "菜鸟教程",
        "url" : "http://www.runoob.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```
以上实例中类似于 WHERE 语句：WHERE by='菜鸟教程' AND title='MongoDB 教程'

## MongoDB OR 条件
MongoDB OR 条件语句使用了关键字 $or,语法格式如下：
```
>db.col.find(
   {
      $or: [
         {key1: value1}, {key2:value2}
      ]
   }
).pretty()
```
## 实例
以下实例中，我们演示了查询键 by 值为 菜鸟教程 或键 title 值为 MongoDB 教程 的文档。
```
>db.col.find({$or:[{"by":"菜鸟教程"},{"title": "MongoDB 教程"}]}).pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "菜鸟教程",
        "url" : "http://www.runoob.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
>
```

## AND 和 OR 联合使用
以下实例演示了 AND 和 OR 联合使用，类似常规 SQL 语句为： 'where likes>50 AND (by = '菜鸟教程' OR title = 'MongoDB 教程')'
```
>db.col.find({"likes": {$gt:50}, $or: [{"by": "菜鸟教程"},{"title": "MongoDB 教程"}]}).pretty()
{
        "_id" : ObjectId("56063f17ade2f21f36b03133"),
        "title" : "MongoDB 教程",
        "description" : "MongoDB 是一个 Nosql 数据库",
        "by" : "菜鸟教程",
        "url" : "http://www.runoob.com",
        "tags" : [
                "mongodb",
                "database",
                "NoSQL"
        ],
        "likes" : 100
}
```


# 条件操作符
条件操作符用于比较两个表达式并从mongoDB集合中获取数据。

我们使用的数据库名称为"runoob" 我们的集合名称为"col"，以下为我们插入的数据。
为了方便测试，我们可以先使用以下命令清空集合 "col" 的数据：

`` db.col.remove({})db.col.remove({})``

插入以下数据
```
>db.col.insert({
    title: 'PHP 教程', 
    description: 'PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['php'],
    likes: 200
})
>db.col.insert({title: 'Java 教程', 
    description: 'Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['java'],
    likes: 150
})
db.col.insert({title: 'MongoDB 教程', 
    description: 'MongoDB 是一个 Nosql 数据库',
    by: '菜鸟教程',
    url: 'http://www.runoob.com',
    tags: ['mongodb'],
    likes: 100
})
```

使用*find()**命令查看数据：

```
> db.col.find()
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb" ], "likes" : 100 }

```

## MongoDB (>) 大于操作符 - $gt
如果你想获取 "col" 集合中 "likes" 大于 100 的数据，你可以使用以下命令：
``db.col.find({"likes" : {$gt : 100}})``
类似于SQL语句：
``Select * from col where likes > 100;``
输出结果：
```
> db.col.find({"likes" : {$gt : 100}})
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "java" ], "likes" : 150 }
> 
```

## MongoDB（>=）大于等于操作符 - $gte
如果你想获取"col"集合中 "likes" 大于等于 100 的数据，你可以使用以下命令：
``db.col.find({likes : {$gte : 100}})``
类似于SQL语句：
``Select * from col where likes >=100;``
输出结果：
```
> db.col.find({likes : {$gte : 100}})
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb" ], "likes" : 100 }
> 
```

## MongoDB (<) 小于操作符 - $lt
如果你想获取"col"集合中 "likes" 小于 150 的数据，你可以使用以下命令：
``db.col.find({likes : {$lt : 150}})``
类似于SQL语句：
``Select * from col where likes < 150;``
输出结果：
```
> db.col.find({likes : {$lt : 150}})
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb" ], "likes" : 100 }
```

## MongoDB (<=) 小于操作符 - $lte
如果你想获取"col"集合中 "likes" 小于等于 150 的数据，你可以使用以下命令：
``db.col.find({likes : {$lte : 150}})``
类似于SQL语句：
``Select * from col where likes <= 150;``
输出结果：
```
> db.col.find({likes : {$lte : 150}})
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb" ], "likes" : 100 }
```

## MongoDB 使用 (<) 和 (>) 查询 - $lt 和 $gt
如果你想获取"col"集合中 "likes" 大于100，小于 200 的数据，你可以使用以下命令：
``db.col.find({likes : {$lt :200, $gt : 100}})``
类似于SQL语句:
``Select * from col where likes>100 AND  likes<200;``
输出结果：
```
> db.col.find({likes : {$lt :200, $gt : 100}})
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "java" ], "likes" : 150 }
>
```

# $type 操作符
$type操作符是基于BSON类型来检索集合中匹配的数据类型，并返回结果。
$type操作符是基于BSON类型来检索集合中匹配的数据类型，并返回结果。
MongoDB 中可以使用的类型如下表所示：
|类型|数字|备注|
|---|---|---|
| Double | 1 ||
| String | 2 |
| Object | 3||
| Array | 4||
| Binary data | 5 ||
| Undefined | 6 | 已废弃。|
| Object id |  7| |
| Boolean | 8 |
| Date | 9 | |
| Null | 10| |
| Regular Expression | 11||
| JavaScript | 13 | |
| Symbol | 14 | |
| JavaScript (with scope) | 15 |
| 32-bit integer | 16 ||
| Timestamp |  17 ||
| 64-bit integer | 18 |
| Min key | 255 | Query with -1.|
| Max key | 127 |

## 实例
如果想获取 "col" 集合中 title 为 String 的数据，你可以使用以下命令：
``db.col.find({"title" : {$type : 2}})``

# MongoDB Limit与Skip方法
## MongoDB Limit() 方法
如果你需要在MongoDB中读取指定数量的数据记录，可以使用MongoDB的Limit方法，limit()方法接受一个数字参数，该参数指定从MongoDB中读取的记录条数。
### 语法
``db.COLLECTION_NAME.find().limit(NUMBER)``
注：如果你们没有指定limit()方法中的参数则显示集合中的所有数据。
### 实例
集合 col 中的数据如下：
```
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb" ], "likes" : 100 }
```
以下实例为显示查询文档中的两条记录：
```
> db.col.find({},{"title":1,_id:0}).limit(2)
{ "title" : "PHP 教程" }
{ "title" : "Java 教程" }
>
```

## MongoDB Skip() 方法
我们除了可以使用limit()方法来读取指定数量的数据外，还可以使用skip()方法来跳过指定数量的数据，skip方法同样接受一个数字参数作为跳过的记录条数。
### 语法
``db.COLLECTION_NAME.find().limit(NUMBER).skip(NUMBER)``
### 实例
以上实例只会显示第二条文档数据:注:skip()方法默认参数为 0 。

```
>db.col.find({},{"title":1,_id:0}).limit(1).skip(1)
{ "title" : "Java 教程" }
>
```
注:skip()方法默认参数为 0 。

# 排序
## MongoDB sort()方法
### 语法
``db.COLLECTION_NAME.find().sort({KEY:1})``
### 实例
col 集合中的数据如下：
```
{ "_id" : ObjectId("56066542ade2f21f36b0313a"), "title" : "PHP 教程", "description" : "PHP 是一种创建动态交互性站点的强有力的服务器端脚本语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "php" ], "likes" : 200 }
{ "_id" : ObjectId("56066549ade2f21f36b0313b"), "title" : "Java 教程", "description" : "Java 是由Sun Microsystems公司于1995年5月推出的高级程序设计语言。", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "java" ], "likes" : 150 }
{ "_id" : ObjectId("5606654fade2f21f36b0313c"), "title" : "MongoDB 教程", "description" : "MongoDB 是一个 Nosql 数据库", "by" : "菜鸟教程", "url" : "http://www.runoob.com", "tags" : [ "mongodb" ], "likes" : 100 }
```
以下实例演示了 col 集合中的数据按字段 likes 的降序排列：
```
>db.col.find({},{"title":1,_id:0}).sort({"likes":-1})
{ "title" : "PHP 教程" }
{ "title" : "Java 教程" }
{ "title" : "MongoDB 教程" }
>
```
注： 如果没有指定sort()方法的排序方式，默认按照文档的升序排列。

# 索引
索引通常能够极大的提高查询的效率，如果没有索引，MongoDB在读取数据时必须扫描集合中的每个文件并选取那些符合查询条件的记录。

这种扫描全集合的查询效率是非常低的，特别在处理大量的数据时，查询可以要花费几十秒甚至几分钟，这对网站的性能是非常致命的。

索引是特殊的数据结构，索引存储在一个易于遍历读取的数据集合中，索引是对数据库表中一列或多列的值进行排序的一种结构

## ensureIndex() 方法
### 语法
``db.COLLECTION_NAME.ensureIndex({KEY:1})``
语法中 Key 值为你要创建的索引字段，1为指定按升序创建索引，如果你想按降序来创建索引指定为-1即可。
ensureIndex() 方法中你也可以设置使用多个字段创建索引（关系型数据库中称作复合索引）。
ensureIndex() 接收可选参数，可选参数列表如下：
| Parameter  | Type  |  Description |
| ---------- |-------| -------------|
| background | Boolean | 建索引过程会阻塞其它数据库操作，background可指定以后台方式创建索引，即增加 "background" 可选参数。 "background" 默认值为false。|
| unique | Boolean | 建立的索引是否唯一。指定为true创建唯一索引。默认值为false.|
| name  | string |索引的名称。如果未指定，MongoDB的通过连接索引的字段名和排序顺序生成一个索引名称。|
| dropDups  |  Boolean  |在建立唯一索引时是否删除重复记录,指定 true 创建唯一索引。默认值为 false. |
| sparse | Boolean | 对文档中不存在的字段数据不启用索引；这个参数需要特别注意，如果设置为true的话，在索引字段中不会查询出不包含对应字段的文档.。默认值为 false.|
| expireAfterSeconds | integer 指定一个以秒为单位的数值，完成 TTL设定，设定集合的生存时间。|
| v | index version | 索引的版本号。默认的索引版本取决于mongod创建索引时运行的版本。|
| weights | document |  索引权重值，数值在 1 到 99,999 之间，表示该索引相对于其他索引字段的得分权重。|
| default_language | string | 对于文本索引，该参数决定了停用词及词干和词器的规则的列表。 默认为英语 |
| language_override | string |对于文本索引，该参数指定了包含在文档中的字段名，语言覆盖默认的language，默认值为 language. |

### 实例
在后台创建索引：
``db.values.ensureIndex({open: 1, close: 1}, {background: true})``
通过在创建索引时加background:true 的选项，让创建工作在后台执行

# 聚合
MongoDB中聚合(aggregate)主要用于处理数据(诸如统计平均值,求和等)，并返回计算后的数据结果。有点类似sql语句中的 count(*)。

## aggregate() 方法
### 语法
``>db.COLLECTION_NAME.aggregate(AGGREGATE_OPERATION)``
### 实例
集合中的数据如下：
```
{
   _id: ObjectId(7df78ad8902c)
   title: 'MongoDB Overview', 
   description: 'MongoDB is no sql database',
   by_user: 'w3cschool.cc',
   url: 'http://www.w3cschool.cc',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 100
},
{
   _id: ObjectId(7df78ad8902d)
   title: 'NoSQL Overview', 
   description: 'No sql database is very fast',
   by_user: 'w3cschool.cc',
   url: 'http://www.w3cschool.cc',
   tags: ['mongodb', 'database', 'NoSQL'],
   likes: 10
},
{
   _id: ObjectId(7df78ad8902e)
   title: 'Neo4j Overview', 
   description: 'Neo4j is no sql database',
   by_user: 'Neo4j',
   url: 'http://www.neo4j.com',
   tags: ['neo4j', 'database', 'NoSQL'],
   likes: 750
},
```
现在我们通过以上集合计算每个作者所写的文章数，使用aggregate()计算结果如下：
```
> db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : 1}}}])
{
   "result" : [
      {
         "_id" : "w3cschool.cc",
         "num_tutorial" : 2
      },
      {
         "_id" : "Neo4j",
         "num_tutorial" : 1
      }
   ],
   "ok" : 1
}
>
```
以上实例类似sql语句： select by_user, count(*) from mycol group by by_user

在上面的例子中，我们通过字段by_user字段对数据进行分组，并计算by_user字段相同值的总和。
下表展示了一些聚合的表达式:
| 表达式 | 描述 | 实例 |
| --- | --- | --- |
| $sum  |  计算总和。| db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$sum : "$likes"}}}])|
| $avg  |  计算平均值 |  db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$avg : "$likes"}}}])|
| $min  |  获取集合中所有文档对应值得最小值。| db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$min : "$likes"}}}])|
| $max  |  获取集合中所有文档对应值得最大值。|db.mycol.aggregate([{$group : {_id : "$by_user", num_tutorial : {$max : "$likes"}}}]) |
| $push |  在结果文档中插入值到一个数组中。| db.mycol.aggregate([{$group : {_id : "$by_user", url : {$push: "$url"}}}])|
| $addToSet  | 在结果文档中插入值到一个数组中，但不创建副本。| db.mycol.aggregate([{$group : {_id : "$by_user", url : {$addToSet : "$url"}}}])|
| $first | 根据资源文档的排序获取第一个文档数据。| db.mycol.aggregate([{$group : {_id : "$by_user", first_url : {$first : "$url"}}}])|
| $last |  根据资源文档的排序获取最后一个文档数据| db.mycol.aggregate([{$group : {_id : "$by_user", last_url : {$last : "$url"}}}])|

## 管道
管道在Unix和Linux中一般用于将当前命令的输出结果作为下一个命令的参数。

MongoDB的聚合管道将MongoDB文档在一个管道处理完毕后将结果传递给下一个管道处理。管道操作是可以重复的。

表达式：处理输入文档并输出。表达式是无状态的，只能用于计算当前聚合管道的文档，不能处理其它的文档。

这里我们介绍一下聚合框架中常用的几个操作：

>$project：修改输入文档的结构。可以用来重命名、增加或删除域，也可以用于创建计算结果以及嵌套文档。
>$match：用于过滤数据，只输出符合条件的文档。$match使用MongoDB的标准查询操作。
>$limit：用来限制MongoDB聚合管道返回的文档数。
>$skip：在聚合管道中跳过指定数量的文档，并返回余下的文档。
>$unwind：将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值。
>$group：将集合中的文档分组，可用于统计结果。
>$sort：将输入文档排序后输出。
>$geoNear：输出接近某一地理位置的有序文档。

### 管道操作符实例
1、$project实例
```
db.article.aggregate(
    { $project : {
        title : 1 ,
        author : 1 ,
    }}
 );
```
这样的话结果中就只还有_id,tilte和author三个字段了，默认情况下_id字段是被包含的，如果要想不包含_id话可以这样:
```
db.article.aggregate(
    { $project : {
        _id : 0 ,
        title : 1 ,
        author : 1
    }});
```
2.$match实例
```
db.articles.aggregate( [
                        { $match : { score : { $gt : 70, $lte : 90 } } },
                        { $group: { _id: null, count: { $sum: 1 } } }
                       ] );
```
$match用于获取分数大于70小于或等于90记录，然后将符合条件的记录送到下一阶段$group管道操作符进行处理。

3.$skip实例
```
db.article.aggregate(
    { $skip : 5 });
```
经过$skip管道操作符处理后，前五个文档被"过滤"掉。


# 复制(副本集)
MongoDB复制是将数据同步在多个服务器的过程。
复制提供了数据的冗余备份，并在多个服务器上存储数据副本，提高了数据的可用性， 并可以保证数据的安全性。
复制还允许您从硬件故障和服务中断中恢复数据。

## 什么是复制?
保障数据的安全性
数据高可用性 (24*7)
灾难恢复
无需停机维护（如备份，重建索引，压缩）
分布式读取数据
MongoDB复制原理
mongodb的复制至少需要两个节点。其中一个是主节点，负责处理客户端请求，其余的都是从节点，负责复制主节点上的数据。

mongodb各个节点常见的搭配方式为：**一主一从、一主多从。**

主节点记录在其上的所有操作oplog，从节点定期轮询主节点获取这些操作，然后对自己的数据副本执行这些操作，从而保证从节点的数据与主节点一致。

MongoDB复制结构图：

以上结构图总，客户端总主节点读取数据，在客户端写入数据到主节点是， 主节点与从节点进行数据交互保障数据的一致性。

## 副本集特征
1. N 个节点的集群
2. 任何节点可作为主节点
3. 所有写入操作都在主节点上
4. 自动故障转移
5. 自动恢复

## MongoDB副本集设置
在本教程中我们使用同一个MongoDB来做MongoDB主从的实验， 操作步骤如下：

1、关闭正在运行的MongoDB服务器。
现在我们通过指定 --replSet 选项来启动mongoDB。--replSet 基本语法格式如下：
```
mongod --port "PORT" --dbpath "YOUR_DB_DATA_PATH" --replSet "REPLICA_SET_INSTANCE_NAME"
```

### 实例
```
mongod --port 27017 --dbpath "D:\set up\mongodb\data" --replSet rs0
```
以上实例会启动一个名为rs0的MongoDB实例，其端口号为27017。
启动后打开命令提示框并连接上mongoDB服务。
在Mongo客户端使用命令rs.initiate()来启动一个新的副本集。
我们可以使用rs.conf()来查看副本集的配置
查看副本集姿态使用 rs.status() 命令

## 副本集添加成员
添加副本集的成员，我们需要使用多条服务器来启动mongo服务。进入Mongo客户端，并使用rs.add()方法来添加副本集的成员。

## 语法
rs.add() 命令基本语法格式如下：
```
>rs.add(HOST_NAME:PORT)
```
## 实例
假设你已经启动了一个名为mongod1.net，端口号为27017的Mongo服务。 在客户端命令窗口使用rs.add() 命令将其添加到副本集中，命令如下所示：
```
rs.add("mongod1.net:27017")
```
MongoDB中你只能通过主节点将Mongo服务添加到副本集中， 判断当前运行的Mongo服务是否为主节点可以使用命令db.isMaster() 。

MongoDB的副本集与我们常见的主从有所不同，主从在主机宕机后所有服务将停止，而副本集在主机宕机后，副本会接管主节点成为主节点，不会出现宕机的情况。

# 分片
![MongoDB分片集群结构分布](images/MongoDB-sharding.png)

上图中主要有如下所述的三个主要组件:
1. **Shard**
用于存储实际的数据块,实际生产环境中一个 shard server 角色,可由几台机器组合一个 relica set 承担,防止主机单点故障.
2. **Config Server**
mongod 实例,存储了整个 ClusterMetadata,其中包括 chunk 信息.
3. **Query Routers**
前端路由,客户端由此接入,且让整个集群看上去像单一数据库,前端应用可以透明使用.

## 分片实例
分片结构端口分布如下：
```shell
Shard Server 1：27020
Shard Server 2：27021
Shard Server 3：27022
Shard Server 4：27023
Config Server ：27100
Route Process：40000
```

步骤一：启动Shard Server
```shell
[root@100 /]# mkdir -p /www/mongoDB/shard/s0
[root@100 /]# mkdir -p /www/mongoDB/shard/s1
[root@100 /]# mkdir -p /www/mongoDB/shard/s2
[root@100 /]# mkdir -p /www/mongoDB/shard/s3
[root@100 /]# mkdir -p /www/mongoDB/shard/log
[root@100 /]# /usr/local/mongoDB/bin/mongod --port 27020 --dbpath=/www/mongoDB/shard/s0 --logpath=/www/mongoDB/shard/log/s0.log --logappend --fork
....
[root@100 /]# /usr/local/mongoDB/bin/mongod --port 27023 --dbpath=/www/mongoDB/shard/s3 --logpath=/www/mongoDB/shard/log/s3.log --logappend --fork
```

步骤二： 启动Config Server
```
[root@100 /]# mkdir -p /www/mongoDB/shard/config
[root@100 /]# /usr/local/mongoDB/bin/mongod --port 27100 --dbpath=/www/mongoDB/shard/config --logpath=/www/mongoDB/shard/log/config.log --logappend --fork
注意：这里我们完全可以像启动普通mongodb服务一样启动，不需要添加—shardsvr和configsvr参数。因为这两个参数的作用就是改变启动端口的，所以我们自行指定了端口就可以。
```

步骤三： 启动Route Process
```
/usr/local/mongoDB/bin/mongos --port 40000 --configdb localhost:27100 --fork --logpath=/www/mongoDB/shard/log/route.log --chunkSize 500
mongos启动参数中，chunkSize这一项是用来指定chunk的大小的，单位是MB，默认大小为200MB.
```

步骤四： 配置Sharding
接下来，我们使用MongoDB Shell登录到mongos，添加Shard节点
```
[root@100 shard]# /usr/local/mongoDB/bin/mongo admin --port 40000
MongoDB shell version: 2.0.7
connecting to: 127.0.0.1:40000/admin
mongos> db.runCommand({ addshard:"localhost:27020" })
{ "shardAdded" : "shard0000", "ok" : 1 }
......
mongos> db.runCommand({ addshard:"localhost:27029" })
{ "shardAdded" : "shard0009", "ok" : 1 }
mongos> db.runCommand({ enablesharding:"test" }) #设置分片存储的数据库
{ "ok" : 1 }
mongos> db.runCommand({ shardcollection: "test.log", key: { id:1,time:1}})
{ "collectionsharded" : "test.log", "ok" : 1 }
步骤五： 程序代码内无需太大更改，直接按照连接普通的mongo数据库那样，将数据库连接接入接口40000
```


# 备份与恢复

## MongoDB数据备份
在Mongodb中我们使用mongodump命令来备份MongoDB数据。该命令可以导出所有数据到指定目录中。

mongodump命令可以通过参数指定导出的数据量级转存的服务器。

### 语法
mongodump命令脚本语法如下：
```
>mongodump -h dbhost -d dbname -o dbdirectory
```

>-h：
MongDB所在服务器地址，例如：127.0.0.1，当然也可以指定端口号：127.0.0.1:27017

>-d：
需要备份的数据库实例，例如：test

>-o：
备份的数据存放位置，例如：c:\data\dump，当然该目录需要提前建立，在备份完成后，系统自动在dump目录下建立一个test目录，这个目录里面存放该数据库实例的备份数据。

### 实例
在本地使用 27017 启动你的mongod服务。打开命令提示符窗口，进入MongoDB安装目录的bin目录输入命令mongodump:
```
>mongodump
```

执行以上命令后，客户端会连接到ip为 127.0.0.1 端口号为 27017 的MongoDB服务上，并备份所有数据到 bin/dump/ 目录中。

mongodump 命令可选参数列表如下所示：

| 语法 | 描述 | 实例 |
| --- | --- | --- |
|mongodump --host HOST_NAME --port PORT_NUMBER|该命令将备份所有MongoDB数据|mongodump --host w3cschool.cc --port 27017|
|mongodump --dbpath DB_PATH --out BACKUP_DIRECTORY||mongodump --dbpath /data/db/ --out /data/backup/|
|mongodump --collection COLLECTION --db DB_NAME|该命令将备份指定数据库的集合。|mongodump --dbpath /data/db/ --out /data/backup/|

## MongoDB数据恢复

mongodb使用 mongorerstore 命令来恢复备份的数据。

### 语法
mongorestore命令脚本语法如下：
```
>mongorestore -h dbhost -d dbname --directoryperdb dbdirectory
```
>-h：
MongoDB所在服务器地址

>-d：
需要恢复的数据库实例，例如：test，当然这个名称也可以和备份时候的不一样，比如test2

>--directoryperdb：
备份数据所在位置，例如：c:\data\dump\test，这里为什么要多加一个test，而不是备份时候的dump，读者自己查看提示吧！

>--drop：
恢复的时候，先删除当前数据，然后恢复备份的数据。就是说，恢复后，备份后添加修改的数据都会被删除，慎用哦！

接下来我们执行以下命令:
```
mongorestore
```

# Mongodb 高级教程
## 关系
## 数据库引用
## 覆盖索引查询
## 查询分析
## 原子操作
## 高级索引
## ObjectId
## Map Reduce
## 全文检索
## 正则表达式
## 管理工具
## GridFS
## 固定集合
## 自动增长






