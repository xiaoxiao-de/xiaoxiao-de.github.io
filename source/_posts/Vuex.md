---
title: Vuex
date: 2022/05/13 20:46:25
tags: [Vue,JavaScript]
categories: 技术
cover: static\img\photo-1571361656693-d7602246ce3c.jpg
---
### Vuex

专门在 Vue 中实现集中式状态管理的一个 Vue 插件,对 vue 应用中多个组件的共享状态进行集中式的管理,也是一种组件间通信的方式，且适用于任意组件间通信

##### 什么时候使用Vuex

多个组件依赖同一状态

来自不同组件的行为需要变更同一状态

##### Vuex工作原理图

![](C:\Users\Administrator\Downloads\96f683295b77044c190baaea188ccb90.png)

##### Vuex基本环境

```
//引入Vue核心库
import Vue from 'vue'
//引入Vuex
import Vuex from 'vuex'
//应用Vuex插件
Vue.use(Vuex)
   
//准备actions对象——响应组件中用户的动作、处理业务逻辑
const actions = {}
//准备mutations对象——修改state中的数据
const mutations = {}
//准备state对象——保存具体的数据
const state = {}
   
//创建并暴露store
export default new Vuex.Store({
   	actions,
   	mutations,
   	state
})
```

初始化数据state, 配置actions, mutations

组件中读取vuex中的数据: $store.state."数据名"

组件中修改vuex中的数据: $store.dispatch('actions的方法名',数据)

或$store.commit("mutations中的方法名",数据)

若没有网络请求或其他业务逻辑，组件中也可以越过`actions`，即不写`dispatch`，直接编写commit`

##### getters配置项

当state中的数据需要经过加工后再使用时,可以使用getters加工

在store.js中追加getters配置

```
const getters = {
}
```

组件读取数据: $store.getters."数据"

##### map方法的使用

mapState: 用于映射state中的数据(一般用于计算属性)

```
computed: {
    //借助mapState生成计算属性：sum、school、subject（对象写法）
     ...mapState({sum:'sum',school:'school',subject:'subject'}),
         
    //借助mapState生成计算属性：sum、school、subject（数组写法）
    ...mapState(['sum','school','subject']),
},
```

mapGetters:用于映射getters中的数据(一般用于计算属性)

```
computed: {
    //借助mapGetters生成计算属性：bigSum（对象写法）
    ...mapGetters({bigSum:'bigSum'}),

    //借助mapGetters生成计算属性：bigSum（数组写法）
    ...mapGetters(['bigSum'])
},
```

mapActions方法:用于生成actions对话的方法(包含$store.dispath()函数)

```
methods:{
    //靠mapActions生成：incrementOdd、incrementWait（对象形式）
    ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})

    //靠mapActions生成：incrementOdd、incrementWait（数组形式）
    ...mapActions(['jiaOdd','jiaWait'])
}
```

mapMutations: 用于生成与mutations对话的方法(包含$store.commit()函数)

```
methods:{
    //靠mapMutations生成：increment、decrement（对象形式）
    ...mapMutations({increment:'JIA',decrement:'JIAN'}),
    
    //靠mapMutations生成：JIA、JIAN（对象形式）
    ...mapMutations(['JIA','JIAN']),
}
```

`mapActions`与`mapMutations`使用时，若需要传递参数，则需要在模板中绑定事件时传递好参数，否则参数是事件对象