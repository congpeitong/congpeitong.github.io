---
title: "9-Vue-Router-默认子路由"
date: 2022-11-22T12:09:30+08:00
draft: false
tags: ["Vue-Router"]
categories: ["Vue"]
---
# 默认子路由

通常一个父级路由下都会有很多的子路由，每一个子路由都会有自己的 `path`，例如：

```js
const routes = [
    {
        path: '/home',
        name: 'Home',
        component: HomeView,
        children: [
            {
                path: 'studentsList',
                name: 'StudentsList',
                component: StudentsList
            },
            // ...
        ]
    }
]
```

## 一、基础语法

当我们希望，访问 `/home` 时，也能渲染一个子组件，我们就可以给 Home 设置一个默认子路由：

```js
const routes = [
    {
        path: '/home',
        component: HomeView,
        children: [
            {
                path: '',
                name: 'Home',
                component: HomeCharts,
            },
            {
                path: 'studentsList',
                name: 'StudentsList',
                component: StudentsList
            },
            // ...
        ]
    }
]
```

注意：默认子路由和父级路由其实就是同一个路由，所以父级路由的 name 属性，需要设置在默认子路由上。

## 二、动态设置默认子路由

如果我们的 Home 路由是通过 `addRoute` 动态添加的，那么默认子路由也可以动态生成：

```js
import HomeCharts from '@/views/charts/HomeCharts.vue'
router.addRoute({
    path: '/home',
    component: HomeView,
    children: [
        // 默认子路由
        {
            path: '',
            name: 'Home',
            component: HomeCharts,
            meta: {
                title: ['图表统计']
            }
        },
        // 其他权限子路由
        ...fullRoutes.filter(item => {
            return store.state.authMenus.includes(item.path.split('/')[0]);
        })
    ]
})
```

