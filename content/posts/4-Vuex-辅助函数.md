---
title: "4-Vuex-辅助函数"
date: 2022-11-22T12:23:30+08:00
draft: false
tags: ["Vuex"]
categories: ["Vue"]
---
# 辅助函数

如果用传统的方式去获取 Vuex 中的数据或方法，每次都需要通过 `this.$store` 去进行操作，这样的操作方式比较繁琐。所以，Vuex 中还给我们提供了辅助函数的形式，来优化对 Vuex 的操作。

## 一、辅助函数的概念

Vuex 总共提供了四个辅助函数：

- `mapState()`：获取 state 数据
- `mapGetters()`：获取 getters 数据
- `mapMutations()`：获取 mutations 方法
- `mapActions()`：获取 actions 方法

mapState 和 mapGetters 这两个辅助函数的原理，就是将 state 和 getters 中数据，拿到组件自己的 computed 中来，变成组件自己的计算属性来使用。

mapMutations 和 mapActions 这两个辅助函数的原理，就是 mutations 和 actions 中的方法，拿到组件自己的 methods 中来，变成组件自己的方法来调用。

## 二、使用

### 1、引入

```js
import { mapState, mapGetters, mapMutations, mapActions } from 'vuex'
```

### 2、调用

```js
export default {
    computed: {
        ...mapState(['state中的数据名', 'state中的数据名']),
        // 例如：...mapState(['userInfo', 'subjects']),
        ...mapGetters(['getters中的数据名'])
        // 例如：...mapGetters(['username']),
    },
    methods: {
        ...mapMutations(['mutations中的方法名', 'mutations中的方法名']),
        // 例如：...mapMutations(['SET_SUBJECTS']),
        ...mapActions(['actions中的方法名'])
        // 例如：...mapActions(['getSubjectsAsync']),
    }
}
```

通过以上辅助函数的调用获取后，Vuex 中的数据和方法，都会变成组件自己的计算属性和方法。

