---
title: Java基础语法03
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva3.sinaimg.cn/large/87c01ec7gy1fsnqqxqwj6j21kw0w01a0.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/00c70a2ab8b2b.jpg
ai: 
  - 本文介绍了Java语言中常见的运算符，包括算术运算符、赋值运算符、关系运算符、逻辑运算符、位运算符、条件运算符以及扩展赋值运算符。针对每种运算符，给出了相应的符号、语法格式和示例代码，并解释了其具体用法和注意事项。此外，还列出了运算符的优先级表格供参考。
tags:
  - Java
  - Java基础语法
abbrlink: 3323fdd0
date: 2022-08-25 22:37:23
authorAbout:
authorDesc:
keywords:
description:
---

## 一、运算符

### 1.1 Java语言支持的运算符

### 1.2 算术运算符

#### **1.2.1 符号**

算术运算符符号：**+   ,   -   ,   *   ,   /   ,   ++   ,   --**

```java
package Operator;

public class Demo01 {
    public static void main(String[] args) {
        //算数运算符的基本操作
        int a = 10;
        int b = 20;
        int c = 30;
        int d = 40;
        System.out.println(a+b);
        System.out.println(a-b);
        System.out.println(a*b);
        System.out.println(a/(double)b);
        System.out.println(a%b);
    }
}

```

![输出结果](https://img30.360buyimg.com/pop/jfs/t1/148544/13/22533/14522/6225a46dE4784cb8c/0af0c243012edde2.png)

---

#### 1.2.2 **小tips**

- 当参与运算的变量中有**long**型时，输出的结果也为**long**型

- 若参与运算的变量中五**long**型时，**无论参与运算的变量是何类型**，则输出结果均为**int**型

eg：

```java
package Operator;

public class Demo01 {
    public static void main(String[] args) {
        //算数运算符的基本操作
        long a = 10;
        int b = 20;
        byte c = 30;
        short d = 40;
        System.out.println((String) (a+b));
        System.out.println((String) (b+c));
        System.out.println((String) (c+d));
        System.out.println((String) (b+d));
        

    }
}
```

![test](https://img30.360buyimg.com/pop/jfs/t1/130784/18/22547/135851/6225a642Ed39e84f6/f1da4cc8820f49fb.png)

---

** 自增（++）、自减（--）运算符**

```java
package Operator;

public class Demo03 {
    public static void main(String[] args) {
        //++  --  自增 自减  一元运算符
        int a = 1;
        int b = a++;                //执行这行代码时，先进行赋值操作把a的初始值1赋值给b；然后a再执行自增操作a = a+1 = 2。
        System.out.println(a);
        int c = ++a;                //执行这行代码时，先进行a = 2自增操作a = a+1 =3；然后再执行赋值操作把a值3赋值给c。
        System.out.println(a);
        System.out.println(b);
        System.out.println(c);
    }
}

```

![自增自减](https://img30.360buyimg.com/pop/jfs/t1/110217/22/23354/120513/6225ab39E72783746/456fafc3cbbcbd8c.png)

---

### 1.3 赋值运算符

#### 1.3.1 符号

赋值运算符符号：**=**

#### 语法格式

```java
//语法格式如下
int a = 10;  //初始化int类型变量a，并将10赋值给a。
```

---

### 1.4 关系运算符

#### 1.4.1 符号

：关系运算符符号：**>   ,   <   ,   >=   ,   <=   ,   ==   ,   !=   ,   instanceof**

- **关系运算符返回的是 布尔值，其结果只有true和false两种情况，常用于if语句中**

#### 语法格式

```java
package Operator;

public class Demo02 {
    public static void main(String[] args) {
        //关系运算符返回的是 布尔值，其结果只有true和false两种情况，常用于if语句中
        int a = 10 ;
        int b = 20 ;
        System.out.println(a == b);
        System.out.println(a != b);
        System.out.println(a < b);
        System.out.println(a > b);

    }
}

```

![result](https://img30.360buyimg.com/pop/jfs/t1/93862/1/21707/83677/6225a800E75cd18a9/34f214e69921edad.png)


---

### 1.5 逻辑运算符

#### 1.5.1 符号

逻辑运算符符号：**&&   ,   ||   ,   !**

#### 1.5.2 语法格式

```java
package Operator;

public class Demo04 {
    public static void main(String[] args) {
        boolean a = true;
        boolean b = false;
        System.out.println("a&&b:\t"+(a && b));             //逻辑与运算：只要有一个是假的，则结果就是假的
        System.out.println("a||b:\t"+(a || b));             //逻辑或运算：只要有一个是真的，则结果就是真的
        System.out.println("!(a&&b):\t"+!(a && b));         //逻辑非运算：结果取反
        //短路运算：当与运算中第一个变量为假时，就不会再去判断第二个变量。
        int c = 5;
        boolean d = b && (c++ > 5);
        System.out.println(d);
        System.out.println(c);
    }
}
```

![result01](https://img30.360buyimg.com/pop/jfs/t1/114452/5/20887/125235/6225aebeE1a0cac60/c63f3bb94e130e05.png)

---

### 1.6 位运算符

#### 1.6.1 符号

位运算符符号：**&  ,  |  ,  ^  ,  ~  ,  >>  ,  <<   ,  >>>**

#### 1.6.2 语法格式

语法格式如下

```java
package Operator;

public class Demo05 {
    public static void main(String[] args) {
        /*
        A = 	0010 1101
        B = 	1011 1001
        -------------------
        A&B =   0010 1001
        A|B =   1011 1101
        A^B =   1001 0100   异或运算：变量真假相同时结果为假，不同时为真
        ~B  =   0100 0110
        */
        //快速计算2*8
        System.out.println(2<<3);
    }
}
```

---

### 1.7 条件运算符

#### 1.7.1 符号

条件运算符符号：**x ? y : z :**

#### 1.7.2 语法格式

语法格式如下：

```java
package Operator;

public class Demo05 {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        // 条件运算符 x ? y : z
        // 先判断x是否为真，若x为真，则结果是y；若x为假，则结果是z
        System.out.println((a>b) ? 50 : 60);
        System.out.println((a<b) ? 50 : 60);
    }

}

```

---

### 1.8 扩展赋值运算符

#### 1.8.1 符号

扩展赋值运算符符号：**+=  ,  -=  ,  *=  ,  /=**

#### 1.8.2 语法格式

语法格式如下

```java
package Operator;

public class Demo05 {
    public static void main(String[] args) {
        int a = 10;
        int b = 20;
        a+=b;
        System.out.println(a); // a = a+b -->30 = 10+20
        a-=b;
        System.out.println(a); // a = a-b -->10 = 30-20

        //字符串连接符 +
        //当+号两侧的任意一侧出现了String类型，则它会自动将两侧的变量都转换为String类型并进行连接。
        System.out.println("" + a + b);
        //但是如果String类型前面出现了运算，则会先运算再连接。
        System.out.println(b+ a + "" );
    }

}

```

![result02](https://img30.360buyimg.com/pop/jfs/t1/211389/19/18820/122997/6225b52dEbff02f91/33d468e313578960.png)

---

### 1.9 运算符优先级

| **优先级** |                          **运算符**                          |                          **简介**                           | **结合性** |
| :--------: | :----------------------------------------------------------: | :---------------------------------------------------------: | :--------: |
|     1      |                     `[ ]`、` .`、` ( ) `                     |                     方法调用，属性获取                      |  从左向右  |
|     2      |                        !、~、 ++、 --                        |                         一元运算符                          |  从右向左  |
|     3      |                          * 、/ 、%                           |                    乘、除、取模（余数）                     |  从左向右  |
|     4      |                            + 、 -                            |                           加减法                            |  从左向右  |
|     5      |                        <<、 >>、 >>>                         |                 左位移、右位移、无符号右移                  |  从左向右  |
|     6      |                 < 、<= 、>、 >=、 instanceof                 | 小于、小于等于、大于、大于等于， 对象类型判断是否属于同类型 |  从左向右  |
|     7      |                           == 、!=                            |      2个值是否相等，2个值是否不等于。 下面有详细的解释      |  从左向右  |
|     8      |                              &                               |                           按位与                            |  从左向右  |
|     9      |                              ^                               |                          按位异或                           |  从左向右  |
|     10     |                              \|                              |                           按位或                            |  从左向右  |
|     11     |                              &&                              |                           短路与                            |  从左向右  |
|     12     |                             \|\|                             |                           短路或                            |  从左向右  |
|     13     |                              ?:                              |                         条件运算符                          |  从右向左  |
|     14     | =、 += 、-= 、*= 、/=、 %=、 &=、 \|=、 ^=、 <、<= 、>、>= 、>>= |                       混合赋值运算符                        |  从右向左  |

点击查看[Java基础语法之注释、标识符、数据类型（一）](http://110.42.139.30:8000/index.php/2022/03/07/java基础语法之注释、标识符、数据类型/)

点击查看[Java基础语法之类型转换和变量（二）](http://110.42.139.30:8000/index.php/2022/03/07/java基础之类型转换和变量（二）/)

