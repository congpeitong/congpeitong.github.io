---
title: "6-vue-cli-混入"
date: 2022-11-22T12:46:30+08:00
draft: false
tags: ["vue-cli"]
categories: ["Vue"]
---
# 混入

Vue 中的混入，就是用来提取多个组件中公共的 JS 代码。

## 一、创建混入对象

我们在 src 中创建一个 `mixins` 目录，在该目录中来创建混入对象：

```js
const pageMixin = {
    data() {
        return {
            pageData: {
                currentPage: 1,
                pageSize: 5,
            }
        };
    },
    methods: {
        handlePageData(pageData) {
            this.pageData = pageData;
        }
    }
}
export default pageMixin;
```

## 二、引入混入对象

```js
import pageMixin from "@/mixins/pageMixin.js";

export default {
    mixins: [pageMixin]
}
```

组件中引入的混入对象，会和组件自己的属性方法等做合并。
