---
title: "2-vue-cli-动态路由控制权限"
date: 2022-11-22T12:42:30+08:00
draft: false
tags: ["vue-cli"]
categories: ["Vue"]
---
# 动态路由控制权限
在一般的管理系统中，在项目的权限页面中，除了要判断用户是否登录外，我们还需要对“已登录”的用户权限等级进行区分。

不同等级的用户，在系统首页中看到的菜单不一样。同时，为了防止低权限用户通过地址访问其他高权限页面，所以，我们需要将所有的权限路由都替换成动态路由。

只有当用户登录成功后，我们才开始配置对应权限的路由。

总结下来，我们需要做两件事情：

1. 首页菜单的分权限显示；
2. 路由分权限进行动态配置；

## 一、权限菜单渲染

首先，需要发送请求获取当前用户权限能访问的菜单数据。

### 1、配置状态机

```js
export default new Vuex.Store({
    state: {
        authMenus: [], // 权限菜单数据
    },
    mutations: {
        SET_AUTH_MENUS(state, payload) {
            state.authMenus = payload;
        }
    },
    actions: {
        async getAuthMenusAsync({ commit }) {
            const res = await api.users.getAuthMenus();
            if (res.code) {
                commit('SET_AUTH_MENUS', res.data);
            }
        }
    }
})
```

### 2、筛选菜单数据

在菜单组件中，调用 action 的方法，发送请求获取权限数据，然后根据权限数据来筛选菜单数据：

```js
import { mapActions, mapState } from "vuex";
export default {
    data() {
        return {
            menus: [
                // ... 完整的菜单数据
            ],
        };
    },
    created() {
        this.getAuthMenusAsync();
    },
    computed: {
        ...mapState(["authMenus"]),
        // 根据权限数据 authMenus，筛选完整菜单 menus，得到一个权限菜单 filterMenus
        filterMenus() {
            return this.menus
                .map((item) => {
                    const authChild = item.children.filter((child) => {
                        return this.authMenus.includes(child.path);
                    });
                    // 返回一级菜单对象
                    return { ...item, children: authChild };
                })
                .filter((item) => item.children.length);
        },
    },
    methods: {
        ...mapActions(["getAuthMenusAsync"]),
    },
};
```

为了能够查看到首页中菜单渲染的效果，可以先将 home 的路由配置成静态路由。

```js
// router/index.js 
const routes = [
    // ...
    {
        path: '/home',
        name: 'Home', 
        component: HomeView
    }
]
```

## 二、配置动态路由

我们直接将首页以及首页下的所有二级路由，全都配置成动态路由。所以，首先需要先将之前配置的 home 的静态路由删掉。

```js
const routes = [
    // ...
    // {
    // 	path: '/home',
    // 	name: 'Home',  // 路由名称，可选
    // 	component: HomeView
    // }
]
```

### 1、判断用户是否登录

```js
router.beforeEach(async (to, from, next) => {
    // 当用户进入的页面不是登录也不是注册
    if (!to.path.includes('login') && !to.path.includes('register')) {
        const token = localStorage.token;
        if (token) {
            // 发送请求验证 token 是否过期，如果 token 没有过期，就获取用户信息
            const res = await api.users.getUserInfo();
            if (res.code) {
                next();
                return;
            }
        }
        MessageBox.alert('你还未登录，请先登录', '提示', {
            confirmButtonText: '确定',
            callback: action => {
                next('/login');
            }
        });
        return;
    }
    // 代码执行到这，表示用户进入的页面是登录或注册页面
    next();
})
```

以上配置完成后，**用户可以随意进入登录和注册页**。同时，如果**本地没有 token，在进入权限页面时会提示用户去登录**，如果**本地有 token，但是 token 过期，也会提示用户去登录**。

### 2、配置动态路由

我们在 router 目录中，创建一个 `addRoutes.js` 文件，专门用来实现动态路由的配置。

首先，先将完整的子路由配置，以及路由对应的组件，在 `addRoutes.js` 文件保存：

```js
import HomeView from '../views/home/HomeView.vue'
import StudentsList from '../views/students/StudentsList.vue'
// ... 引入所有路由对应的组件

// 项目中完整的路由
const fullRoutes = [
    // ... 完整的路由对象的配置
]
```

封装一个生成动态路由的方法，并暴露出去：

```js
const addRoutes = () => {

}

export default addRoutes;
```

在全局导航守卫中，当我们确认**用户已登录，且登录状态未过期**时，就可以调用该方法来生成动态路由的配置。

```js
import addRoutes from './addRoutes';
router.beforeEach(async (to, from, next) => {
    // 当用户进入的页面不是登录也不是注册
    if (!to.path.includes('login') && !to.path.includes('register')) {
        // ...
        if (token) {
            // 发送请求验证 token 是否过期，如果 token 没有过期，就获取用户信息
            const res = await api.users.getUserInfo();
            if (res.code) {
                store.commit('SET_USER_INFO', res.data); // 将用户信息保存到仓库
                next();
                addRoutes();   // 开始生成动态路由
                return;
            }
        }
        // ...
    }
    // ...
})
```

配置动态路由，需要先获取到权限菜单数据。

前面我们在菜单组件中调用了请求获取了数据，但是，按照当前的代码执行，在配置动态路由时，菜单组件还没有创建，也就没办法发送请求获取权限数据。

因此，我们将菜单组件中发送请求的代码，换到配置动态路由的方法中来：

```js
export default {
    // created() {
    //     this.getAuthMenusAsync();
    // },
    // methods: {
    //     ...mapActions(["getAuthMenusAsync"]),
    // },
}
```

动态路由配置代码参考如下：

```js
const addRoutes = async () => {
    // 等待请求完成，并将数据保存到 state 中
    await store.dispatch('getAuthMenusAsync');

    router.addRoute({
        path: '/home',
        name: 'Home',
        component: HomeView,
        children: fullRoutes.filter(item => store.state.authMenus.includes(item.path))
    })
    router.addRoute({
        path: '*',
        name: 'NotFound',
        component: NotFound,
    })
}
```



