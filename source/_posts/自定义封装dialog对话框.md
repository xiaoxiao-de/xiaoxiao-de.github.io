---
title:  ui库input文本框
date: 2022/06/27 11:48:12
tags: [ui库]
categories: ui库
cover: static\img\photo-1571361656693-d7602246ce3c.jpg
---
## 自定义封装input文本框

##### 前置知识

1. vue过渡动画
2. sync修饰符
3. 具名插槽与v-slot指令

参数

| 参数名  | 参数描述                        | 参数类型 | 默认值 |
| ------- | ------------------------------- | -------- | ------ |
| title   | 对话框标题                      | string   | 提示   |
| width   | 宽度                            | string   | 50%    |
| top     | 与顶部距离                      | string   | 200px  |
| visible | 是否显示dialog（支持sync修饰符) | boolean  | false  |

事件

| 事件名 | 事件描述       |
| ------ | -------------- |
| opened | 模态框显示事件 |
| closed | 模态框关闭事件 |

插槽

| 插槽名称 | 插槽描述         |
| -------- | ---------------- |
| default  | dialog的内容     |
| title    | dialog标题       |
| footer   | dialog底部操作区 |

```
<template>
<!-- 使用transition标签包裹之后就会自动添加动画类 -->
<!-- name指定动画名称 -->
<transition name="dialog">
  <!-- 通过v-show来进行显示隐藏 -->
  <!-- self代表点击自己才触发事件 -->
 <div class="qi-dialog_wrapper" v-show="visible" @click.self="handleClose">
   <div class="qi-dialog" :style="{ width: width, 'margin-top': top}">
     <div class="qi-dialog_header">
       <!-- 使用具名插槽方式 -->
       <slot name='title'>
         <span class="qi-dialog_title">
         {{ title }}
       </span>
       </slot>
       <button class="qi-dialog_headerbtn">
         <i class="qi-icon-close" @click.self="handleClose"></i>
       </button>
     </div>
     <div class="qi-dialog_body">
       <!-- 自定义body内容 -->
       <slot></slot>
     </div>
     <div class="qi-dialog_footer" v-if="$slots.footer">
       <slot name="footer">

       </slot>
     </div>
   </div>
 </div>
</transition>
</template>
<script>
export default {
  name: 'QiDialog',
  props: {
    // 标题
    title: {
      type: String,
      default: '提示'
    },
    // 宽度
    width: {
      type: String,
      default: '50%'
    },
    // 高度
    top: {
      type: String,
      default: '200px'
    },
    // 显示隐藏
    visible: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    handleClose () {
      // 通过update:visible方式可以使用sync修饰符来简化子传父
      // 回调给父元素进行更改
      // 作用于事件名和属性相同时
      this.$emit('update:visible', false)
    }
  }
}
</script>
<style lang='scss' scoped>
.qi-dialog_wrapper{
  position: fixed;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  overflow: auto;
  margin: 0;
  z-index: 2001;
  background-color: rgba(0,0,0,0.5);
  .qi-dialog{
    position: relative;
    margin: 15vh auto 50px;
    background: #fff;
    border-radius: 2px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.3);
    box-sizing: border-box;
    width: 30%;
    &_header{
      padding: 20px 20px 10px;
      .qi-dialog_title{
        line-height: 24px;
        font-size: 18px;
        color: #303133;
      }
      .qi-dialog_headerbtn{
        position: absolute;
        top: 20px;
        right: 20px;
        padding: 0;
        background: transparent;
        border: none;
        outline: none;
        cursor: pointer;
        font-size: 16px;
        .qi-icon-close{
          color:909399
        }
      }
    }
    &_body{
      padding: 30px 20px;
      color: #606266;
      font-size: 14px;
      word-break: break-all;
    }
    &_footer{
      padding: 10px 20px 20px;
      text-align: right;
      box-sizing: border-box;
      ::v-deep .qi-button:first-child{
        margin-right: 20px;
      }
    }
  }
}
// 进入页面
.dialog-enter-active {
  animation: run .9s;
}
// 离开页面
.dialog-leave-actuve {
  animation: run .9s reverse;
}
// 动画
@keyframes run {
  0% {
    opacity: 0;
    transform: translateY(-30px);
  }
  100% {
    opacity: 1;
    transform: translateY(0px);
  }
}
</style>

```

