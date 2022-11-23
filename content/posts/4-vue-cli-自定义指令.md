---
title: "4-vue-cli-自定义指令"
date: 2022-11-22T12:44:30+08:00
draft: false
tags: ["vue-cli"]
categories: ["Vue"]
---
# 自定义指令

Vue 中提供了许多以  `v-` 开头的特殊属性用来标签身上，这些特殊属性都称为“指令”。

自定义指令，指的就是我们可以自己定义一个以 `v-` 开头的指令用来标签身上，而指令具体的功能，也由我们自己来定义。

## 一、基础语法

自定义指令分为全局指令和局部指令。

### 1、全局指令

全局指令可以作用于应用中的所有组件。

```js
import Vue from 'vue';

Vue.directive('指令名', {
    // 指令内容
});
```

### 2、局部指令

局部指令只可以作用于当前组件。

```js
export default {
    directives: {
        指令名: {
            // 指令内容
        }
    }
}
```

## 二、钩子函数

每一个自定义指令的对象中，都提供了以下几个钩子函数：

1. `bind`：只会执行一次，当指令第一次绑定到元素身上时执行，我们可以在这里面做一些一次性的初始化设置；
2. `inserted`：当指令所在元素被添加到父节点中时执行；
3. `update`：组件更新时执行；
4. `unbind`：指调用一次，当指令与元素解绑时执行；

语法结构：

```js
Vue.directive('指令名', {
    inserted(el, binding) {
        
    }
});
```

## 三、示例

例如我们要根据不同权限用户来控制按钮的权限。

我们可以在 src 目录中创建一个 `directives/index.js` 文件，设置以下代码：

```js
import store from "@/store";
import Vue from "vue";

Vue.directive('auth', {
    inserted(el, binding) {
        // 如果当前登录用户不是最高权限
        if (store.state.userInfo.auth != 2) {
            // el.remove();
            el.disabled = true;   // 有禁用功能
            el.classList.add('is-disabled');  // 有禁用样式
        }
    }
});
```

然后在 `main.js` 中引入该文件：

```js
import './directives'
```

最后，在需要控制权限的按钮身上使用该指令即可：

```html
<el-button v-auth size="mini" type="danger" @click="handleDelete(scope.row)">
    删除
</el-button>
```



