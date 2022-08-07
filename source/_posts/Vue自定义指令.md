---
title: Vue自定义指令
date: 2022/06/12 21:21:42
tags: [vue, ES6, JavaScript]
categories: 技术
cover: static\img\photo-1516166328576-82e16a127024.jpg
---
### Vue自定义指令

Vue自定义指令有全局注册和局部注册两种方式

全局注册通过Vue.directive(id,[definition])

局部注册通过directives注册指令

```
// 全局注册
// src/main.js
import Vue from "vue";
import App from "./App.vue";

Vue.config.productionTip = false;
Vue.directive("resize", {
  bind() {},
  inserted() {},
  update() {},
  componentUpdated() {},
  unbind() {},
});
new Vue({
  render: (h) => h(App),
}).$mount("#app");
//局部注册
<script>
export default {
  name: "App",
  components: {},
  directives: {
    resize: {
      bind() {},
      inserted() {},
      update() {},
      componentUpdated() {},
      unbind() {},
    },
  },
};
</script>
```

##### 钩子函数

**bind**:只调用一次，指令第一次绑定到元素时调用。在这里可以进行一次性的初始化设置

**inserted**:被绑定元素插入父节点时调用 (仅保证父节点存在，但不一定已被插入文档中)。

**update**:所在组件的 VNode 更新时调用，但是可能发生在其子 VNode 更新之前。指令的值可能发生了改变，也可能没有。但是你可以通过比较更新前后的值来忽略不必要的模板更新

**componentUpdated**:指令所在组件的 VNode及其子 VNode全部更新后调用。

**unbind**:只调用一次，指令与元素解绑时调用。

简单理解钩子函数顺序：指令绑定到元素时（bind）、元素插入时（inserted）、组件更新时（update）、组件更新后（componentUpdated）、指令与元素解绑时（unbind）

##### 钩子函数参数

**el**:指令所绑定的元素，可以用来直接操作 DOM。

**binding**:一个对象，包含以下属性

- `name`：指令名，不包括 `v-` 前缀。
- `value`：指令的绑定值，例如：`v-my-directive="1 + 1"` 中，绑定值为 `2`。
- `oldValue`：指令绑定的前一个值，仅在 `update` 和 `componentUpdated` 钩子中可用。无论值是否改变都可用。
- `expression`：字符串形式的指令表达式。例如 `v-my-directive="1 + 1"` 中，表达式为 `"1 + 1"`。
- `arg`：传给指令的参数，可选。例如 `v-my-directive:foo` 中，参数为 `"foo"`。
- modifiers`：一个包含修饰符的对象。例如：`v-my-directive.foo.bar` 中，修饰符对象为 `{ foo: true, bar: true }

**vnode**:Vue 编译生成的虚拟节点。

**oldVnode**:上一个虚拟节点，仅在`update`和`componentUpdated`钩子中可用。