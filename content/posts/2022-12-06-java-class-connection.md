+++
title = "Java类之间的关系"
author = ["congpeitong"]
date = 2022-12-06T11:04:00+08:00
lastmod = 2022-12-06T11:04:38+08:00
categories = ["Java"]
draft = false
+++

## 依赖 {#依赖}

所谓依赖关系，A类中有1个或多个方法用类B做参数或局部变量，我们就叫A依赖于B,依赖关系最重要的特点是在A类加载的时候B类不会加载，只有在方法调用的时候才会加载B类，即为B类为局部变量

{{< highlight java >}}
public class A {
    public void function1(B b) {

    }

    public B function2() {

    }
}

public class B {

}
{{< /highlight >}}


## 关联 {#关联}

B 作为 A 类变量或成员变量存在，而不是局部变量存在，我们称之为A关联B,也可以循环依赖

{{< highlight java >}}
public class A {
    static final BBBBB b;
    @AutoWired
    private BBBBBBBBBB   bbb;
}
{{< /highlight >}}


## 组合 {#组合}

1.  在关联关系的基础上，比如B,C为A类的变量或者成员变量，并且组成了部分和整体的关系，且部分不可以独立于整体单独使用，称之为组合关系。
2.  比如对于跑步来说，四肢和躯干等来组成完整的身体来完成跑步这个动作，缺少哪一部分都不能完成跑步这一动作

<!--listend-->

{{< highlight java >}}
public void run() {
    Leg leg = new Leg();
    Arm arg = new Arm();
}
{{< /highlight >}}


## 聚合 {#聚合}

在关联关系的基础上，比如B,C为A的类变量或者成员变量，并且组成了部分和整体的关系，且部分可以独立于整体单独使用

{{< highlight java >}}
public class School{
    private Student student;
    private Teacher teacher;
}

public class aloneUse {
    public static void mian(String[] args) {
        Student student = new Student();
        Teacher teacher = new Teacher();
    }
}
{{< /highlight >}}


## 继承 {#继承}

extend关键字
