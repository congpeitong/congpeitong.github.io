---
title: "3-vue-cli-动态路由优化"
date: 2022-11-22T12:43:30+08:00
draft: false
tags: ["vue-cli"]
categories: ["Vue"]
---
# 动态路由优化

前面我们实现了动态菜单和动态路由的基本配置，但是会有一些 bug，所以我们还需要对它们做进一步的优化。

## 一、防止已登陆用户重复登录

正常情况下来说，一个已经登录了的用户，除非退出登录，否则是不能再手动跳转到登录页面去重新登录的。

所以，我们需要在全局导航守卫中对“跳转到登录”做进一步的判断：

```js
router.beforeEach(async (to, from, next) => {
    // 当用户进入的页面不是登录也不是注册(例如进入首页)
    if (!to.path.includes('login') && !to.path.includes('register')) {
        // ....
    }
    // 判断用户是否想要进入登录页
    if (to.path.includes('login')) {
        const token = localStorage.token;
        // 如果有 token，判定为已登录，不让用户进入登录页面
        if (token) {
            next(false);  // 停留在当前页面
            return;
        }
    }
    next();
})
```

以上配置代码的逻辑是：如果进入登录页的用户，本地还有 token，我们就判定为是已登录用户，就强制让其停留在当前页面，不能跳转到登录页。

但是注意！！！这样处理了之后会有另一个 bug，就是用户在页面中，如果因为正常的 token 过期想要跳转到登录页，也跳转不过去了。因为我们刚刚在导航守卫里限制的是“只要本地有 token，就不让用户去重新登录”。

解决方案也很简单，我们直接在 `utils/request.js` 文件的 axios 响应拦截器中，当判断用户是 401 身份过期后，我们就将本地存储的 token 删掉，再跳转到登录页：

```js
axios.interceptors.response.use((res) => {
    return res.data;
}, (err) => {
    if (err && err.response && err.response.status) {
        if (err.response.status === 401) {
            MessageBox.alert('登录已过期，请重新登录', '提示', {
                confirmButtonText: '确定',
                callback: action => {
                    localStorage.removeItem('token');  // 删除 token
                    router.replace('/login');
                }
            });
        }
    }
    return Promise.reject(err.message);
});
```

以上配置完成后，就可以让已登录用户无法进入登录页面，同时也可以让 token 过期的用户进入到登录页面。

## 二、退出登录

已登录的用户如果想要切换账号登录，必须退出登录后，再重新登录。

所以我们可以在页面中给用户一个“退出”按钮，然后在退出登录的事件中做以下处理：

```js
export default {
    methods: {
        logout() {
            localStorage.removeItem("token");
            this.$router.push("/login");
            // 刷新页面（重置路由、重置权限菜单的数据）
            location.reload();
        },
    }
}
```

## 三、防止重复添加动态路由

由于我们是在导航守卫中进行的“动态路由”的添加，所以每次进入导航守卫时，都有可能重复的添加动态路由，导致我们浏览器的控制台的中会不断的抛出黄色警告说路由重复：

![image-20220804183105018](https://woniumd.oss-cn-hangzhou.aliyuncs.com/web/jianglan/20220804183105.png)

因此，我们需要在 `router/addRoutes.js` 中添加一个判断，来阻止动态路由的重复添加：

```js
const addRoutes = async () => {
    // 判断权限数据是否为空，为空的话才进入 if 请求权限数据、添加动态路由
    // 目的就是为了防止重复请求权限数据、重复添加动态路由
    if (store.state.authMenus.length === 0) {
        // 等待请求完成，并将数据保存到 state 中
        await store.dispatch('getAuthMenusAsync');
        router.addRoute({
            path: '/home',
            name: 'Home',
            component: HomeView,
            children: fullRoutes.filter(item => {
                return store.state.authMenus.includes(item.path);
            })
        })
        // 404 放到所有路由的最后
        router.addRoute({
            path: '*',
            name: 'NotFound',
            component: NotFound,
        })
    }
}
```

## 四、筛选 path 的动态路由

在我们的项目中，有一个页面的路由路径是带动态参数的动态路由，例如修改学生的路由路径配置如下：

```js
{
    path: 'studentsUpdate/:id',
    // ...
}
```

这种动态路径，如果按照我们之前筛选路由的方式来添加动态路由，就会匹配不上，所以，我们还需要对前面筛选动态路由进行修改：

```js
const addRoutes = async () => {
    // ....
    router.addRoute({
        path: '/home',
        name: 'Home',
        component: HomeView,
        children: fullRoutes.filter(item => {
            // item.path.split('/')[0] 获取动态路由路径的前半部分
            return store.state.authMenus.includes(item.path.split('/')[0]); 
        })
    })
    // ...
}
```

