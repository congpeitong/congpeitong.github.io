---
title: "1-Vue-Router-路由与SPA"
date: 2022-11-22T12:01:30+08:00
draft: false
tags: ["Vue-Router"]
categories: ["Vue"]
---
# 路由与 SPA

## 一、SPA

SPA 的全称是“Single Page Application”，翻译成“单页应用”。单页应用，指的就是应用程序中只有一个 `.html` 页面。

### 实现原理

单页应用的原理，实际上就是通过切换页面中的组件，来达到类似多页应用的效果。

## 二、路由

路由的作用，就是用来管理**浏览器路径**和**页面组件**之间的关系。

每当浏览器的路径发生变化时，路由就可以检测到，同时会更新页面，显示当前路径对应的组件。

例如，我们有登录、注册、首页三个页面。

那么路由中管理的对应关系：

```bash
/login     --->  Login.vue
/register  --->  Register.vue
/home      --->  Home.vue
```





