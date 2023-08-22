---
title: Java异常01
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva2.sinaimg.cn/large/87c01ec7gy1fsnqqyk36yj21kw0w0k97.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/0d7c154d6b634.jpg
tags:
  - Java
  - Java异常
abbrlink: 822ebc87
date: 2022-08-25 22:42:03
authorAbout:
authorDesc:
keywords:
description:
---

## 异常

### 定义

- 实际工作中，遇到的情况不可能是非常完美的。比如：你写的某个模块，用户输入不一定符合你的要求、你的程序要打开某个文件，这个文件可能不存在或者文件格式不对，你要读取数据库的数据，数据可能是空的等。我们的程序在跑着，内存或硬盘可能就满了。等等
- 软件程序再运行过程中，非常可能遇到刚刚提到的这些异常问题，我们叫异常，英文是：Exception，意思是例外。这些，例外情况，或者叫异常，怎么让我们写的程序做出合理的处理，而不至于程序崩溃？
- 异常指程序运行中出现的不期而至的各种状况，如：文件找不到、网络连接失败、非法参数等。
- 异常发生在程序运行期间，他影响了程序正常的程序执行流程

### 分类

#### 简单分类

需要掌握以下三种类型的异常

1. 检查性异常：最具代表的检查性异常时用户错误或问题引起的异常，这是程序羊无法预见的。例如要打开一个不存在的文件时，一个异常就发生了，这些异常在编译时不能被简单的忽略
2. 运行时异常：运行时异常是可能被程序员避免的异常。与检查性异常相反，运行时异常可以在编译时被忽略。
3. **错误ERROR**：错误不是异常，而是脱离程序员控制的问题，错误在代码中通常被忽略。例如，当栈溢出时，一个错误就发生了，他们在编译时也检查不到的。

### 异常体系结构

- Java把异常当作对象来处理，并定义一个基类java.lang,Throwable作为所有异常的超类
- 在Java API中已经定义了许多异常类，这些异常类分为两大类：**错误ERROR**和**异常Exception**

![异常体系结构](https://img30.360buyimg.com/pop/jfs/t1/185417/8/21799/155982/622e14c1E0fbc4c54/bbd56e3a7d9a2d3a.png)

#### ERROR

- Error类对象是由Java虚拟机生成并抛出，大多数错误与代码编写者所执行的操作无关。
- Java虚拟机运行时错误（Virtual MachineError），当JVM不再有继续执行操作所需的内存资源时将出现 **OutOfMemoryError**。这些异常发生时，Java虚拟机（JVM）一般会选择线程终止；
- 还有发生在虚拟机试图执行应用时，如类定义错误（NoClassDefFoundError）、链接错误（LinkageError）。这些错误时不可查的，因为它们在应用程序的控制和处理能力之外，而且绝大多数时程序运行时不允许出现的状况。

#### Exception

- 在Exception分支中有一个重要的子类RuntimeException（运行时异常）
  - ArrayIndexOutOfBoundsException（数组下标越界异常）
  - NullPointerException（空指针异常）
  - ArithmeticException（算数异常）
  - MissingResourceException（丢失资源）
  - ClassNotFoundException（找不到类）等异常
- 这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生；
- Error和Exception的区别：Error通常时灾难性的致命的错误，是程序无法控制和处理的，当出现这些异常时，Java虚拟机（JVM）一般会选择终止线程；Exception通常情况下时可以被程序处理的，并且在程序中应该尽可能地去处理这些异常。

