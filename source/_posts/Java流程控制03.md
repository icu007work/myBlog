---
title: Java流程控制03
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
abbrlink: 5ac22c70
date: 2022-08-25 22:41:01
authorAbout:
authorDesc:
keywords:
description:
---

## 循环结构

### while 循环

- while 是最基本的循环
- 语法

```java
while(布尔表达式){
    //循环内容
}
```

- 只要布尔表达式为true，则循环一直执行下去
- **我们大多数情况下会让循环停止下来，我们需要一个让表达式失效的方式来结束循环**
- 少部分情况循环需要一直执行，比如服务器的请求响应监听
- 循环条件一直为true就会造成无限循环【死循环】，正常业务中，应当避免死循环。它会影响程序性能或者造成程序卡死崩溃

**设计一个程序计算1+2+3+4+5+……+100；**

代码：

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 17:44
 * @Description
 */
public class WhileDemo01 {
    public static void main(String[] args) {
        int i = 0;
        int total = 0;
        while( i < 100)
        {
            i++;
            total += i ;
        }
        System.out.println(total);
    }
}

```

---

### do …… while循环

- 对于while语句而言，如果不满足条件，则不能进入循环。但有时候我们需要即使不满足条件时 也至少执行一次。
- do ……while循环 和while循环相似，不同的是，多……while循环至少会执行一次。
- 语法

```java
do{
    //代码语句
}while(布尔表达式);
```

- while和do-While的区别：
  - while先判断后执行，dowhile是先执行后判断！
  - Do……while总是保证循环体会被至少执行一次！这是他们的主要差别

代码：

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 17:56
 * @Description
 */
public class DoWhileDemo01 {
    public static void main(String[] args) {
        int i = 0;
        int total = 0;
        do{
            i++;
            total += i;
        }while(i < 100);
        System.out.println(total);
    }
}

```



### for循环

- 虽然所有循环结构都可以用while或者do……while表示，但Java提供了另一种语句---for循环，使一些循环结构变得更加简单。
- for循环语句是支持迭代的一种通用结构，是最有效，最灵活的循环
- for循环执行的次数在执行前就确定。语法格式如下：

```java
for(初始化; 布尔表达式 ; 更新){
    //代码语句
}

```

**设计一个程序计算出0-100的奇数和与偶数和**

代码：

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 22:27
 * @Description
 */
public class ForDemo01 {
    public static void main(String[] args) {
        int oddtotal = 0;//奇数和
        int eventotal = 0;//偶数和
        for (int i = 0; i <= 100; i++) {
            if (i%2 == 0){
                eventotal += i;
            }else {
                oddtotal += i;
            }

        }
        System.out.println("偶数和：\t"+eventotal);
        System.out.println("奇数和: \t"+oddtotal);
    }

}

```

**运行截图**

![运行结果](https://img30.360buyimg.com/pop/jfs/t1/118310/15/21353/108352/6228618aEbef0fb54/833806668fd876ba.png)

**设计一个程序输出1-1000之间能被5整除的数，每行输出三个**

代码：

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/08/ 22:27
 * @Description
 */
public class ForDemo01 {
    public static void main(String[] args) {
        int oddtotal = 0;//奇数和
        int eventotal = 0;//偶数和
        for (int i = 0; i <= 100; i++) {
            if (i%2 == 0){
                eventotal += i;
            }else {
                oddtotal += i;
            }

        }
        System.out.println("偶数和：\t"+eventotal);
        System.out.println("奇数和: \t"+oddtotal);

        for (int i = 0; i <= 1000; i++) {
            if ((i%5) == 0){                    //对
                System.out.print(i+"\t");

            }
            if (((i+1) % (5*3)) == 0){
                System.out.print("\n");

            }
        }
    }


}

```

**设计一个程序打印出99乘法表**

程序

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/09/ 16:56
 * @Description
 */
public class ForDemo02 {
    public static void main(String[] args) {
        for (int i = 1; i <= 9; i++) {
            for (int j = 1; j <= i; j++) {           //行由i控制，第i行输出i个依次类推
                System.out.print( j + "*" + i + "=" + i*j + "\t");
            }
            System.out.println();                   //输出完一行就换一次行
        }
    }
}

```

**运行结果**

![输出结果2](https://img30.360buyimg.com/pop/jfs/t1/209292/37/19171/117235/62286e6fE53dcfc2e/196b64fb564fe713.png)

### 增强for循环

- Java5引入了一种主要用于数组或集合的增强for循环。
- 增强for循环语法格式

```java
for(声明语句:表达式){
    //代码语句
}
```

- 声明语句：声明新的局部变量，该变量的类型必须和数组元素的类型匹配。其作用域限制在循环语句块，其值与此时数组元素的值相等。
- 表达式：表达式是要访问的数组名，或者是返回值为数组的方法。

代码示例：简单的遍历代码

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/09/ 18:56
 * @Description
 */
public class ForDemo03 {
    public static void main(String[] args) {
        int [] numbers = {10,20,30,40,50};
        for (int x : numbers){
            System.out.println(x);
        };
    }
}

```

### break 和 continue

- break在任何循环语句的主体部分，均可用break控制循环的流程。**break用于强行退出循环，不执行循环中剩余的语句**。（break语句也可以在switch语句中使用）

示例：

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/09/ 19:13
 * @Description
 */
public class Break {
    public static void main(String[] args) {
        for (int i = 1; i < 100; i++) {
            if (i  == 30){
                System.out.println();
                break;                              //break;程序运行到这里的时候就会跳出循环，但是还会执行循环后面的语句
            }
            System.out.println(i);
        }
        System.out.println("我还可以继续运行");         //跳出循环后程序还是会继续运行
    }
}

```

输出结果：当i自增到30时，会跳出这个for循环，但是程序还会继续往下运行。

![输出结果0](https://img30.360buyimg.com/pop/jfs/t1/221351/31/12303/124973/6228d19cE7d14fe26/dc9f80fe880977db.png)

---

- continue语句用在循环语句体中，**用于终止某次循环过程，即跳过循环体中尚未执行的语句，接着进行下一次是否执行循环的判定。**

示例

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/09/ 19:21
 * @Description
 */
public class Continue {
    public static void main(String[] args) {
        int i = 0;
        while (i < 100){
            i ++;
            if (i%10 == 0){
                System.out.println();
                continue;                       //每当遇到能被10整除的数时换行之后自动跳过本次循环，但是后续循环还会继续运行。
            }
            System.out.print(i+"\t");
        }
    }
}

```

输出结果：输出时遇到能被10整除的数自动跳过。

![输出结果](https://img30.360buyimg.com/pop/jfs/t1/101932/7/25404/115745/6228ceebEcd5cb408/370e34fed6ef0449.png)

- 拓展：关于goto关键字
  - goto关键字很早就在程序设计中出现，但仍是Java的一个保留字，并未在语言中得到正式使用；Java没有goto，然而我们在break和continue这两个关键字上，可以看到goto的影子------带标签的break和continue
  - 标签是指后面跟一个冒号的标识符，例如：label；
  - 对Java来说唯一用到标签的地方是在循环语句之前，而在循环之前设置标签的唯一理由是：我们希望在其中嵌套另一个循环，由于break和continue关键字通常只中断当前循环，但若随同标签使用，他们就会中断到存在标签的地方。

### 联系

```markdown
设计一个程序打印出一个三角形。
**********
****  ****
***    ***
**      **
*        *

```

```java
package com.xiheya.struct;

/**
 * @Author {xiheya}
 * @Date: 2022/03/11/ 15:56
 * @Description
 */
public class TestDemo01 {
    public static void main(String[] args) {
        for (int i = 1; i <= 5; i++) {
            for (int j = 5; j >= i; j--) {
                System.out.print("*");
            }
            for (int k = 1; k < i; k++) {
                System.out.print("  ");
            }
            for (int k = 5; k >= i; k--) {
                System.out.print("*");
            }
            System.out.println();
        }
    }
}

```

