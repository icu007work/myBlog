---
title: Java面向对象04
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva3.sinaimg.cn/large/87c01ec7gy1fsnqqm6x00j21kw0w0h09.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/f9a43fa4f09af.jpg
tags:
  - Java
  - Java面向对象
abbrlink: bc05bf18
date: 2022-08-25 22:41:51
authorAbout:
authorDesc:
keywords:
description:
---

## static 关键字

### 静态变量与静态方法

- 静态的变量  多线程中会用到(包含static关键字)
- 非静态的变量（不包含static关键字）
- 非静态方法中可以直接调用静态方法,而静态方法中无法调用非静态方法.
- 如果变量是静态变量我们就可以直接通过类名去访问这个变量,而非静态变量不可以直接通过类名来访问。
- 静态方法可以直接被调用，非静态方法需要实例化类对象之后，才可以通过对象来调用。

代码

```java
package com.xiheya.oop.demo07;

/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 20:44
 * @Description
 */
public class Student {
    private static int age;                 //静态的变量  多线程中会用到
    private double score;                   //非静态的变量
    public void run(){                      //非静态方法
        go();                               //非静态方法中可以直接调用静态方法
    }
    public static void go(){                //静态方法
        //run();                            而静态方法中无法调用非静态方法
    }
    public static void main(String[] args) {
        Student s1 = new Student();

        System.out.println(s1.score);       //可以看到非静态变量需要实例化类对象之后，才可以通过对象来访问。
        System.out.println(s1.age);

        System.out.println(Student.age);    //如果变量是静态变量我们就可以直接通过类名去访问这个变量
        //System.out.println(Student.score);  而非静态变量不可以直接通过类名来访问。
        go();                               //静态方法可以直接被调用
        //run();
        s1.run();                           //非静态方法需要实例化类对象之后，才可以通过对象来调用。
    }
}
```

---

### 静态代码块

**在程序运行过程中，先执行父类，再执行子类。先执行静态代码块（且静态代码块只执行一次），然后再执行匿名代码块。最后再执行构造方法。**

```java
package com.xiheya.oop.demo07;

/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 21:03
 * @Description
 */
public class Person {
    {
        System.out.println("我是匿名代码块");
    }
    static {
        System.out.println("我是静态代码块");
    }
    public Person(){
        System.out.println("我是构造方法");
    }

    public static void main(String[] args) {
        Person person = new Person();
    }
}
```

![静态代码块](https://img30.360buyimg.com/pop/jfs/t1/95087/34/23979/100331/622deca3E97877a38/3c8b3745c1e813d3.png)

---

### 静态导入包（不常用）

语法：

```java
import static java.lang.Math.random;
import static java.lang.Math.PI;
System.out.println(random());
System.out.println(PI);
```

当包被静态导入之后，在程序中就可以直接通过包内的方法名来调用这个方法。但是并不常用。

---

## 抽象类

- ***abstract*修饰符可以用来修饰方法也可以修饰类，如果修饰方法，那么该方法就是抽象方法；如果修饰类，那么该类就是抽象类。**
- **抽象类中可以没有抽象方法，但是有抽象方法的类一定要声明为抽象类。**
- 抽象类，不能用new关键字来创建对象，它是用来让子类继承的
- 抽象方法，只有方法的声明，没有方法的实现，它是用来让子类实现的。
- 子类继承抽象类，那么就必须要实现抽象类没有实现的抽象方法，否则该子类也要声明为抽象类。
- 抽象类存在的意义：抽象出来，提高开发效率。

```java
package com.xiheya.oop.demo08;

/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 21:27
 * @Description
 */
//用abstract 关键字修饰的类叫做抽象类，
public abstract class Action {
    //abstract 是一个约束，抽象方法 只有方法名字，没有方法的实现（没有方法体）  抽象方法由子类实现。
    public abstract void doSomething();
    //1.不能new这个抽象类，只能靠子类去实现
    //2.抽象类中可以写普通方法
    //3.抽象方法必须在抽象类中
    //抽象的抽象：约束。

    public Action() {
    }
}
```

继承抽象类，就一定要实现抽象类里面的抽象方法。不然这个类就变成抽象类，然后让子类来实现。

```java
package com.xiheya.oop.demo08;

/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 22:05
 * @Description
 */
//继承抽象类，就一定要实现抽象类里面的抽象方法
public class A extends Action{
    @Override
    public void doSomething() {

    }
}
```

---

## 接口

- 普通类：只有具体实现
- 抽象类：具体实现和规范（抽象方法）都有
- 接口：只有规范！自己无法写方法~专业的约束！约束和实现分离：面向接口编程~

- 接口就是规范，定义的是一组规则，体现了现实世界中“如果你是……则必须能……”的思想。**eg：如果你是天使，则必须能飞。如果你是汽车，则必须能跑。如果你是好人，则必须干掉坏人；如果你是坏人，则必须欺负好人。**
- **接口的本质是契约**，就像我们人间的法律一样，制定好后大家都遵守。
- 接口的精髓，是对对象的抽象，最能体现这一点的就是接口。为什么我们讨论设计模式都只针对具备了抽象能力的语言（比如c++、Java、c#等）就是因为设计模式所研究的，时间上就是如何合理的去抽象。

---

### 代码

```java
package com.xiheya.oop.demo09;

/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 22:36
 * @Description
 */

//抽象类是继承  extends
// 类也可以实现接口 implements 接口
// 实现了接口的类，就必须重写接口中的方法
// 接口就间接的实现了多继承。
public class UserServiceImpl implements UserService,TimeService{
    @Override
    public void timer() {

    }

    @Override
    public void add() {

    }

    @Override
    public void delete() {

    }

    @Override
    public void update() {

    }

    @Override
    public void query() {

    }
}

/*
public interface TimeService {
    void timer();
}

// interface 定义的关键字 ， 接口都需要有实现类
public interface UserService {
    //接口中所有属性类型 都是 public static final（但我们通常不在接口中定义属性）
    public static final int AGE = 99;
    //接口中所有定义其实默认都是抽象的 public abstract
    public abstract void add();
    void delete();
    void update();
    void query();
}

 */
```

---

### 作用

1. 接口是一个约束
2. 定义一些方法，让不同人实现。
3. 方法的默认属性是 public abstract
4. 常量的默认属性是public static final
5. 接口不能被实例化~接口中没有构造方法~
6. implements可以实现多个接口
7. 必须要重写接口中的方法

---

## 内部类

### 定义

内部类就是在一个类的内部再定义一个类，比如，A类中定义一个B类，那么B类相对于A类来说就称为内部类，而A类相对于B类来说就是外部类了。

### 分类

#### 成员内部类

```java
public class Application {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        inner.in();
        inner.gerAge();

    }
}

/*
public class Outer {
    private int age = 10;
    public void out(){
        System.out.println("I'm the outer");
    }

    public class Inner{
        public void in(){
            System.out.println("I'm the inner");
        }
        public void gerAge(){
            System.out.println(age);
        }
    }
}

*/
```

- 内部类可以获得外部类的私有属性

- 要通过外部类来实例化内部类。

  Outer outer = new Outer();
  Outer.Inner inner = outer.new Inner();

---

#### 静态内部类

```java
public class Application {
    public static void main(String[] args) {
        Outer outer = new Outer();
        Outer.Inner inner = outer.new Inner();
        inner.in();
        inner.gerAge();

    }
}

/*
public class Outer {
    private int age = 10;
    public void out(){
        System.out.println("I'm the outer");
    }

    public static class Inner1{
        public void in(){
            System.out.println("I'm the inner1");
        }
    }
}
*/
```

---

#### 局部内部类

```java
public class Outer {
        public void method(){
            //局部内部类
            class Inner{
                public void in(){
                    
                }
            }
        }
}
```

---

#### 匿名内部类

```java
public class Test {
    public static void main(String[] args) {
        //匿名内部类：没有名字初始化类，不用将实例保存到变量中
        new Apple().eat();
    }
}

class Apple{
    public void eat(){
        System.out.println("eat apple");
    }
}
```

