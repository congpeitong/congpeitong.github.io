---
layout: post
title: windwos创建计划任务
categories: [Windows]
---
# 计划任务
在windows系统中，我们可以通过任务计划程序来实现定时或者事件触发运行事务
# 计划任务的创建
windows中谁创建的计划任务谁能看到，A创建的计划任务默认B是看不到的

创建任务的命令行是  schtasks /create,后面再加上其它参数，比较常用的参数如下：
``` sh
    /sc   计划任务类型，可选值为MINUTE、HOURLY、DAILY、WEEKLY、ONCE、ONSTART、ONLOGON、ONIDLE、MONTHLY、ONEVENT
    /tn   计划任务名称，后续查询、修改、删除、执行时使用
    /tr   需要运行的程序或命令，传入的命令中间如果有空格会被截断为程序和参数，因此需要将双引号转义并传入。
    /ru   运行任务的用户账户名，不使用此参数的话使用执行schtasks命令的账户运行计划任务
    /rp   运行任务的用户账户密码
    /mo   指定任务在计划类型中的运行间隔
    /d    指定任务在一个月或者星期的某一天运行，只适用于MONTHLY和WEEKLY类型。
    /m    指定任务在某个月运行，只适用于MONTHLY类型。
    /i    当计划任务类型为ONIDLE时，运行任务前计算机处于空闲状态的分钟数。
    st    当计划任务类型为MINUTE、HOURLY、DAILY、WEEKLY、MONTHLY时使用，指定任务的开始时间，默认为本地计算机的当前时间。
    /ri   指定计划任务的重复间隔，以分钟为单位。不适合计划类型：MINUTE、HOURLY、ONSTART、ONLOGON、ONIDLE
    /et   指定计划任务的结束时间，适用于计划类型：MINUTE、HOURLY， 在指定的结束时间之后，schtasks 不会再次启动任务，除非当前系统时间调回开始时间。默认情况下，没有结束时间。
    /du   指定任务计划的持续时间，与/et类似，默认情况下没有持续时间。
    /k    在指定计划任务的结束时间或持续时间后停止任务，如果不加此参数，则在时间到了会继续运行或者重启该任务。
    /it   只在用户登录时运行
    /z    在任务计划完成后删除任务计划
    /f    在创建任务时如果任务已存在不显示警告
    /RL   为作业设置运行级别。有效值为LIMITED 和 HIGHEST。默认值为 LIMITED。
    /F    如果指定的任务已经存在，则强制创建任务并抑制警告。

```
当使用了/sc参数为MINUTE,HOURLY,DAILY,WEEKLY时，我们需要指定/mo运行的间隔时间
+ MINUTE: 1到1439分钟
+ HOURLY: 1-23小时
+ DAILY: 1-365天
+ WEEKLY: 1-52周
+ ONCE: 无修改者
+ ONSTART: 无修改者
+ ONLOGON: 无修改者
+ ONIDLE: 无修改者
+ MONTHLY: 1-12或
+ FIRST,SECOND,THIRD,FOURTH,LAST,LASTDAY
+ ONEVENT: XPath事件查询字符串

在windows7上执行schtasks /query显示错误，无法加载列资源，解决办法
``` bat
    chcp437
```
# 示例
## 创建计划任务
创建名字为test的计划任务，每隔一分钟运行一次，任务执行时指定执行的程序为calc.exe

重启后也会执行(程序启动的默认权限为当前用户)

``` bat
    schtasks /create /sc minute /mo 1 /tn test /tr C:\WINDOWS\system32\calc.exe
    #以system权限运行
    schtasks /create /sc minute /mo 1 /tn test /tr C:\WINDOWS\system32\calc.exe /ru system

```
## 开机上线
1. 系统启动时上线，无需登录，系统启动时运行poc.exe,但只当指定了运行账好为system时才成功上线：
``` bat
    schtasks /create /tn test /tr C:\Users\Administrator\Desktop\poc.exe /sc onstart /ru 不运行（计划任务显示准备就绪）
    schtasks /create /tn test /tr C:\Users\Administrator\Desktop\poc.exe /sc onstart /ru administrator  不运行（计划任务显示准备就绪）
```

## 账号登录时上线
``` bat
    schtasks /create /tn test /tr C:\Users\Administrator\Desktop\poc.exe /sc onlogon   如添加计划任务时使用的账号，登录成功后即上线
```
## 应用案例
创建计划任务‘updatex’,触发程序为poc.exe,运行级别为高级别，以system权限每隔4小时运行一次
``` bat
    schtasks /create /tn "updatex" /tr C:\Users\Administrator\Desktop\poc.exe /rl highest /F /sc hourly /mo 4 /RU system
```
如果当前为普通用户执行计划任务，/ru参数值可以设置为 %username% 即当前用户，如果是管理员可以指定为system

# 查看计划任务
查询计划任务的命令是 schtasks /query, 常用参数如下：
+ /fo 指定格式输出，可选Table,List,或者CSV,没加此参数默认是table
+ /v 显示详细信息，如任务执行的程序位置等
1. 查询名称为test的计划任务
``` bat
    schtasks /query | findstr test
```
# 删除计划任务
删除计划任务的命令是schtasks /delete, 计划任务已运行的程序不会中断
``` bat

    schtasks /delete /tn test /f

```

# 结束计划任务
该命令会结束此次运行的计划任务，即将计划任务执行的程序停止，但是到下一次任务执行的时间依然会执行任务，因为没有删除任务
```bat

    schtasks /end /tn test

```

# 运行计划任务
可以立即运行test计划任务，但是不会影响计划，也不会改变设置的下一个运行时间
``` bat

    schtasks /run /tn test

```
