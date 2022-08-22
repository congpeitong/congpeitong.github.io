---
layout: post
title: windows下安装绿色版mysql
categories: [Mysql]
---

# windows下安装绿色版mysql
## 版本
mysql.5.7
## 安装过程
1. 解压mysql绿色版文件夹，放置于你想要安装的位置。
2. 进入解压的mysql文件夹，创建data文件夹和my.ini文件
3. 打开my.ini文件写入一下内容
```ini
    [mysqld]
    bind-address = 0.0.0.0
    #mysql端口号设置
    port = 3307
    #basedir（刚创建的data文件夹目录） 和 datadir视自己情况而定
    basedir=D:\Program Files\mysqls\mysql-5.7.21-winx64
    datadir=D:\Program Files\mysqls\mysql-5.7.21-winx64\data\
    max_connections=1000
    character_set_server = utf8
    #mysql7
    sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER
    #mysql8
    #sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
    [client]
    default-character-set=utf8
    [mysql]
    default-character-set=utf8
```
4. 将bin目录加入到环境变量之中。
5. 以管理员权限进入bin目录。
6. 执行以下命令
``` sh
    mysqld install

    mysqld --initialize --console
```
7. 执行完毕后，会在 *root@localhost:* 后生成一个随机的mysql密码， 一定要将记住它。
8. 启动mysql服务
``` sh
    net start mysql
```
9. 命令行中登录mysql
```sh
    mysql -u root -p

    回车（Enter）后让你输入密码，该密码就是步骤7的密码
```
10. 进入Mysql后修改密码
``` sh
    SET PASSWORD = PASSWORD('此处是你需要设置的密码');
```
