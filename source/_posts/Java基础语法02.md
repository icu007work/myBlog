---
title: Java基础语法02
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva3.sinaimg.cn/large/87c01ec7gy1fsnqqxqwj6j21kw0w01a0.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/00c70a2ab8b2b.jpg
ai: 
  - 文章讨论了Java中的类型转换和变量声明。在Java中，由于其强类型语言的特性，类型转换在进行某些运算时是必需的。文中介绍了低类型转换为高类型的层次关系，并指出小数具有比整数更高的优先级。不同类型的数据在进行运算前需要先转换为同一类型。强制类型转换通过(类型)变量名的形式实现从高到低的转换，而自动类型转换则是从低到高的转换。文章还提到了几个注意点，首先是布尔值不能进行转换，其次是不允许将对象类型转换为不相关的类型。在将高容量类型转换为低容量类型时，需要进行强制转换，但可能会出现内存溢出或精度问题。另外，文章也探讨了变量的概念和命名规范。变量是可变化的量，在Java中必须声明其类型。每个变量都有类型，可以是基本类型或引用类型。变量的命名应当遵循合法标识符的规则，类成员变量和局部变量采用驼峰原则，常量使用大写字母和下划线，类名使用驼峰原则。总之，文章详细介绍了Java中的类型转换和变量声明的规则和注意事项，为读者提供了相关的知识。
tags:
  - Java
  - Java基础语法
abbrlink: 4424cd46
date: 2022-08-25 22:36:16
authorAbout:
authorDesc:
keywords:
description:
---

## 类型转换

- 由于Java是强类型语言，所以要进行有些运算的时候，需要用到类型转换

```java
//低----------------------------------------->高
byte,short,char --->  int ---> long ---> double;
//小数的优先级一定大于整数

```



- 运算中，不同类型的数据先转换为同一类型，然后再进行运算
- 强制类型转换:              (类型)变量名;         高--->低
- 自动类型转换:                                              低--->高
- 注意点：
  * 不能对布尔值进行转换
  * 不能打对象类型转换为不相干的类型
    * 把高容量类型转换到低容量类型时，需要强制转换
          * 2.转换可能会出现内存溢出或精度问题

```java
public class Demo03 {
    public static void main(String[] args) {
        int i = 128 ;
        byte b = (byte) i;          //内存溢出
        //强制转换 (类型)变量名;         高--->低
        //自动转换                     低--->高
        System.out.println(i);
        System.out.println(b);
        System.out.println("=========================");
        /*
        * 注意点：
        * 1.不能对布尔值进行转换
        * 2.不能打对象类型转换为不相干的类型
        * 3.把高容量类型转换到低容量类型时，需要强制转换
        * 4.转换可能会出现内存溢出或精度问题
        * */

        System.out.println((int) 30.7);
        System.out.println((int) 43.33f);
        System.out.println("=========================");
        char c = 'a';
        int d = c + 1;
        System.out.println(d);
        System.out.println((char) d);
    }
}

```

## 变量

- 变量：即为可以变化的量
- Java是一种强类型语言，每个变量都必须声明其类型。
- Java变量时程序中最基本的存储单元，其要素包括变量名，变量类型和作用域

```java
type varName [=value][{,varName[=value]}];
//数据类型 变量名 = 值；可以用逗号隔开来声明多个同类型变量
int a,b,c = 10;
```

- **注意事项**
  - 每个变量都有类型，类型可以是基本类型，也可以是引用类型
  - 变量名必须是合法的标识符
  - 变量声明是一条完整的语句，因此每一个声明都必须以分号结束。
- 变量的命名规范
  - 所有变量、方法、类名：**见名知意**
  - 类成员变量：首字母小写和驼峰原则：monthSalary
  - 局部变量：首字母小写和驼峰规则
  - 常量：大写字母和下划线：MAX_VALUE
  - 类名：首字母大写和驼峰原则：Man、GoodMan
  - 方法名：首字母小写和驼峰原则：run(),runRun();
