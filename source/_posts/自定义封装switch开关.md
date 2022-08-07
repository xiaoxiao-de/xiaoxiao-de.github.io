---
title: ui库switch开关
date: 2022/06/27 11:48:12
tags: [ui库]
categories: ui库
cover: static\img\photo-1571361656693-d7602246ce3c.jpg
---
## 自定义封装switch开关

| 参数名        | 参数描述         | 参数类型 | 默认值  |
| ------------- | ---------------- | -------- | ------- |
| v-model       | 双向绑定         | Boolean  | false   |
| name          | name属性         | String   | text    |
| activeColor   | 自定义的激活颜色 | String   | #1ec63b |
| inactiveColor | 自定义不激活颜色 | String   | #dd001b |

事件

| 事件名称 | 事件描述           |
| -------- | ------------------ |
| change   | change时触发的事件 |

```
<template>
<!-- 点击时触发滑块滑动改变样式 -->
   <div class="qi-switch" 
   :class="{'is-checked': value}" 
   @click="handleClick">
   ///因为switch是表单组件 有可能会用到name这边使用复选框
     <input class="qi-switch_input"
     type="checkbox"
     :name='name'
     ref='input'/>
    <span class="qi-switch_core" ref="core">
      <span class="qi-switch_button"></span>
    </span>
   </div>
</template>

<script>
export default {
  name: 'QiSwitch',
  props: {
    // value值
    value: {
      type: Boolean,
      default: false
    },
    // 自定义颜色按下
    activeColor: {
      type: String,
      default: ''
    },
    // 自定义颜色松开
    inactiveColor: {
      type: String,
      default: ''
    },
    // name属性
    name: {
      type: String,
      default: ''
    }
  },
  // watch: {
  //   'value' (newValue) {
  //     // 修改开关颜色
  //     if (this.activeColor || this.inactiveColor) {
  //       const color = this.value ? this.activeColor : this.inactiveColor
  //       this.$refs.core.style.borderColor = color
  //       this.$refs.core.style.backgroundColor = color
  //     }
  //   }
  // },
  mounted () {
    this.setColor()
    this.$refs.input.checked = this.value
  },
  methods: {
    async handleClick () {
      // 通过父组件绑定v-model获取value属性和input
      // 发射事件传给父元素改变值
      this.$emit('input', !this.value)
      await this.$nextTick()
      this.$refs.input.checked = this.value
      this.setColor()
    },
    setColor () {
      if (this.activeColor || this.inactiveColor) {
        const color = this.value ? this.activeColor : this.inactiveColor
        this.$refs.core.style.borderColor = color
        this.$refs.core.style.backgroundColor = color
      }
    }
  }
}
</script>

<style lang='scss' scoped>
.qi-switch{
    display: inline-block;
    align-items: center;
    position: relative;
    font-size: 14px;
    line-height: 20px;
    vertical-align: middle;
  .qi-switch_core{
    margin: 0;
    display: inline-block;
    position: relative;
    width: 40px;
    height: 20px;
    border: 1px solid #dcdfe6;
    outline: none;
    border-radius: 10px;
    box-sizing: border-box;
    background: #dcdfe6;
    cursor: pointer;
    transition: border-color .3s,background-color .3s;
    vertical-align: middle;
    .qi-switch_button{
      position:absolute;
      top: 1px;
      left: 1px;
      border-radius: 100%;
      transition: all .3s;
      width: 16px;
      height: 16px;
      background-color: #fff;
      }
    }
  }
  // 滑动样式
  .is-checked {
    .qi-switch_core{
      border-color: #409eff;
      background-color: #409eff;
      .qi-switch_button {
        transform: translateX(20px);
      }
    }
  }
  // 隐藏input
  .qi-switch_input{
    position:absolute;
    width: 0;
    height: 0;
    opacity: 0;
    margin: 0;
  }
</style>

```

