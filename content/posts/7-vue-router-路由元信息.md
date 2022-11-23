---
title: "7-Vue-Router-路由元信息"
date: 2022-11-22T12:07:30+08:00
draft: false
tags: ["Vue-Router"]
categories: ["Vue"]
---
# 路由元信息

## 一、基础语法

### 1、设置路由元信息

在配置路由时，每一个路由对象，除了必填的 `path` 和 `component` 属性外，Vue Router 还给它们提供了一个属性 `meta`，用来设置路由元信息。

```js
const routes = [
    {
        // ...,
        children: [
            {
                path: 'studentsList', 
                name: 'StudentsList',
                component: StudentsList,
                // 路由元信息
                meta: {

                }
            },
        ]
    }
]
```

除了路由提供了基本属性外，如果我们还想要给路由对象身上添加一个额外的信息，就可以在 `meta` 对象中设置。

### 2、获取路由元信息

在组件中，通过 `this.$route` 可以获取到当前页面的路由信息对象。在该对象身上，就有一个 `meta` 属性，可以获取到当前路由的元信息数据。

```js
export default {
    created() {
        console.log(this.$route.meta);
    }
}
```

## 二、应用场景

### 1、面包屑

我们可以通过路由元信息来实现面包屑组件的渲染。

#### 1）设置路由元信息

将面包屑需要用到的标题，都设置在路由元信息中：

```js
const routes = [
    // ...
    {
        //...
        children: [
            {
                path: 'studentsList',  
                name: 'StudentsList',
                component: StudentsList,
                meta: {
                    title: ['学生管理', '学生列表']
                }
            },
            {
                path: 'studentsAdd',
                name: 'StudentsAdd',
                component: StudentsAdd,
                meta: {
                    title: ['学生管理', '新增学生']
                }
            },
            {
                path: 'studentsUpdate/:id?',
                name: 'StudentsUpdate',
                component: StudentsUpdate,
                props: true,
                meta: {
                    title: ['学生管理', '修改学生']
                }
            },
        ]
    },
    // ...
]
```

#### 2）渲染路由元信息

```html
<template>
    <el-breadcrumb separator="/">
        <el-breadcrumb-item :to="{ path: '/home' }"> 首页 </el-breadcrumb-item>
        <el-breadcrumb-item v-for="(item, index) in title" :key="index">
            {{ item }}
        </el-breadcrumb-item>
    </el-breadcrumb>
</template>

<script>
export default {
    computed: {
        title() {
            return this.$route.meta.title;
        },
    },
};
</script>
```

### 2、区分组件缓存

#### 1）设置路由元信息

```js
const routes = [
    // ...
    {
        //...
        children: [
            {
                path: 'studentsList',  
                name: 'StudentsList',
                component: StudentsList
            },
            {
                path: 'studentsAdd',
                name: 'StudentsAdd',
                component: StudentsAdd,
                meta: {
                    isKeepAlive: true
                }
            },
            {
                path: 'studentsUpdate/:id?',
                name: 'StudentsUpdate',
                component: StudentsUpdate,
                props: true
            },
        ]
    },
    // ...
]
```

#### 2）按条件渲染组件

```html
<keep-alive>
    <router-view v-if="$route.meta.isKeepAlive"></router-view>
</keep-alive>
<router-view v-if="!$route.meta.isKeepAlive"></router-view>
```





