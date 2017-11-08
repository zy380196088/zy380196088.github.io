---
title: webpack 小结
categories: 学习
date: 2017-04-12 19:29:44
tags: [webpack]
---

<!--more-->

# 安装 node.js 和 webpack
终端输入``node -v`` 成功显示node 版本,证明已经安装好, 否则,请自行百度 node 安装教程...
## 全局安装 webpack
```
npm install webpack -g
```
## 配置 package.json
如果没有,``npm init``初始化安装.
webpack 命令行须知:
> webpack : 执行开发编译
> webpack -p : 发布发布环境的编译(压缩代码)
> webpack --watch : 监听项目
> webpack -d : 包含 source maps 方便调试
> webpack --colors : 让打包界面更好看

构建项目时,可以按需求写进 package.json
```json
//package.json
{
    //...
    "script":{
        "dev":"webpack-dev-server --devtoll eval --progress --colors",
        "deploy":"NODE_ENV=production webpack -p"
    },
    //...
}
```

# entry 入口文件
用哪个文件作为项目入口

# output 出口
把处理完成的文件放在哪里

# module 模块
用什么不同的模块来处理各种类型的文件

代码示例:
```js
// webpack.config.js
module.exports = {  
  entry: './index.js',
  output: {
    filename: 'bundle.js'       
  },
  module: {
    loaders: [
                  {
                test: /\.scss$/,
                // loader: extractCSS.extract(['css','sass'])
                loaders: ['style', 'css']
            },
            {
                test: /\.css$/,
                // loader: extractCSS.extract(['css'])
                loaders: ['style', 'css', 'sass']
            },// loaders 可以接受 querystring 格式的参数
    ]
  }
};
```

# 项目目录结构

Vue-cli webpack 项目参考:
```bash
.
|-- build                            // 项目构建(webpack)相关代码
|   |-- build.js                     // 生产环境构建代码
|   |-- check-version.js             // 检查node、npm等版本
|   |-- dev-client.js                // 热重载相关
|   |-- dev-server.js                // 构建本地服务器
|   |-- utils.js                     // 构建工具相关
|   |-- webpack.base.conf.js         // webpack基础配置
|   |-- webpack.dev.conf.js          // webpack开发环境配置
|   |-- webpack.prod.conf.js         // webpack生产环境配置
|-- config                           // 项目开发环境配置
|   |-- dev.env.js                   // 开发环境变量
|   |-- index.js                     // 项目一些配置变量
|   |-- prod.env.js                  // 生产环境变量
|   |-- test.env.js                  // 测试环境变量
|-- src                              // 源码目录
|   |-- components                     // vue公共组件
|   |-- store                          // vuex的状态管理
|   |-- App.vue                        // 页面入口文件
|   |-- main.js                        // 程序入口文件，加载各种公共组件
|-- static                           // 静态文件，比如一些图片，json数据等
|   |-- data                           // 群聊分析得到的数据用于数据可视化
|-- .babelrc                         // ES6语法编译配置
|-- .editorconfig                    // 定义代码格式
|-- .gitignore                       // git上传需要忽略的文件格式
|-- README.md                        // 项目说明
|-- favicon.ico 
|-- index.html                       // 入口页面
|-- package.json                     // 项目基本信息
.
```
