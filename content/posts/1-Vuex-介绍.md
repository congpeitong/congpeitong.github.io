---
title: "1-Vuex-介绍"
date: 2022-11-22T12:20:30+08:00
draft: false
tags: ["Vuex"]
categories: ["Vue"]
---
# 关于 Vuex

## 一、概念

Vuex，是 Vue 官方提供的“状态管理模式”，我们将其称为“状态机”。

状态，实际上指的就是 Vue 应用中的数据。大部分的状态（数据）我们都是在组件自己的 data 中进行管理，但是，部分公共状态（数据），我们可以交给状态机来进行管理。

## 二、工作原理

Vuex 的工作原理，实际上就是在 Vue 项目中，引入了一个公共仓库。我们可以将项目中的公共数据，都保存在 Vuex 的仓库中。这样的话，当组件中想要使用公共数据时，就可以直接从仓库中获取。同样的道理，如果我们想要修改仓库中的数据，仓库也会提供修改方法给我们调用即可。

## 三、五大核心属性

```js
export default new Vuex.Store({
    state: {
    },
    getters: {
    },
    mutations: {
    },
    actions: {
    },
    modules: {
    }
})
```

- `state`：状态，用来保存公共数据；
- `getters`：依赖 state 计算出的新数据，可以理解为是仓库中的计算属性；
- `mutations`：修改 state 的方法，唯一修改 state 的途径；
- `actions`：异步操作的方法；
- `modules`：仓库的模块化；

