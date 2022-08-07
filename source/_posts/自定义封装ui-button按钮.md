---
title: ui库ui-button按钮
date: 2022/06/27 11:48:12
tags: [ui库]
categories: ui库
cover: static\img\photo-1571361656693-d7602246ce3c.jpg
---
### 自定义封装ui-button按钮

知识点

1. 组件通信
2. 组件插槽
3. props校验

参数

| 参数名   | 参数描述                                     | 参数类型 | 默认值  |
| -------- | -------------------------------------------- | -------- | ------- |
| type     | 按钮类型(info,success,primary,waring,danger) | strign   | default |
| plain    | 是否朴素按钮                                 | boolean  | false   |
| round    | 是否圆角按钮                                 | boolean  | false   |
| circle   | 是否圆形按钮                                 | boolean  | false   |
| disabled | 是否禁用按钮                                 | boolean  | false   |
| icon     | 图标类型                                     | strign   | null    |

事件

| 事件名 | 事件描述 |
| ------ | -------- |
| click  | 点击事件 |

```
<template>
  <button class="qi-button"
  // 判断是哪个按钮
  :class="[`qi-button-${type}`,
  // 是否开启朴素按钮
   {'is-plain': plain},
   // 是否是圆角
   {'is-round': round},
   // 是否是圆形按钮
   {'is-circle': circle},
   // 是否禁用
   {'is-disabled': disabled}]"
   @click="handlerClick"
   :disabled='disabled'>
   <i v-if="icon" :class="[`qi-icon-${icon}`]"></i>
   <!-- 通过$slots.default进行判断默认插槽是否是文字按钮是否有图标 -->
    <span v-if="$slots.default">
      <slot></slot>
    </span>
    </button>
</template>
<script>
export default {
  name: 'QiButton',
  props: {
    // 按钮类型 如果不传值默认为default
    type: {
      type: String,
      default: 'default'
    },
    // 是否是朴素按钮
    plain: {
      type: Boolean,
      default: false
    },
    // 圆角按钮
    round: {
      type: Boolean,
      default: false
    },
    // 圆形按钮
    circle: {
      type: Boolean,
      default: false
    },
    // 图标
    icon: {
      type: String,
      default: ''
    },
    // 是否禁用
    disabled: {
      type: Boolean,
      default: false
    }
  },
  created () {
    console.log(this.$slots)
  },
  methods: {
    handlerClick (e) {
      this.$emit('click', e)
    }
  }
}
</script>
<style lang="scss">
// 按钮样式
 .qi-button{
    display: inline-block;
    line-height: 1;
    white-space: nowrap;
    cursor: pointer;
    background: #ffffff;
    border: 1px solid #dcdfe6;
    color: #606266;
    -webkit-appearance: none;
    text-align: center;
    box-sizing: border-box;
    outline: none;
    margin: 0;
    transition: 0.1s;
    font-weight: 500;
    //禁止元素的文字被选中
    -moz-user-select: none;
    -webkit-user-select: none;
    -moz-user-select: none;
    -ms-user-select: none;
    padding: 12px 20px;
    font-size: 14px;
    border-radius: 4px;
    &:hover,
    &:focus{
      color: #409eff;
      border-color: #c6e2ff;
      background-color: #ecf5ff;
    }
  }
  .qi-button-primary{
  color:#fff;
  background-color: #409eff;
  border-color: #409eff;
  &:hover,
  &:focus{
    background: #66b1ff;
    background-color: #66b1ff;
    color: #fff;
    }
  }
  .qi-button-success{
  color:#fff;
  background-color: #67c23a;
  border-color: #67c23a;
  &:hover,
  &:focus{
    background: #85ce61;
    background-color: #85ce61;
    color: #fff;
    }
  }
  .qi-button-info{
  color:#fff;
  background-color: #909399;
  border-color: #909399;
  &:hover,
  &:focus{
    background: #a6a9ad;
    background-color: #a6a9ad;
    color: #fff;
    }
  }
  .qi-button-warning{
  color:#fff;
  background-color: #e6a23c;
  border-color: #e6a23c;
  &:hover,
  &:focus{
    background: #ebb563;
    background-color: #ebb563;
    color: #fff;
    }
  }
  .qi-button-danger{
  color:#fff;
  background-color: #f56c6c;
  border-color: #f56c6c;
  &:hover,
  &:focus{
    background: #f78989;
    background-color: #f78989;
    color: #fff;
    }
  }
  // 朴素按钮样式
.qi-button.is-plain{
  &:hover,
  &:focus{
    background: #fff;
    border-color: #489eff;
    color: #409eff;
  }
}
.qi-button-primary.is-plain{
  color: #409eff;
  background: #ecf5ff;
  &:hover,
  &:focus{
    background: #409eff;
    border-color: #409eff;
    color: #fff;
  }
}
.qi-button-success.is-plain{
  color: #67c23a;
  background: #c2e7b0;
  &:hover,
  &:focus{
    background: #67c23a;
    border-color: #67c23a;
    color: #fff;
  }
}
.qi-button-info.is-plain{
  color: #909399;
  background: #d3d4d6;
  &:hover,
  &:focus{
    background: #909399;
    border-color: #909399;
    color: #fff;
  }
}
.qi-button-warning.is-plain{
  color: #e6a23c;
  background: #f5dab1;
  &:hover,
  &:focus{
    background: #e6a23c;
    border-color: #e6a23c;
    color: #fff;
  }
}
// round
.qi-button.is-round{
  border-radius: 20px;
  padding: 12px 23px;
}
// 圆形

.qi-button.is-circle{
  border-radius: 50%;
  padding: 12px;
}
// 去除图标相隔太近
.qi-button [class*=qi-icon-]+span{
  margin-left: 5px;
}
// 禁用
.qi-button.is-disabled {
  cursor: no-drop;
}
</style>

```

