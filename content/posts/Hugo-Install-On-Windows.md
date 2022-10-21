---
title: "Windows 下安装Hugo"
date: 2022-10-21T13:43:30+08:00
draft: true
categories: ["Hugo"]
---

# windows下安装hugo
各平台安装hugo的方法可以参考官方教程[hugo安装教程](https://gohugo.io/getting-started/installing/)
## 使用安卓 choco 包管理器安装hugo
### 使用管理员权限打开powershell并运行
```shell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
### 使用choco安装hugo
```shell
choco install hugo --confirm
```
# 新建博客
## 新建博客站点
```shell
hugo new site blogName
```
使用git管理博客系统，将新建的博客站点上传至github
## 安装hugo主题meme
```shell
git submodule add --depth 1 https://github.com/reuixiy/hugo-theme-meme.git themes/meme
```
## 测试博客
将 *themes/meme/config-examples/en* 复制替换到博客根目录下
### 创建页面
```shell
~/blog $ hugo new "posts/hello-world.md"
~/blog $ hugo new "about/_index.md"
```
### 运行
```shell
~/blog $ hugo server -D
```
如果提示某个错误（这个错误忘记截图了），根据官方文档安装hugo-extended
```shell
choco install hugo-extended -confirm
```
再次运行，根据提示的URL地址可查看此博客界面
