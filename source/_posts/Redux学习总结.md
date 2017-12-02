---
title: Redux学习总结
categories: 学习
date: 2017-02-17 14:25:03
tags: Redux
---

<!--more-->
# Redux

Redux 主要分为三部分: Action , Reducer , Store .

## Action
Action 主要用于传递造作 State 的信息,例如:
```js
{
    type:'ADD_FILM',
    name:'极限特工'
}
```
type属性是必要的,一般用来表达处理 state 数据的方式.
但是当增加的电影越来越多时,这种直接声明的 Object就会越来越多.
我们可以通过函数来生产 action ,如:
```js
function addFile(name){
    return {type:'ADD_FILM',name:name};
}
```


## Reducer
Reducer 一般为简单地处理函数,通过传入旧的 state 和指示操作的 action 来更新 state,如:
```js
function films ( state = initialState, action){
    switch (action.type){
        case 'ADD_FILM':
        // 更新 state 中的 films 字段
        return[{
            id:state.films.reduce((maxId,film)=>Math.max(film.id,maxId),-1)+1,
            name:action.name
            },...state];
        case 'DELETE_FILM':
        return state.films.filter(film=> film.id !==action.id);
        case 'SHOW_ALL_FILM':
        return Object.assign({},state,{
            visibilityFiter:action.filter
            });
        default:
        return state;
    }
}
```
上面代码展示了 Reducer 根据传入的 action.type 来匹配 case 进行不同的 state 更新.
在上面的代码中，我们可以把 'ADD_FILM' 和 'DELETE_FILM' 归为操作 state.films 的类，而 'SHOW_ALL_FILM' 为过滤显示类，所以可以把大的 filmReducer 拆分成 filmReducer 和 filterReducer.
```js
function filmReducer(state =[],action){
    switch(action.type){
        case 'ADD_FILM':
        //更新 state 中的 films 字段
        eturn[{
            id:state.films.reduce((maxId,film)=>Math.max(film.id,maxId),-1)+1,
            name:action.name
            },...state];
        case 'DELETE_FILM':
        return state.films.filter(film=> film.id !==action.id);
        default:
        return state;
    }
}

function filterReducer(state.action){
    case 'SHOW_ALL_FILM':
        return Object.assign({},state,{
            visibilityFiter:action.filter
            });
        default:
        return state;
}
```
然后,通过组合函数将上面两个 reducers 组合起来:
```js
function rootReducer(state={},action){
    return{
        films:filmReducer(state.films,action),
        filter:filterReducer(state.filter,action)
    };
}
```
上面的 rootReducer 将不同部分的 state 传给对应的 reducer 处理，最终合并所有 reducer 的返回值，组成整个state。

Redux提供了 **combineReducers()**方法,用它重构上面的代码:
```js
var rootReducer = combineReducers({
    films:filmReducer,
    filter:filterReducer
    })
```
combineReducers() 将调用一系列 reducer，并根据对应的 key 来筛选出 state 中的一部分数据给相应的 reducer，这样也意味着每一个小的 reducer 将只能处理 state 的一部分数据，如：filterReducer 将只能处理及返回 state.filter 的数据，如果需要使用到其他 state 数据，那还是需要为这类 reducer 传入整个 state。

在 Redux 中，一个 action 可以触发多个 reducer，一个 reducer 中也可以包含多种 action.type 的处理。属于多对多的关系。

## Store
Action 用来表达操作消息,Reducer 根据 Action 来更新 State.

在 Redux 项目中, Store 是单一的.维护着一个全局的 State,并且根据 Action 来进行事件分发处理 State.

Redux 提供了 createStore() 来生产 Store.
```js
var store = createStore(rootReducer);
//rootReducer 为顶级的 Reducer
```

store.getState()用来获取 state 数据;
store.subscribe(listener)用于注册监听函数.每当 state 数据更新时,将会触发监听函数.
store.dispatch(action)用于将一个 action 对象派发给 reducer 进行处理.

# React-Redux
Redux 并不依赖于 React, 但它更适合 由数据更新 UI 的框架(例如React).
React-Redux 主要提供 Connect 和 Provider 两个组件来实现上述功能;

