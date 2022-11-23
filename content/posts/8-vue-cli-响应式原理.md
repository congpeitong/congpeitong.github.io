---
title: "8-vue-cli-响应式原理"
date: 2022-11-22T12:48:30+08:00
draft: false
tags: ["vue-cli"]
categories: ["Vue"]
---
# 响应式原理

在 Vue 实例或组件中，data 中的数据都会加入到 Vue 的响应式系统。

每当 data 中的数据发生改变时，Vue 的响应式系统会检测到数据的变化，然后自动更新对应的页面节点。

## 一、核心概念

响应式原理的核心：

1. **数据劫持**：把 data 对象中的普通数据变成“响应式数据”；
2. **发布订阅者模式（观察者模式）**：

## 二、数据劫持

```js
class Vue {
    constructor(option) {
        this.$el = document.querySelector(option.el);
        this.$data = option.data();
        new Observer(this.$data);
    }
}

class Observer {
    constructor(data) {
        this.$data = data;
        this.walk();
    }
    walk() {
        for (let key in this.$data) {
            this.defineReactive(this.$data, key);
        }
    }
    defineReactive(data, key, value = data[key]) {
        Object.defineProperty(data, key, {
            // 当我们访问 data.name 属性时，会自动触发 get 方法
            get() {
                console.log('有人在访问我的数据了', key);
                return value;
            },
            set(newValue) {
                console.log('有人在修改我的数据了', key);
                value = newValue;
            }
        })
    }
}
```

### 处理 this 的数据

前面我们把 this.$data 对象中的数据都进行了数据劫持，但是在 Vue 中源码中，this 身上也有和 this.$data 中相同的一份数据，所以，我们还要在 this 身上添加一份相同的数据：

```js
class Vue {
    constructor(option) {
        this.$el = document.querySelector(option.el);
        this.$data = option.data();
        this.proxy();  // 调用
        new Observer(this.$data);
    }
    // 将 this.$data 身上所有的属性，添加一份到 this 身上
    proxy() {
        for (let key in this.$data) {
            Object.defineProperty(this, key, {
                get() {
                    return this.$data[key];
                },
                set(newValue) {
                    this.$data[key] = newValue;
                }
            });
        }
    }
}
```

## 三、数据渲染

```js
class Vue {
    constructor(option) {
        // ...
        new Compiler(this.$el, this.$data);
    }
    // ...
}

// 负责将数据渲染到页面
class Compiler {
    constructor(el, data) {
        this.$el = el;
        this.$data = data;
        this.compile();
    }
    compile() {
        const node = this.$el.firstElementChild;
        const key = node.innerText.slice(2, -2);
        node.innerText = this.$data[key];
    }
}
```

## 四、发布订阅者模式

创建观察者和订阅者：

```js
// 观察者：每一条数据每被用一次，就产生一个对应的观察者
class Watcher {
    constructor(render) {
        this.render = render;
        window.target = this;
        this.update();
        window.target = null;
    }
    // 只要执行 update 方法，页面当前节点就会更新
    update() {
        this.render();
    }
}
// 订阅者：每一条数据对应着一个订阅者，用来管理该数据所有的观察者
class Dep {
    constructor() {
        this.subs = [];
    }
    // 将 watcher 添加到 dep 的数组中
    addSubs(watcher) {
        this.subs.push(watcher);
    }
    // 通知当前负责的所有 watcher 更新节点
    notify() {
        this.subs.forEach((watcher) => {
            watcher.update();
        })
    }
}
```

生成观察者

```js
class Compiler {
    //...
    compile() {
        //...
        const render = () => {
            node.innerText = this.$data[key];
        }
        new Watcher(render);
    }
}
```

生成订阅者以及数据发生改变时更新页面：

```js
class Observer {
    // ....
    defineReactive(data, key, value = data[key]) {
        const dep = new Dep();
        Object.defineProperty(data, key, {
            // 当我们访问 data.name 属性时，会自动触发 get 方法
            get() {
                console.log('有人在访问我的数据了', key);
                if (window.target) {
                    dep.addSubs(window.target);
                }
                return value;
            },
            set(newValue) {
                console.log('有人在修改我的数据了', key);
                value = newValue;
                dep.notify();   // 订阅者通知观察者更新节点
            }
        })
    }
}
```



## 五、总结

1. 通过 `Object.defineProperty()` 对 data 中的数据进行“数据劫持”；
2. 每劫持一条 data 中的数据，就产生一个对应的订阅者（dep）；
3. 每一条数据被一个节点使用时，产生一个对应的观察者（watcher）；
4. 同一条数据下的观察者（watcher）都统一交给该数据的订阅者（dep）管理；
5. 当数据发生改变时，该数据的订阅者（dep）会通知它负责的所有观察者（watcher）去更新节点；

