---
title: Java面向对象02
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva3.sinaimg.cn/large/87c01ec7gy1fsnqqm6x00j21kw0w0h09.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/f9a43fa4f09af.jpg
tags:
  - Java
  - Java面向对象
abbrlink: 55661a2d
date: 2022-08-25 22:41:45
authorAbout:
authorDesc:
keywords:
description:
---

## 构造器

- 类中的构造器也称为构造方法，是在进行创建对象的时候必须要调用的。并且构造器有以下两个特点：
  - 1.必须和类的名字相同
  - 2.必须没有返回类型，也不能写void

代码：

```java
package com.xiheya.oop.demo02;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 22:50
 * @Description
 */
//学生类
public class Student {
    //一个类即使什么都不写，也会存在一个构造方法
    // 显示的定义构造期。
//    属性：字段
    String name;
    int age;
//    方法
//    实例化初始值
//    1.使用new关键字，实质上是调用构造器
//    2.构造器一般用来初始化值 .
    public Student(){

    }
//    一旦定义了有参构造，无参构造就必须显示定义
    public Student(String name,int age){
        this.setName(name);
        this.setAge(age);
    }
    public void study(){
        System.out.println(this.name + " is study");
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }
}

/*
//一个项目应该只存在一个main方法
public class Application {
    public static void main(String[] args) {
//类是一个抽象的事物，我们在使用它时要先实例化
        Student student = new Student();
        student.study();
        Student Tom = new Student("Tom",3);
        Tom.study();
    }
}

    构造器：
        1.和类名相同
        2.没有返回值
    作用：
        1.new本质在调用构造器
        2.初始化对象的值
    注意点:
        1.定义有参构造后，如果想使用无参构造，无参构造一定要显式定义
    快捷键：Alt + Insert
 */
```

构造器：
        1.和类名相同
        2.没有返回值
    作用：
        1.new本质在调用构造器
        2.初始化对象的值
    注意点:
        1.定义有参构造后，如果想使用无参构造，无参构造一定要显式定义
    **快捷键：Alt + Insert**

## 创建对象内存分析

代码：

```java
package com.xiheya.oop.demo03;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 23:57
 * @Description
 */
public class Pet {
    String name;
    int age;
    public void shout(){
        System.out.println("wow  wow  wow");
    }
}


/*
public class Application {
    public static void main(String[] args) {
        Pet dog = new Pet();
        dog.name = "旺财";
        dog.age = 3;
        dog .shout();

        Pet cat = new Pet();
    }
}

 */
```

### 内存分析

1. 先将Application类中的main()方法以及常量池中的旺财，加载到方法区。
2. 将main()方法压入栈底
3. Pet dog = new Pet（实例化一个dog对象）时，将Pet类及其name、age属性和shout方法加载到方法区当中。
4. 实例化dog对象时，会在栈当中引用dog变量名并指向堆中正在初始化的Pet类，而堆中的Pet又会指向方法去中的Pet。
5. Pet cat= new Pet（实例化一个cat对象）时，将Pet类及其name、age属性和shout方法加载到方法区当中。
6. 实例化cat对象时，会在栈当中引用dog变量名并指向堆中正在初始化的Pet类，而堆中的Pet又会指向方法去中的Pet。

图解：

![图解](https://img30.360buyimg.com/pop/jfs/t1/84746/36/24207/99884/622cc702E4c383ae1/294d1996197be10e.png)

---

## 小结

1. 类与对象
   1. 类是一个模板---抽象；对象是一个具体的实例
2. 方法
   1. 方法的定义及调用
3. 对应的应用
   1. 引用类型：基本类型（8），对象是通过引用来操作的：栈---->堆（地址）
4. 属性：字段Field  成员变量
   1. 默认初始化：
      1. 数字 ： 0  0.0
      2. char： u0000
      3. boolean：false
      4. 引用：null
   2. 属性的定义
      1. 修饰符 + 属性类型 + 属性名 = 属性值
5. 对象的创建和使用
   1. 必须使用new 关键字创造对象，构造器Person xiheya = new Person();
   2. 对象的属性 xiheya.name
   3. 对象的方法 xiheya.sleep();
6. 类
   1. 静态的属性  属性
   2. 动态的行为  方法
