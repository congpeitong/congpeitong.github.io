---
layout: post
title: windows下安装Jekyll
categories: [Jekyll]
---
# 网站托管
在这里就不进行赘述
# 安装Jekyll
jekyll相当于一个编译工具，安装后可以通过jekyll创建一个网站模板，可以实时修改内容，并可以实时通过本地url预览修改
后的效果。然后推送到Github,访问自己的博客。
1. 安装Ruby[https://rubyinstaller.org/]
2. 下载RubyGems[https://rubygems.org/pages/download]下载完成后解压到你想安装的位置，然后打开命令行
进入Ruby根目录，分为两步执行以下命令
``` bat
    ruby setup.rb


    gem install jekyll
```
3. 完成安装后，使用jekyll创建一个博客模板，打开命令行执行
``` bat
    jekyll new BlogTemplate

    cd BlogTemplate

    jekyll server

```

4. 打开 http://127.0.0.1:4000/ 即可浏览刚刚创建的模板
