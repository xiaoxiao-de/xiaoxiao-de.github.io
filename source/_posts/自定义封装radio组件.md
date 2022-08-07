---
title:  ui库radio与radio-group组件
date: 2022/06/27 11:48:12
tags: [ui库]
categories: ui库
cover: static\img\photo-1571361656693-d7602246ce3c.jpg
---
## 自定义封装radio与radio-group组件

参数

| 参数名称 | 参数描述      | 参数类型                | 默认值 |
| -------- | ------------- | ----------------------- | ------ |
| v-model  | 双向绑定      | boolean                 | fasle  |
| label    | 单选框和value | string，number，Boolean | 无     |
| name     | name          | string                  | 无     |

```
<template>
<!-- 点击是样式为选中状态 -->
<!-- label等于model时为选中状态 -->
  <label class="qi-radio"
  :class="{'is-checked': label === model}">
    <span class="qi-radio_input">
      <span class="qi-radio_inner"></span>
      <input
      type="radio"
      class="qi-radio_original"
      :value="label"
      :name="name"
      v-model="model"
      >
    </span>
    <span class="qi-radio_label">
      <slot></slot>
      <template v-if="!$slots.default">
        {{ label }}
      </template>
    </span>
  </label>
</template>

<script>
export default {
  name: 'QiRadio',
  // 通过group传来的参数在计算属性中进行计算
  inject: {
    RadioGroup: {
      default: ''
    }
  },
  props: {
    label: {
      type: [String, Number, Boolean],
      default: ''
    },
    value: null,
    name: {
      type: String,
      default: ''
    }
  },
  computed: {
    model: {
      /*
      实现radio组件的双向绑定外还需要控制radio的样式
      实现radio组件数据绑定需要父组件的label和value
      value通过父组件的v-model获得
      当value点击时应将value的值绑定为label属性
      通过计算属性方式将值绑定在input标签中
      当值发生变动时传给父组件
      在得到父组件传来的值
       */
      // 判断是否是RadioGroup组件的value
      get () {
        return this.isGroup ? this.RadioGroup.value : this.value
      },
      set (value) {
        this.isGroup ? this.RadioGroup.$emit('input', value) : this.$emit('input', value)
      }
    },
    isGroup () {
      return !!this.RadioGroup
    }
  }
}
</script>
<style lang="scss" scoped>
  .qi-radio{
    color: #606266;
    font-weight: 500;
    line-height: 1;
    position: relative;
    cursor: pointer;
    display: inline-block;
    white-space: nowrap;
    outline: none;
    font-size: 14px;
    margin-right: 30px;
    -moz-user-select: none;
    -webkit-user-select: none;
    -moz-user-select: none;
    .qi-radio_input{
      white-space: nowrap;
      cursor: pointer;
      outline: none;
      display: inline-block;
      line-height: 1;
      position: relative;
      vertical-align: middle;
      .qi-radio_inner{
        border: 1px solid #dcdfe6;
        border-radius: 100%;
        width: 14px;
        height: 14px;
        background-color: #fff;
        position: relative;
        cursor: pointer;
        display: inline-block;
        box-sizing: border-box;
        &:after{
          width: 4px;
          height: 4px;
          border-radius: 100%;
          background-color: #fff;
          content: "";
          position: absolute;
          left: 50%;
          top: 50%;
          transform: translate(-50%,-50%) scale(0);
          transition: transform .15s ease-in;
        }
      }
      .qi-radio_original{
        opacity: 0;
        outline: none;
        position: absolute;
        z-index: -1;
        top: 0;
        left: 0px;
        right: 0;
        bottom: 0;
        margin: 0;
      }
    }
    .qi-radio_label{
      font-size: 14px;
      padding-left: 10px;;
    }
  }
  // 选中的样式
  .qi-radio.is-checked{
    .qi-radio_input{
      .qi-radio_inner{
        border-color: #409eff;
        background-color: #409eff;
        &:after{
          transform: translate(-50%,-50%) scale(1);
        }
      }
    }
    .qi-radio_label{
      color:#409eff;
    }
  }
</style>

```

```
<template>
  <div class="qi-radio-group">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'QiRadioGroup',
  /*
  radio-group组件目的是在使用radio时不用给每个组件添加v-model
  通过给它绑定一个v-model实现功能
  组件件通信需要使用provide和inject进行祖孙通信
  */
  provide () {
    return {
      RadioGroup: this
    }
  },
  props: {
    value: null
  }
}
</script>
<style>
</style>

```

