---
title: ReactHooks
date: 2022/06/13 22:29:12
tags: [ES6, JavaScript, React]
categories: 技术
cover: static\img\photo-1591693117893-7cbfc0a7ac69.jpg
---
## ReactHooks

hooks优点:

1. 高阶组件为了复用，导致代码层级复杂 
2.  生命周期的复杂 
3.  写成functional组件,无状态组件 ，因为需要状态，又改成了class,成本高
4. 不存在生命周期，所以不要把 Class Component 的生命周期概念搬过来试图对号入座

```
// useState保存组件状态
const [ state, setstate] = useState(initialState)
```

```
// useEffect处理副作用
useEffect( () => {
  return () => {}
},[依赖的状态;空数组表示不依赖])
// 案例
let id = props.match.params.myid 
useEffect(()=>{    
axios.get(`/articles/${id}`).then(res => {        
settitle(res.data.title)        
setcontent(res.data.content)        
setcategory(res.data.category)   
}) },[id])
/*
简单来说就是调用时机不同， useLayoutEffect 和原来 componentDidMount & componentDidUpdate 一致，在 react完成DOM更新后马上同步调用的代码，会阻塞页面渲染。而 useEffect 是会在整个页面渲染完才会调用的 代码
*/
/*
在实际使用时如果想避免页面抖动（在 useEffect 里修改DOM很有可能出现）的话，可以把需要操作DOM的代码 放在 useLayoutEffect 里。在这里做点dom操作，这些dom修改会和 react 做出的更改一起被一次性渲染到屏幕 上，只有一次回流、重绘的代价。
*/
```

```
// useCallback记忆函数
防止因为组件重新渲染，导致方法被重新创建 ，起到缓存作用; 只有第二个参数 变化了，才重新声明一次
let handleClick = useCallback( () => 
{    
console.log(name)  
},[name])   
<button onClick={()=>handleClick()}>hello</button>    
//只有name改变后， 这个函数才会重新声明一次，   
//如果传入空数组， 那么就是第一次创建后就被缓存， 如果name后期改变了,拿到的还是老的name。  
//如果不传第二个参数，每次都会重新声明一次，拿到的就是最新的name
```

```
// useMemo 记忆组件
// useCallback 的功能完全可以由 useMemo 所取代
// 如果你想通过使用 useMemo 返回一个记忆函数也是完全可以的。
/* useMemo是针对一个函数，是否多次执行
useMemo主要用来解决使用React hooks产生的无用渲染的性能问题
在方法函数，由于不能使用shouldComponentUpdate处理性能问题，react hooks新增了useMemo
*/
let richChild = useMemo(() => {
		//执行相应的函数
		return getRichChild();
	}, [props.name]);
/*
useCallback 常用记忆事件函数，生成记忆后的事件函数并传递给子组件使用。而 useMemo 更适合经过函数 计算得到一个确定的值，比如记忆组件
*/
```

```
// useRef保存引用值
mport React, {useRef} from "react";

function Ref (){
    const box = useRef()

    return (
        <div>
            <div ref={box}>useRef</div>
            <button onClick={() => console.log(box)}>+1</button>
        </div>
    )
}
export default Ref;
/*
 当我们需要获取元素对象的时候， 首先引入useRef， 其次调用useRef（）方法接收它的返回值，我们需要获取那个DOM元素就在那个DOM元素上进行绑定，通过ref属性将useRef的返回值绑定到元素身上，这样useRef的返回值，通过useRef返回一个对象，对象内部有个current属性，这个属性就对应着我们需要的元素对象；
*/
import React, {useRef, useEffect, useState} from "react";

function Ref (){
    let timerId = useRef()
    const [count, setCount] = useState(0)
    useEffect(() => {
        timerId.current = setInterval(() => {
            setCount(count => count + 1)
        }, 1000)
    }, [])
    const stop = () => {
        console.log(timerId)
        clearInterval(timerId.current)
    }
    return (
        <div>
            <div>{count}</div>
            <button onClick={stop}>停止</button>
        </div>
    )
}
export default Ref;
```

```
//useReducer和useContext减少组件层级
import React, { useContext, useReducer } from 'react'

const intialState = {
  a: 1,
  b: 1
}
const reducer = (preState, action) => {
  let newState = {...preState}
  switch(action.type){
    case 'add1':
        newState.a = action.value
        return newState
    case 'add2':
        newState.b++
        return newState
    default:
        return preState
  }
}
const GlobalContext = React.createContext()
export default function App() {
  const [state, dispatch] = useReducer(reducer,intialState)
  return (
    <GlobalContext.Provider value={
      {
        state,
        dispatch
      } 
    }>
    <div>
      <Chilren01>
        
      </Chilren01>
      <Chilren02 >
        
      </Chilren02>
      <Chilren03 >

      </Chilren03>
    </div>
    </GlobalContext.Provider>
  )
}
function Chilren01(){
  const { dispatch } = useContext(GlobalContext)
  return (
    <div>
        <button onClick={
           () => {
            dispatch({
                type: 'add1',
                value: 222222
              })
           }
        }>改变a</button>
        <button 
        onClick={
           () => {
            dispatch({
                type: 'add2'
              })
           }
            }>改变b</button>
    </div>
  )
}
function Chilren02(){
    const { state } = useContext(GlobalContext)
  return (
    <div style={{background: 'gray'}}>
        <span>
            {state.a}
        </span>
    </div>
  )
}
function Chilren03(){
    const { state } = useContext(GlobalContext)
  return (
    <div style={{background: 'pink'}}>
      <span>
        {state.b}
      </span>
    </div>
  )
}
```

> 自定义hooks
>
> 当我们想在两个函数之间共享逻辑时，我们会把它提取到第三个函数中。
> 必须以“use”开头吗？必须如此。这个约定非常重要。不遵循的话，由于无法判断某个函数是否包含对其内部 Hook 的调用，React 将无法自动检查你的 Hook 是否违反了 Hook 的规则