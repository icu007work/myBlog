---
title: Java面向对象01
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
abbrlink: cc6f4b97
date: 2022-08-25 22:41:42
authorAbout:
authorDesc:
keywords:
description:
---

## 面向过程& 面向对象

### 面向过程思想--->自上而下

面向对象就是分析出解决问题所需要的步骤，然后用函数把这些步骤一步一步实现，使用的时候一个一个依次调用就可以了。

- 步骤清晰简单，第一步做什么，第二步做什么……
- 面向过程适合处理一些较为简单的问题

### 面向对象思想--->自下而上

**面向对象**就是根据类创建对象，每个对象都有自己的属性和方法，而这些属性和方法都是围绕对象服务的，你会发现用到的属性和方法都是对象。

- 物以类聚，**分类**的思维模式，思考问题首先会解决问题需要哪些分类，然后对这些分类进行单独思考。最后，才对某个分类下的细节进行面向过程的思索。
- 面向对象适合处理复杂的问题，适合处理需要多人协作的问题。

**对于描述复杂的事物，为了从宏观上把握，从整体上合理分析，我们需要使用面向对象的思路来分析整个系统。但是，具体到微观操作，仍然需要面向过程的思路去处理。**

### 什么是面向对象

- 面向对象编程（Object_Oriented Programming， OOP）
- 面向对象编程的本质就是：**以类的方式组织代码，以对象的形式（封装）数据**
- 抽象   --->将有共同特征的物体抽象成一个类，比如泰迪、哈士奇、金毛，他们都有个共同的特征，**那就是他们都是狗**。
- 三大特征
  - **封装**   
  - **继承**
  - **多态**
- 从认识论的角度考虑：先有对象后有类。对象，是具体的事物。类，是抽象的，是对对象的抽象
- 从代码运行的角度考虑，是先有类后有对象。类是对象的模板。

## 回顾方法及加深

### 方法的定义

- 修饰符
- 返回类型
- **break：（跳出switch，结束循环） 和 return（结束方法，返回一个结果）的区别**
- 方法名：注意规范就可以（首字母小写驼峰法），见名知意
- 参数列表：（参数类型，参数名）……
- 异常抛出：

### 方法的调用:递归

- 静态方法
- 非静态方法

```java
package com.xiheya.oop;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 22:30
 * @Description
 */
public class Demo02 {
    public static void main(String[] args) {
        Demo02 demo02 = new Demo02();
        int sum = demo02.add(1, 2);            //实例化对象demo02后，才可以调用非静态方法add
        System.out.println(sum);
        System.out.println(add(1, 2, 3));   //而静态方法add则可以直接调用
    }
    public static int add(int a,int b, int c){      //静态方法，main方法中可以直接调用。
        return a+b+c;
    }
    public int add(int a,int b){                    //非静态方法，调用的话需要实例化对象后才能调用。
        return a+b;
    }
}

```

---

- 形参和实参
- 值传递和引用传递

代码：

```java
package com.xiheya.oop;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 21:52
 * @Description
 */
// 值传递和引用传递
public class Demo01 {
    public static void main(String[] args) {
        int a = 1;
        System.out.println(a);
        change(a);                          //调用change方法时，传给chang的只是a的值，change方法中对a的一系列操作均不会对main中的a产生影响
        System.out.println(a);              //可以看到尽管在change方法中对a进行了赋值操作，但仍然没有改变a的值。这就是Java的值传递。
        Person person = new Person();       //实例化一个类，new一个Person类。
        System.out.println(person.name);
        changeName(person);                 //由于传递给changeName方法的是一个类对象，方法对person.name 的修改是对Person类中 name的修改，所以是一定可以修改成功的，
        System.out.println(person.name);    //这就是引用传递（实质上还是值传递）
    }
    public static void change(int a){
        a = 10;
    }
    public static void changeName(Person person){
        //person是一个对象，指向的是Person这个类，这是一个具体的人，可以改变属性
        person.name = "xiheya";
    }
}

class Person{
    String name;

}

```

运行结果：

![值传递与引用传递](https://img30.360buyimg.com/pop/jfs/t1/105338/38/25767/135240/622cb020Eeaee7461/8344ccb832a825cb.png)

---

- this关键字

## 类与对象的关系

- **类是一种抽象的数据类型，它是对某一类事物整体描述/定义，但是并不能代表某一个具体的事物。**
  - 动物、植物、手机、电脑
  - Person类、Pet类、Car类等，这些类都是用来描述/定义某一类具体的事物应该具备的特点和行为
- **对象是抽象概念的具体实例**
  - eg：张三就是人的一个具体实例，而张三家的旺财就是狗的一个具体实例
  - 能够体现出特点，展现出功能的是具体的实例，而不是一个抽象的概念。

## 创建和初始化对象

- **使用new关键字创建对象**
- 使用new关键字创建的时候，除了分配内存空间之外，还会给创建好的对象进行默认的初始化以及对类中构造器的调用。
- 类中的构造器也称为构造方法，是在进行创建对象的时候必须要调用的。并且构造器有以下两个特点：
  - 1.必须和类的名字相同
  - 2.必须没有返回类型，也不能写void
- **构造器方法必须掌握**

