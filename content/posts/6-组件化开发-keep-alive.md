---
title: "6-Vue组件化开发-keep-alive"
date: 2022-11-22T11:06:30+08:00
draft: false
tags: ["Vue组件化开发"]
categories: ["Vue"]
---
# keep-alive

## 一、基础语法

Vue 中提供了一组标签 `<keep-alive>`，所有被 `<keep-alive>` 包裹的组件，都会被缓存，可以让组件在切换时不被销毁。

```html
<keep-alive>
	<!-- 其他组件 -->
</keep-alive>
```

示例代码：

```html
<keep-alive>
    <component :is="currentTab"></component>
</keep-alive>
```

## 二、生命周期函数

所有被 `<keep-alive>` 包裹的组件，除了首次显示时，会执行创建和挂载的生命周期函数，后续我们再在这些组件之间进行切换时，都不会再反复触发原本的生命周期函数了。

但是，所有被 `<keep-alive>` 包裹的组件会额外的新增两个生命周期函数：

```js
export default {
    activated() {
        console.log('进入组件时');
    },
    deactivated() {
        console.log('离开组件时');
    }
};
```

## 三、筛选组件

`<keep-alive>` 身上还提供了两个属性，用来对缓存的组件进行筛选：

- `include`：值是字符串或正则，用来设置需要缓存的组件名称；
- `exclude`：值是字符串或正则，设置不需要缓存的组件名称；

示例代码：

```html
<keep-alive exclude="TabA">
    <component :is="currentTab"></component>
</keep-alive>
```

```html
<keep-alive :include="/(TabA)|(TabB)/">
    <component :is="currentTab"></component>
</keep-alive>
```



