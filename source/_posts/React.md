---
title: React
date: 2021-07-11 08:05
tags: React
abbrlink: '0'
categories: React
---

------



# 一. React

## 1.React 概述

### 1.1 什么是React

用于构建用户界面的JavaScript库

用户界面: HTML页面(前端)

React主要用来写HTML页面,或者构建Web应用

如果从MVC的角度来看,React仅仅是视图层(V), 也就是只负责试图的渲染,而非提供了完整的M和C功能

### 1.2 React的特点

- 声明式 (只需要描述UI看起来是什么样,都跟写HTML一样)

```jsx
const jsx = <div className = "app">
  <h1>Hello React! 动态变化数据: {count} </h1>
</div>
```

- 基于组件
  - 组件是Reat最重要的内容
  - 组件表示页面中的部分内容
  - 组合、复用多个组件,可以实现完整的页面功能
- 学习一次,随处使用
  - 使用 React可以开发Web应用
  - 使用 React可以开发移动端原生应用( react- native)
  - 使用 React可以开发VR(虚拟现实)应用( react360)

## 2. React 的基本使用

### 2.1 React的安装

安装命令: `npm i react react-dom`

- react 包是核心,提供创建元素,组件等功能
- react-dom包提供DOM相关功能等

```jsx
// 1. 引入react和react-dom两个js文件
<script src="./node_modules/react/umd/react.development.js"></script>
<script src="./node_modules/react-dom/umd/react-dom.development.js"></script>
// 2. 创建React元素
<script>
	// 参数一: 元素名称
	// 参数二: 元素属性
	// 参数三: 元素的子节点
	const title = React.createElement('h1', null, 'Hello React')
</script> 
// 3. 渲染React元素到页面中
<div id="root"></div> //创建id属性为root的元素
<script>
	const title = React.createElement('h1', null, 'Hello React')
	// 参数一: 要渲染的react元素
	// 参数二: 挂载点
	ReactDOM.render(title, document.getElementById('root'))
</script>
```

### 2.2 方法说明

- React.creatElement()说明(知道)

```jsx
<div id="root"></div> //创建id属性为root的元素
<script>
	// 参数一: 要渲染的react元素
	// 参数二: 挂载点
  // 第三个及其后面的参数: 元素的子节点
	const title = React.createElement(
		'p', 
		{ title: '我是标题', id: 'p1' },
	  'Hello React',
		React.createElement('span', null, '我是span节点')
		)

	// 参数一: 要渲染的react元素
	// 参数二: 挂载点
	ReactDOM.render(title, document.getElementById('root'))
</script>
```

- **ReactDOM.render()说明**

```jsx
// 第一个参数: 要渲染的React元素
// 第二个参数: DOM对象, 用于指定渲染到页面中的位置
ReactDOM.render(el, document.getElementById('root'))
```

## 3. React脚手架的使用

### 3.1 React脚手架意义

1. 脚手架是开发现代Web应用的必备
2. 充分利用Webpack, Babel, ESlint等工具辅助项目开发
3. 零配置,无需手动配置繁琐的工具即可使用
4. 关注业务而不是工具配置

### 3.2 使用React脚手架初始化项目

1. 初始化项目,命令: `npx create-react-app my-app`
2. 启动项目,在项目根目录执行 `npm start`

### 3.3 在脚手架中使用React

1. 导入react 和react-dom两个包

```jsx
imoort React from 'react'
import ReactDOM from 'react-dom'
```

1. 调用React.createElement()方法来创建React元素
2. 调用ReactDOM.render()方法渲染react元素到页面中

### 3.4 React基础总结

1. Reat是构建用户界面的 JavaScript库
2. 使用 react时,推荐使用脚手架方式
3. 初始化项目命令: npx create- react- app my-app
4. 启动项目命令: yarn start(或 npm start)
5. React. createElement0方法用于创建 react元素(知道)
6. ReactDoM. render0方法负责渲染 react元素到页面中

# 二. JSX

目标:

- 能够知道什么是JSX
- 能够使用JSX创建 React元素
- 能够在JsX中使用 JavaScript表达式
- 能够使用JSX的条件渲染和列表渲染
- 能够给JsX添加样式
- JSX的基本使用
- JSX中使用 JavaScript表达式
- JSX的条件渲染
- JSX的列表渲染
- JSX的样式处理

## 1. JSX的基本使用

### 1.1 creatElement()的问题

- 繁琐不简洁
- 不直观,无法一眼看出所描述的结构
- 不优雅,用户体验不爽

### 1.2 JSX简介

JsX是 JavaScript XML的简写,表示在 JavaScript代码中写XML(HTML)格式的代码 优势:声明式语法更加直观、与HTM结构相同,降低了学习成本、提升开发效率

JSX的React的核心内容

- 推荐使用JSX语法创建 React元素
- 写JSX就跟写HTML一样,更加直观、友好
- JSX语法更能体现 React的声明式特点(描述U长什么样子)
- 使用步骤

1. 使用JSX语法创建React元素

```jsx
// 使用JSX语法,创建React元素
const tltle = <h1>Hello JSX</h1>
```

1. 使用ReactDOM.render()方法渲染react元素到页面中

```jsx
// 渲染创建好的React元素
ReactDOM.render(title,root)
```

### 1.3 思考

为什么脚手架中可以使用JSX语法? 1.J5X不是标准的 ECMAScript语法,它是 ECMAScript的语法扩展 2.需要使用 babel编译处理后,才能在浏览器环境中使用。 3. create-react-app脚手架中已经默认有该配置,无需手动配置 4.编译SX语法的包为:@ babel/preset-react

### 1.4 注意点

1. React元素的属性名使用驼峰命名法
2. 特殊属性名: class-> **className**、for-> htmlFor、 tabindex-> tabIndex
3. 没有子节点的Reac元素可以用**/>**结束
4. 推荐:使用**小括号包裹JSX**,从而避免JS中的自动插入分号陷阱

```jsx
const title = <h1 className="title">Hello JSX <span /></h1>
ReactDOM.render(title, document.getElementById('root'))

// 使用小括号包裹JSX
const dv = (
	<div>Hello JSX</div>
)

const title = (
	<h1 className="title">
	Hello JSX 
	<span />
	</h1>
)
```

## 2. JSX中使用JavaScript表达式

### 嵌入JS表达式

- 数据存储在JS中
- 语法: **{JavaScript表达式}**
- 注意:语法中是单大括号,不是双大括号

```jsx
const name = 'Jack'
const dv = (
	<div>你好,我叫: {name}</div>
)
```

### 注意点:

1. 单大括号中可以使用任意的 JavaScript表达式
2. JSX自身也是JS表达式
3. 注意:JS中的对象是一个例外,一般只会出现在stye属性中
4. 注意:不能在中出现语句(比如:if/for等

## 3. JSX的条件渲染

```jsx
// 条件渲染
// if-else
const isloading = true
const loadData = () => {
	if (isLoading) {
		return <div>loading...</div>
	}
	
	return <div>数据加载完成,此处显示加载后的数据</div>
}

// 三元表达式
const loadData = () => {
	return isloading ? (<div>loading...</div>) : (<div>数据加载完成,此处显示加载后的数据</div>)
}

// 逻辑与运算符
const loadData = () +> {
	return isLoading && (<div>loading...</div>)
}

const title = (
	<h1>
	条件渲染:
	{loadData()}
	</h1>
)
// 渲染react元素
ReactDOM.render(title, document.getElementById('root'))
```

## 4. JSX的列表渲染

- 如果要渲染一组数据,应该使用数组的map0方法
- 注意:渲染列表时应该添加key属性,key属性的值要保证唯一
- 原则:map0遍历谁,就给谁添加key属性

```jsx
const songs = [
	{id:1,name:"痴心绝对'}
	{id:2,name:·像我这样的人'},
	{id:3,name:"南山南'},
]
const list = (
	<ul>
	  {songs. map(item =><li key={item id}>{item.name}</li>)}
	</u1>
)
```

## 5. JSX的样式处理

### 1. 行内样式 - style

```jsx
<h1 style={{color: 'red', backgroundColor: 'skyblue'}}>
	JSX的样式处理
</h1>
```

### 2. 类名 - className(推荐)

```jsx
<h1 className = 'title'>
	JSX的样式处理
</h1>
```

### 总结:

JSX

1. JSX是React的核心内容
2. JSX表示在J5代码中写HTML结构,是React声明式的体现
3. 使用J5X配合嵌入的J5表达式、条件渲染、列表渲染,可以描述任意UI结构
4. 推荐使用[lassName的方式给JSX添加样式
5. React完全利用J语言自身的能力来编写圈,而不是造轮子增强HTML功能

## 三. React组件基础

### 1. React组件介绍

- 组件是Reat的一等公民,使用 React就是在用组件
- 组件表示页面中的部分功能
- 组合多个组件实现完整的页面功能
- 特点:可复用、独立、可组合

### 2. React组件的两种创建方式

- 使用函数创建组件
- 使用类创建组件

### 2.1 使用函数创建组件

- 函数组件:使用js的函数(或箭头函数)创建的组件叫做:函数组件
- 约定1:函数名称必须以大写字母开头,React据此区分组件和普通的 React元素
- 约定2: 函数组件必须有返回值,表示该组件的结构
- 如果返回值为null, 表示不渲染任何内容
- 渲染函数组件:  用函数名作为组件标签名
- 组件标签可以是单标签也可以是双标签

```jsx
// 函数组件
function Hello(){
	return (
		<div>这是我的第一个函数组件!</div>
	)
}

// 箭头函数写法
const Hello = () => <div>这是我的第一个函数组件!</div>

// 渲染组件
ReactDOM.render(<Hello />, document.getElementById('root'))
```

### 2.2 使用类创建组件

- 类组件: 使用ES6的class创建的组
- 约定1: 类名称也必须以大写字母开头
- 约定2: 类组件应该继承React.Component父类,从而可以使用父类中提供的方法或属性
- 约定3: 类组件必须提供 render0方法
- 约定4: render0方法必须有返回值,表示该组件的结构

```jsx
class Hello extends React.Compontent {
	reder() {
		return <div>Hello Class Component!</div>
	}
}
ReactDOM.render(<Hello />, document.getElementById('root'))
```

### 2.3 抽离为独立JS文件

组件作为一个独立的个体,一般都会放到一个单独的JS文件中

1. 创建 Hello.js
2. 在 Hello.js中导入 React
3. 创建组件(函数或类)
4. 在 Hello.js中导出该组件
5. 在 index.js中导入Hllo组件
6. 渲染组件

## 3. React事件处理

### 3.1 事件绑定

- React事件绑定语法与DOM事件语法相似
- 语法: on+事件名称={事件处理程序}, 比如: onClick={() = > {}}
- 注意: React事件采用**驼峰命名法**, 比如: onMouseEnter, onFocus

```jsx
// 类组件事件绑定
class Hello extends React.Compontent {
	handleClick() {
		console.log('单击事件触发了')
	}
	reder() {
		return (
			<button onClick={this.handleClick}>点我</buttom>
		)
	}
}
ReactDOM.render(<Hello />, document.getElementById('root'))

// 函数组件事件绑定
function App() {
	// 事件处理程序
	function handClick() {
		console.log('函数组件中的事件绑定,单击事件触发了')
	}

	return (
		<button onClick={handleClick}>点我</buttom> //函数组件中没有this
	)
}
// 渲染组件
ReactDOM.render(<Hello />, document.getElementById('root'))
```

### 3.2 事件对象

- 可以通过事件处理程序的参数获取到事件对象
- React中的事件对象叫做: 合成事件(对象)
- 合成事件: 兼容所有浏览器, 无需担心跨浏览器兼容的问题

```jsx
/* React事件对象 */
class App extends React.Component {
	handleClick(e) {
		// 阻止浏览器的默认行为
		e.preventDefault()
		console.log('a标签的单机事件触发了')
	}
	render() {
		return(
			<a href="<https://www.baidu.com/>" onCLick={this.handleClick}>baidu</a>>
		)
	}
}
// 渲染组件
ReactDOM.render(<App />, document.getElementById('root'))
```

## 4. 有状态组件和无状态组件

- 函数组件又叫做无状态组件,类组件又叫做有状态组件
- 状态( state)即数据
- 函数组件没有自己的状态,只负责数据展示(静)
- 类组件有自己的状态,负责更新UI,让页面“动”起来

## 5. 组件中的state 和setState

### 5.1 state的基本使用

- 状态( state)即数据,是组件内部的私有数据,只能在组件内部使用
- state的值是对象,表示一个组件中可以有多个数据

```jsx
class Hello extends React.Component {
	constructor() {
		super()
		// 初始化state
		this.state = {
			count: 0
		}
	}

	// 简化语法初始化state
	state = {
		count: 0
	}
	render() {
		return (
			<div>计数器: {this.state.count}</div>
		)
	}
}

// 渲染组件
ReactDOM.render(<APP />, document.getElementVyId('root'))
```

### 5.2 setState()修改状态

- 状态是可变的
- 语法: this setstate((要修改的数据
- 注意:不要直接修改 state中的值,这是错误的!!

```jsx
class Hello extends React.Component {
	state = {
		count: 0
	}
	render() {
		return (
			<div>计数器: {this.state.count}</div>
			<button onClick={() => {
				this.setState({
					count: this.state.count + 1
				})
			}}>+1</button>
		)
	}
}

// 渲染组件
ReactDOM.render(<APP />, document.getElementVyId('root'))
```

- setState()作用: 1.修改state 2.更新UI
- 思想: 数据驱动视图

## 6. 事件绑定this指向

### 6.1 从JSX中抽离事件处理程序

- JSX中掺杂过多的JS逻辑代码,会显得非常混乱
- 推荐: 将逻辑抽离到单独的方法中,保证JSX结构清晰

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a2b35525-3f95-4f8d-b980-937bd84d1a13/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a2b35525-3f95-4f8d-b980-937bd84d1a13/Untitled.png)

- 原因: 事件处理程序中this的值为undefined
- 希望: this指向组件实例(render方法中的this即为组件实例)

### 6.2 如何处理事件绑定this指向的问题

1. 箭头函数

   1. 利用箭头函数自身不绑定this的特点
   2. render()方法中的this为组件实例,可以获取到setState()

   ```jsx
   class Hello extends React.Component {
   	state = {
   		count: 0
   	}
   	// 事件处理程序
   	onIncrement() {
   		console.log('事件处理程序中的this:', this)
   		this.setState({
   			count: this.state.count + 1
   		})
   	}
   
   	render() {
   		return (
   			<div>计数器: {this.state.count}</div>
   			<button onClick={() => this.onIncrement()}>+1</button>
   			// <button onClick={this.onIncrement}>+1</button>
   		)
   	}
   }
   
   // 渲染组件
   ReactDOM.render(<APP />, document.getElementVyId('root'))
   ```

2. Function.prototype.bind()

   1. 利用ES5中的bind方法,将事件处理程序中的this与组件实例绑定到一起

   ```jsx
   class Hello extends React.Component {
   	constructor() {
   		super()
   		this.state = {
   			count: 0
   		}
   		this.onIncrement = this.onIncrement.bind(this)
   	}
   	// 事件处理程序
   	onIncrement() {
   		console.log('事件处理程序中的this:', this)
   		this.setState({
   			count: this.state.count + 1
   		})
   	}
   	render() {
   		return (
   			<div>计数器: {this.state.count}</div>
   			<button onClick={this.onIncrement}>+1</button>
   		)
   	}
   }
   
   // 渲染组件
   ReactDOM.render(<APP />, document.getElementVyId('root'))
   ```

3. class的实例方法

   1. 利用箭头函数形式的实例方法
   2. 注意: 该语法是实验性语法,但是,由于babel的存在可以直接使用

   ```jsx
   class Hello extends React.Component {
   	state = {
   		count: 0
   	}
   	// 事件处理程序
   	onIncrement = () => {
   		console.log('事件处理程序中的this:', this)
   		this.setState({
   			count: this.state.count + 1
   		})
   	}
   
   	render() {
   		return (
   			<div>计数器: {this.state.count}</div>
   			<button onClick={this.onIncrement}>+1</button>
   			// <button onClick={this.onIncrement}>+1</button>
   		)
   	}
   }
   
   // 渲染组件
   ReactDOM.render(<APP />, document.getElementVyId('root'))
   ```

### 6.3 总结

1. 推荐: 使用class的实例方法

   ```jsx
   class Hello extends React.Component {
   	onIncrement = () => {
   		this.setState({...})
   	}
   }
   ```

2. 箭头函数

   ```jsx
   <button onClick={() => this.onIncrement()} /></button>
   ```

3. bind

   ```jsx
   constructor() {
   	super()
   	this.onIncrement = this.onIncrement.bind(this)
   }
   ```

## 7. 表单处理

1. 受控组件
2. 非受控组件

### 7.1 受控组件

- HTML中的表单元素是可输入的, 也就是有自己的可变状态
- 而React中可变状态通常保存在state中,并且只能通过setState() 方法来修改
- React将state与表单元素值value绑定到一起,由state的值来控制表单元素的值
- 受控组件: 其值受到React控制的表单元素

```jsx
/*
受控组件:其值受到React控制的表单元素

操作文本框的值:
*/
class App extends React.component{
	state = {
		txt: ''
	}
}
handleChange = e => {
	this.setState({
		txt: e.target.value
	})
}
render () {
	return (
		<div>
			<input type="txt" value={this.state.txt} onChange={this.handleChange} />
		</div>
	)
}
// 渲染组件
ReactDOM.render(<APP />, document.getElementById('root'))
```

- 步骤:

1. 在state中添加一个状态,作为表单元素的value值(控制表单元素值的来源)

   1. 给表单元素绑定change事件,将表单元素的值设置为state的值(控制表单元素值的变化)

   ```jsx
   state = { txt: '' }
   <input type="text" value={this.state.txt} 
   	onChange={e => this.setState({ txt: e.target.value })}
   />
   ```

示例:

1. 文本框,富文本框,下拉框
2. 复选框

```jsx
/*
受控组件示例
*/
class App extends React.component{
	state = {
		txt: '',
		content: '',
		city: 'bj',
		isChecked: false
	}
}
handleChange = e => {
	this.setState({
		txt: e.target.value
	})
}
//处理富文本框的变化
handleContent = e => {
	this.setState({
		content: e.target.value
	})
}
//处理下拉框的变化
handleCity = e => {
	this.setState({
		city: e.target.value
	})
}
// 处理复选框的变化
handleChecked = e => {
	this.setState({
		isChecked: e.target.checked
	})
}
render () {
	return (
		<div>
			{/*文本框*/}
			<input type="txt" value={this.state.txt} onChange={this.handleChange} />
			<br />
			{/*富文本框*/}
			<textarea value={this.state.content} onChange={this.handeleContent}></textarea>
			<br />
			{/*下拉框*/}
			<select value='this.state.city' onChange={this.handleCity}>
				<option value='sh'>上海</option>
				<option value='bj'>北京</option>
				<option value='gz'>广州</option>
			</select>
			{/*复选框*/}
			<input type="checkbox" checked={this.state.isChecked} onChange={this.handleChecked} />
		</div>
	)
}
// 渲染组件
ReactDOM.render(<APP />, document.getElementById('root'))

// 示例总结:
// 1. 文本框,富文本框, 下拉框操作value属性
// 2. 复选框操作checked属性
```

多表单元素优化步骤:

1. 给表单元素添加name属性,名称与state相同
2. 根据表单元素类型获取对应值
3. 在change时间处理程序中通过[name]来修改对应的state

```jsx
/*
受控组件示例
*/
class App extends React.component{
	state = {
		txt: '',
		content: '',
		city: 'bj',
		isChecked: false
	}
}
handleFrom = e => {
	// 获取当前的DOM对象
	const target = e.target
	// 根据类型获取值
	const value = target.type === 'checkbox'
		? target.checked
		: target.value

	// 获取name 
	const name = target.name
	this.setState({
		[name]: value
	})
}

render () {
	return (
		<div>
			{/*文本框*/}
			<input type="txt" name="txt" value={this.state.txt} onChange={this.handleFrom } />
			<br />
			{/*富文本框*/}
			<textarea name="content" value={this.state.content} onChange={this.handleFrom }></textarea>
			<br />
			{/*下拉框*/}
			<select name="city" value='this.state.city' onChange={this.handleFrom }>
				<option value='sh'>上海</option>
				<option value='bj'>北京</option>
				<option value='gz'>广州</option>
			</select>
			{/*复选框*/}
			<input name="isChecked" type="checkbox" checked={this.state.isChecked} onChange={this.handleFrom } />
		</div>
	)
}
// 渲染组件
ReactDOM.render(<APP />, document.getElementById('root'))
```

### 7.2 非受控组件

使用步骤:

1. 调用React.createRef()方法创建一个ref对象

   ```jsx
   constructor() {
   	super()
   	this.txtRef = React.createRef()
   }
   ```

2. 将创建好的ref对象添加到文本框中

   ```jsx
   <input type="text" ref={this.txtRef} />
   ```

3. 通过ref对象获取到文本框的值

   ```jsx
   console.log(this.txtRef.current.value)
   ```

React组件基础小案例:

需求分析:

1. 渲染评论列表(列表渲染)
2. 没有评论数据时渲染:暂无评论(条件渲染)
3. 获取评论信息,包括评论人和评论内容(受控组件)
4. 发表评论,更新评论列表(setState() )

```jsx
class App extends React.Component {
	render() {
		<div className="app">
			<div>
				<inout className="user" type="text" placeholder="请输入评论人" />
				<br />
				<textarea>
					className="content"
					cols="30"
					rows="10"
					placeholder="请输入评论内容"
				/>
				<br />
				<button>发表评论</button>
		</div>

		<div className="no-comment">暂无评论,快去评论吧~</div>
			<ul>
				<li>
					<h3>评论人: jack</h3>
					<p>评论内容: 沙发!!!</p>
				</li>
			</ul>
		</div>
	}
}
```

实现步骤:

1. 渲染评论列表

   1. 在state中初始化列表数据
   2. 使用数组的map方法遍历state中的列表数据
   3. 给每个被遍历的元素添加key属性

   ```jsx
   /*
   	评论列表案例
   	
   	comments: [
   		{id: 1, name: 'jack', content: '沙发!!!'},
   		{id: 2, name: 'rose', content: '板凳~~'},
   		{id: 3, name: 'tom', content: '楼主好人~'}
   	]
   */
   
   import '/.index.css'
   
   class App extends React.Component {
   	// 初始化状态
   	state ={
   		comments: [
   		{id: 1, name: 'jack', content: '沙发!!!'},
   		{id: 2, name: 'rose', content: '板凳~~'},
   		{id: 3, name: 'tom', content: '楼主好人~'}
   		]
   	}
   	render() {
   		return (
   			<div className="app">
   			<div>
   				<input className="user" type="text" placeholder="请输入评论人" />
   				<br />
   				<textarea 
   					className="content"
   					cols="30"
   					rows="10"
   					placeholder="请输入评论内容"
   				/>
   				<br />
   				<button>发表评论</button>
   			</div>
   
   			<div className="no-comment">暂无评论,快去评论吧~</div>
   			<ul>
   				{
   					this.state.comments.map(item => (
   						<li key={item.id}>
   							<h3>评论人: {item.name}</h3>
   							<p>评论内容: {item.content}</p>
   						</li>
   					))	
   				}
   			</ul>
   		</div>
   		)
   	}
   }
   ```

2. 渲染暂无评论

   1. 判断列表数据的长度是否为0
   2. 如果为0,则渲染暂无评论

   ```jsx
   /*
   	评论列表案例
   	
   	comments: [
   		{id: 1, name: 'jack', content: '沙发!!!'},
   		{id: 2, name: 'rose', content: '板凳~~'},
   		{id: 3, name: 'tom', content: '楼主好人~'}
   	]
   */
   
   import '/.index.css'
   
   class App extends React.Component {
   	// 初始化状态
   	state ={
   		comments: [
   		{id: 1, name: 'jack', content: '沙发!!!'},
   		{id: 2, name: 'rose', content: '板凳~~'},
   		{id: 3, name: 'tom', content: '楼主好人~'}
   		]
   	}
   
   	// 渲染评论列表
   	renderList() {
   		return this.state.comments.length === 0 ? (
   				<div className="no-comment">暂无评论,快去评论吧~</div>
   			) : (
   				<ul>
   				{
   					this.state.comments.map(item => (
   						<li key={item.id}>
   							<h3>评论人: {item.name}</h3>
   							<p>评论内容: {item.content}</p>
   						</li>
   					))}
   			</ul>
   			)
   	}
   	render() {
   		return (
   			<div className="app">
   			<div>
   				<input className="user" type="text" placeholder="请输入评论人" />
   				<br />
   				<textarea 
   					className="content"
   					cols="30"
   					rows="10"
   					placeholder="请输入评论内容"
   				/>
   				<br />
   				<button>发表评论</button>
   			</div>
   				
   			{/*通过条件渲染决定渲染什么内容*/}
   			{this.renderList()}	
   			/*{this.state.comments.length === 0 ? (
   				<div className="no-comment">暂无评论,快去评论吧~</div>
   			) : (
   				<ul>
   				{
   					this.state.comments.map(item => (
   						<li key={item.id}>
   							<h3>评论人: {item.name}</h3>
   							<p>评论内容: {item.content}</p>
   						</li>
   					))}
   			</ul>
   			)}*/	
   		</div>
   		)
   	}
   }
   ```

## 7.3 React 组件基础综合案例

1. 案例需求分析
   1. 渲染评论列表(列表渲染)
   2. 没有评论数据时渲染: 暂无评论(条件渲染)
   3. 获取评论信息,包括评论人和评论内容(受控组件)
   4. 发表评论,更新评论列表(setState())

# 四. React组件进阶

## 1. 组件通讯介绍

## 2. 组件的props

- 组件是封闭的,要接收外部数据应该通过props来实现
- props的作用: 接收传递给组件的数据
- 传递数据: 给组件标签添加属性
- 接收数据: 函数组件通过参数props接收数据,类组件通过this.props接收数据

```jsx
/*
props
*/

// 函数组件
// 2. 接收数据
const Hello = (props) => {
	// props是一个对象
	return (
		<div>
			// 3. 使用
			<h1>props: {props.name}</h1>
		</div>
	)
}

// 1. 传递数据
ReactDOM.render(<Hello name="jack" age={19} />, document.getElementById('root'))

/*
props
*/

// 类组件
// 2. 接收数据
class Hello extends React.Component {
	render() {
		// console.log(this.props);
		console.log('props:', props)
		props.fn()

		return (
			<div>
				<h1>props:{this.props.age}</h1>
				{props.tag}
			</div>
		)
	}
}

// 1. 传递数据
ReactDOM.render(
	<Hello 
		name="rose" 
		age={19} 
		colors={['red', 'green', 'blue']} 
		fn={[() => console.log('这是一个函数')]} 
		tag={<p>这是一个p标签</p>}
	/>, 
	document.getElementById('root')
)
```

特点:

1. 可以传递任意类型的数据

2. props是只读的对象, 只能读取属性的值,无法修改对象

3. 注意: 使用类组件时, 如果写了构造函数,应该将props传递给super() , 否则, 无法在构造函数中获取到props!

   ```jsx
   // 类组件
   class Hello extends React.Component {
   	// 推荐使用props作为constructor的参数!
   	constructor(props){
   		super(props)
   	}
   	console.log(props)
   }
   
   render() {
   	console.log('render:', this.props)	
   	
   	return (
   		<div>
   			<h1>props: </h1>
   		</div>
   	)
   }
   ```

### 3. 组件通讯的三种方式

组件之间的通讯分为3种:

1. 父组件 - > 子组件
2. 子组件 - > 父组件
3. 兄弟组件

3.1 父组件传递数据给子组件

1. 父组件提供要传递的state数据
2. 给子组件标签添加属性, 值为state中的数据
3. 子组件中通过props接受父组件中传递的数据

```jsx
// 父组件
class Parent extends React.Component {
	state = {
		lastName: '王'
	}
	
	render() {
		return (
			<div className="parent">
				父组件:
				<child name={this.state.lastName}/>
			</div>
		)
	}
}

// 子组件
const Child = (props) => {
	console.log('子组件', props)
	return (
		<div className="child">
			<p>子组件,接收到父组件的数据: {props.name}</p>
		</div>
	)
}

ReactDOM.render(<Parent />, document.getElementById('root'))
```

3.2 子组件传递数据给父组件

思路: 利用回调函数,父组件提供回调,子组件调用,将要传递的数据作为回调函数的参数

1. 父组件提供一个回调函数(用于接收数据)
2. 将该函数作为属性的值,传递给子组件
3. 子组件通过props调用回调函数
4. 将子组件的数据作为参数传递给回调函数

```jsx
// 父组件
class Parent extends React.Component {
	state = {
		parentMsg: ''
	}
	// 提供回调函数,用来接收数据
	getChildMsg = (data) => {
		console.log('接收到子组件中传递过来的数据', data)
		this.setState({
			parentMsg: data
		})
	} 
		
	render() {
		return (
			<div>
				子组件: <Child getMsg={this.getChildMsg} />
			</div>
		)
	}
}
// 子组件
class Child extends React.Component {
	state = {
		msg: '刷抖音'
	}
	
	handClick = () => {
		// 子组件调用父组件中传递过来的回调函数
		this.props.getMsg()
	}
	render() {
		<div className="child">
			子组件: <button onClick={this.handleClick}>点我,给父组件传递数据</button>
		</div>
	}
}
```

3.3 兄弟组件

- 将共享状态提升到最近的公共父组件中,由公共父组件管理这个状态
- 思想: 状态提升
- 公共父组件职责: 1. 提供共享状态 2. 提供操作共享状态的方法

### 4. Context

作用 : 跨组件传递数据(比如: 主题,语言等)

使用步骤:

1. 调用React.createContext()创建Provider(提供数据)和Consumer(消费数据)两个组件.

   ```jsx
   const { Provider, Consumer } = React.createContext()
   ```

2. 使用Provider组件作为父节点.

   ```jsx
   <Provider>
   	<div className="App">
   		<child1 />
   	</div>
   </Provider>
   ```

3. 设置value属性,表示要传递的数据

   ```jsx
   <Provider value="pink">
   ```

4. 使用Consumer组件接收数据

   ```jsx
   <Consumer>
   	{data => <span>data参数表示接收到的数据 -- {data}</span>}
   </Consumer>
   ```

### 5. props深入

5.1. children属性

- children属性: 表示组件婊气啊俺的子节点.当组件标签有子节点时,props就会有该属性

5.2. prpos校验

1. 安装包props-types( yarn add prop-types/npm i props-types)

2. 导入props-types包

3. 使用组件名.propTypes = {} 来给组件的props添加校验规则

4. 校验规则通过PropTypes对象来指定

   ```jsx
   import PropTypes from 'prop-types'
   function App(props) {
   	return (
   		<h1>Hi,{props.colors}</h1>
   	)
   }
   App.propTypes = {
   	// 约定colors属性为array类型
   	// 如果类型不对,则爆出明确错误,便于分析错误原因
   	colors: PropTypes.array
   }
   ```

