---
title: "6-Vue-Router-query路由传参"
date: 2022-11-22T12:06:30+08:00
draft: false
tags: ["Vue-Router"]
categories: ["Vue"]
---
# query 路由传参

Vue Router 中除了用动态路由传参外，路由也提供了专门的传参方法。

## 一、传递参数

例如，我们要跳转“修改学生”的页面，并将学生 id 传递过去：

```html
<router-link to="/home/studentsUpdate?id=001">修改</router-link>
```

```js
this.$router.push('/home/studentsUpdate?id=001');
```

除了用 `?` 将路径和参数分割开以外，还可以有 query 属性：

```html
<router-link :to="{ path: '/home/studentsUpdate?id=001', query: { id: '001'}}">修改</router-link>
```

```js
this.$router.push({
    path: '/home/studentsUpdate',
    query: {
        id: '001',
        // 其他参数
    }
});
```

以上两种语法，最终运行的效果是一样的。

## 二、路由配置

**`query` 的传参方式，路由不需要做额外配置。**

注意：`query` 的传参方式，路由是一个普通路由，在路由配置时不需要设置成动态路由。

## 三、接收参数

在组件中，如果要接收通过 `query` 传递的参数，接收时通过以下方式：

```js
console.log(this.$route.query);
```

通常，我们需要用路由传递多个参数时，可以选择使用 query 的传参方式。



