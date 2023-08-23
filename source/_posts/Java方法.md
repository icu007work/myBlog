---
title: Java方法
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva4.sinaimg.cn/large/87c01ec7gy1fsnqqhqfnzj21kw0w0ao2.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/9dcc989cf7d0a.jpg
ai: 
  - 本文主要介绍了：Java的方法是一组语句的集合，用于实现特定的功能。方法包含在类或对象中，可以在程序中创建并在其他位置引用。编写方法时，应该保持方法的原子性，即每个方法只实现一个功能，以方便后续扩展。方法的命名规则是首字母小写，后面使用驼峰命名规则。还介绍了方法的重载、命令行传参等等。
tags:
  - Java
  - Java方法
abbrlink: 7a37a823
date: 2022-08-25 22:41:09
authorAbout:
authorDesc:
keywords:
description:
---

## 方法

### 定义

- Java方法是语句的集合，它们在一起执行一个功能
  - 方法是解决一类问题的步骤的有序组合
  - 方法包含于类或对象当中
  - 方法在程序中被创建，在其他地方被引用
- 设计方法的原则
  - 方法的本意是功能块，就是实现某个功能的语句块的集合。我们设计方法的时候，最好保持方法的**原子性，就是一个方法只完成一个功能，这样利于我们后期的扩展**
- 方法的命名规则：**首字母小写，后面是驼峰命名规则。**

**设计一个方法：实现简单的两个数的加法：**

```java
package com.xiheya.Method;

/**
 * @Author {xiheya}
 * @Date: 2022/03/11/ 16:23
 * @Description
 */
public class Demo01 {
    public static void main(String[] args) {
        System.out.println(add(1,2));		 //方法的调用
    }
    public static int add(int a , int b){    //方法的定义
        return a+b;
    }
}

```

- Java的方法类似于其他语言的函数，是一段**用来完成特定功能的代码片段**，一般情况下，定义一个方法包含以下语句：
- **方法包含一个方法头和一个方法体**下面是一个方法的所有部分
  - **修饰符**：修饰符是可选的，它告诉编译器该如何调用该方法。定义了该方法的访问类型
  - **返回值类型**：方法可能会返回值，returnValueType是方法返回值的数据类型。有些方法执行所需的操作，但没有返回值。在这种情况下，returnValueType是关键字void。
  - **方法名**：是方法的实际名称。方法名和参数表共同构成方法签名。
  - **参数类型**： 参数像是一个占位符。当方法被调用时，传递值给参数。这个值被称为实参或变量。参数列表是指方法的参数类型、顺序和参数的个数。参数是可选的，方法可以不包含任何参数
    - 形式参数：在方法被调用时用于接收外界输入的数据。
    - 实参：调用方法时实际传给方法的数据。
  - **方法体**：方法体包含具体的语句，定义该方法功能

```java
修饰符 返回值类型 方法名(参数类型 参数名){
	……
	方法体
	……
	return 返回值;
}
```

**小tips**：值传递和应用传递 **Java为值传递**

- 值传递(pass by value)：在调用函数时，将实际参数复制一份传递到函数中，这样在函数中对参数进行修改，就不会影响到原来的实际参数；
- 引用传递(pass by reference):在调用函数时，将实际参数的地址直接传递到函数中。这样在函数中对参数进行的修改，就会影响到实际参数；

---

### 方法的重载

- 重载就是在一个类中，**有相同的函数名称**，但形参不同的函数。
- 方法重载的规则
  - 方法名称必须相同
  - 参数列表必须不同（个数不同、或类型不同、参数排列顺序不同等）
  - 方法的返回类型可以相同也可以不相同
  - **仅仅返回类型不同不足以成为方法的重载。**
- 实现理论：
  - 方法名称相同时，编译器会根据调用方法的参数个数，参数类型等去逐个匹配，以选择对应的方法，如果匹配失败，则编译器报错。

**实现max()函数的重载**

```java
package com.xiheya.Method;

/**
 * @Author {xiheya}
 * @Date: 2022/03/11/ 21:56
 * @Description
 */
public class Demo02 {
    public static void main(String[] args) {
        int max = max(10,20);
        System.out.println(max);
        double dmax = max(20.0,30.0);
        System.out.println(dmax);
    }
    public static int max(int a, int b){
        return a > b ? a : b;                             //三目运算符：进行判断，a大于b吗？如果大于返回a，否则返回b。
    }
    public static double max(double a, double b){         // 方法重载，方法名一样均为max，但是返回值与参数类型不一样，所以可以构成重载
        return a > b ? a : b;                             //三目运算符：进行判断，a大于b吗？如果大于返回a，否则返回b。
    }
}

```

运行结果：

![运行结果](https://img30.360buyimg.com/pop/jfs/t1/144758/6/25718/101275/622b5709Ebd1e27b5/75a5ef236a3b61b4.png)

---

### 命令行传参

- 有时候你希望运行一个程序时候再给他传递消息，这要靠传递命令行参数来给main()函数实现
- 通过在运行时使用命令行给main()函数来实现。

### 可变参数

- JDK 1.5开始，Java支持传递同类型的可变参数给一个方法。
- 在方法声明中，在指定参数类型后加一个省略号（……）
- 一个方法中只能指定一个可变参数，它必须是方法的最后一个参数。任何普通的参数必须在它之前声明

**设计一个程序计算出可变字长数组的最大值**

代码：

```java
package com.xiheya.Method;

/**
 * @Author {xiheya}
 * @Date: 2022/03/11/ 22:16
 * @Description
 */
public class Demo04 {       //可变参数
    public static void main(String[] args) {
        double [] test1 = {10,20,30,40};
        Demo04 demo04 = new Demo04();
        demo04.printMax(10.0,20.0,30.0);
        demo04.printMax(test1);
    }

    public static void printMax(double... i){
        if (i.length == 0){
            System.out.println("error!!the length is 0");
            return;
        }
        double result = i[0];
        for (int j = 0; j < i.length; j++) {
            if (i[j] > result){
                result = i[j];
            }
        }
        System.out.println("the max number is " + result);
    }
}

```

运行结果：

![可变参数](https://img30.360buyimg.com/pop/jfs/t1/62091/27/17177/112808/622b5c9aE22522567/07f6d7aff5cd42a3.png)

---

### 递归

- A方法可以调用B方法，这是很常见的。
- 而递归就是：**A方法调用A方法，自己调用自己**
- 利用递归可以用简单的程序解决一些复杂的问题。它通常把一个大型复杂的问题层层转化为一个与原问题相似的规模较小的问题来求解，递归策略只需少量的程序就可以描述出解题过程所需要的多次重复计算，大大减少了程序的代码量。递归的能力在于用有限的语句来定义对象的无限集合
- 递归结构包括两个部分：
  - **递归头：什么时候不调用自身方法。如果没有头，会陷入死循环**
  - **递归体：什么时候需要调用自身方法。**

**设计一个程序计算递归**

代码：

```java
package com.xiheya.Method;

/**
 * @Author {xiheya}
 * @Date: 2022/03/11/ 22:37
 * @Description
 */
public class Demo03 {
    public static void main(String[] args) {
        System.out.println(function(3));        //打印出6的阶乘
    }
    public static int function(int n){              //计算阶乘的方法
        if (n == 1){
            return 1;
        }else {
            return n*function(n-1);
        }
    }
}

```

![递归](https://img30.360buyimg.com/pop/jfs/t1/187601/21/21695/79046/622b5f66E5bd55680/8cf9080773c4a685.png)

---

