---
title: "5-Vuex-模块化"
date: 2022-11-22T12:24:30+08:00
draft: false
tags: ["Vuex"]
categories: ["Vue"]
---
# 模块化

我们可以通过 modules 这个属性，对仓库进行模块化拆分，可以将一个 store，拆分成多个模块。

## 一、仓库配置模块化

### 1、创建模块文件

```bash
store
  |--- modules
  |       |--- students.js
  |       |--- classes.js
  |       |--- subjects.js
  |       |--- ...
  |--- index.js     		  # 主仓库的配置文件
```

### 2、配置模块

```js
export default {
    // 解决命名空间冲突
    namespaced: true,
    state: {},
    getters: {},
    mutations: {},
    actions: {}
}
```

每一个模块都需要设置一个 `namespaced: true` 的属性，如果不设置这个属性，那么所有模块文件中的代码，最终还是会编译成主仓库的内容。

剩下的五大核心属性，和主仓库中的用法一模一样。

案例代码：

```js
export default {
    // 解决命名空间冲突
    namespaced: true,
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
}
```

### 3、主仓库引入模块

```js
import subjects from './modules/subjects'

export default new Vuex.Store({
    modules: {
        subjects
    }
})
```

## 二、组件中使用模块

组件中要使用模块中的数据或方法，也有两种方式：

1. 原生方式（不用辅助函数）
2. 辅助函数

### 1、原生方式（不用辅助函数）

在组件中用原生的方式获取**仓库模块**中的 state 和 getters 时：

```js
export default {
    computed: {
        计算属性名() {
            return this.$store.state.模块名.state数据名
        },
        计算属性名() {
            return this.$store.getters['模块名/getters数据名']
        },
    }
}
```

在组件中用原生的方式调用**仓库模块**中的 mutations 和 actions 方法：

```js
export default {
    methods: {
        方法名() {
            this.$store.commit('模块名/mutations方法名')
        },
        方法名() {
            this.$store.dispatch('模块名/actions方法名')
        }
    }
}
```

### 2、辅助函数

如果在组件中需要通过辅助函数的形式来获取模块中的数据和方法。

在引入辅助函数时，需要用模块化的方式来引入：

```js
import { createNamespacedHelpers } from "vuex";
const { mapState, mapGetters, mapMutations, mapActions } = createNamespacedHelpers("模块名");
```

拿到辅助后，后续的用法就和之前辅助函数的用法一样了。

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

### 3、组件中使用多个仓库模块

```js
import { createNamespacedHelpers } from "vuex";
const { mapState, mapGetters, mapMutations, mapActions } = createNamespacedHelpers("subejctsModule");
const { 
    mapState: classesState, 
    mapGetters: classesGetters, 
    mapMutations: classesMutations, 
    mapActions: classesActions
} = createNamespacedHelpers("classesModule");
```





