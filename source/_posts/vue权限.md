---
title: vue权限
date: 2022/06/12 21:21:42
tags: [vue, ES6, JavaScript]
categories: 技术
cover: static\img\photo-1516166328576-82e16a127024.jpg
---
## vue权限 

##### 什么是权限

权限是对特定资源访问,也就是确保用户只能访问到被分配的资源

前端发起的请求都会触发路由和视图

可以从路由和视图方面进行控制

- 路由方面:用户登录后只能看到自己有权访问的菜单导航
- 视图方面: 用户只能看到自己有权浏览的内容和控件

前端权限控制分为四个方面:

- 接口权限
- 按钮权限
- 菜单权限
- 路由权限

##### 接口权限

接口权限一般采用jwt的形式验证,没有通过的话返回401,跳转到登录页面重新登录

登录完成拿到token,将token存起来通过axios请求拦截器进行拦截,每次请求的时候请求头携带token

##### 菜单权限

菜单与路由分离,菜单由后端返回

```
// 引入vuex vue vuerouter 菜单接口
import Vue from 'vue'
import VueRouter from 'vue-router'
import { getData } from '@/utils/api.js'
import store from '@/store/index.js'
// 挂载到vue上
Vue.use(VueRouter)
// 定义路由配置
const routes = [
  {
    path: '/',
    component: () => import(/* webpackChunkName: "about" */ '../views/home.vue')
  },
]
// 动态生成数据
// arr.forEach(item => {
//   routes.push({
//     path: item.path,
//     name: item.name,
//     meta: { title: item.title },
//     component: () => import(`../views/rightMain/content/${item.component}`)
//   })
// })
// 声明路由器
const router = new VueRouter({
  routes
})
//路由前置守卫
router.beforeEach(async(to, from, next) => {
  /* 由于每次点击页面菜单是都会向后台发起请求
    为减少页面开销,将登录成功后后端返回的路由菜单存在vuex缓存中
    让页面点击时不用每次都请求一次,减少不必要的操作
  */
  // 判断vuex是否存在数据 存在放行 不存在获取数据
  if(store && store.state.nav.length === 0){
  // 获取数据
  let { menuData }  = await getData().then(res => res.data.Data)
  // 数据缓存
  store.dispatch('SETNAV',menuData)
  // 处理路由配置
  let routerConfig = addmyRoute(menuData)
  // 动态添加路由
  router.addRoutes(routerConfig)
  // 进行跳转
  next({path: to.path})
  } else {
    next()
  }
})
function addmyRoute(menuData){
  console.log(menuData)
  menuData.forEach(item => {
  routes.push({
    path: item.path,
    name: item.name,
    // meta: { title: item.title },
    component: () => import(/* webpackChunkName: "about" */ `../views/${item.component}.vue`)
  })
})
return routes
}
export default router

```

```
// vuex部分
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
    nav: []
  },
  getters:{
    navData: state => state.nav
  },
  mutations: {
    SETNAV(state,data){
      state.nav = data
    }
  },
  actions: {
    SETNAV({commit}, data){
      commit('SETNAV',data)
    }
  },
  modules: {
  }
})
---------------------------
// 当登录成功后可以将菜单存储在vuex中
async getNav(){
    let { data } = await getData()
      this.$store.dispatch('SETNAV',data.list)
 }
---------------------------
// 请求拦截器 响应拦截器
import axios from 'axios'
axios.defaults.baseURL = 'http://localhost:9999/'
// 请求拦截器
axios.interceptors.request.use(config => config)
//响应拦截器
axios.interceptors.response.use(res => {
  return res
}, err => {
  return Promise.reject(err)
})
export default axios
```

