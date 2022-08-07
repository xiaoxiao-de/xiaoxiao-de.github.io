---
title: Promise
date: 2022/05/15 20:46:25
tags: [ES6,JavaScript]
categories: 技术
cover: static\img\photo-1571361656693-d7602246ce3c.jpg
---
## Promise的理解和使用

- promise是一门新的技术(es6规范)
- promise是异步编程的新解决方案
- 从语法上来说:promise是一个构造函数
- 从功能上来说:promise对象用来封装异步操作并可以获取成功和失败的结果
- promise: 启动异步任务 => 返回promise对象 => 给promise对象绑定回调函数

#### promise的优点

支持链式调用,可以解决回调地狱问题

> 什么是回调地狱？
>
> 回调函数嵌套调用,外部回调函数异步执行的结果是嵌套的回调执行条件

> 回调地狱的缺点？
>
> 不便于阅读
>
> 不便于异常处理

------

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <div class="container">
    <button class="btn" id="btn">点击抽奖</button>
  </div>
</body>
<script>
  function rand(m,n) {
    return Math.ceil(Math.random()*(n-m+1)) + m+1
  }
  let btn =  document.querySelector("#btn")
  btn.addEventListener('click',function(){
    // 原生回调形式
    // setTimeout(() => {
    //   let n = rand(1,100)
    //   if(n <= 30) {
    //     console.log(n)
    //     alert("恭喜中奖")
    //   } else {
    //     console.log(n)
    //     alert("谢谢惠顾")
    //   }
    // },2000)
    // promise形式 promise有两个参数 resolve 返回成功操作
    // reject 表示失败操作  它可以包裹异步操作并对其进行封装
    let p =  new Promise((resolve, reject) => {
      setTimeout(() => {
      let n = rand(1,100)
      if(n <= 30) {
        console.log(n)
        resolve() // 将promise对象的状态设置为成功
      } else {
        console.log(n)
        reject() // 将promise对象的状态设置为失败
      }
    },1000)
    }) 
    p.then(() =>{
      alert("恭喜中奖")
    }, () => {
      alert('谢谢惠顾')
    })
  })
</script>
</html>
```

promise的状态是实例对象[PromiseState]的一个属性它有3种状态 

初始状态/未决定的  pending 

当执行结果为成功时状态为pending会变为fullfilled

当执行结果失败时状态为pending会变为rejected

一个promise对象只能改变一次无论变为成功还是失败都会返回一个数据结果

成功数据结果一般称为value

失败数据结果一般称为reason

Promise对象的值是实例对象的另一个属性[PromiseResult] 保存成功和失败的结果

![](C:\Users\Administrator\Downloads\promise工作流程.png)

相关API

promise构造函数: new Promise() 接收一个函数作为参数

参数内部接收两个函数分别为 resolve(成功)  reject(失败) 它们分别执行失败和成功的回调

Promise.protype.then方法 （'函数一','函数二'）=> {} 

指定用于得到一个成功或失败的回调返回一个新的promise对象

Promise.protype.catch 方法 （）=> {}

指定一个失败的回调

Promise.resolve 方法 (value) => {}

传入的参数为非promise类型的对象,则返回的结果为成功的promise

传入的参数为promise对象.则参数的结果决定了resolve的结果

Promise.reject 方法() => {}

返回一个失败的promise对象

Promise.all方法 ([]) => {}

包含n个promise数组 

返回一个新的promise,只有所有的promise都成功才成功,只要失败了一个就直接失败

Promise.race 方法 () => {}

包含n个promise的数组

返回一个新的promise 第一个完成的promise的结果状态就是最终的结果状态
