---
title: "3-Vue-Router-路由跳转"
date: 2022-11-22T12:03:30+08:00
draft: false
tags: ["Vue-Router"]
categories: ["Vue"]
---
# 路由跳转

Vue Router 中提供了两种路由跳转的方式：

1. 标签（组件）跳转
2. API 跳转

##  一、标签（组件）跳转

Vue Router 中提供了一个 `<router-link>` 来实现路由的跳转：

```html
<router-link to="/register">没有账号？去注册</router-link>
```

 其中，`to` 属性用来设置跳转的路径。

### to

`to` 属性的值，除了可以给一个路径字符串以外，还可以设置成对象：

```html
<router-link :to="{ path: '/register' }">没有账号？去注册</router-link>
<router-link :to="{ name: 'Register' }">没有账号？去注册</router-link>
```

## 二、API 跳转

```js
export default {
    methods: {
        register() {
            alert("注册成功");
            
            this.$router.push("/login");
            
            this.$router.push({
                path: "/login",
            });
            
            this.$router.push({
                name: "Login",
            });
        },
    },
};
```

## 三、其他跳转方式

### 1、replace 跳转

replace 跳转，指的就是跳转后的新路由会替换掉旧路由的访问记录，意味着用户能再返回上一个页面。

```html
<router-link to="/home" replace>首页</router-link>
<router-link to="/home" :replace="true">首页</router-link>
```

```js
export default {
    methods: {
        toHome() {
            this.$router.replace('/home');
        }
    }
};
```

### 2、其他跳转

```js
this.$router.go(1);     // 前进一个页面
this.$router.go(-1);    // 回退一个页面
this.$router.go(0);     // 刷新当前页面
this.$router.forward(); // 前进一个页面
this.$router.back();    // 回退一个页面
```

