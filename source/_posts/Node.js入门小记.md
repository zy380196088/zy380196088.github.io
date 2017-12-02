---
title: 初学Node.js小记
categories: 学习
date: 2017-04-13 20:12:33
tags: [Node.js]
---

<!--more-->

# require 和 module
Node提供了两个核心模块require,module来管理模块依赖. 他们在全局范围内可以用.不需要用 require('require')和 require('module')医用.

# path

## path.dirname(filepath) 获取路径
## path.basename(filepath) 获取文件名
## path.extname(filepath) 获取扩展名

## path.join([...paths])

## path.resolve([...paths])

## path.normalize(filepath)

## path.format(pathObject) 文件路径分解
将pathObject 的 root ,dir , base , name , ext属性按照一定的规则,组合成一个文件路径.

## path.parse(filepath) 文件路径解析

## path.relative(from,to) 获取相对路径

## path.delimiter
```shell
//Linux
console.log(process.env.PATH)
// '/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin'

process.env.PATH.split(path.delimiter)
// returns ['/usr/bin', '/bin', '/usr/sbin', '/sbin', '/usr/local/bin']

//Windows
console.log(process.env.PATH)
// 'C:\Windows\system32;C:\Windows;C:\Program Files\node\'

process.env.PATH.split(path.delimiter)
// returns ['C:\\Windows\\system32', 'C:\\Windows', 'C:\\Program Files\\node\\']
```

## util
### until.inherits(constructor,superConstructor)
实现对象间原型继承的函数.

### util.inspect(object,[showHidden],[depth],[colors])
将任意对象转换为字符串的方法，通常用于调试和错误输出.

### util.isArray()

### xutil.isRegExp()

### util.isDate()

### util.isError()

### util.format()

### util. debug()

## 事件驱动 events
### events.EventEmitter
事件发射与事件监听器功能的封装.

#### EventEmitter.on(event, listener)
为指定事件注册一个监听器.

#### EventEmitter.emit(event, [arg1], [arg2], [...])
发射 event 事件.

#### EventEmitter.once(event, listener)
为指定事件注册一个***单次***监听器

#### EventEmitter.removeListener(event, listener)
 除指定事件的某个监听器，listener必须是该事件已经注册过的监听器。

#### EventEmitter.removeAllListeners([event])
移除所有事件的所有监听器,如果指定 event，则移除指定事件的所有监听器。

### 继承 EventEmitter
包括 fs、net、 http 在内的，只要是支持事件响应的核心模块都是 EventEmitter 的 子类。

## 文件系统 fs
### fs.readFile(filename,[encoding],[callback(err,data)])
### fs.readFileSync(filename, [encoding])
### fs.open(path,flags,[mode],[callback(err,fd)])
### fs.read(fd, buffer, offset, length, position, [callback(err, bytesRead, buffer)])

## HTTP服务器与客户端
