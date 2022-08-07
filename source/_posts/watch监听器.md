---
title: watch监听器
date: 2022/06/12 21:21:42
tags: [vue, ES6, JavaScript]
categories: 技术
cover: static\img\photo-1516166328576-82e16a127024.jpg
---
## watch监听器

##### 监听简单数据类型

```
data(){
  return{
    name: '张三'
  }
}
watch: {
  name(val){
    console.log(val)
  }
}
```

##### 监听复杂数据类型

```
data(){
  return{
    obj: {
      name: '张三',
      age： 18
    }
  }
}
watch: {
  obj: {
    immediate: true // watch首次绑定时监听 默认false不监听
    deep: true // 深度监听 默认false不开启深度监听
    handler(val){  //  handler函数不能为箭头函数,this会取上下文
    //深度监听必须为hander否着不生效
      console.log(val)
    }
  }
}
```

##### 监听对象某个属性

```
data(){
  return{
    obj: {
      name: '张三',
      age： 18
    }
  }
}
watch: {
  'obj.name'(val){
    console.log(val)
  }
}
```

