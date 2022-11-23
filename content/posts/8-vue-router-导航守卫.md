---
title: "8-Vue-Router-导航守卫"
date: 2022-11-22T12:08:30+08:00
draft: false
tags: ["Vue-Router"]
categories: ["Vue"]
---
# 导航守卫

导航，指的是“路由正在发生改变”，实际上就是“路由正在跳转”。

而导航守卫，就是在路由跳转过程中，将路由拦截下来，然后经过一系列的操作后，由守卫来决定当前路由是否可以继续跳转。

## 一、分类

Vue Router 中大致将导航守卫分为三大类：

1. 全局守卫：作用于项目中所有路由；
2. 路由独享守卫：只作用于指定路由；
3. 组件内守卫：作用于与当前组件有关的路由；

## 二、全局守卫

全局守卫又可以分为以下三类：

1. 全局前置守卫：
2. 全局解析守卫：
3. 全局后置守卫：

### 1、全局前置守卫

全局前置守卫，指的是在进入任何一个路由之前，都会触发该守卫。

```js
router.beforeEach((to, from, next) => {
    
})
```

例如，我们可以通过全局前置守卫来对用户的登录状态进行验证：

```js
// 设置全局前置守卫
router.beforeEach((to, from, next) => {
    if (to.path.includes('home')) {
        const token = localStorage.token;
        if (token) {
            next();
        } else {
            alert('你还未登录，请先登录');
            next('/login');
        }
    } else {
        next();
    }
})
```

## 三、路由独享守卫

路由独享守卫又可以分为以下三类：

1. 路由前置守卫：
2. 路由解析守卫：
3. 路由后置守卫：

### 1、路由前置守卫

```js
const routes = [
    {
        path: '/home',
        name: 'Home',  
        component: HomeView,
        // 路由独享前置守卫
        beforeEnter: (to, from, next) => {

        }
    }
]
```

案例代码：

```js
const routes = [
    // ...
    {
        path: '/home',
        name: 'Home',  // 路由名称，可选
        component: HomeView,
        // 路由独享前置守卫
        beforeEnter: (to, from, next) => {
            const token = localStorage.token;
            if (token) {
                next();
            } else {
                alert('你还未登录，请先登录');
                next('/login');
            }
        },
        children: [
            // ...
        ]
    }
    // ...
]
```

## 四、组件内守卫

组件内守卫也可以分为以下三类：

1. 进入路由前：
2. 路由发生改变：
3. 离开路由前：

### 1、离开路由前的守卫

```js
export default {
    beforeRouteLeave(to, from, next) { 

    }
}
```

案例代码：

```js
export default {
    data() {
        return {
            isUpdate: false
        }  
    },
    methods: {
      	updateStudents() {
            // ...
            // 如果修改成功
            this.isUpdate = true;
        }  
    },
    // 当离开组件前
    beforeRouteLeave(to, from, next) {
        if (!this.isUpdate) {
            // 进入 if，说明当前的数据还未提交
            this.$confirm("你还有数据未提交，确认离开吗？", "提示", {
                confirmButtonText: "确定",
                cancelButtonText: "取消",
                type: "warning",
            }).then(() => {
                next();
            }).catch(() => {});
        } else {
            // 进入 else，说明当前的数据已经提交
            next();
        }
    },
}
```



