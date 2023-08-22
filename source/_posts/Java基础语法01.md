---
title: Java基础语法01
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
abbrlink: dd2d9cfc
date: 2022-08-25 22:34:29
authorAbout:
authorDesc:
keywords:
description:
---

## 注释

- 平时编写代码时，在代码量较少的时候。代码的可读性更强，但是当项目结构一旦复杂起来，我们就需要注释。
- **注释不会被执行**，只是给我们写代码的人看的
- ***书写注释是一个非常好的编程习惯***

### Java中的注释

- 单行注释 //

```java	
public class HelloWorld {
    public static void main(String[] args) {
        //我是单行注释
        System.out.println("HelloWorld!");
    }
}
```

---

- 多行注释/**/

```java	
public class HelloWorld {
    public static void main(String[] args) {
        /*我是多行注释
        我是多行注释
        */
        System.out.println("HelloWorld!");
    }
}
```

---

- 文档注释/** */

```java	
public class HelloWorld {
    public static void main(String[] args) {
        /*
        *我是文档注释
        我是文档注释
        */
        System.out.println("HelloWorld!");
    }
}
```

---

## Java标识符

**关键字**

- ***Java所有的组成部分都需要名字。类名、变量名以及方法名都被称为标识符***

**标识符注意点**

- 所有标识符都应该以字母（A-Z或者a-z），美元（￥）或者下划线（_）开始
- 首字符过后可以是字母（A-Z或者a-z）、美元（￥）、下划线（_）或者数字的任何字符组合
- ***不能使用关键字作为变量名或方法名***
- 标识符是**大小写敏感**的
- **不建议使用中文或者拼音作为变量名或方法名**

## Java数据类型

- 强类型语言
  - 要求变量的使用**必须要严格符合规定**，所有变量都必须先定义后才能使用
- Java数据类型分为两大类
  - 基本类型(primitive type)
  - 引用类型(reference type)

**基本类型**

| No.  |      数据类型      | 大小/位 |              可表示数据范围              |  默认值  |
| :--: | :----------------: | :-----: | :--------------------------------------: | :------: |
|  1   |  `byte`（字节型）  |    8    |                 -128~127                 |    0     |
|  2   | `short`（短整型）  |   16    |               -32768~32767               |    0     |
|  3   |   `int`（整型）    |   32    |          -2147483648~2147483647          |    0     |
|  4   |  `long`（长整型）  |   64    | -9223372036854775808~9223372036854775807 |    0     |
|  5   | `float`（单精度）  |   32    |              -3.4E38~3.4E38              |   0.0    |
|  6   | `double`（双精度） |   64    |             -1.7E308~1.7E308             |   0.0    |
|  7   |   `char`（字符）   |   16    |                  0~255                   | '\u0000' |
|  8   | `boolean`（布尔）  |    -    |               true或false                |  false   |

**引用类型**

引用数据类型非常多，大致包括：
类、 接口类型、 数组类型、 枚举类型、 注解类型、 字符串型

如**String为引用类型**



```java
public class Demo01 {
    public static void main(String[] args) {
        //整型：int(4字节)、byte(1字节)、short(2字节)还有long(8字节)
        int num01 = 10;
        byte num02 = 20;
        short num03 = 30;
        long num04 = 40l;
        //浮点型：float(4字节)、double(8字节)
        float num05 = 50.66f;
        double num06 = 66.66;
        //字符：char(2字节)
        char usr = 'x';
        //布尔值：boolean
        boolean flag = true;
    }
}
```



**小科普**

- **位(bit)**:是计算机内部数据存储的最小单位，10100101是一个八位二进制数
- **字节(Byte)**: 是计算机中数据处理的基本单位，习惯上用大写的B来表示
- 1B(Byte,字节) = 8bit(位)
- **字符**:是指计算机中使用的字母、数字、字和符号
  - 1bit = 1位;
  - 1Byte = 1B = 8b;
  - 1024B = 1KB
  - 1024KB = 1M
  - 1024M = 1G



### Java数据类型拓展

- 整数拓展， 0b表示二进制数、0表示八进制、十进制直接输入、0x表示十六进制

```java
import java.math.BigDecimal;

public class Demo02 {
    public static void main(String[] args) {
        //整数拓展， 0b表示二进制数、0表示八进制、十进制直接输入、0x表示十六进制
        int b = 0b10;
        int i = 10;
        int i1 = 010;
        int i2 = 0x10;
        System.out.println(b);
        System.out.println(i);
        System.out.println(i1);
        System.out.println(i2);

        System.out.println("===================================");
        }
}
```

- 浮点数扩展 
  - 银行业务表示，
  - 常使用数学工具类BigDecimal，来表示银行业务。float数据类型是有限的，而且是离散的。它会舍入误差只表示一个大概的数，----->接近但不等于！最好避免完全使用浮点数进行比较！！！
- a

```java
public class Demo02 {
    public static void main(String[] args) {
        //浮点数扩展 银行业务表示，
        //通常使用数学工具类BigDecimal，来表示银行业务。
        System.out.println("===================================");
        //float             float数据类型是有限的，而且是离散的。它会舍入误差只表示一个大概的数，----->接近但不等于
        //double
        //最好避免完全使用浮点数进行比较
        //最好避免完全使用浮点数进行比较
        //最好避免完全使用浮点数进行比较
        float f = 0.1f;
        double d = 1.0/10;
        System.out.println(f == d);
        System.out.println("===================================");
        float f1 = 12345667486234f;
        float f2 = f1 + 1;
        System.out.println(f1 == f2);
        System.out.println("===================================");
    }
}
```

- 字符扩展
  - System.out.println((int)c1);        //将char型的c1 强制转换为int型的Unicode编码
  - 所有字符的本质还是数字，他们存放在一个Unicode编码表内（97 = a 、 65 = A） ，他占两个字节；
  - 转义字符：  \t 制表符、 \n 换行符……
- a

```java
import java.math.BigDecimal;

public class Demo02 {
    public static void main(String[] args) {
        //字符扩展
        System.out.println("===================================");
        char c1 = 'a';
        char c2 = '荣';
        System.out.println(c1);
        System.out.println((int)c1);        //将char型的c1 强制转换为int型的Unicode编码

        System.out.println(c2);
        System.out.println((int)c2);        //将char型的c2 强制转换为int型的Unicode编码
        System.out.println("===================================");
        char c3 = '\u0066';                 //将Unicode编码0066转义为char型数据c3
        System.out.println(c3);
        //所有字符的本质还是数字，他们存放在一个Unicode编码表内（97 = a 、 65 = A） ，他占两个字节；

        //转义字符  \t 制表符、 \n 换行符……
        System.out.println("Hello\t World!");
    }
}

```

- 布尔值扩展：
  - less is more   代码要精简易读；

```java
import java.math.BigDecimal;

public class Demo02 {
    public static void main(String[] args) {

        //布尔值扩展
        boolean flag = true;
        if (flat == true){};
        if (flag){};
        //less is more   代码要精简易读；
    }
}

```

