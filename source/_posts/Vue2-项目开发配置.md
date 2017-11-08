---
title: Vue2 项目开发配置
categories: 学习
date: 2017-06-02 20:27:10
tags: [Vue2,vue-cli]
---

首先 Node.js 开发环境需要提前安装好, 当然老司机都必备了...
没装好的自行搜索解决...就不多说了

<!--more-->

# 安装 vue-cli 官方脚手架搭建工具
## 全局安装:

```shell
sudo npm install vue-cli -g
```

## vue-cli 创建项目
终端进入你想存放项目的文件夹目录下,执行下方命令:
```shell
vue init webpack project
```
project 为你创建项目的文件目录名,自行拟定.

命令行会一步一步提示并指引你完成项目创建,注意项目名不能大写.
建议开启 vue-router , 其他不熟悉的就不要配置了...

建好后,项目基本结构如下:
![](/images/vue-folder.png)

>build : webpack 打包构建相关配置
>config : 关于 webpack打包构建 开发环境 和 上线环境的配置

src 目录下文件目录简述:
>assets : 存放静态资源(图片,字体,音频等)
>components : vue 组件
>router : 路由配置
>.babelrc 转码
>.gitignore git 提交上传忽略的文件目录

# 引入 Vuex 状态管理系统
```shell
npm install vuex --save
```

在 src 文件夹下新建一个store文件夹,并在其中新建几个基本文件:
```shell
mkdir store
cd store
touch actions.js getters.js index.js mutations.js
```

>actions.js : vuex 状态管理核心之一
>getters.js : 工具集接口,构建全局 state 自定义方法
>index.js : 所有状态汇聚于此 store
>mutations.js : 改变 store 中各个数据的唯一方法

# 配置 store
编辑src/store/index.js
```js
import Vue from 'vue';
import Vuex from 'vuex';
import * as actions from './actions';
import * as mutations from './mutations';
import * as getters from './getters';
import state from './rootState';

Vue.use(Vuex)

const store = new Vuex.Store({
  state,
  getters,
  actions,
  mutations
})

export default store;
```

# 挂载 store到 vue 上
编辑 src/main.js
```js
import Vue from 'vue';
import App from './App';
import router from './router';
import store from './store';

/* eslint-disable no-new */
new Vue({
  el: '#app',
  store,
  router,
  template: '<App/>',
  components: { App }
})
```









