+++
title = "shell编程基础"
lastmod = 2022-11-13T09:44:04+08:00
tags = ["Shell"]
categories = ["Linux"]
draft = false
author = "congpeitong"
+++

## 简介 {#简介}

-   shell是一个用C语言编写的程序，通过shell用户可以访问操作系统内核服务。
-   shell既是一种命令语言又是一种程序设计语言。
-   Shell script 是一种为shell编写的脚本程序。shell编程一般是指shell脚本编程，不是指开发shell自身
-   shell编程跟Java，PHP编程一样，只要有一个能编写代码的文本编译器和一个能解释执行的脚本解释器就可以了
-   Linux shell解释器种类繁多，一个系统就可以存在多个shell, 可以通过 cat /etc/shells命令查看系统中安装的shell解释器
-   Bash由于易用和免费，在日常生活中被广泛使用。同时，Bash也是大多数Linux系统默认的Shell


## shell解释器 {#shell解释器}

java需要虚拟机解释器，同理shell脚本也需要解释器，如下所示：

{{< highlight sh >}}
#命令：
cat /etc/shells
#输出如下：
/bin/sh
/bin/bash
/sbin/nologin
/bin/dash
/bin/tcsh
/bin/csh
{{< /highlight >}}


## 入门 {#入门}


### 编写脚本 {#编写脚本}

新建 /export/hello.sh 文件内容如下：

{{< highlight sh >}}

#!/bin/bash
echo 'hello world'
{{< /highlight >}}

\#!是一个约定的标记，它告诉系统这个脚本使用哪种解释器来执行，即使用哪一种shell。

echo命令用于向窗口输出文本。


### 执行脚本 {#执行脚本}


#### 执行方式一 {#执行方式一}

{{< highlight sh >}}
/bin/sh hello.sh
{{< /highlight >}}

或者

{{< highlight sh >}}
/bin/bash hello.sh
{{< /highlight >}}

sh是bash的快捷方式


#### 执行方式二 {#执行方式二}

方式一的简化方式:

{{< highlight sh >}}
bash hello.sh
sh hello.sh
{{< /highlight >}}

由于PATH环境变量中增加了/bin/目录，所以使用/bin/sh 等类似指令时，可以省略 /bin


#### 执行方式三 {#执行方式三}

./文件名

{{< highlight sh >}}
./hello.sh
{{< /highlight >}}

如果遇到权限不够，执行下面命令

{{< highlight sh >}}
chmod 755 hello.sh
#再次执行
./hello.sh
{{< /highlight >}}


## shell 变量 {#shell-变量}


### 简介 {#简介}

1.  在shell脚本中定义变量时，变量名子不加美元符号$，例如：

    your_name="congpeitong"
2.  注意：变量名和等号之间不能有空格

3.变量名的命名需遵守如下规则：

-   命名只能使用英文字母，数字和下划线，首个字母不能以下划线开头。
-   中间不能有空格，可以使用下划线_。
-   不能使用标点符号。
-   不能使用bash里面的关键字（可用help命令查看保留关键字）。

-   有效的Shell变量名示例如下：
    {{< highlight sh >}}
    RUNOOB
    LD_LIBARY_PATH
    _var
    var2
    {{< /highlight >}}
-   无效的变量命名
    {{< highlight sh >}}
    ?var=123
    user*name=runoob
    {{< /highlight >}}
-   除了显式的直接赋值还可以用语句给变量赋值(将/etc目录下的文件名循环出来)
    -   for file in \`ls /etc\`
    -   for file in $(ls /etc)


### 使用变量 {#使用变量}

使用一个定义过的变量，只要在变量名前面加美元符号即可，如：

{{< highlight sh >}}
your_name="zhangsan"
echo $your_name
echo ${your_name}
{{< /highlight >}}

变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：

{{< highlight sh >}}
for skill in java php python; do
    echo "I am good at ${skill}Script"
done
{{< /highlight >}}

如果不给 skill 变量加花括号，写成echo "I am good at $skillScript"，解释器就会把$skillScript当成一个变量（其值为空），代码执行结果就不是我们期望的样子了。

推荐给所有变量加上花括号，这是个好的编程习惯。

已定义的变量，可以被重新定义，如：

{{< highlight sh >}}
your_name="tom"
echo $your_name
your_name="alibaba"
echo $your_name
{{< /highlight >}}

这样写是合法的，但注意，第二次赋值的时候不能写$your_name="alibaba"，使用变量的时候才加美元符。


### 删除变量 {#删除变量}

使用 unset 命令可以删除变量。语法：

{{< highlight sh >}}
unset variable_name
{{< /highlight >}}

变量被删除后不能再次使用。unset 命令不能删除只读变量。


#### 实例 {#实例}

{{< highlight sh >}}
#!/bin/sh
myUrl="http://www.gitee.com"
unset myUrl
echo $myUrl
{{< /highlight >}}

以上实例执行没有任何输出


### 只读变量 {#只读变量}

使用 readonly 命令可以将变量定义为只读变量，只读变量的值不能被改变。

{{< highlight sh >}}

#!/bin/bash
myUrl="http://www.google.com"
readonly myUrl
myUrl="http://www.runoob.com"
{{< /highlight >}}

运行结果如下

{{< highlight sh >}}
/bin/sh: NAME: This variable is read only.
{{< /highlight >}}


### 字符串 {#字符串}

字符串是 shell 编程中最常用最有用的数据类型（除了数字和字符串，也没啥其它类型好用了），字符串可以用单引号，也可以用双引号，也可以不用引号。

1.  单引号
    {{< highlight sh >}}
    skill='java'
    str='I am goot at $skill'
    echo $str
    {{< /highlight >}}
    输出结果为：
    {{< highlight sh >}}
    I am goot at $skill
    {{< /highlight >}}

单引号字符串的限制

-   单引号里面的任何字符都会原样输出，单引号之中的变量是无效的
-   单引号字符串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但是可以成对出现，作为字符串拼接使用
-   双引号
    {{< highlight sh >}}
    skill='java'
    str="I am goot at $skill"
    echo $str
    {{< /highlight >}}
    输出结果为：
    {{< highlight sh >}}
    I am goot at java
    {{< /highlight >}}
    双引号的优点

    -   双引号里可以有变量
    -   双引号里可以出现转义字符
-   获取字符串长度
    {{< highlight sh >}}
    skill='java'
    echo ${skill}    # 输出结果: java
    echo ${#skill}   # 输出结果: 4
    或者:  expr length "iamlilei"   #输出结果:  8
    {{< /highlight >}}
-   提取子字符串

    -   substring(2)
    -   substring(2,3)

    以下实例从字符串第二个字符开始截取四个字符
    {{< highlight sh >}}
    str="I am goot at $skill"
    echo ${str:2}    # 输出结果为: am goot at java  从第二个字符开始截取,到结尾
    echo ${str:2:2}    # 输出结果为: am  从第二个字符开始截取,截取2个字符
    {{< /highlight >}}
-   查找子字符串

查找字符i或者o的位置（哪个字母先出现就计算哪个）

{{< highlight sh >}}
str="I am goot at  $skill"
echo `expr index "$str" am`    # 输出是: 3
或者:
expr index "iamlilei" am      #输出结果: 2   返回在STRING中找到CHARS字符串的位置；否则，返回0
{{< /highlight >}}


### 传递参数 {#传递参数}

我们可以在执行脚本时，向脚本传递参数，脚本内获取参数的格式为：$n。

n代表一个数字，1为执行脚本的第一个参数，2为执行脚本的第二个参数，依次类推

以下实例我们将向脚本传递三个参数，并分别输出，其中$0为执行的文件名

vim /export/sh/param.sh

{{< highlight sh >}}

#!/bin/bash
echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
{{< /highlight >}}

执行脚本

{{< highlight sh >}}
$ ./param.sh 1 2 3
{{< /highlight >}}

shell 传递参数实例输出

{{< highlight sh >}}

执行的文件名：./param.sh
第一个参数为：1
第二个参数为：2
第三个参数为：3
{{< /highlight >}}

几个特殊字符用来处理参数:

| 参数处理 | 说明                                                                         |
|------|----------------------------------------------------------------------------|
| $#   | 传递到脚本的参数个数                                                         |
| $\*  | 以一个单字符串显示所有向脚本传递的参数。如"$\*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。 |
| $$   | 脚本运行的当前进程 ID 号                                                     |
| $!   | 后台运行的最后一个进程的 ID 号                                               |
| $@   | 与 $\*相同，但是使用时加引号，并在引号中返回每个参数。   如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。 |
| $-   | 显示 Shell 使用的当前选项，与 set 命令功能相同。                             |
| $?   | 显示最后命令的退出状态。0 表示没有错误，其他任何值表明有错误。               |
|      |                                                                              |

{{< highlight sh >}}

#!/bin/bash
echo "Shell 传递参数实例！";
echo "第一个参数为：$1";
echo "参数个数为：$#";
echo "传递的参数作为一个字符串显示：$*";
{{< /highlight >}}

输出如下：

{{< highlight sh >}}
$ chmod +x test.sh
$ ./test.sh 1 2 3
Shell 传递参数实例！
第一个参数为：1
参数个数为：3
传递的参数作为一个字符串显示：1 2 3
{{< /highlight >}}

$\* 与 $@ 区别：

-   相同点：都是引用所有参数
-   不同点：只有在双引号中体现出来。假设在脚本运行时写了三个参数 1、2、3，，则 " \* " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。

<!--listend-->

{{< highlight sh >}}
#!/bin/bash
echo "-- $* 演示 ---"
for i in "$*"; do
    echo $i
done
echo "-- $@ 演示 ---"
for i in "$@"; do
    echo $i
done
{{< /highlight >}}

执行脚本输出结果如下：

{{< highlight sh >}}
$ chmod +x test.sh

$ ./test.sh 1 2 3

-- $* 演示 ---
1 2 3

-- $@ 演示 ---
1
2
3
{{< /highlight >}}


## shell算术运算符 {#shell算术运算符}


### 简介 {#简介}

Shell 和其他编程一样，支持包括：算术、关系、布尔、字符串等运算符。

原生 bash 不支持简单的数学运算，但是可以通过其他命令来实现，例如 expr。

expr 是一款表达式计算工具，使用它能完成表达式的求值操作。

例如，两个数相加：

{{< highlight sh >}}
val=`expr 2 + 2`
echo $val
{{< /highlight >}}

注意：

-   表达式和运算符之间要有空格，例如 2+2 是不对的，必须写成 2 + 2。
-   完整的表达式要被 \` 包含，注意不是单引号，在 Esc 键下边。
-   下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：

    | 运算符 | 说明                      | 举例                   |
    |-----|-------------------------|----------------------|
    | +   | 加法                      | expr $a + $b 结果为 30。 |
    | -   | 减法                      | expr $a - $b 结果为 -10。 |
    | \*  | 乘法                      | expr $a \* $b 结果为 200。 |
    | %   | 取余                      | expr $b % $a 结果为 0。 |
    | =   | 赋值                      | a=$b 将把变量 b 的值赋给 a。 |
    | ==  | 相等。用于比较两个数字，相同则返回 true。 | [ $a == $b ] 返回 false。 |
    | !=  | 不相等。用于比较两个数字，不相同则返回 true。 | [ $a != $b ] 返回 true。 |
    |     |                           |                        |

    注意：条件表达式要放在方括号之间，并且要有空格，例如: [$a==$b] 是错误的，必须写成 [ $a == $b ]。


### 举例 {#举例}

{{< highlight sh >}}

#!/bin/bash
a=4
b=20
#加法运算
each expr $a + $b
#减法运算
echo expr $a - $b
#乘法运算，注意*号前面需要反斜杠
echo expr $a \* $b
#除法运算
echo $a / $b
此外，还可以通过(())、$(())、$[]进行算术运算。
 ((a++))
echo "a = $a"
c=$((a + b))
d=$[a + b]
echo "c = $c"
echo "d = $d"
{{< /highlight >}}


## 流程控制 {#流程控制}


### if {#if}

if 语句语法格式：

{{< highlight sh >}}

if condition; then
    command1
    command2
    ...
    commandN
fi
{{< /highlight >}}

demo:

{{< highlight sh >}}

[root@hadoop01 export]# cat if_test.sh
#!/bin/bash
a=20
if [ $a -gt 10 ]; then
        echo "a 大于 10"
fi
{{< /highlight >}}

末尾的 fi 就是 if 倒过来拼写，后面还会遇到类似的。


### if else {#if-else}

语法格式

{{< highlight sh >}}

if condition; then
    command1
    command2
    ...
    commandN
else
    command
fi
{{< /highlight >}}


### if else-if else {#if-else-if-else}

语法格式：

{{< highlight sh >}}

if condition1; then
    command1
elif condition2; then
    command2
else
    commandN
fi
{{< /highlight >}}

以下实例判断两个变量是否相等：

**关系运算符**

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

下表列出了常用的关系运算符，假设变量a为10，变量b为20：

| 运算符 | 说明                          | 英文                     | 举例                    |
|-----|-----------------------------|------------------------|-----------------------|
| -eq | 检测两个数是否相等，相等返回 true。 | equal                    | [ $a -eq $b ] 返回 false。 |
| -ne | 检测两个数是否不相等，不相等返回 true。 | not equal                | [ $a -ne $b ] 返回 true。 |
| -gt | 检测左边的数是否大于右边的，如果是，则返回 true。 | greater than             | [ $a -gt $b ] 返回 false。 |
| -lt | 检测左边的数是否小于右边的，如果是，则返回 true。 | less than                | [ $a -lt $b ] 返回 true。 |
| -ge | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | Greater than or equal to | [ $a -ge $b ] 返回 false。 |
| -le | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | Less than or equal to    | [ $a -le $b ] 返回 true。 |

案例：

{{< highlight sh >}}

#!/bin/bash

a=20
b=10

# 需求1: 判断 a 是否 100
if [ $a > 100 ]; then
        echo "$a 大于 100"
fi


# 需求2: 判断 a 是否等于 b
if [ $a -eq $b ]; then
        echo "$a 等于 $b"
else
        echo "$a 不等于 $b"
fi

# 需求3: 判断 a 与 b 比较
if [ $a -lt $b ]; then
        echo "$a 小于 $b"
elif [ $a -eq $b ]; then
        echo "$a 等于 $b"
else
        echo "$a 大于 $b"
fi


# 需求4: 判断 (a + 10) 和 (b * b) 比较大小
if test $[ a + 10 ] -gt $[ b * b ]; then
        echo "(a+10) 大于 (b * b)"
else
        echo "(a+10) 小于或等于 (b*b)"
fi
{{< /highlight >}}


## for循环 {#for循环}

格式：

{{< highlight sh >}}
for variable in (list); do
    command
    command
    ...
done
{{< /highlight >}}

示例：

{{< highlight sh >}}
# 需求1: 遍历 1~5
for i in 1 2 3 4 5; do
        echo $i;
done
# 需求2: 遍历 1~100
for i in {1..100}; do
        echo $i
done
# 需求3: 遍历 1~100之间的奇数
for i in {1..100..2}; do
        echo $i
done
# 需求4: 遍历 根目录 下的内容
for f in `ls /`; do
        echo $f
done
{{< /highlight >}}


## while语句 {#while语句}

while 循环用于不断执行一系列命令，也用于从输入文件中读取数据；命令通常为测试条件。其格式为：

{{< highlight sh >}}
while condition; do
    command
done
{{< /highlight >}}

需求: 计算 1~100 的和:

{{< highlight sh >}}
#!/bin/bash

sum=0
i=1
while [ $i -le 100 ]; do
        sum=$[ sum + i]
        i=$[ i + 1 ]
done

echo $sum
{{< /highlight >}}

执行脚本输出：5050

使用中使用了 Bash let 命令，它用于执行一个或多个表达式，变量计算中不需要加上 $ 来表示变量，具体可查阅：Bash let 命令：<http://www.runoob.com/linux/linux-comm-let.html>。


## 无限循环 {#无限循环}

无限循环语法格式：

{{< highlight sh >}}
while true; do
    command
done
#需求: 每隔1秒 打印一次当前时间
while true; do
      date
      sleep 1
done
{{< /highlight >}}


## case(swith) {#case--swith}

Shell case 语句为多选择语句。可以用 case 语句匹配一个值与一个模式，如果匹配成功，执行相匹配的命令。case 语句格式如下：

{{< highlight sh >}}
case 值 in

 模式1)
     command1
     command2
     ...
     commandN
     ;;
 模式2）
        command1
     command2
     ...
     commandN
     ;;
esac
{{< /highlight >}}

case 工作方式如上所示。取值后面必须为单词 in，每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 ;;。

取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号 \* 捕获该值，再执行后面的命令。

下面的脚本提示输入 1 到 4，与每一种模式进行匹配：

{{< highlight sh >}}
echo '输入 1 到 4 之间的数字:'

read aNum

case $aNum in
    1)  echo '你选择了 1'
    ;;

    2)  echo '你选择了 2'
    ;;

    3)  echo '你选择了 3'
    ;;

    4)  echo '你选择了 4'
    ;;

    *)  echo '你没有输入 1 到 4 之间的数字'
    ;;
esac
{{< /highlight >}}

输入不同的内容，会有不同的结果，例如：

{{< highlight sh >}}
输入 1 到 4 之间的数字:

你输入的数字为:

3

你选择了 3
{{< /highlight >}}


## 跳出循环 {#跳出循环}

在循环过程中，有时候需要在未达到循环结束条件时强制跳出循环，Shell 使用两个命令来实现该功能：break 和 continue。


## **break** 命令 {#break-命令}

break 命令允许跳出所有循环（终止执行后面的所有循环）。

{{< highlight sh >}}

# 需求: 执行死循环 每隔1秒打印当前时间, 执行10次停止
i=0;
while true; do
        sleep 1
        echo $i `date +"%Y-%m-%d %H:%M:%S"`

        i=$[ i + 1]
        if [ $i -eq 10 ]; then
                break
        fi
done
{{< /highlight >}}


## continue {#continue}

continue 命令与 break 命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环。

{{< highlight sh >}}
# 需求: 打印 1~30, 注意 跳过3的倍数

for i in {1..30}; do
        if test $[ i % 3 ] -eq 0; then
                continue
        fi
        echo $i
done
{{< /highlight >}}


## 函数使用 {#函数使用}


### 函数快速入门 {#函数快速入门}


#### 格式 {#格式}

{{< highlight sh >}}
[ function ] funname()
{
    action;
    [return int;]
}
{{< /highlight >}}

-   可以带 function fun() 定义，也可以直接 fun() 定义,不带任何参数。
-   参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。return 后跟数值 n(0-255)


### 快速入门 {#快速入门}

{{< highlight sh >}}
demoFun () {
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"
{{< /highlight >}}


### 传递参数给函数 {#传递参数给函数}

在 Shell 中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数...

带参数的函数示例：

{{< highlight sh >}}
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
{{< /highlight >}}

输出结果：

{{< highlight sh >}}
第一个参数为 1 !

第二个参数为 2 !

第十个参数为 10 !

第十个参数为 34 !

第十一个参数为 73 !

参数总数有 11 个!

作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !
{{< /highlight >}}

注意，\\(10 不能获取第十个参数，获取第十个参数需要\\){10}。当n&gt;=10时，需要使用${n}来获取参数。

另外，还有几个特殊字符用来处理参数：

| 参数处理 | 说明                             |
|------|--------------------------------|
| $#   | 传递到脚本的参数个数             |
| $\*  | 以一个单字符串显示所有向脚本传递的参数 |
| $$   | 脚本运行的当前进程 ID 号         |
| $!   | 后台运行的最后一个进程的 ID 号   |
| $@   | 与$\*相同，但是使用时加引号，并在引号中返回每个参数。 |
| $-   | 显示 Shell 使用的当前选项，与 set 命令功能相同。 |
| $?   | 显示最后命令的退出状态。0 表示没有错误，其他任何值表明有错误。 |


## 数组 {#数组}


### 定义数组 {#定义数组}

数组中可以存放多个值。Bash Shell 只支持一维数组（不支持多维数组），初始化时不需要定义数组大小（。

与大部分编程语言类似，数组元素的下标由 0 开始。

Shell 数组用括号来表示，元素用空格符号分割开，语法格式如下：

array_name=(value1 value2 value3 ... valuen)

**示例**

{{< highlight sh >}}
my_array=(A B "C" D)

我们也可以使用下标来定义数组:

array_name[0]=value0

array_name[1]=value1

array_name[2]=value2
{{< /highlight >}}


### 读取数组 {#读取数组}

读取数组元素值的一般格式是：

{{< highlight sh >}}
${array_name[index]}
{{< /highlight >}}

**示例**

{{< highlight sh >}}
#!/bin/bash

my_array=(A B "C" D)

echo "第一个元素为: ${my_array[0]}"

echo "第二个元素为: ${my_array[1]}"

echo "第三个元素为: ${my_array[2]}"

echo "第四个元素为: ${my_array[3]}"
{{< /highlight >}}

执行脚本，输出结果如下所示：

{{< highlight sh >}}
第一个元素为: A
第二个元素为: B
第三个元素为: C
第四个元素为: D
{{< /highlight >}}

**获取数组中的所有元素**

使用 @ 或 \* 可以获取数组中的所有的元素，例如：

{{< highlight sh >}}
my_array[0]=A
my_array[1]=B
my_array[2]=C
my_array[3]=D

echo "数组的元素为: ${my_array[*]}"
echo "数组的元素为: ${my_array[@]}"
{{< /highlight >}}

执行脚本结果如下所示：

{{< highlight sh >}}
数组的元素为: A B C D
数组的元素为: A B C D
{{< /highlight >}}

**获取数组长度**

获取数组长度方法与获取字符串长度的方法相同。例如：

{{< highlight sh >}}
my_array[0]=A
my_array[1]=B
my_array[2]=C
my_array[3]=D

echo "数组元素个数为: ${#my_array[*]}"
echo "数组元素个数为: ${#my_array[@]}"
{{< /highlight >}}

执行脚本，结果如下：

{{< highlight sh >}}
数组元素个数为: 4
数组元素个数为: 4
{{< /highlight >}}


### 遍历数组 {#遍历数组}


#### 方式1 {#方式1}

{{< highlight sh >}}
my_arr=(AA BB CC)
for var in ${my_arr[*]}
do
  echo $var
done
{{< /highlight >}}


#### 方式2 {#方式2}

{{< highlight sh >}}
my_arr=(AA BB CC)
my_arr_num=${#my_arr[*]}
for((i=0;i<my_arr_num;i++));
do
  echo "-----------------------------"
  echo ${my_arr[$i]}
done
{{< /highlight >}}


## 加载其它文件的变量 {#加载其它文件的变量}


### 简介 {#简介}

和其他语言一样，Shell 也可以包含外部脚本。这样可以很方便的封装一些公用的代码作为一个独立的文件。

Shell 文件包含的语法格式如下：

{{< highlight sh >}}
. filename   # 注意点号(.)和文件名中间有一空格

或

source filename
{{< /highlight >}}


### **练习** {#练习}

定义两个文件 test1.sh 和 test2.sh，在 test1 中定义一个变量arr=(java c++ shell),在 test2 中对arr进行循环打印输出。

-   第一步  vim test1.sh
    {{< highlight sh >}}
    #!/bin/bash
    my_arr=(AA BB CC)
    {{< /highlight >}}

-   第二步 vim test2.sh
    {{< highlight sh >}}
    #!/bin/bash
    source ./test1.sh  # 加载test1.sh 的文件内容
    for var in ${my_arr[*]}
    do
      echo $var
    done
    {{< /highlight >}}

-   第三步 执行 test2.sh
    {{< highlight sh >}}
    sh test2.sh
    {{< /highlight >}}


### 好处 {#好处}

1.  数据源 和 业务处理 分离
2.  复用代码扩展性更强
