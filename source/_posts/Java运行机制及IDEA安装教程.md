---
title: Java运行机制及IDEA安装教程
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva4.sinaimg.cn/large/87c01ec7gy1fsnqqm5vu5j21kw0w0aon.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/094720ae14ec2.jpg
tags: Java
abbrlink: 6b48db66
date: 2022-08-25 22:32:04
authorAbout:
authorDesc:
keywords:
description:
---

## Java运行机制及IDEA安装教程

### Hello World 

1. 新建一个Java文件
   - 文件后缀名为.java
   - Hello.java
2. 编写代码

```java	
public class Hello{
	public static void main(String[] args){
		System.out.print("hello world");
	}
}
```

3. 编译java文件  cmd：javac Hello.java。会生成一个class文件

4. 运行class文件 cmd：java Hello

5. **可能会遇到的问题**

   1. Java是大小写敏感的语言，每个单词的大小写不能出现问题
   2. 尽量使用中文
   3. 文件名与类名必须保持一致

   ---

### Java运行机制

   - 编译型：先整个程序通过编译器先编译完成后再运行(操作系统、c/c++)
   - 解释型：执行什么就读取什么(网页、Javascript)
   - Java程序运行机制

![java_run](https://img30.360buyimg.com/pop/jfs/t1/216026/11/15424/19299/62373668E88ab9271/8335afd2a83f0fc5.png)

---

## IDEA安装教程

### 1. 打开jetbrains官网下载IDEA

![](https://img30.360buyimg.com/pop/jfs/t1/120004/31/23287/83690/6237369aEfcaf86ee/1341013e53c23cf5.png)

- 下载地址：https://www.jetbrains.com/zh-cn/idea/download/#section=windows

- 点击[此处](https://www.jetbrains.com/zh-cn/idea/download/#section=windows)进入IDEA下载界面

![](https://img30.360buyimg.com/pop/jfs/t1/114487/39/23094/72904/623736b3E95bd557c/6e25a3fa36db191e.png)

### 2. 打开安装包之后，~~无脑next。~~

![](https://img30.360buyimg.com/pop/jfs/t1/198629/33/20709/45058/623736ceEd59fdd11/a717d392d82f8ad0.png)

### 3. **安装目录不建议放在C盘**

![](https://img30.360buyimg.com/pop/jfs/t1/217105/6/15162/49712/623736e8Ea3906364/843e0641342b9718.png)

---

## 创建一个Java程序

1. 双击打开IDEA快捷方式，首先new一个Project

   ![new_project](https://img30.360buyimg.com/pop/jfs/t1/121170/18/24773/77888/623737f3Ecdc108e4/ebb5390c0cf9950d.png)

   

2. 然后选择Java语言导入JDK环境

   ![JDK](https://img30.360buyimg.com/pop/jfs/t1/92215/16/24676/66704/62373815E42130a06/66f052194fdcc4b6.png)

3. 鼠标右击src文件夹 new一个 java class

   ![new class](https://img30.360buyimg.com/pop/jfs/t1/181017/16/22231/364250/6237383cEd4ba2e02/58d0e87d267ffbca.png)

4. 编写hello world

```java	
public class Hello{
	public static void main(String[] args){
		System.out.print("hello world");
	}
}
```

![HELLO WORLD](https://img30.360buyimg.com/pop/jfs/t1/179802/26/21860/77145/6237385dE25eb7d73/e0a4a0a96b8c321a.png)



> 

---

