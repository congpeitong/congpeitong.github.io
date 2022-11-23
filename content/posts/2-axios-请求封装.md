---
title: "2-axios-请求封装"
date: 2022-11-22T12:11:30+08:00
draft: false
tags: ["axios"]
categories: ["Vue"]
---
# 请求封装

请求封装，主要分为两个部分：

1. axios 配置的封装处理；
2. 请求 API 的封装处理；

## 一、axios 配置的封装处理

通常，我们会在项目的 `src` 目录中，新建一个 `utils` 目录，然后在 `utils` 中新建一个 `request.js` 文件，用来作为 axios 的配置文件。

### 1、配置公共路径

所有的请求路径中，都会有相同的一部分，指向服务器的地址。通常，可以通过配置 axios 的公共路径，将服务器地址提取出来。

```js
import axios from "axios";
axios.defaults.baseURL = "http://47.98.128.191:3000";
```

后续，我们发送的所有 axios 请求的 URL，就都不需要再写基础路径了。

### 2、处理返回的数据格式

axios 得到的数据是一个响应对象，而后端真正返回给前端的数据，在响应对象的 `data` 属性中。因此，我们可以对 axios 返回的数据格式进行筛选处理：

```js
axios.interceptors.response.use(res => res.data);
```

说明：这里给 axios 设置了一个响应拦截器。当请求的结果在返回给前端的过程中，会先被“响应拦截器”拦截下来，然后响应拦截器中对数据进行处理，最后将 `res.data` 返回给前端。

### 3、运行配置文件

配置完成后，我们需要在 `main.js` 中引入该文件，让文件的配置运行生效：

```js
import './utils/request.js'
```

## 二、请求 API 的封装处理

因为在实际的项目开发中，会涉及到很多的网络请求，每一个网络请求的代码都有可能分布在各个组件中。

网络请求的代码太散，会导致我们写很多重复的代码，同时后期更新维护也很不方便。因此，**我们需要将项目中所有的网络请求集中到一起来进行管理。**

### 1、创建目录文件

我们在 `src` 中创建一个 `api` 目录，用来管理项目中所有的请求 API。

```bash
src
 |--- api
 |     |--- modules
 |     |       |--- students.js   # 设置所有关于学生的请求
 |     |       |--- classes.js    # 设置所有关于班级的请求
 |     |       |--- ...		      # 设置所有关于 xx 的请求
 |     |--- index.js              # 汇总 modules 中所有模块的请求
```

### 2、封装请求模块 API

我们以学生请求为例：

```js
import axios from 'axios';

const students = {
    get(params) {
        return axios({
            url: '/students/getStudents',
            method: 'GET',
            params: params
        })
    },
    delete() {

    },
    add() {

    },
    update() {

    }
}

export default students;
```

### 3、汇总请求模块 API

```js
import students from './modules/students.js'
import classes from './modules/classes.js'

const api = {
    students: students,
    classes: classes
    // ... 其他模块
}

export default api;
```

### 4、全局挂载 API

为了在组件中方便使用 api 对象，我们在 Vue 中，会将 api 对象通过 `prototype` 挂载到全局。

在 `main.js` 中配置以下代码，来实现 api 的全局挂载：

```js
import api from '@/api';
Vue.prototype.$api = api;
```

## 三、组件中调用 API

```js
export default {
    data() {
        return {
            tableData: [],
        };
    },
    created() {
        this.getStudents();
    },
    methods: {
        async getStudents() {
            const res = await this.$api.students.get();
            if (res.code) {
                this.tableData = res.data.rows;
            }
        },
    },
};
```
