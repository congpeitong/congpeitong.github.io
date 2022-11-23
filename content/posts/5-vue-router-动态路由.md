---
title: "5-Vue-Router-动态路由"
date: 2022-11-22T12:05:30+08:00
draft: false
tags: ["Vue-Router"]
categories: ["Vue"]
---
# 动态路由

动态路由，指的就是路由的路径中，有一部分内容是动态发生变化的，这种路由我们就称为“动态路由”。

例如，我们要通过动态路由进入到“修改学生”的页面：

```bash
/home/studentsUpdate/001
/home/studentsUpdate/002
/home/studentsUpdate/003
```

其中，`001` 等数字就是当前路径中动态的部分。

## 一、配置动态路由

```js
const routes = [
    {

        path: '/home',
        name: 'Home',  // 路由名称，可选
        component: HomeView,
        children: [
            {
                path: 'studentsUpdate/:id',
                name: 'StudentsUpdate',
                component: StudentsUpdate
            }
        ]
    }
]
```

其中，`id` 可以是任意变量名。

## 二、跳转动态路由

默认情况下，动态路由在跳转时，必须在跳转路径中设置动态部分。

例如跳转到学生修改页面：

```js
export default {
    methods: {
        handleEdit(row) {
            this.$router.push(`/home/studentsUpdate/${row._id}`);
        }
    }
}
```

如果我们希望，在跳转时，动态部分可有可无，那么我们在配置动态路由时，可以添加一个 `?`：

```js
const routes = [
    {

        path: '/home',
        name: 'Home',  // 路由名称，可选
        component: HomeView,
        children: [
            {
                path: 'studentsUpdate/:id?',
                name: 'StudentsUpdate',
                component: StudentsUpdate
            }
        ]
    }
]
```

## 三、获取动态路由的参数

Vue Router 中提供了两种获取动态路由参数的方式。

### 1、通过 $route 获取

```js
export default {
    created() {
        console.log(this.$route.params)
    }
}
```

### 2、通过 props 获取

如果我们想要在组件中通过 props 来获取动态路由的参数，需要先在路由配置中添加以下属性：

```js
const routes = [
    {

        path: '/home',
        name: 'Home',  // 路由名称，可选
        component: HomeView,
        children: [
            {
                path: 'studentsUpdate/:id',
                name: 'StudentsUpdate',
                component: StudentsUpdate,
                props: true
            }
        ]
    }
]
```

配置成功后，就可以直接在组件的 props 中获取动态路由的参数了：

```js
export default {
    props: ["id"],
    created() {
        console.log(this.id);
    },
}
```





