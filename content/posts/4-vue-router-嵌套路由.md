---
title: "4-Vue-Router-嵌套路由"
date: 2022-11-22T12:04:30+08:00
draft: false
tags: ["Vue-Router"]
categories: ["Vue"]
---
# 嵌套路由

嵌套路由，指的是在原有路由的基础上，继续添加下一级路由。

## 一、配置嵌套路由

```js
const routes = [
    {
        path: '/home',
        name: 'Home', 
        component: HomeView,
        // 嵌套路由
        children: [
            {
                path: 'studentsList',  // 子路由的 path 不要以“/”开头
                name: 'StudentsList',
                component: StudentsList
            },
            {
                path: 'studentsAdd',
                name: 'StudentsAdd',
                component: StudentsAdd
            },
        ]
    },
]
```

配置完成后，我们就可以在浏览器中通过 `/home/studentsList` 来进行访问。

## 二、配置子路由出口

子路由配置成功后，还需要在父级路由的组件中，来设置子路由组件的出口。

```html
<el-main>
    <router-view></router-view>
</el-main>
```

## 三、嵌套路由的跳转

我们在前面学习的路由跳转方式，都可以用来做嵌套路由的跳转。需要注意的是，**在跳转嵌套路由时，必须设置完整的跳转路径。**

例如，我们想要跳转到首页中的学生列表：

```html
<router-link to="/home/studentsList">学生列表</router-link>
```

```js
this.$router.push('/home/studentsList')
```

