---
title:  ui库input文本框
date: 2022/06/27 11:48:12
tags: [ui库]
categories: ui库
cover: static\img\photo-1571361656693-d7602246ce3c.jpg
---
## 自定义封装input文本框

参数

| 参数名称      | 参数描述                  | 参数类型 | 默认值 |
| ------------- | ------------------------- | -------- | ------ |
| placeholder   | 占位符                    | string   | 无     |
| type          | 文本框类型(text/password) | string   | text   |
| disabled      | 禁用                      | boolean  | false  |
| clearable     | 是否显示清空按钮          | boolean  | false  |
| show-password | 是否显示密码切换          | boolean  | false  |
| name          | name属性                  | string   | 无     |

事件

| 事件名称 | 事件描述     |
| -------- | ------------ |
| blur     | 失去焦点事件 |
| change   | 内容改变事件 |
| focus    | 获取焦点事件 |

```
<template>
  <div class="qi-input"
  :class="{'qi-input_suffix': showSuffix}">
  <!-- 判断showPassword是否为真 -->
  <!-- 为真通过passWordVisible去判断type类型来控制隐藏 -->
    <input
    :type="showPassword ? (passWordVisible ? 'text' : 'password') : type"
    class="qi-input_inner"
    :class="[{'is-disabled': disabled}]"
    :placeholder='placeholder'
    :name="name"
    :disabled="disabled"
    :value="value"
    @input="handlerInput"
     />
      <span class="qi-input_suffix" v-if="showSuffix">
        <!--触发input回调当点击时清空targrt值当没值是不显示文本  -->
   <i class="qi-input_icon qi-icon-cancel" 
   v-if="clearable && value"
   @click="clear"></i>
   <i class="qi-input_icon qi-icon-visible"
   v-if="showPassword"
   :class="{'qi-icon-view-active': passWordVisible}"
   @click="handleShow"></i>
 </span>
  </div>
</template>

<script>
export default {
  name: 'QiInput',
  data () {
    return {
      passWordVisible: false
    }
  },
  props: {
    // 输入框提示
    placeholder: {
      type: String,
      default: ''
    },
    // 输入框类型
    type: {
      type: String,
      default: 'text'
    },
    // 输入框name
    name: {
      type: String,
      default: ''
    },
    // 是否禁用
    disabled: {
      type: Boolean,
      default: false
    },
    // 输入框值
    value: {
      type: String,
      default: ''
    },
    // 清空
    clearable: {
      type: Boolean,
      default: false
    },
    // 显示密码
    showPassword: {
      type: Boolean,
      default: false
    }
  },
  computed: {
    showSuffix () {
      return this.clearable || this.showPassword
    }
  },
  methods: {
    handlerInput ({ target }) {
         /*
     当给一个innput标签进行双向绑定要给父元素传值时
     需要获得value值 在使用input事件监听标签内数据
     在组件内使用v-model显然时不合适的
     我们可以通过v-mode特性进行操作
     由于v-model绑定的是value值
     且自动绑定了input事件
     可以通过发射input事件传输入当前target值进行双向绑定
    */
      this.$emit('input', target.value)
    },
    clear () {
      this.$emit('input', '')
    },
    handleShow () {
      this.passWordVisible = !this.passWordVisible
    }
  }
}
</script>
<style lang='scss' scoped>
.qi-input{
    width: 100%;
    position: relative;
    font-size: 14px;
    display: inline-block;
    .qi-input_inner{
      -webkit-appearance: none;
      background-color: #fff;
      background-image: none;
      border: 1px solid #dcdfe6;
      border-radius: 4px;
      box-sizing: border-box;
      color: #606266;
      display: inline-block;
      font-size: inherit;
      height: 40px;
      line-height: 40px;
      outline: none;
      padding: 0 15px;
      transition: border-color .2s cubic-bezier(.645,045,.355,1);
      width: 100%;
      &:focus{
        outline: none;
        border-color: #409eff;
      }
      // input禁用样式
      &.is-disabled{
        background-color: #f5f7fa;
        border-color: #e4e7ed;
        color: #c0c4cc;
        cursor:not-allowed;
      }
    }
  }
  .qi-input_suffix{
    .qi-input_inner{
      padding-right: 30px;
    }
    .qi-input_suffix{
      position: absolute;
      right: 10px;
      height: 100%;
      top: 0;
      line-height: 40px;
      text-align: center;
      color: #c0c4cc;
      transition: all .3s;
      z-index: 900;
      i{
        color: #c0c4cc;
        font-size: 14px;
        cursor: pointer;
        transition: color .2s cubic-bezier(.645,.045,.355,1);
      }
      .qi-icon-view-active {
        color: #409eff;
      }
    }
  }
</style>

```

