+++
title = "Nginx基础"
lastmod = 2022-11-14T08:58:07+08:00
tags = ["Nginx"]
draft = false
author = "congpeitong"
+++

## Nginx 基本命令 {#nginx-基本命令}

{{< highlight shell >}}


# 1. 启动
start nginx
ningx.exe
start nginx -c conf/nginx.conf #特殊设置nginx的配置文件路径
# 2. 暂停
nginx.exe -s stop # 快速暂停
nginx.exe -s quit # 完整有序的停止nginx
# 3. 重启
nginx -s reload
# 4. Windows下查看Nginx进程
tasklist /fi "imagename eq nginx.exe"
{{< /highlight >}}
