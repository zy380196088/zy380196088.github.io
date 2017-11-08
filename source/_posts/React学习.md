---
title: React学习
date: 2016-09-14 13:27:46
tags: [react]
---

# Component 的创建和复合
## React简介
### 对 React.JS 的认识 及其优点
React 并不是一个完整的 MVC 框架,最多可以认为是 MVC 中的 V(View);
React 不是一个新的模板语言, JSX 只是一个表现,没有 JSX 的 React也能工作.

React本质上是一个"状态机",可以帮助开发者管理复杂的随着时间变化的状态.
React只关心两件事情:

1. 更新 DOM
2. 响应时事件

<!--more-->

React运用一个虚拟的 DOM 实现了一个非常强大的渲染系统,在 React 忠对 DOM 只更新不读取.
它以渲染函数为基础. 当函数读入当前的状态,将其转换为目标页面上的一个虚拟表现.只要 React 被告知状态有变化,它就会重新运行这些函数,计算出页面的一个新的虚拟表现,接着自动的把结果转换成必要的 DOM 更新来反映新的表现.

## JSX

### 在 React 中使用 JSX 的好处:
1. 允许使用熟悉的语法来定义 HTML 元素树;
2. 提供更加语义化且易懂的标签;
3. 程序结构更容易被直观化;
4. 抽象了 React Element 的创建过程;
5. 可以随时掌控 HTML 标签以及生成这些标签的代码;
6. 是原生的 JavaScript;

JSX 允许你在应用程序中使用所有预定义的 HTML5标签及自定义组件.

### 复合组件

1. 定义一个自定义组件
假设我们想写一个分页组件,希望输出如下 HTML:

```javascript
<div className = "divider">
	<h2>Questions</h2> <hr />
</div>
```

想要将 HTML 片段表示为 React Component , 只需要像下面这样包装起来,然后再 render 方法中返回这些标签.

```javascript
var Divider = React.createClass({
	render: function(){
		return (
		<div className ="divider">
			<h2>Questions</h2> <hr />
		</div>
		);
	}
});
```

目前这还是个一次性的组件,想要让这个组件变得实用,还需要一种将 h2标签中的文本表示出来的动态方法.

2. 使用动态值
JSX 将两个花括号之间的内容{...}渲染为动态值.花括号指明了一个 JavaScript 上下文环境--你在花括号中放入的任何东西都会被进行求值,得到的结果被渲染为标签中的若干节点.

React 通过将数组中的每个元素渲染为一个节点的方式对数组进行自动求值;

```html
	var text = ['hello','world'];
	<h2>{text}</h2>
	//<h2>helloworld</h2>
```

3. 子节点
在 HTML 中,使用<h2>Questions</h2>来渲染一个 header元素,这里" Question" 就是 h2元素的子文本节点.而在 JSX 中,我们的目标是用下面的方式来表示它:

```html
<Divider>Question</Divider>
```

React 将开始标签与结束标签之间的所有子节点保存在一个名为 this.props.children的特殊组件属性中.
在上面的例子中,``this.props.children == ["Questions"].``

```html
var Divider = React.createClass({
	render: function(){
		return (
			<div className ="divider">
				<h2>{this.props.children}</h2> <hr />
			</div>
		);
	}
});
```

将上面的 JSX 代码转换为 JavaScript 时,会得到下面的结果:

```html
var Divder = React.createClass({displayName:'Divider',
	render: function(){
		return (
			React.createElement("div",{className:"divider"},
			React.createElement("h2",null,this.props.children),
			React.createElement("hr",null)
			)
		);
	}
});
```

### JSX与 HTML 有何不同
#### 属性
JSX 提供了将属性设置为动态 JavaScript 变量的便利,要设置动态的属性,需要将原本用引号括起来的文本替换成花括号包裹的 JavaScript 变量.

```html
	var surveyQuestionId = this.props.id;
	var classes ='some-class-name';
	...
	<div id={surveyQuestionId} className ={classes}>...</div>
```

对于更复杂的情景,还可以将属性设置为一个函数调用返回的结果:

``<div id={this.getSurveyId()}>...</div>``

#### 条件判断

想要在组件中添加条件判断似乎是件很困难的事情,若直接往 JSX 中加入 if 语句会渲染出无效的 JavaScript,解决的办法可以使用以下某种方法:

1. 使用三目运算符

```html
	render: function(){
		return <div className = {
			this.state.isComplete ? 'is-complete' :''
		}>...</div>
	}
```

2. 设置一个变量并在属性中引用它

```html
	getIsComplete: function(){
		return this.state.isComplete ? 'is-complete' : '';
	},
	render: function(){
		var isComplete = this.getIsComplete();
		return <div className = {isComplete}>...</div>;
	}
```

3. 将逻辑转化到函数中

```html
	getIsComplete: function(){
		return this.state.isComplete ? 'is-complete' : '';
	},
	render: function(){
		return <div className = {this.getIsComlete()}>...</div>;	}
```

4. 使用&&运算符
由于对于null  或 false 值 React 不会输出任何内容,因此你可以使用一个后面跟随了期望字符串的布尔值来实现条件判断,如果这个布尔值为 true, 那么后续的字符串就会被使用.

```html
	render: function(){
		return <div className = {this.state.isComplete && 'is-complete'}>
		...
		</div>;
	}
```
	
#### 非 DOM 属性

下面特殊属性只在 JSX 中存在:

1. key

	key 是一个可选的唯一标识符.在程序运行的过程中,一个组件可能会在组件树中调整位置,比如当用户在进行搜索操作时,或者当一个列表中的物品被增加,删除时,当这些情况发生时,组件可能并不需要被销毁并重新创建.
	通过给组件设置一个独一无二的键,并确保它在一个渲染周期中保持一致,似的 react 能够更智能地决定应该重用一个组件,还是销毁并重新创建一个组件,进而提升渲染性能.当两个已经存在于 DOM 中的组件交换位置是, react 能够匹配对应的键并进行相应的移动,且不需要完全重新渲染 DOM.

2. ref

	ref 允许父组件在 render 方法之外保持对子组件的一个引用.
	JSX 中,你可以通过在属性中设置期望的引用名来定义一个引用.
	
	```JavaScript
	render: function(){
		return <div>
			<input ref="myInput" .../>
		</div>;
	}
	```

	之后,你就可以在组件中的任何地方使用 this.refs.myInput获取这个引用.通过引用获取到的这个对象被称为**支持实例**.它并不是真正的 DOM, 而是 react 在需要时用来创建 DOM 的一个描述对象.可以使用 this.refs.myInput.getDOMNode()访问真实的 DOM 节点.
	
	3. dangerouslySetInnerHTML
	
####  事件

在 JSX 中,捕获一个事件就像给组件的方法设置一个属性一样简单.

```html
handleClick : function(event){...},
render : function(){
	return <div onClick = {this.handleClick}>...</div>
}
```
注意, react 自动绑定了组件所有方法的作用域,因此你永远都不需要手动绑定.

#### 注释
注释可以用以下两种形式添加:
 1. 当作一个元素的在子节点
 2. 内年在元素属性中

#### 特殊属性
由于 JSX 会转换为原生的 JavaScript 函数,因此有些关键词使我们不能用的---例如 for 和 class.
要给表单里的标签添加for属性需要使用htmlFor.

```html
<label htmlFor ="for-text"...>
```

#### 样式

React把所有内敛样式都规划为了驼峰形式,与JavaScript中 DOM 的 style 属性一致.
要添加一个自定义的样式属性,只需简单地把驼峰形式的属性名及期望的 CSS 值拼装为对象即可.

```html
var styles = {
	borderColor:"#999",
	borderThickness:"1px"
};
React.renderComponent(<div style ={styles}>...</div>,node);
```

### 没有JSX的React
如果不打算在 react 中使用 JSX, 在 React中创建元素时需要知道以下三点:
1. 定义组件类.
2. 创建一个为组件类产生实例的工厂.
3. 使用工厂来创建 ReactElement实例.
#### 创建 React 元素
对于普通的 HTML 元素, react 在 React.DOM.* 命名空间下提供了一系列的工厂.这些预定义的工厂都是 React.createElement 的简写,只是帮你预置了第一个参数而已.下面两行语句会得到同样的结果.

```js
React.createElement('div');
React.DOM.div();
```

然而,对于自定义组件来说,你必须为组件类创建一个工厂.
回想之前我们定义的一个 Divider组件类.
下面将它重命名为 DividerClass, 以此来明确它的目的.
```javascript
var DividerClass = React.createClass({
	displayName:'Divider',
	render: function(){
		return (
			React.createE;e,emt("div",{className:'divider'},
			React.)
		)
	}
});
```
#### 简写
尽管 React.DOM.*命名空间非常方便,但重复地输入相同的内容总是让人觉得繁琐.我们可以用较短的变量名保存一个对 React.DOM的引用.

```js
var R = React.DOM;
var DividerClass = React.createClass({displayName:'Dividider',
	render : function(){
		return R.div({className:"divider"},
		R.h2(null,"Label Text"),
		R.hr()
		);
	}
});
```

## 组件的生命周期

### 生命周期方法
#### 实例化
一个实例初次被创建时所调用的生命周期方法与其他各个后续实例被创建时所调用的略有不同.当首次使用一个组件类时,可以看到下面这些方法依次被调用:

1. getDefaultProps
	对于组件类来说这个方法只会被调用一次.对于那些没有被父辈组件制定props 属性的新建实例来说,这个方法返回的对象可用于为十里设置默认的props值.
	
2. getInitialState
	对于组件的每个实例来说,这个方法的调用次数有且只有一次,在这里你将有机会初始化每个实例的 state.与 getDefaultProps方法不同的是,每次实例创建时该方法都会被调用一次.在这个方法里,我们可以访问到 this.props

3. componentWillMount
	在完成首次渲染之前被调用,**是在 render 方法调用前可以修改组件 state 的最后一次机会.**

4. render
	在这创建一个虚拟 DOM,用来表示组件的输出.对于一个组件来说, render 是唯一一个**必需**的方法,并且有特定的规则.
	render 方法需要满足下面几点:
	-- 只能通过 this.props 和 this.state访问数据;
	-- 可以返回 null ,false 活着任何 React组件;
	-- 只能出现一个顶级组件(不能返回一组元素);
	-- 必需**纯净**,意味着不能改变组件的状态活着修改 DOM 的输出.
	render 方法返回的结果不是真正的 DOM, 而是一个虚拟的表现, React随后会把它和真实的 DOM 做对比,来判断是否有必要做出修改.

5. componentDidMount
	在 render 方法成功调用,并且正式的 DOM 已经被渲染之后,可以在 componentDidMount 内部通过this.getDOMNode()方法访问到它.
	这就是可以用来访问原始 DOM 的**生命周期钩子函数**.比如,但你需要测量渲染出 DOM 元素的高度,或者使用计时器来操作它,亦或运行一个自定义的 jQuery 插件时,可以将这些操作挂载到这个方法上.

	举例来说,假设需要在一个通过 React 渲染出的表单元素上使用 jQuery UI 的 Autocomplete 插件,则可以像下面这样使用它:
	
	```JSX
	//需要自动补全的字符串列表
	var datasource =[...];

	var MyComponent = React.creatClass({
		render:function(){
			return <input .../>;
		},
		componentDidMount : function(){
			$(this.getDOMNode()).autocomplete({
				source:datasource
				});
			}
		});
	```
注意,当 React运行在服务器端时, componentDidMount方法不会被调用.

对于该组件类的所有后续应用,你将会看到下面的方法被一次调用.

1. getInitialState
2. componentWillMount
3. render
4. componentDidMount

#### 存在期

随着应用状态的改变,以及组件逐渐受到影响.下面的方法依次被调用:

1. componentWillReceiveProps
2. shouldComponentUpdate
3. componentWillUpdate
4. render
5. componentDidUpdate

#### 销毁&清理期

 最后,当该组件被使用完成后,componentWillUnmount犯法将会被调用,目的是给这个实例提供清理自身的机会.

#### 反模式:把计算后的赋值给 state
在getInitialState方法中,尝试通过this.props来创建state的做法是一种范模式.
**React专注于维护数据的单一来源.**

当组件的state值和它所基于的prop不同吧,因而无法了解到render函数的内部结构时,可以认定为一种范模式.

```JSX
//反模式经过计算后值不应该赋给state
getDefaultProps:function(){
	return {
		date:new Date()
	};
},
getInitialState:function(){
	return {
		day :this.propsdate.getDay()
	}
},
render: function(){
	return <div>Day:{this.state.day}</div>
}
```

正确的模式因该是在渲染时计算这些值.这保证了计算和的值永远不会与派生出它的props值不同步.

```JSX
//在渲染时计算值是正确的
getDefaultProps:function(){
	return{
		date:new Date()
	};
},
render:function(){
	var day = this.props.date.getDay();
	return <div>Day:{day}</div>;
}
```
### 总结

## 数据流
### Props
props是 properties 的缩写,可以使用它把任意类型的数据传递给组件.
可以在挂在组件的时候设置它的 props:

```JSX
var surveys = [{sex:'man'}];
<ListSurveys surveys ={surveys}/>
```
可以通过 this.props 访问 props,但绝对不能通过这种方式修改它.**一个组件绝对不可以修改自己的 props**
在 JSX 中,可以把 props 设置为字符串:
```JSX
<a href='/surveys/add'>Add survey</a>
```
也可以使用{}语法来设置,注入 JavaScript 传递任意类型的变量:
```JSX
<a href={'/surveys/'+survey.id}>{survey.title}</a>
```
还可以使用 JSX的展开语法把 props 设置成一个对象:
```JSX
var ListSurveys = React.createClass({
	render:function(){
		var props = {
			one:'foo',
			two:'bar'
		};
		return <SurveyTable{...props}/>;
	}
});
```
props 还可以用来添加事件处理器:
```JSX
//通过链接标签传递了一个 onClick属性,值为 handleClick函数,当用户点击链接时,handleClick方法将被调用.
var SaveButtom = React.createClass({
	render: function(){
		return(
			<a className = 'button save' onClick={this.handleClick}>Save</a>
		);
	},
	handleClick:function(){

	}
});
```

### PropTypes
通过在组件中定义一个配置对象, React 提供了一种验证 props 的方式:
```JSX
var SurveyTableRow = React.createClass({
	PropTypes:{
		survey :React.PropTypes.shape({
			id:React.PropTypes.number.isRequired
		}).isRequired,
		onClick:React.PropTypes.func
	},
	//...
	})
```
组件初始化时,如果传递的属性和 propTypes不匹配,则会答应一个 console.warn日志.
如果是可选的配置,则可以去掉.isRequired.

### getDefaultProps
可以为组建添加 getDefaultProps 函数来设置属性的默认值.(针对那些非必需属性)
getDefaultProps并不是在组件实例化时被调用.而是在 React.createClass 调用时就被调用了,返回值会被缓存起来.

### State
每一个 React 组件都可以拥有自己的 state, state 与 props 的却别在于state 只存在于组件的内部.
state 可以用来确定一个元素的试图状态.下面代码为一个自定义的<Dropdown/>组件:
```JSX
var CountryDropdown = React.createClass({
	getInitialState:function(){
		return {
			showOptions: false
		};
	},
	render:function(){
		var options;
		if (this.state.showOptions){
			options = (
				<ul className = 'options'>
					<li>option1</li>
					<li>option2</li>
					<li>option3</li>
				</ul>
			);
		}
		return (
			<div className = "dropdown" onClick = {this.handleClick}>
				<label>choose a country</label>
			</div>
		);
	},
	handleClick:function(){
		this.setSate({
			showOptions:true
		});
	}
})
```
上述例子中,state被用来记录是否在下拉框中显示可选项.
**千万不能直接修改 this.state,永远要记得通过 this.setSate 方法修改**

### 总结
>使用 props 在整个组件树中传递数据和配置.
>避免在组建内部修改 this.props或调用 this.setProps,请把 props 当做是只读的
>使用props来做事件处理器,与子组件通信
>使用 state 存储简单地视图状态,比如说下拉框是否可见这样的状态
>使用 this.setState 来设置状态,而不要使用 this.state 直接修改状态.

## 事件处理
### 绑定事件处理器
### 事件和状态
### 根据状态进行渲染
组件状态默认是 null, 可以通过 getInitialState方法将其初始化为合理的值,例如:
```
getInitialState:function(){
	return {
		dropZoneEntered:false,
		title:'',
		introduction:'',
		questions:[]
	};
}
```

### 更新状态
更新组件的内部状态会触发组件重绘,更新组件状态有两种方案:
1. setState
把传入的对象合并到已有的state 对象
2. replaceState
用一个全新的 state 对象完整的替换掉原有的 state.(使用不可变数据结构来表示状态时)

### 事件对象

## 组件的复合
### 扩展 HTML
### 组件复合的例子
1. 接收一组选项作为输入
2. 把选项渲染给用户
3. 只允许用户选择一个选项

### 组装 HTML

### 追踪状态
### 整合到父组件当中

### 父组件,子组件关系
子组件和其父组件通信的最简单方式就是使用属性( props).父组件需要通过属性传入一个回调函数,子组件在需要时进行调用.
### 总结

## mixing

# 进阶
## DOM 操作
想要访问受 React控制的 DOM 节点,首先必须能够访问到负责控制这些 DOM 的组件.可以通过为子组件添加一个 ref 属性来实现.
```jsx
var DoodleArea = React.createClass({
	render:function(){
		return <canvas ref = "mainCanvas"/>;
	}
});
```
**必须保证赋值给没个子组件的 ref 值在所有子组件中是唯一的**
可以通过**getDOMNode()方法访问到底层的 DOM 节点,但请不要再 render 方法中这样做,因为在 render 方法完成并且 React 执行更新之前,底层的 DOM 节点可能不是最新的(甚至尚未创建)**
### DOM 操作
### 整合非 React类库
### 侵入式插件
### 总结

## 表单
## 动画
## 性能优化
## 服务端渲染
## 周边类库

# React 工具
## 开发工具
## 测试

# React 实践
## 架构模式
## 其他使用场景
