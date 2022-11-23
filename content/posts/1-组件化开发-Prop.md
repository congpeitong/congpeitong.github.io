---
title: "1-Vue组件化开发-Prop"
date: 2022-11-22T11:01:30+08:00
draft: false
tags: ["Vue组件化开发"]
categories: ["Vue"]
---
# Prop

Vue 中提供了 Prop 的属性，来实现父组件向子组件传值。

## 一、父组件传递数据

父组件在向子组件传递数据时，基础语法如下：

```html
<子组件名 属性名="传递的数据值"></子组件名>
```

其中，属性名可以自己任意定义。

### 传递的数据类型

在传递数据时，除了普通的静态字符串外，其他任意类型的数据，在传递时都需要添加 `v-bind`：

```html
<Child str="hello" v-bind:num="20" v-bind:arr="[1, 2, 3]"></Child>
```

## 二、子组件接收数组

子组件中，都通过 `props` 属性来接收外部传递的数据。

基础语法如下：

```js
export default {
    props: ['属性名一', '属性名二', ...]
}
```

子组件中通过 `props` 接收的数据，都可以直接使用：

```html
<template>
	<p>{{num}}</p>
</template>
<script>
export default {
    props: ['num'],
    computed: {
        sum() {
            return this.num + this.num;
        }
    }
}
</script>
```

## 三、单向数据流

### 概念

当父组件将数据传递给子组件后，父组件如果更新数据，子组件会同步更新，但是，**子组件中不能修改 props 接收到的数据**。

如果修改 props 的值，会出现类似以下的报错：

![image-20220726110221300](https://woniumd.oss-cn-hangzhou.aliyuncs.com/web/jianglan/20220726110221.png)

### 解决方案

如果确实有需要修改 props 的需求，可以有如下两种解决方案：

#### 1、将 props 赋值给 data

```js
export default {
    props: ['num'],
    data() {
        return {
            count: this.num
        }
    }
}
```

但是注意，data 中只能接收到 props 的初始值。后续 props 的值发生改变，data 不会再同步更新。

#### 2、将 props 赋值给 computed

```js
export default {
    props: ['num'],
    computed: {
        count() {
            return this.num;
        }
    }
}
```

## 四、Props 验证

```js
export default {
    props: {
        msg: String,
        num: [String, Number],
        name: {
            type: String,
            required: true,  // 父组件必须传
        },
        gender: {
            type: String,
            default: "男",  // 默认值
        },
        friends: {
            type: Array,
            default: () => ["张三"],  // 数组的默认值
        },
        student: {
            type: Object,
            default: () => ({ name: "张三" }),   // 对象的默认值
        },
    },
};
```



