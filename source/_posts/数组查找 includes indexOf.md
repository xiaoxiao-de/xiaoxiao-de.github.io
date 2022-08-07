---
title: JavaScript复习
date: 2022/06/06 21:30:11
tags: [JavaScript复习]
categories: 技术
cover: static\img\photo-1571361656693-d7602246ce3c.jpg
---
 *// 数组查找 includes indexOf* 

  *// ** 幂运算  === Math.pow()*

```
 const school = {
        name: "尚硅谷",
        cities: ["北京", "上海", "深圳"],
        xueke: ["Java", "Python", "CSS"]
    }
    console.log(Object.keys(school)) // 获得每个对象的键集合成数组
    console.log(Object.values(school)) // 获得每个对象的值集合成数组
    console.log(Object.entries(school)) // 把对象转为二维数组
    const map = new Map(Object.entries(school)) // 转为Map结构
    console.log(map)
    Object.getOwnPropertyDescriptors(school) // 查看对象创建时的内置属性有哪些
```

 *// trim 清除字符串两边空格* 

  *// trimStart 清楚字符串左边空格*

  *// trimEnd 清除字符串右边空格*

```
// flat 将多维数组转化为低维数组 参数为深度 是一个数字
  /* const arr = [1,2,3,4,[5,6], [7,8]] */
  /* console.log(arr.flat()) */
  // flatMap 返回的map结果如果是个多维数组可以转化为低维数组
  const arr1 = [1, 2, 3, 4]
  const result = arr1.flatMap( item => [item * 10])
  console.log(result)
```

#声明的属性为私有属性

Object,is() 判断两个值是否相等

 **Object**.**assign**('合并到的目标对象', "参数对象1", "参数对象二") *// 对象合并 浅拷贝*

border-radius: 25px 25px; 如果设置了两个值，第一个用于左上角和右下角，第二个用于右上角和左下角

 box-shadow: 0px 0px 10px #888; 水平阴影 垂直阴影 阴影大小 颜色                                                                         
