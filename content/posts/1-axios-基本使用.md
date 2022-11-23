---
title: "1-axios-基本使用"
date: 2022-11-22T12:10:30+08:00
draft: false
tags: ["axios"]
categories: ["Vue"]
---
# axios 基本使用

## 一、下载

在任何项目中，如果要使用 axios 来发送网络请求，都需要先安装下载：

```bash
npm i axios
```

## 二、使用

### 1、引入

```js
import axios from 'axios';
```

### 2、发送请求

```js
axios({
    url: '',  // 请求路径
    method: 'POST', // 请求类型，例如 GET、POST、DELETE、PUT...
    data: {
        参数名: 参数值
    }
})

axios({
    url: '',  // 请求路径
    method: 'GET', // 请求类型，例如 GET、POST
    params: {
        参数名: 参数值
    }
})
```

注意：除了 GET 请求在传参需要使用 `params` 属性外，其他类型的请求，传参都是通过 `data` 属性。

### 3、接收请求结果

axios 返回的结果是一个 Promise 对象，因此，我们可以选择用 `then()` 方法来接收后端返回的结果，也可以选择使用 `async await` 来接收后端返回的结果。

```js
axios({
    // ...
}).then(res => {
    console.log('后端返回的结果', res.data);
})
```

```js
async 方法名() {
    const res = await axios({
        // ...
    });
    console.log('后端返回的结果', res.data)
}
```

