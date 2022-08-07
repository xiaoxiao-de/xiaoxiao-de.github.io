---
title: Minxins
date: 2022/06/06 21:30:11
tags: [Vue,JavaScirpt]
categories: 技术
cover: static\img\photo-1496449903678-68ddcb189a24.jpg
---
### Mixins

分发Vue组件复用功能

mixins是一个js对象,它可以包含我们组件中script的任意选项

类如: `data`  `components` `methods` `created` `computed` 等等

将组件中共用功能以对象的方式传入mixins选项

当组件使用mixins对象时所有mixins对象的选项都将被混入该组件

##### 使用场景

当存在多个组件中功能很相近时,就可以利用mixins将公共部分提取出来,通过mixins进行封装

```
// minxin是一个对象 所以应该以对象的形式来定义minxins可以和vue组件一样定义自身属性
export const myMinxins = {
  components:{},
  data() {
      return {
        
      }
  },
  create(){},
  mounted() {
     
  },
  computed: {},
  methods: {
  
  },
}
```

##### mixins的使用

```
// 在需要使用的组件中引入minxins 在mixins配置中使用
// minxins文件夹下minxins.js
<script>
import { myMinxins } from '@/minxins/minxins'
export default {
  name: 'Home',
  mixins: [myMinxins]
}
</script>
```

方法和参数在各组件中不共享，虽然组件调用了mixins并将其属性合并到自身组件中来了，但是其属性只会被当前组件所识别并不会被共享，也就是其他组件无法从当前组件中获取到mixins中的数据和方法。

值为对象(components、methods 、computed、data)的选项，混入组件时选项会被合并，键冲突时优先组件，组件中的键会覆盖混入对象的

值为函数(created、mounted)的选项，混入组件时选项会被合并调用，混合对象里的钩子函数在组件里的钩子函数之前调用

##### 与vuex的区别

**vuex：**状态管理机，里面定义的变量在每个组件中均可以使用和修改，在任一组件中修改此变量的值之后，其他组件中此变量的值也会随之修改。

**Mixins：**可以定义共用的变量，在每个组件中使用，引入组件中之后，各个变量是相互独立的，值的修改在组件中不会相互影响。

##### 与公共组件的区别

**组件**：在父组件中引入组件，相当于在父组件中给出一片独立的空间供子组件使用，然后根据props来传值，但本质上两者是相对独立的。

**Mixins：**则是在引入组件之后与组件中的对象和方法进行合并，相当于扩展了父组件的对象与方法，可以理解为形成了一个新的组件。
