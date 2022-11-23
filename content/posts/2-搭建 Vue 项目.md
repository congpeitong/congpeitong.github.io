---
title: "2-Vue基础-搭建Vue项目"
date: 2022-11-22T10:02:30+08:00
draft: false
tags: ["Vue基础"]
categories: ["Vue"]
---

# 搭建 Vue 项目

## 一、安装 CLI 工具

### 1、查看 CLI 版本号

我们可以通过查看版本号来判断本机是否已经安装过 CLI 工具：

```bash
vue -V
# @vue/cli 5.0.6
```

如果本机电脑中安装的版本是 1.x 或 2.x，需要先执行以下命令，卸载旧版本，再重新安装最新的版本。

```bash
npm uninstall vue-cli -g
```

如果本机电脑中安装的版本是 3.x 或以上，就不需要卸载，可以直接安装最新的版本进行覆盖。

如果提示“**Vue 不是内部或外部命令**”，则表示本机电脑中没有安装过 CLI 工具。

### 2、安装 CLI

安装最新的 CLI，要求本机电脑的 Node.js 版本在 8.9 以上。可以通过以下命令，查看本机电脑中 Node.js 的版本号：

```bash
node -v
# v14.19.1
```

执行以下命令，全局安装 CLI 工具：

```bash
npm install -g @vue/cli
```

## 二、搭建 Vue 项目

### 1、创建项目

将终端定位到需要创建项目的路径中，执行以下命令：

```bash
vue create vue-demo
```

其中，`vue-demo` 是项目名称，会作为项目根目录的文件名。

### 2、选择安装方式

```bash
? Please pick a preset: (Use arrow keys)
  Default ([Vue 3] babel, eslint)
  Default ([Vue 2] babel, eslint)
> Manually select features
```

这里我们选择第三个 `Manually select features`，自定义安装。

### 3、选择安装插件

```bash
? Check the features needed for your project: (Press <space> to select, <a> to toggle all, <i> to invert
selection, and <enter> to proceed)
>(*) Babel
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support
 ( ) Router
 ( ) Vuex
 ( ) CSS Pre-processors
 ( ) Linter / Formatter
 ( ) Unit Testing
 ( ) E2E Testing
```

这里我们暂时只选择安装第一个 Babel 插件，其他插件全都不安装。

注意要取消 Linter / Formatter 插件。

### 4、选择 Vue 版本

```bash
? Choose a version of Vue.js that you want to start the project with
  3.x
> 2.x
```

我们主要以 Vue2 的学习为主，所以这里选择 2.x 的版本安装。

### 5、选择配置代码位置

```bash
? Where do you prefer placing config for Babel, ESLint, etc.?
> In dedicated config files
  In package.json
```

`In dedicated config files` 表示生成新文件来保存插件的配置代码，`In package.json` 表示将配置代码添加在 `package.json` 文件中。

这里我们选第一个。

### 6、是否保存供以后使用

```bash
? Save this as a preset for future projects? (y/N) n
```

当前项目的配置已经可以生效了。

由于我们当前的配置比较简单，因此这里选择 `n`，暂时不保存。

## 三、启动项目

**将终端定位到项目的根目录**，执行以下命令启动项目：

```bash
npm run serve
```

启动成功后，会出现类似以下提示：

```bash
DONE  Compiled successfully in 484ms


App running at:
- Local:   http://localhost:8080/
- Network: http://10.211.55.3:8080/
```

浏览器访问提示中的路径，就可以看到项目的欢迎界面了。
