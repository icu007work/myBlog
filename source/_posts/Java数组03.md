---
title: Java数组03
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva4.sinaimg.cn/large/87c01ec7gy1fsnqqzns32j21kw0w01ao.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/c3936f01a70c3.jpg
ai: 
  - 本文介绍了Array类：在Java中，有一个名为Arrays的工具类，它提供了一些对数组进行操作的静态方法。可以通过该类来对数组进行赋值、排序、比较和查找等操作。
tags:
  - Java
  - Java数组
abbrlink: b43ce0db
date: 2022-08-25 22:41:24
authorAbout:
authorDesc:
keywords:
description:
---

## Arrays类

- 数组的工具类java.util.Arrays

- 由于数组对象本身并没有什么方法可以供我们调用，但API中提供了一个工具类Arrays供我们使用，从而可以对数据进行一些基本操作。

- **查看JDK帮助文档**

- Arrays类中的方法都是static修饰的静态方法，在使用的时候可以直接使用类名进行调用，而”不用“使用对象来调用**（注意：是”不用”而不是“不能”）**

- 具有以下常用功能：

  - 给数组赋值：通过fill方法。
  - 对数组排序：通过sort方法，按升序
  - 比较数组：通过equals方法比较数组中元素值是否相等
  - 查找数组元素：通过binarySearch方法能对排列好的数组进行二分查找法操作

  代码实现：

  ```java
  package com.xiheya.Array;
  
  import java.util.Arrays;
  
  /**
   * @Author {xiheya}
   * @Date: 2022/03/12/ 16:19
   * @Description
   */
  public class ArrayDemo05 {
      public static void main(String[] args) {
          int[] a = {1,2,3,4,65,98,54,21,0};
          System.out.println(Arrays.toString(a));         //Arrays 里的toString方法
          Arrays.sort(a);                                 //sort，将a中的数据从小到大排列
          System.out.println(Arrays.toString(a));         
      }
  }
  
  ```

  运行结果：

  ![方法调用](https://img30.360buyimg.com/pop/jfs/t1/214285/35/14784/132267/622c58b6Ed4b0a90a/7d2d32977748d2fb.png)

---

### 冒泡排序

- 冒泡排序是最出名的算法之一，总共有八大排序！

```java
package com.xiheya.Array;

import java.util.Arrays;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 16:32
 * @Description
 */
public class ArrayDemo06 {
    public static void main(String[] args) {
        int[] a = {1,6,5,3,2,9};
        System.out.println(Arrays.toString(a));
        sort(a);
        System.out.println(Arrays.toString(a));
    }

    //冒泡排序：
    // 1.比较两个相邻的数，如果第一个数比第二个数大，则交换他们的位置。
    // 2.每一次排序都会产生一个最大或最小的数字；
    // 3.下一轮则可以少一次排序
    // 4.依次循环直到结束
    public static void sort(int[] a){
        int temp = 0;
        for (int i = 0; i < a.length-1; i++) {
            for (int j = 0; j < a.length-i-1; j++){
                if (a[j] > a[j+1]){
                    temp = a[j];
                    a[j] = a[j+1];
                    a[j+1] = temp;
                }
            }
        }
    }
}

```

运行结果：

![冒泡排序](https://img30.360buyimg.com/pop/jfs/t1/146835/17/23128/89205/622c640bEcd377f10/1c0d484bf68a1ec7.png)

---

### 稀疏数组

- 需求：编写五子棋游戏中，有存盘退出和续上盘的功能。
- 介绍：当一个数组中大部分元素为0，或者为同一值的数组时，可以使用稀疏数组来保存该数组。
- 稀疏数组的处理方式是：
  - 记录数组一共有几行几列，有多少个不同值
  - 把具有不同值的元素和行列及值记录在一个小规模的数组中，从而缩小程序的规模
- 稀疏数组与原数组图示：

![原数组与稀疏数组](https://img30.360buyimg.com/pop/jfs/t1/87424/23/24953/40132/622c9a2dE118667fd/967867ab4c66a1ea.png)

---

**设计一个程序实现 稀疏数组与普通数组 的互换**

代码：

```java
package com.xiheya.Array;

/**
 * @Author {xiheya}
 * @Date: 2022/03/12/ 17:39
 * @Description
 */
public class ArrayDemo07 {
    public static void main(String[] args) {
        int[][] array = new int[11][11];
        array[2][3] = 2;
        array[1][4] = 1;
        printArray(array);                                  //打印原数组
        int[][] array1 = toArray(array);                    //将原数组转换为稀疏数组
        printArray(array1);                                 //打印转换好的稀疏数组
        int[][] restore = restore(array1);                  //将稀疏数组再转换为普通数组
        printArray(restore);                                //打印转换完成后的数组
    }

    public static void printArray(int[][] array){           //通过for_each遍历方法，打印数组
        System.out.println("start to print the array:");
        for (int[] ints : array) {                          //for_each遍历外层
            System.out.print("[");
            for (int anInt : ints) {                        //for_each遍历内层
                System.out.print(anInt + "\t");
            }
            System.out.print("]");
            System.out.println();
        }
    }

    public static int[][] toArray(int[][] a){               //将普通数组转换为稀疏数组的方法
        int sum = 0;                                        //用sum来统计，不为0元素的个数，sum为稀疏数组的行号。即int[][] temp = new int[sum+1][3];
        for (int[] ints : a) {
            for (int anInt : ints) {
                if (anInt != 0){
                    sum++;
                }
            }
        }
        int[][] temp = new int[sum+1][3];                   //统计完sum声明并创建稀疏数组temp
        temp[0][0] = a.length;                              //temp[0][0] 存放行数
        temp[0][1] = a[0].length;                           //temp[0][1] 存放列数
        temp[0][2] = sum;                                   //temp[0][2] 存放数组内有效数据个数
        int tempnum = 1;                                    //稀疏数组行号tempnum
        for (int i = 0; i < a.length; i++) {                //遍历普通数组，当遍历到普通数字内有效数字时
            for (int j = 0; j < a[i].length; j++) {
                if (a[i][j] != 0){
                    temp[tempnum][0] = i;                   //将原普通数组行号赋值给temp[tempnum][0]
                    temp[tempnum][1] = j;                   //将原普通数组行号赋值给temp[tempnum][1]
                    temp[tempnum][2] = a[i][j];             //将原普通数组第i行j列的数据 赋值给temp[tempnum][2]
                    tempnum++;                              //装载完成后，稀疏数组行号 tempnum  自增1
                }
            }
        }
    return temp;
    }

    public static int[][] restore (int[][] array){              //将稀疏数组还原为普通数组的方法
        int[][] result = new int[array[0][0]][array[0][1]];     //声明并创建还原后的数组：result
        for (int i = 1; i < array.length ; i++) {               //遍历稀疏数组，取出原普通数组的行号和列号
            result[array[i][0]][array[i][1]] = array[i][2];     //array[i][0]代表原数组有效数组的行号、array[i][1]代表其列号，遍历到这里时，将原数组第array[i][0]行第array[i][1]列的数据array[i][2]赋值回去。
        }


    return result;                                              //返回还原完成后的普通数组。
    }

}

```

运行结果：

![输出结果](https://img30.360buyimg.com/pop/jfs/t1/93458/11/24636/39688/622c9987E9c3afe46/8673d96930c28c8c.png)
