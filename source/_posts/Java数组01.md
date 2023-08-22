---
title: Java数组01
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva4.sinaimg.cn/large/87c01ec7gy1fsnqqzns32j21kw0w01ao.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/c3936f01a70c3.jpg
tags:
  - Java
  - Java数组
abbrlink: 5a3281f7
date: 2022-08-25 22:41:17
authorAbout:
authorDesc:
keywords:
description:
---

### 数组的定义

- 数组是相同类型数据的有序集合
- 数组描述的是相同类型的若干个数据，按照一定先后次序排列组合而成。
- 其中，每一个数据称作一个数组元素，每个数组元素可以通过一个下标来访问它。

---

### 数组声明创建

- 首先必须声明数组变量，才能再程序中使用数组。语法如下：

```java
dataType[] arrayRefVar;       //首选方法
dataType arrayRefVar[];		  //效果相同，但不是首选方法
```

- Java语言使用new操作符来创建数组，语法如下：

```java
dataType [] arrayRefVar = new dataType[arraySize];
```

- 数组的元素是通过索引访问的，数组索引从0开始。
- 获取数组长度：arrays.length

```
package com.xiheya.Array;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 10:21
 * @Description
 */
public class Demo01 {
    public static void main(String[] args) {
        int[] nums;                     //1.声明一个数组
        nums = new int[10];             //2.给数组开辟空间，创建一个数组

        int[] numrs = new int[10];      //直接声明数组并创建

        

    }
}
```

---

### 数组的四个基本特点

- 其长度是确定的。数组一旦被创建，它的大小就是不可以改变的。
- 其元素必须是相同类型，不允许出现混合类型。
- 数组中的元素可以是任何数据类型，包括基本类型和引用类型。
- 数组变量属引用类型，数组也可以看成是对象，数组中每个元素相当于该对象的成员变量。
- 数组本身就是对象，Java中对象是在堆中的，因此 数组无论保存原始类型还是其他对象类型，**数组对象本身是在堆中的**

