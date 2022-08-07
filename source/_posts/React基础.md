---
title: React基础
date: 2022/06/07 22:24:09
tags: [React基础]
categories: 技术
cover: static\img\photo-1496449903678-68ddcb189a24.jpg
---

## React基础

##### react安装

```
npm install -g create-react-app // 全局安装
```

##### 创建一个项目

```
create-react-app 项目名
```

##### 不想全局安装可以使用npx方式

```
npx create-react-app 项目名
// 需要等待一段时间,在等待时间内实际是安装react库
// react: react顶级库
// react-dom: web的react运行环境 比如app端的react-native
// react-scripts: 包含运行和打包react应用程序的所有脚本配置
// 安装成功后会生成如下的目录结构
|-- README.md 使用方法的文档
|-- node_modules 所有依赖安装的目录
|-- package-lock.json 锁定安装时包的版本号，确保多人开发时依赖能保持一致
|-- package.json 下载的安装包插件
|-- public  静态资源公共目录
|-- src 源代码目录
```

##### npm安装失败

- 切换npm镜像源
- 使用yarn安装
- 删除`node_modules`及package-lock.json重新执行 `npm install`
- 清除npm缓存 `npm cache clean --force` 再执行 `npm install`

##### react基本结构

```
// 从react的包当中引入react,只要写react.js组件就必须引入
import React from 'react'
// react-dom可以把组件渲染到web页面
import ReactDOM from 'react-dom'
// react-dom中有一个render方法,负责将组件渲染与构造dom树,然后插入到页面元素上
ReactDOM.render('组件名',document.getElementById('挂载的页面id节点'))

```

##### JSX语法

- JSX将HTMl语法直接加入到JavaScript代码中
- 由Babel的JSX编译器实现
- 通过编译时会转换为纯JavaScript

##### JSX原理

```
// 要明白JSX的原理，需要先明白如何用 JavaScript 对象来表现一个 DOM 元素的结构? 
//基本结构
div class='app' id='appRoot'>  
<h1 class='title'>欢迎进入React的世界</h1> 
<p>    
React.js 是一个帮助你构建页面 UI 的库  
/p> 
</div> 
// 使用JavaScript对象
{  tag: 'div',
attrs: { className: 'app', id: 'appRoot'},  
children: [    
{ tag: 'h1', 
attrs: { className: 'title' },
children: ['欢迎进入React的世界'] }, 
{  
  tag: 'p', 
  attrs: null, 
  children: ['React.js 是一个构建页面 UI 的库']    
  }  
  ] 
 }
 /*
JavaScript 写起来太长了，结构看起来又不清晰，用 HTML 的方式写起来就方便很多了
 React.js 就把 JavaScript 的语法扩展了一下
 让 JavaScript 语言能够支持这种直接在 JavaScript 代码里面编写类似 HTML 标签结构的语法
 编译的过程会把类似 HTML 的 JSX 结构转换成 JavaScript 的对象结构
*/
// react编译前代码
import React from 'react' 
import ReactDOM from 'react-dom'
class App extends React.Component { 
  render () {
    return (
      <div className='app' id='appRoot'> 
      <h1 className='title'>欢迎进入React的世界</h1>
      <p>
      React.js 是一个构建页面 UI 的库
      </p>
      </div> 
      ) 
      } 
     }
ReactDOM.render(    <App />,  document.getElementById('root') )
// react编译后代码
import React from 'react' 
import ReactDOM from 'react-dom'
class App extends React.Component {  
  render () {
    return (
      React.createElement(
      "div",
      { 
        assName: 'app',
        id: 'appRoot'
      },
      React.createElement("h1", 
      { 
      className: 'title' 
      },          
      "欢迎进入React的世界"        
      ),        
      React.createElement("p",
                           null,          
                           "React.js 是一个构建页面 UI 的库"        
                           )      
                         )    
                       )  
                     } 
                   }
ReactDOM.render(    React.createElement(App),  document.getElementById('root') )
// React.createElement会构建一个 JavaScript 对象来描述你 HTML 结构的信息，包括标签名、属性、 还有子元素等, 语法为
// 编译顺序 JSX —使用react构造组件，bable进行编译—> JavaScript对象 — ReactDOM.render() —>DOM 元素 —>插入页面
```

##### class组件

```
// es6 class组件其实就是一个构造器,每次使用组件都相当于实列化组件
import React from 'react'
import ReactDOM from 'react-dom'
class App extends React.Component {  
  render () {    
    return (          
      <h1>欢迎进入React的世界</h1>    
    )  
  } 
} 
/*
const app = new App({
  name: 'react'
}).render()
*/
ReactDOM.render(  <App/>,  document.getElementById('root') )
```

##### 函数式组件

```
import React from 'react'
import ReactDOM from 'react-dom'
// 函数式组件 组件名必须大写
const App = () => <h1>欢迎进入React的世界</h1> 

ReactDOM.render(
  <App/>
  document.getElementById('root')
)
```

##### 组件样式

```
// react中给虚拟dom添加行内样式,需要使用表达式传入样式对象的方式实现
const App = () => <h1 style={{color: 'red'，fontSize: '14px'}}>
欢迎进入React的世界
</h1>
// 给元素添加类名
const App = () => <h1 clssName="styleName">
欢迎进入React的世界
</h1>
// 由于css的class样式跟js的class同名因而发起冲突因此react中class为className
// label中的for为htmlFor
// class ==> className for ==> htmlFor(label)
```

##### 事件绑定

- `react`事件采用 on+事件名方式绑定
- `react`的事件是以驼峰命名显示`onClick`
- `react`的事件并不是原生事件,而是合成事件

###### 因为this指向与函数执行的原因因此

```
//react推荐在组件内使用箭头函数定义一个方法
const App = () => <h1 clssName="styleName" onClick={ handlerClick() }>
handlerClick = () => {
 console.log(123)
}
const App = () => <h1 clssName="styleName" onClick={ this.handlerClick  }>
handlerClick(){
 console.log(123)
}
/*react不推荐
直接在组件内定义一个非箭头函数方法,然后render里直接使用onClick={this.handlerClick.bind(this)}
*/
//直接在render里写行内的箭头函数(不推荐) 
const App = () => <h1 clssName="styleName" onClick={ () => { this.handlerClick() } }>
handlerClick(){
 console.log(123)
}
```

##### Event对象

和普通浏览器一样，事件handler会被自动传入一个 event 对象，这个对象和普通的浏览器 event 对 象所包含的方法和属性都基本一致。不同的是 React中的 event 对象并不是浏览器提供的，而是它自 己内部所构建的。它同样具有 event.stopPropagation 、 event.preventDefault 这种常用的方法

##### Ref应用

```
// 给标签设置ref
myRef = React.createRef() // 创建ref
    <div>
        <input type='text' ref={this.myRef}></input> // 引用ref
        <button onClick={ () => {
          console.log(this.myRef.current.value)
        }}>点击</button>
      </div>
```

##### 状态(state)

状态就是组件描述某种显示情况的数据，由组件自己设置和更改，也就是说由组件自己维护，使用状态 的目的就是为了在不同的状态下使组件的显示不同

```
// 定义状态的两种方式
import React, { Component } from 'react'
import ReactDOM from 'react-dom'

class App extends Component {
 /* state = {
    name: 'React',
    isLiked: false
  } */
  constructor(){
    super()
    this.state = {
      name: 'React',
      isLiked: false
    }
  }
  handleBtnClick = () => {
    this.setState({
      isLiked: !this.state.isLiked
    })
  }
  render () {
    return (
      <div>
        <h1>欢迎来到{ this.state.name } 的世界</h1>
        <button onClick={this.handleBtnClick}>
        {
          this.state.isLiked ? '取消' : '收藏'
        }
        <button>
      </div>
    )
  }
}
/*this.state 是纯js对象,在vue中，data属性是利用 Object.defineProperty 处理过的，更改 data的 数据的时候会触发数据的 getter 和 setter ，但是React中没有做这样的处理，如果直接更改的话， react是无法得知的，所以，需要使用特殊的更改状态的方法 setState */
```

setState有两个参数第一个参数可以是对象,也可以是方法return一个对象

```
// 参数是对象
this.setState({
  isLiked: !this.state.isLiked
})
// 参数是方法
this.setState((preState, props) => {
  return {
    isLiked: !preState.isLiked 
  }
})
/*
注意的是这个方法接收两个参数，第一个是上一次的state, 第二个是props setState 是异步的，所以想要获取到新的state，没有办法获取，就有了第二个参数，这是一个可选 的回调函数
*/
this.setState((preState, props) => {
  return {
    isLiked: !preState.isLiked
  }
}, () => {
  console.log('回调里的',this.state.isLiked)
} )
```

##### 属性(props)

props 是正常是外部传入的，组件内部也可以通过一些方式来初始化的设置，属性不能被组件自己更 改，但是你可以通过父组件主动重新渲染的方式来传入新的 props
属性是描述性质、特点的，组件自己不能随意更改。
之前的组件代码里面有 props 的简单使用，总的来说，在使用一个组件的时候，可以把参数放在标签的 属性当中，所有的属性都会作为组件 props 对象的键值。通过箭头函数创建的组件，需要通过函数的 参数来接收 props 

- 在组件上通过key=value 写属性,通过this.props获取属性,这样组件的可复用性提高了
- 注意在传参数时候，如果写成isShow="true" 那么这是一个字符串    如果写成isShow={true} 这个
  是布尔值
-  {...对象}  展开赋值
- 默认属性值

```
// 父组件
import React, { Component } from 'react'
import Navbar from './Navbar/index'
export default class App extends Component {
    state = {}
    render() {
    return (
      <div>
          <div>
           <h2>首页</h2>
          <Navbar title="首页" leftShow={false}/>
          </div>
          <div>
            <h2>列表</h2>
            <Navbar title="列表" leftShow={true}/>
          </div>
          <div>
            <h2>购物车</h2>
            <Navbar title="购物车" leftShow={true}/>
          </div>
      </div>
    )
  }
}
// 子组件
import React, { Component } from 'react'
export default class Navbar extends Component {
  render() {
    let { title,leftShow } = this.props
    return (
      <div>
          {leftShow && <button>返回</button>}
          { title }
          <button>home</button>
      </div>
    )
  }
}

```

prop-types属性验证

```
import propTypes from 'prop-types' // 引入属性验证包
// 需要验证的属性类属性
*.propTypes = {
  name: propTypes.string,
  age: propTypes.number
}
// 在class中写法
static propTypes = {
  myname: propTypes.string,
   myshow: propTypes.bool
}
```

##### 属性与状态

相似点：都是纯js对象，都会触发render更新，都具有确定性（状态/属性相同，结果相同）

不同点： 
1. 属性能从父组件获取，状态不能 
2. 属性可以由父组件修改，状态不能 
3.  属性能在内部设置默认值，状态也可以，设置方式不一样 
4.  属性不在组件内部修改，状态要在组件内部修改 
5.  属性能设置子组件初始值，状态不可以 
6.  属性可以修改子组件的值，状态不可以 

state 的主要作用是用于组件保存、控制、修改自己的可变状态。 state 在组件内部初始化，可以被 组件自身修改，而外部不能访问也不能修改。你可以认为 state 是一个局部的、只能被组件自身控制 的数据源。 state 中状态可以通过 this.setState 方法进行更新， setState 会导致组件的重新渲 染。

props 的主要作用是让使用该组件的父组件可以传入参数来配置该组件。它是外部传进来的配置参 数，组件内部无法控制也无法修改。除非外部组件主动传入新的 props ，否则组件的 props 永远保持 不变。

没有 state 的组件叫无状态组件（stateless component），设置了 state 的叫做有状态组件 （stateful component）。因为状态会带来管理的复杂性，我们尽量多地写无状态组件，尽量少地写有 状态的组件。这样会降低代码维护的难度，也会在一定程度上增强组件的可复用性。

##### 渲染数据