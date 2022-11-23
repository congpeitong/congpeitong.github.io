---
title: "3-Vuex-action"
date: 2022-11-22T12:22:30+08:00
draft: false
tags: ["Vuex"]
categories: ["Vue"]
---
# actions

## 一、创建 action 方法

```js
export default new Vuex.Store({
    actions: {
        方法名(context, payload) {
            // 发送异步请求
        }
    }
})
```

参数说明：

- `context`：当前的 store 对象；
- `payload`：接收外部传递的参数，可选；

## 二、调用 action 方法

在组件中，如果要调用 action 的方法，需要通过 `dispatch()` 方法：

```js
this.$store.dispatch('getSubjectsAsync');
```

## 三、处理 action 请求结果

action 的请求结果，通常有两种处理方法：

1. **请求结果多个组件需要使用**：将请求结果保存到 state 中；
2. **请求结果只有一个组件需要使用**：将请求结果 return 返回给组件；

### 1、请求结果保存到 state

action 中不能直接修改 state，所以，如果要**将 action 的请求结果保存到 state 中**，需要经过以下步骤：

1. action 调用 mutations 的方法，并将数据传递给 mutations；
2. mutations 中接收到数据后，用新数据去修改 state；

示例代码：

```js
export default new Vuex.Store({
    state: {
        subjects: [],  // 专业数据的初始值
    },
    mutations: {
        SET_SUBJECTS(state, payload) {
            state.subjects = payload.rows;
        }
    },
    actions: {
        async getSubjectsAsync(context) {
            const res = await api.subjects.get();
            if (res.code) {
                context.commit('SET_SUBJECTS', res.data);
            }
        }
    }
})
```



