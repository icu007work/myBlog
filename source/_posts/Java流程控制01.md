---
title: Java流程控制01
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva2.sinaimg.cn/large/87c01ec7gy1fsnqqanu0wj21kw0w07h2.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/e21f1f24bd613.jpg
tags:
  - Java
  - Java流程控制
abbrlink: b4cc4d5c
date: 2022-08-25 22:38:42
authorAbout:
authorDesc:
keywords:
description:
---

## 用户交互Scanner

- Java给我们提供了一个工具类，我们可以根据这个工具类来获取用户的输入。java.util.Scanner是Java5的新特征，**我们可以通过Scanner类来获取用户的输入**
- 基本语法

```java
Scanner s = new Scanner(System.in);
```

- 通过Scanner类的next()与nextLine()方法获取输入的字符串，在读取前我们一般需要使用hasNext()与hasNextLine()来判断是否还有输入的数据.

---

1. 使用hasNext()方法判断是否还有输入的数据；next()方法接收输入的字符

```java
package com.xiheya.Scanner;

import java.util.Scanner;

/**
 * @Author {xiheya}
 * @Date: 2022/03/07/ 23:32
 * @Description
 */
public class Demo01 {
    public static void main(String[] args) {
        //声明一个Scanner类型的变量
        Scanner scanner = new Scanner(System.in);
        //声明一个String类型的变量
        //String str;
        //判断是否还有输入的数据
        System.out.println("请从键盘上输入任意字符，以空格键或回车键结束");
        if (scanner.hasNext()){
            String str = scanner.next();
            System.out.println("从键盘上输入字符为：" + str);
        }
        //用完一定要记得关闭！！！
        scanner.close();
    }
}

```

---

2. 使用hasNextLine()方法判断是否还有输入的数据；nextLine()方法接收输入的字符

```java
package com.xiheya.Scanner;

import java.util.Scanner;

/**
 * @Author {xiheya}
 * @Date: 2022/03/07/ 23:32
 * @Description
 */
public class Demo01 {
    public static void main(String[] args) {
        //声明一个Scanner类型的变量
        Scanner scanner = new Scanner(System.in);
        //声明一个String类型的变量
        //String str;
        //判断是否还有输入的数据
        System.out.println("请从键盘上输入任意字符，以回车键结束");
        if (scanner.hasNextLine()){
            String str = scanner.nextLine();
            System.out.println("从键盘上输入字符为：" + str);
        }
        //用完一定要记得关闭！！！
        scanner.close();
    }
}

```

---

3. 不判断是否还有输入的数据；直接nextLine()方法接收输入的字符

```java
package com.xiheya.Scanner;

import java.util.Scanner;

/**
 * @Author {xiheya}
 * @Date: 2022/03/07/ 23:32
 * @Description
 */
public class Demo01 {
    public static void main(String[] args) {
        //声明一个Scanner类型的变量
        Scanner scanner = new Scanner(System.in);
        //声明一个String类型的变量
        //String str;
        //判断是否还有输入的数据
        System.out.println("请从键盘上输入任意字符，以回车键结束");
        String str = scanner.nextLine();
        System.out.println("从键盘上输入字符为：" + str);
        //用完一定要记得关闭！！！
        scanner.close();
    }
}

```

---

**IO流的类用完一定要关掉，不然他会在后台一直占用资源!!!!**

### next()与nextLine()方法的区别

- Scanner是一个扫描器，我们录取到键盘的数据，先存到缓存区等待读取，它判断读取结束的标示是  空白符；比如空格，回车，tab 等等。

![API](https://img30.360buyimg.com/pop/jfs/t1/108362/23/25929/88054/62262eb1E3b5bef2f/061ac863a73b599d.png)

- next()方法是读取到空白符就结束了
  1. 一定要读取到有效字符后才可以结束输入
  2. 对输入有效字符之前遇到的空白，next()方法会自动将其去掉。
  3. 只有输入有效字符后才将其后面输入的空白符作为分隔符或结束符
  4. **next**()不能得到带有空格的字符串
- nextLine()方法是读取到了回车就结束即：\t.
  1. 以enter为结束符，也就是说nextLine()方法 返回的是输入回车之前的所有字符
  2. 可以获得空白符

---

### Scanner中的其他方法

1. **hasNextInt()** 方法与 **hasNextfloat()**方法可以判断下一个是否还有整数或小数输入。

```java
package com.xiheya.Scanner;

import java.util.Scanner;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 12:31
 * @Description
 */
public class Demo02 {
    public static void main(String[] args) {
        int i = 0;
        float f = 0f;
        Scanner scanner = new Scanner(System.in);
        System.out.println("请从键盘上输入任意整数");
        if (scanner.hasNextInt()){
            i = scanner.nextInt();
            System.out.println("输入的整数为" + i);
        }
        else {
            System.out.println("Error！您输入的不是整数");
        }


        System.out.println("请从键盘上输入任意小数");
        if (scanner.hasNextFloat()){
            f = scanner.nextFloat();
            System.out.println("输入的小数为" + f);
        }
        else {
            System.out.println("Error！您输入的不是小数");
        }
        scanner.close();

    }
}

```

![输出结果](https://img30.360buyimg.com/pop/jfs/t1/105157/26/23793/13664/6226e0cdE6a4ab3cb/4be574caab1b11c7.png)

---

2. **hasNextdouble()方法**：判断接下来输入的是不是double型；

学到这里可以做一个简易的数字求和程序，代码如下：

```java
package com.xiheya.Scanner;

import java.util.Scanner;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 12:52
 * @Description
 */
public class Demo03 {
    public static void main(String[] args) {
        double total = 0;
        int m = 0;
        Scanner scanner = new Scanner(System.in);
        System.out.println("请从键盘上输入任意个数，以字符型数据结束");
        while (scanner.hasNextDouble()){

            double x = scanner.nextDouble();
            total = total + x;
            m++;
            System.out.println("您输入了" + m + "个数，和为：" + total);

        }
        double v = total / m;
        System.out.println("您结束了输入，此次您输入了" + m + "个数，和为：" + total + "这些数的平均值为：" + v);
        scanner.close();
    }
}

```

运行结果如下：

![求和程序](https://img30.360buyimg.com/pop/jfs/t1/125778/15/24705/127198/6226e3bdE5d810fb2/9dd413f8f1e4e2a6.png)

