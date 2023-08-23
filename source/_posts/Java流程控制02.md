---
title: Java流程控制02
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva2.sinaimg.cn/large/87c01ec7gy1fsnqqanu0wj21kw0w07h2.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/e21f1f24bd613.jpg
ai: 
  - 本文介绍了Java中的顺序结构和选择结构。顺序结构是基本的算法结构，在执行过程中按照从上到下的顺序依次执行处理步骤。当涉及判断时，可以使用选择结构。选择结构分为单选择、双选择和多选择结构。其中if语句用于单选择和双选择结构，根据条件的真假执行相应的代码块。多选择结构可以使用if...else if...else语句，根据不同的条件选择执行不同的代码块。此外，嵌套的if和else语句也是合法且常见的。另一种多选择结构是switch case语句，根据变量与不同的值进行判断，并执行对应的代码块。在Java SE 7及以后的版本中，switch语句还支持字符串类型的判断。代码示例展示了顺序结构和各种选择结构的应用。其中包括使用if语句判断输入内容是否满足条件并输出结果，根据不同的成绩等级输出评价，以及利用switch case语句根据不同的情况执行相应的操作。注意，在Java SE 7及以后的版本中，switch case语句可以用于字符串类型的判断。
tags:
  - Java
  - Java流程控制
abbrlink: 2dc51ce6
date: 2022-08-25 22:40:57
authorAbout:
authorDesc:
keywords:
description:
---

## 顺序结构

- Java的基本结构就是顺序结构，除非特别指明，否则就按照顺序一句一句执行
- 顺序结构是最简单的算法结构
- 语句与语句之间，框与框之间是按从上到下的顺序进行的，他是由若干个依次执行的处理步骤组成的，**他是一个任何算法都离不开的一种基本算法结构**

代码：

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 16:12
 * @Description
 */
public class orderDemo01 {
    public static void main(String[] args) {
        System.out.println("helloworld1");
        System.out.println("helloworld2");
        System.out.println("helloworld3");
        System.out.println("helloworld4");
    }
}

```

因为Java的基本结构是顺序结构，所以会依次输出helloworld1234

**输出结果：**

![顺序结构](https://img30.360buyimg.com/pop/jfs/t1/127527/10/24234/104851/62271075Ea824da0b/7f876f7510c79bb4.png)

---

## 选择结构

### if单选择结构

- 很多时候需要判断一个东西是否可行，然后再去执行，这个时候我们就需要用到if语句
- 语法：

```java
if(布尔表达式){
    //如果布尔表达式为true就执行这里面的语句
}
```

代码：

```java
package com.xiheya.struct;

import java.util.Scanner;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 16:17
 * @Description
 */
public class ifDemo01 {
    public static void main(String[] args) {

        Scanner scanner = new Scanner(System.in);
        String s = scanner.nextLine();
        if (s.equals("helloworld")){
            System.out.println("helloworld");
        }
        System.out.println("End");
        scanner.close();
    }
}

```

因为是单判断语句，所以输出时会先判断输入的内容是否为helloworld，如果是的话就输出helloworld后输出End，如果不是就直接输出End

**输出结果**

![输出结果2](https://img30.360buyimg.com/pop/jfs/t1/209689/16/18972/92480/62271278Ede476681/e0ba51a97236559b.png)

---

### if双选择结构

语法和单选择结构类似

```java
if(布尔表达式){
    //如果布尔表达式为true就执行这里面的语句
}else{
    //如果布尔表达式为false就执行这里面的语句
}
```



**设计一个程序，输入分数大于60时输出及格，否则输出不及格**


```java
package com.xiheya.struct;

import java.util.Scanner;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 16:27
 * @Description
 */
public class IfDemo02 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("请输入成绩");
        if (scanner.nextInt() > 60){
            System.out.println("您的成绩及格");
        } else{
            System.out.println("您的成绩不及格");
        }
        scanner.close();
    }
}

```

---

### if多选择结构

语法：

```java
if(布尔表达式 1){
    //如果布尔表达式1为true就执行这里面的语句
}else if(布尔表达式 2){
    //如果布尔表达式2为true就执行这里面的语句
}else if(布尔表达式 3){
    //如果布尔表达式3为true就执行这里面的语句
}else{
    //如果以上布尔表达式为false就执行这里面的语句
}
```

**设计一个程序输入的分数为100时输出满分；90-100为A；80-90为B；70-80为C；60-70为D；小于60为不及格，其余成绩为不合法**

代码：

```java
package com.xiheya.struct;

import java.util.Scanner;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 16:27
 * @Description
 */
public class IfDemo03 {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        System.out.println("请输入成绩");
        int score = scanner.nextInt();
        if (score == 100){
            System.out.println("满分！");
        }else if(score < 100 && score >= 90){
            System.out.println("A!");
        }else if(score < 90 && score >= 80){
            System.out.println("B!");
        }else if(score < 80 && score >= 70){
            System.out.println("C!");
        }else if(score < 70 && score >= 60){
            System.out.println("D!");
        }
        else if(score < 60 && score >= 0){
            System.out.println("您的成绩不及格");
        } else
        {
            System.out.println("成绩不合法");
        }
        scanner.close();
    }
}

```

---

### 嵌套的if结构

- 使用嵌套的if……else语句是合法的。也就是说你可以在另一个if或者else if语句中使用if或者else if语句，你可以像if语句一样嵌套else if……else
- 语法

```java
if(布尔表达式1){
    //如果布尔表达式为true就执行
    if(布尔表达式2){
        //如果布尔表达式2为true就执行
    }
}
```

----

### switch多选择结构

- 多选择结构还有一个实现方式就是switch case语句
- switch case语句判断一个变量与一系列值中某个值是否相等，每个值称为一个分支
- 语法：

```java
switch(expression){
    case value :
        //语句
        break;//可选
    case value :
        //语句
        break;//可选
    //你可以有任意数量的case语句
        default : //可选
        //语句
}
```

- switch 语句中的变量类型可以是
  - byte 、short、int或者char
  - **从Java SE 7开始 switch就支持字符串String型了**
  - 同时case标签必须为字符串常量或字面量

**设计一个程序，根据ABCD输出不同的评价。**

代码：

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 17:09
 * @Description
 */
public class SwitchDemo01 {
    public static void main(String[] args) {
        char grade = 'G';
        switch (grade){
            case 'A' :
                System.out.println("优秀");
                break;
            case 'B' :
                System.out.println("良好");
                break;
            case 'C' :
                System.out.println("及格");
                break;
            case 'D' :
                System.out.println("再接再厉");
                break;
            case 'E' :
                System.out.println("挂科");
                break;
            default:
                System.out.println("未知成绩");
        }
    }
}

```

---

**Java SE 7 新特性**

代码

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 17:20
 * @Description
 */
public class SwitchDemo02 {
    public static void main(String[] args) {
        String name = "hahaha";
        switch (name){
            case "xiheya":
                System.out.println("right");
                break;
            case "hahaha":
                System.out.println("error");
                break;
            default:
                System.out.println("???");
        }
    }
}

```

