---
title: "7-vue-cli-可视化图表"
date: 2022-11-22T12:47:30+08:00
draft: false
tags: ["vue-cli","echarts"]
categories: ["Vue"]
---
# 可视化图表

## 一、下载

```bash
npm install echarts --save
```

## 二、使用

### 1、引入

```js
import * as echarts from "echarts";
```

### 2、初始化

```js
var myChart;
export default {
    mounted() {
        myChart = echarts.init(document.getElementById("chart-one"));
    }
}
```

### 3、处理渲染数据

```js
import { createNamespacedHelpers } from "vuex";
const { mapActions, mapState } = createNamespacedHelpers("classesModule");
export default {
    created() {
        this.getClassesAsync();
    },
    computed: {
        ...mapState(["classes"]),
        chartOneData() {
            return {
                title: {
                    text: "班级人数统计",
                },
                tooltip: {},
                xAxis: {
                    data: this.classes.map((item) => item.name),
                },
                yAxis: {},
                series: [
                    {
                        name: "销量",
                        type: "bar",
                        data: this.classes.map((item) => item.students.length),
                    },
                ],
            };
        },
    },
    methods: {
        ...mapActions(["getClassesAsync"]),
    },
};
```

### 4、渲染

```js
export default {
    watch: {
        classes() {
            myChart.setOption(this.chartOneData);
        },
    }
}
```

