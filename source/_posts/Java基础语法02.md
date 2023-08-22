---
title: Java基础语法02
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva3.sinaimg.cn/large/87c01ec7gy1fsnqqxqwj6j21kw0w01a0.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/00c70a2ab8b2b.jpg
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
