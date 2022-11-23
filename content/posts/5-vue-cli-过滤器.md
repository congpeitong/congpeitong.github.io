---
title: "5-vue-cli-过滤器"
date: 2022-11-22T12:45:30+08:00
draft: false
tags: ["vue-cli"]
categories: ["Vue"]
---
# 过滤器

Vue 中的过滤器，就是我们可以传递一个数据给过滤器，然后在过滤器对数据进行处理，最后返回一个过滤之后的新数据。

## 一、创建过滤器

过滤器分为全局过滤器和局部过滤器。

### 1、全局过滤器

全局过滤器可以作用于应用中的所有组件。

```js
import Vue from 'vue';

Vue.filter('过滤器名', function(value) {
    return 新数据;
});
```

### 2、局部过滤器

局部过滤器只作用于当前组件。

```js
export default {
    filters: {
        过滤器名(value) {
            return 新数据;
        }
    }
}
```

## 二、使用过滤器

过滤器可以用在纯文本中，也可以用在标签的属性中。

```html
<p>文本内容 | 过滤器名</p>
<p v-bind:class="内容 | 过滤器名"></p>
```

## 三、示例

我们对学生列表中学生的性别进行数据过滤。

```js
export default {
    filters: {
        genderFilter(value) {
            if (value == 1 || value == "男") {
                return "男";
            } else if (value === 0 || value == "女") {
                return "女";
            }
            return "保密";
        },
    },
}
```

最后，在列表中渲染时使用过滤器：

```html
<el-table-column label="性别">
    <template slot-scope="scope">
        <el-tag>{{scope.row.gender | genderFilter}}</el-tag>
    </template>
</el-table-column>
```

