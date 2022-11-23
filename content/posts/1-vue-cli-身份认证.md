---
title: "1-vue-cli-身份认证"
date: 2022-11-22T12:41:30+08:00
draft: false
tags: ["vue-cli"]
categories: ["Vue"]
---
# 身份认证

身份认证，就是在项目需要权限访问的页面中，对用户的**登录状态**进行判断。简单来说就是：

- 如果用户登录了，就可以访问对应的页面；
- 如果用户没有登录，我们就提示用户去登录；

## 一、用户登录

当用户登录成功后，后端会返回一个 token 给前端，我们需要将 token 保存在本地存储中：

```js
export default {
    methods: {
        async login() {
            // 发送登录请求
            const res = await this.$api.users.login(this.form);
            if (res.code) {
                // 将 token 保存到本地存储
                localStorage.token = res.token;
                this.$router.push("/home");
            } else {
                this.$message.error("账号或密码错误，登录失败。");
            }
        }
    }
}
```

## 二、获取用户信息

在系统首页的路由中，添加一个“前置守卫”，在守卫钩子函数中发送请求，通过 token 来获取用户信息：

```js
import api from '@/api';
const routes = [
    {
        path: '/home',
        name: 'Home', 
        component: HomeView,
        // 路由前置守卫
        beforeEnter: async (to, from, next) => {
            const token = localStorage.token;
            if (token) {
                // 根据 token 获取用户信息
                const res = await api.users.getUserInfo();
                console.log(res);
                return;
            } 
            alert('你还未登录，请先登录');
            next('/login');
        }
    }
]
```

## 三、保存用户信息

我们需要将请求回来的用户信息保存到状态机的 state 中，因此，我们需要在主仓库中进行配置：

```js
export default new Vuex.Store({
    state: {
        userInfo: {},   // 用户信息的初始值
    },
    getters: {
        username: state => state.userInfo.username   // 筛选用户信息中的用户名
    },
    mutations: {
        // 接收最新的用户数据，并保存到 state 中
        SET_USER_INFO(state, payload) {
            state.userInfo = payload;
        }
    }
})
```

仓库配置完成后，回到导航守卫中，将获取到的用户信息，通过 mutations 保存到 state 中：

```js
import store from '@/store';
const routes = [
    {
        //...
        // 路由前置守卫
        beforeEnter: async (to, from, next) => {
            const token = localStorage.token;
            if (token) {
                // 根据 token 获取用户信息
                const res = await api.users.getUserInfo();
                if (res.code) {
                    store.commit('SET_USER_INFO', res.data); // 调用 mutations 将用户信息保存到 state
                    next();   // 让用户进入对应的页面
                    return;
                }
            } 
            alert('你还未登录，请先登录');
            next('/login');
        }
    }
]
```

## 四、渲染用户信息

在组件中，要获取仓库 state 或 getters 中的数据来渲染，有两种方式：

1. 直接在 `template` 中通过 `$store` 来获取并渲染数据；
2. 可以将仓库的数据交给组件自己的计算属性，然后渲染计算属性即可；

```html
<p>欢迎你，{{$store.getters.username}}</p>
<p>欢迎你，{{username}}</p>
```

```js
export default {
    computed() {
        username() {
            return this.$store.getters.username;
        }
    }
}
```

## 五、验证 token 是否过期

如果要验证 token 是否过期，需要通过 axios 请求，**将 token 添加到请求头中**，发送到后端来进行验证。

考虑到实际开发中，每一个请求都需要携带 token，随时对 token 的有效期进行验证，所以，我们可以直接在 axios 的请求拦截器中来统一添加请求头的 token：

```js
// 请求拦截器：当前端的请求在发送到后端去之前，会拦截下来
axios.interceptors.request.use((config) => {
    // 所有请求在发往后端去之间，都先拦截下来，给请求头中添加 token
    config.headers.Authorization = localStorage.token;
    return config;
})
```

## 六、处理 token 过期

如果后端 token 验证已过期，会返回一个 401 的报错，所以，我们需要在 axios 的响应拦截器中拦截到这个报错，然后进行处理：

```js
import router from "@/router";
import { MessageBox } from 'element-ui';

// 响应拦截器：当后端返回的结果到达前端页面之前，会被拦截下来
axios.interceptors.response.use((res) => {
    // 响应成功时执行的函数
    return res.data;
}, (err) => {
    // 响应失败时执行的函数
    if (err && err.response && err.response.status) {
        if (err.response.status === 401) {
            MessageBox.alert('登录已过期，请重新登录', '提示', {
                confirmButtonText: '确定',
                callback: action => {
                    router.replace('/login');
                }
            });
        }
    }
    return Promise.reject(err.message);
});
```

以上步骤完成后，后续，不管是用户没有登录，直接访问权限页面，还是登录已过期，访问权限页面，我们都会提示用户重新登录，并强制跳转到登录页面。



