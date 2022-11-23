---
title: "2-Vuex--state-getters-mutations"
date: 2022-11-22T12:21:30+08:00
draft: false
tags: ["Vuex"]
categories: ["Vue"]
---
# state、getters 和 mutations 的使用

## 一、state 的使用

### 1、设置 state

state 对象中用来保存公共数据。

```js
export default new Vuex.Store({
    state: {
        userInfo: { username: '张三' }
    }
})
```

### 2、获取 state

在所有的组件中，都可以直接通过 `this.$store` 获取到仓库对象。仓库对象身上有一个 state 属性，用来获取仓库中 state 中所有的数据。

```vue
<p>欢迎你，{{$store.state.userInfo.username}}</p>
```

或者将仓库数据交给组件自己的计算属性：

```html
<p>欢迎你，{{username}}</p>

<script>
export default {
    computed: {
        username() {
            return this.$store.state.userInfo.username;
        }
    }
}
</script>
```

## 二、mutations

### 1、设置 mutations

mutations 用来设置修改 state 数据的方法：

```js
export default new Vuex.Store({
    state: {
        userInfo: { username: '张三' }
    },
    mutations: {
        SET_USER_INFO(state, payload) {
            state.userInfo = payload;
        }
    }
})
```

mutations 的参数说明：

- `state`：mutations 中的每一个方法，默认的第一个参数都是 state，用来接收仓库中的 state 对象；
- `payload`：payload 用来接收外部调用当前方法时传递的参数，可选；

### 2、调用 mutations

组件中，要调用 mutations 的方法，需要通过 `commit()` 的方法：

```js
this.$store.commit('SET_USER_INFO', { username: '李四' });
```

如果是在组件以外的其他文件中，要调用 mutations 的方法，需要先引入 store：

```js
import store from '@/store';

store.commit('SET_USER_INFO', { username: '李四' });
```

## 三、getters 的使用

在仓库中，可以通过 getters 来对 state 中的数据进行计算，从而得到一条新的数据。

### 1、设置 getters

仓库中通过 getters 处理 state：

```js
export default new Vuex.Store({
    state: {
        userInfo: { username: '张三' }
    },
    getters: {
        username(state) {
            return state.userInfo.username;
        }
        // username: state => state.userInfo.username 简写
    }
})
```

getters 中所有的属性都是方法，每一个 getters 的方法，默认的第一个参数都是 `state`，用来接收仓库中的 state 对象。

注意：getters 的方法中不能修改state。

### 2、获取 getters

```html
<p>欢迎你，{{$store.getters.username}}</p>
```

```html
<p>欢迎你，{{username}}</p>

<script>
export default {
    computed: {
        username() {
            return this.$store.getters.username;
        }
    }
}
</script>
```





