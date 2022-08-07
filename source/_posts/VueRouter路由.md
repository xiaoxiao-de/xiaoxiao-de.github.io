---
title: VueRouter
date: 2022/05/16 20:46:25
tags: [Vue,JavaScript]
categories: 技术
cover: static\img\20200401220041648.gif
---

### VueRouter路由

vue 的一个插件库，专门用来实现SPA 应用

 vue2中使用的是vue-router@3.x

 vue3中使用的是vue-router@4.x

##### 路由

一个路由就是一组映射关系

key为路径,value就是一个component或者一个方法

路由分为前端路由和后端路由

后端路由是一个方法服务器接收到一个请求时，根据请求路径找到匹配的函数来处理请求，返回响应数据

前端路由:value 是 component，用于展示页面内容，当浏览器的路径改变时，对应的组件就会显示

路由下载

```
// vue2
npm i vue-router
// vue3
npm i vue-router@next
```

在vue2中

1. 每个组件都有自己的`$route`属性，里面存储着自己的路由信息
2. 整个应用只有一个router，可以通过组件的`$router`属性获取到
3. 子路由跳转：`<router-link to="/a/b">a</router-link>`
4. 要写（/）路径来明确子路由地址
5. 路由to可以省略/默认Vue-router给加上了
6. 路由跳转可以携带参数一般存放在query或params
7. 路由的命名可以简化路由的跳转及一些操作
8. 路由携带params参数时，若使用to的对象写法，则不能使用path配置项，必须使用name配置

```
import Vue from 'vue' // 引入vue
import Router from 'vue-router' // 引入vueRouter
//创建一个路由 配置路由
// 声明一个数组 
const routes = [
  {
    path: '/',
    name: 'Home',
    component: () => import('../view/Home.vue')
  }
]
// 挂载到vue上
Vue.use(Router)
// 声明一个路由器并暴露出去
const router = new Router({
  base: '/', // 应用的基本路径
  mode: ''  // 路由模式 hash模式 history模式 abstract模式
  routes
})

export default router
```

在vue3中

vue3中使用组合式API(Composition API)

vue3不在使用newRouter()创建实例而是使用createRouter方法

路由模式也是分别通过 createWebHistory,createWebHashHistory,createMemoryHistory传递base来创建 

```
// 使用组合式api方式引入使用的对象方法而不是引入整个包
import { createRouter, createWebHistory } from 'vue-router'

//创建一个路由 配置路由
// 声明一个数组 
const routes = [
  {
    path: '/',
    name: 'Home',
   component: () => import('../view/Home.vue')
  },
]
// 声明一个路由器并暴露出去
const router = createRouter({
  // 创建了一个history设置了基本路径
  history: createWebHistory(process.env.BASE_URL), 
  routes
})

export default router
```

##### vue2中路由跳转传参

query

```
!-- 跳转并携带query参数，to的字符串写法 -->
<router-link :to="/home/message/detail?id=666&title=你好">跳转</router-link>
				
<!-- 跳转并携带query参数，to的对象写法 -->
<router-link :to="{
	path:'/home/message/detail',
	query:{
		id:666,
        title:'你好'
	}
}">跳转</router-link>


// 接收参数
$route.query.id
$route.query.title
```

params

```
// 传递参数
                <!-- 跳转路由并携带params参数，to的对象写法 -->
                <router-link :to="{
                    name:'xiangqing',
                    params:{
                        id:m.id,
                        title:m.title
                    }
                }">
                    {{m.title}}
                </router-link>
                
               
 // 接收参数
$route.params.id
$route.params.title
```

##### 路由跳转的replace方法

vue2中

- 控制路由跳转时操作浏览器历史记录的模式

- 浏览器的历史记录有两种写入方式：`push`和`replace`，其中`push`是追加历史记录，`replace`是替换当前记录。路由跳转时候默认为`push`方式

- 开启`replace`模式：`<router-link replace ...>News</router-link>`

```
// vue2
this.$router.push() // 跳转路由追加记录
this.$router.replace() // 跳转路由替换当前记录不追加记录
this.$router.forward() //前进
this.$router.back() //后退
this.$router.go() //可前进也可后退
```

vue3中

由于vue3的setup方法没有this,所有不能使用this.$router获取路由对象

vue3中使用useRouter方法进行跳转

```
import { useRouter } fom vue-router

export default {
  setup() {
    const router = useRouter() // 声明一个路由
    router.push() // 跳转路由追加记录
    router.replace() // 跳转路由替换当前记录不追加记录
  }
}
```



##### 缓存路由组件

```
<keep-alive include="">  //include中写想要缓存的组件名，不写表示全部缓存
				<router-view></router-view>
</keep-alive>
```

让不展示的路由组件保持挂载，不被销毁

`activated`和`deactivated`是路由组件所独有的两个钩子，用于捕获路由组件的激活状态

1. `activated`路由组件被激活时触发
2. deactivated`路由组件失活时触发

```
 activated(){
            console.log('News组件被激活了')
            this.timer = setInterval(() => {
                this.opacity -= 0.01
                if(this.opacity <= 0) this.opacity = 1
            },16)
        },
        deactivated(){
            console.log('News组件失活了')
            clearInterval(this.timer)
        }
```

##### 全局路由守卫

```
//全局前置路由守卫————初始化的时候、每次路由切换之前被调用
router.beforeEach((to,from,next) => {
    console.log('前置路由守卫',to,from)
    if(to.meta.isAuth){
        if(localStorage.getItem('school')==='atguigu'){
            next()
        }else{
            alert('学校名不对，无权限查看！')
        }
    }else{
        next()
    }
})

//全局后置路由守卫————初始化的时候被调用、每次路由切换之后被调用
router.afterEach((to,from)=>{
	console.log('后置路由守卫',to,from)
	document.title = to.meta.title || '硅谷系统'
})
//独享守卫，特定路由切换之后被调用
                    beforeEnter(to,from,next){
                        console.log('独享路由守卫',to,from)
                        if(localStorage.getItem('school') === 'atguigu'){
                            next()
                        }else{
                            alert('暂无权限查看')
                        }
                    }
 //通过路由规则，离开该组件时被调用
        beforeRouteEnter (to, from, next) {
            console.log('About--beforeRouteEnter',to,from)
            if(localStorage.getItem('school')==='atguigu'){
                next()
            }else{
                alert('学校名不对，无权限查看！')
            }
        },
        //通过路由规则，离开该组件时被调用
        beforeRouteLeave (to, from, next) {
            console.log('About--beforeRouteLeave',to,from)
            next()
        }
```

对于一个url来说，什么是hash值？—— #及其后面的内容就是hash值

hash值不会包含在 HTTP 请求中，即：hash值不会带给服务器

hash模式：

地址中永远带着#号，不美观
若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法
兼容性较好
history模式：

地址干净，美观
兼容性和hash模式相比略差
应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题

##### 默认路由地址重定向