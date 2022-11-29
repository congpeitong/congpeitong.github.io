+++
title = "Java关键字"
lastmod = 2022-11-29T14:41:53+08:00
tags = ["keywords"]
categories = ["Java"]
draft = true
author = "congpeitong"
+++

## 1. private {#1-dot-private}


### 什么是private关键字 {#什么是private关键字}

是一个修饰符，代表私有的意思，可以修饰成员变量和成员方法和构造方法


### private关键字的特点 {#private关键字的特点}

被private修饰的成员变量和成员方法只能在本类中使用，不能在其他类中使用


### 示例 {#示例}

{{< highlight java >}}
class Person {
    private String name;
    private int age;
    private String sex;

    public void setName(String n) {
        name = n;
    }

    public void setAge(int a) {
        age = a;
    }

    public void setSex(String s) {
        sex = s;
    }

    public String getName() {
        return name;
    }

    public int getAge() {
        return age;
    }

    public String getSex() {
        return sex;
    }
}
{{< /highlight >}}


## 2. this关键字 {#2-dot-this关键字}


### 什么是this关键字 {#什么是this关键字}

this代表对象的引用，哪个对象调用this所在的方法，this就代表哪个对象


### this关键字作用 {#this关键字作用}

1.  它可以解决局部变量隐藏成员变量的问题
2.  可以调用本类中的成员变量和成员方法和构造方法


### 示例 {#示例}

{{< highlight java >}}
class Person {
    private String name;
    private int age;
    private String sex;

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(int aage) {
        this.age = aget;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String getName() {
        return this.name;
    }

    public int getAge() {
        return this.age;
    }

    public String getSex() {
        return this.sex;
    }
}
{{< /highlight >}}


## 3. static {#3-dot-static}


### 什么是static {#什么是static}

static是一个修饰符，它可以修饰 **成员变量** 和 **成员方法** ，但是不能修饰 **构造方法**


### static 关键字特点 {#static-关键字特点}

1.  被static所修饰的成员是随着字节码文件对象的加载而加载，所以是优先于对象存在于内存中
2.  被static所修饰的成员被该类下所有的对象所共享
3.  被static所修饰的成员可以通过类名.直接调用
    {{< highlight java >}}
    类名.属性名;
    类名.方法名();
    {{< /highlight >}}


### static 关键字注意事项 {#static-关键字注意事项}

1.  静态方法中不能有this关键字
2.  静态方法中可以调用静态成员变量和静态成员方法，但是不可以调用非静态的成员变量和成员方法
3.  非静态方法中可以调用静态的成员变量和成员方法，也可以调用非静态的成员变量和成员方法


## 4. supper关键字 {#4-dot-supper关键字}


### supper关键字是什么 {#supper关键字是什么}

super是父类内存空间的标记，在用法上，我们可以当做父类对象的引用来使用，但是我们不能说super就是父类对象的引用


### supper 关键字的使用 {#supper-关键字的使用}

1.  调用构造方法：supper(参数)
2.  调用成员方法：supper.方法名(参数)
3.  调用成员变量：supper.变量名
