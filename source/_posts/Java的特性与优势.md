---
title: Java的特性与优势
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva4.sinaimg.cn/large/87c01ec7gy1fsnqqpyov8j21kw0w0h0t.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/a16739c3b0834.jpg
ai: 
  - 本文主要介绍了Java的特性和优势：Java具有许多特性和优势，并且涵盖了简单性、面向对象、可移植性、高性能等方面。它有三个主要版本，即JavaSE、JavaME和JavaEE。此外，JDK、JRE和JVM是与Java开发相关的重要概念。安装Java开发环境包括卸载、安装和配置JDK的过程。
tags:
  - Java
  - Java特性
abbrlink: ef009d9b
date: 2022-08-25 22:30:29
authorAbout:
authorDesc:
keywords:
description:
---

## Java的特性和优势

### Java特性

- 简单性
- 面向对象
- 可移植性
- 高性能
- 分布式
- 动态性
- 多线程
- 安全性
- 健壮性

---

### Java三大版本

- **JavaSE ： 标准版（桌面程序，控制台开发……**
- ~~**JavaME： 嵌入式开发（手机、小家电）**~~
- **JavaEE： 企业级开发（web端，服务器开发）**

---

### JDK、JRE、JVM

> JDK： Java Development Kit
>
> JRE： Java Runtime Environment
>
> JVM： Java Virtual Machine

![三者区别与联系](https://img30.360buyimg.com/pop/jfs/t1/212587/24/15283/47108/623735e3E54606284/e9980567ac36697e.png)



![架构图](https://img30.360buyimg.com/pop/jfs/t1/212860/32/15267/43018/6237360eEb30a738c/d04f6b8f15c13028.png)

---

### Java开发环境安装

#### 卸载JDK

1. 删除Java安装目录
2. 删除环境变量中的JAVA_HOME
3. 删除环境变量中的Path下关于Java的目录

#### 安装JDK

1. 打开官网找到电脑对应的版本，并下载到本地。[JDk下载页面](https://www.oracle.com/java/technologies/javase/javase8-archive-downloads.html)
2. 双击安装包进行安装
3. 记住安装的路径
4. **配置环境变量**
   1. 我的电脑---> 右键 ---> 属性
   2. 高级系统设置 ---> 环境变量 ---> 新建系统变量
   3. 配置变量名： JAVA_HOME  值：Java的安装目录
   4. 配置Path：鼠标右击Path--- 值为%JAVA_HOME%/lib
5. 检查JDK是否安装成功
   1. Win + r ：cmd
   2. 输入java -version
   3. 若打印出Java版本信息，则安装成功。
