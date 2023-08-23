---
title: Java数组02
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva4.sinaimg.cn/large/87c01ec7gy1fsnqqzns32j21kw0w01ao.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/c3936f01a70c3.jpg
ai: 
  - 本文介绍了Java中的数组以及其使用方法。首先讨论了数组的定义，指出它是相同类型数据的有序集合，并通过下标访问元素。在声明和创建数组时，可以使用[]或arrayRefVar[]语法，并使用new操作符在堆中创建数组空间和赋值操作。接着介绍了三种数组初始化方式：静态初始化、动态初始化以及数组的默认初始化。数组边界相关内容指出合法索引范围为0到数组长度减1，超出范围会导致ArrayIndexOutOfBoundsException异常。对于数组的使用，文章提供了简单for循环和For-Each循环两种迭代方式，并展示了数组作为方法入参和返回值的用法。最后，介绍了多维数组的概念，并给出了二维数组的声明和遍历示例。文章配图展示了Java内存分析和数组的声明与创建过程。
tags:
  - Java
  - Java数组
abbrlink: c33bd04d
date: 2022-08-25 22:41:21
authorAbout:
authorDesc:
keywords:
description:
---

## 内存分析

- Java内存分析

![内存分析](https://img30.360buyimg.com/pop/jfs/t1/89824/16/24315/269122/622c0669Eba17f2c4/761d898097ac2650.png)

**数组的声明在栈当中，创建空间及赋值操作在堆中。**

![声明与创建](https://img30.360buyimg.com/pop/jfs/t1/209995/30/19535/37747/622c0d89E5b6c096c/06b455084f8a6f4b.png)

## 三种初始化

### 静态初始化

```java
int[] a = {1,2,3};
Man[] mans = {new Man(1,1),new Man(2,2)};
```

### 动态初始化

```java
int[] a = new int[5];
a[0] = 1;
a[1] = 2;
……
```

### 数组的默认初始化

- 数组是引用类型，他的元素相当于类的实例变量，因此数组一经分配空间，其中的每个元素也被按照实例变量同样的方式被隐式初始化。

代码：

```java
package com.xiheya.Array;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 10:45
 * @Description
 */
public class ArrayDemo02 {
    public static void main(String[] args) {
        //静态初始化
        int[] a = {1,2,3,4,5};
        System.out.println(a[0]);
        //动态初始化,包含默认初始化
        int[] b = new int[2];
        b[0] = 10;
        System.out.println(b[0]);
        System.out.println(b[1]);

    }
}

```

运行截图：

![初始化](https://img30.360buyimg.com/pop/jfs/t1/217840/37/14582/106438/622c0a8cE3a4d7f15/ba713efe44baedf2.png)

---

### 数组边界

- 下标的合法区间：[0,length-1],如果越界就会报错

```java
public static void main(String[] args){
	int[] a = new int[2];
	System.out.println(a[2]);
}
```

- **ArrayIndexOutOfBoundsException:数组下标越界异常**
- 小结
  - 数组是相同数据类型（数据类型可以为任意类型）的有序集合
  - 数组也是对象，数组元素相当于对象的成员变量
  - 数组长的确定的，不可变的。如果越界，则报：ArrayIndexOutOfBounds

---

## 数组的使用

### 简单for循环

```java
package com.xiheya.Array;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 11:12
 * @Description
 */
public class ArrayDemo03 {
    public static void main(String[] args) {
        int[] a = {1,2,3,4,5};
        for (int i = 0; i < a.length; i++) {
            System.out.println(a[i]);
        }
        
    }
}
```

---

### For-Each循环

```java
package com.xiheya.Array;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 11:12
 * @Description
 */
public class ArrayDemo03 {
    public static void main(String[] args) {
        int[] a = {1,2,3,4,5};
        for (int i : a) {
            System.out.println(i);
        }
    }
}

```

---

### 数组作方法入参

```java
package com.xiheya.Array;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 11:12
 * @Description
 */
public class ArrayDemo03 {
    public static void main(String[] args) {
        int[] a = {1,2,3,4,5};
//        for (int i = 0; i < a.length; i++) {
//            System.out.println(a[i]);
//        }
//        for (int i : a) {
//            System.out.println(i);
//        }
        printArray(a);
        int[] result = reverseArray(a);
        System.out.println("\n=================");
        printArray(result);
    }
    public static void printArray(int[] a){         //将数组作为参数传入方法中，然后打印数组
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i] + "  ");
        }
    }

    public static int[] reverseArray(int[] a){      //将数组作为方法返回值，反转数组后返回结果数组。
        int[] result = new int[a.length];
        for (int i = 0,j = result.length-1 ; i < a.length; i++,j--) {
            result[i] = a[j];
        }
        return result;
    }
}
```

---

### 数组作返回值

```java
package com.xiheya.Array;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 11:12
 * @Description
 */
public class ArrayDemo03 {
    public static void main(String[] args) {
        int[] a = {1,2,3,4,5};
//        for (int i = 0; i < a.length; i++) {
//            System.out.println(a[i]);
//        }
//        for (int i : a) {
//            System.out.println(i);
//        }
        printArray(a);
        int[] result = reverseArray(a);
        System.out.println("\n=================");
        printArray(result);
    }
    public static void printArray(int[] a){         //将数组作为参数传入方法中，然后打印数组
        for (int i = 0; i < a.length; i++) {
            System.out.print(a[i] + "  ");
        }
    }

    public static int[] reverseArray(int[] a){      //将数组作为方法返回值，反转数组后返回结果数组。
        int[] result = new int[a.length];
        for (int i = 0,j = result.length-1 ; i < a.length; i++,j--) {
            result[i] = a[j];
        }
        return result;
    }
}

```

---

运行结果：

![数组操作](https://img30.360buyimg.com/pop/jfs/t1/133563/2/26013/113070/622c14c1E4a86293b/ac9c5bc577d6a7d5.png)

---

## 多维数组

- 多维数组可以看成是数组的数组~~（套娃）~~，比如二维数组就是一个特殊的一维数组，其每一个元素都是一个一维数组。
- 二维数组

语法：

```java
int a[][] = new int[2][5]  //声明并创建一个两行五列的数组
```

- 解析：二维数组a可以看一个两行五列的数组

代码示例：

```java
package com.xiheya.Array;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 11:43
 * @Description
 */
public class ArrayDemo04 {
    public static void main(String[] args) {
        /**
         * array:
         * 1 2
         * 3 4
         * 5 6
         * 7 8
         */
        int[][] array = {{1,2}, {3,4},{5,6},{7,8}};
        for (int i = 0; i < array.length; i++) {
            System.out.println("\nprint  array[" + i + "],nums");
            for (int j = 0; j < array[i].length; j++) {
                System.out.print(array[i][j]+"  ");
            }
        }
    }
}

```

运行结果：

![二维数组](https://img30.360buyimg.com/pop/jfs/t1/109101/32/23645/105709/622c184bEde50375c/38f9c69b5b129b8c.png)

---
