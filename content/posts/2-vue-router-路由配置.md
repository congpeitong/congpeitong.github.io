---
title: "2-Vue-Router-路由配置"
date: 2022-11-22T12:02:30+08:00
draft: false
tags: ["Vue-Router"]
categories: ["Vue"]
---
# 路由配置

## 一、基础配置

在项目的 `/src/router/index.js` 文件中，来进行路由配置：

```js
const routes = [
	{
		path: '/home',
		name: 'Home',  // 路由名称，可选
		component: HomeView
	},
	{
		path: '/login',
		name: 'Login',  // 路由名称，可选
		component: LoginView
	},
	{
		path: '/register',
		name: 'Register',  // 路由名称，可选
		component: RegisterView
	}
]
```

项目中的每一个页面，都在 `routes` 数组中对应着一个对象。每一个对象都有三个属性：

1. `path`：浏览器路径；
2. `name`：路由名称（可选）；
3. `component`：组件；

## 二、路由出口

当用户在浏览器中访问某一个路径时，路由会判断当前路径的 path，然后找到该路径所对应的组件。

但是，路由找到组件后，并不知道组件在什么位置进行渲染，因此，我们需要给路由设置一个路由出口。

Vue Router 中提供了一个 `<router-view>` 标签，用来作为路由出口。

例如，我们可以在 `App.vue` 中设置一个路由出口：

```html
<template>
    <div>
        <router-view></router-view>
    </div>
</template>
```

 `<router-view>` 标签所在的位置，就是当前路径对应组件渲染的位置。

## 三、路由懒加载

传统的路由组件的加载方式，是在页面刷新时，就先将所有的组件都提前加载在内存中。当路由路径匹配成功后，直接渲染内存中的组件。

例如：

```js
import HomeView from '../views/home/HomeView.vue'
import LoginView from '../views/login/LoginView.vue'
```

路由懒加载，指的是只有当用户访问到对应的路由后，才开始加载对应的组件。

```js
const RegisterView = () => import('../views/register/RegisterView.vue');
```

路由配置参考如下：

```js
const routes = [
    {
        path: '/home',
        name: 'Home',  // 路由名称，可选
        component: HomeView
    },
    {
        path: '/login',
        name: 'Login',  // 路由名称，可选
        component: LoginView
    },
    {
        path: '/register',
        name: 'Register',  // 路由名称，可选
        component: RegisterView
    }
]
```

## 四、路由重定向

路由重定向，指的就是可以将一个 `path` 直接重定向到另一个 `path`。

例如，将 `/` 路径重定向到 `/home`，就表示只要用户访问 `/` 时，会直接跳转到 `/home`。

```js
const routes = [
    // 重定向
    {
        path: '/',
        redirect: '/home'
    },
    // .. 其他路由配置
]
```

## 五、404 页面配置

```js
const NotFound = () => import('../views/not-found/NotFound.vue')

const routes = [
    // ...其他路由配置
    // 404 页面
    {
        path: '*',
        name: 'NotFound',
        component: NotFound
    }
]
```

## 六、路由模式

Vue Router 中，路由分为两种模式：

1. `history`：
2. `hash`：浏览器的路径中会出现 `#`

如果我们需要配置成 `history` 模式：

```js
const router = new VueRouter({
	mode: 'history',
	base: process.env.BASE_URL,
	routes
})
```

如果我们需要配置成 `hash` 模式：

```js
const router = new VueRouter({
	routes
})
```

