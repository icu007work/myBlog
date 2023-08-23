---
title: Java面向对象03
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva3.sinaimg.cn/large/87c01ec7gy1fsnqqm6x00j21kw0w0h09.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/f9a43fa4f09af.jpg
ai: 
  - 本文介绍了Java面向对象中的：封装、继承和多态，并且贴出了一些代码方便读者理解。
tags:
  - Java
  - Java面向对象
abbrlink: 22612abb
date: 2022-08-25 22:41:48
authorAbout:
authorDesc:
keywords:
description:
---

## 封装

- 该露的露，该藏的藏
  - 我们程序设计要追求“高内聚，低耦合”。高内聚就是类的内部数据操作细节自己完成，不允许外部干涉；低耦合：仅暴露少量的方法给外部使用
- 封装（数据的隐藏）
  - 通常，应禁止直接访问一个对象中数据的实际表示，而应通过操作接口来访问，这称为信息隐藏。
- 记住这句话：**属性私有，get/set**
- 封装的作用：
  - 1.提高程序的安全性，保护数据
  - 2.隐藏代码的实现细节
  - 3.统一接口
  - 4.系统可维护性增加了

代码：

```java
package com.xiheya.oop.demo04;

/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 12:00
 * @Description
 */
public class Student {
//    属性私有
    private String name;        //姓名
    private int id;            //学号
    private char sex;           //性别

    public Student() {
    }

    public Student(String name, int id, char sex) {
        this.name = name;
        this.id = id;
        this.sex = sex;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }
}
/*
public class Application {
    public static void main(String[] args) {
        Student Tom = new Student();
        System.out.println(Tom.getName());
        Tom.setName("Tom");
        System.out.println(Tom.getName());
        System.out.println(Tom.getId());
        Tom.setId(1234);
        System.out.println(Tom.getId());
    }
}
 */
```

运行结果：

![封装](https://img30.360buyimg.com/pop/jfs/t1/213402/8/14777/201552/622d7354Ebfb0538a/f6e46e96693a982f.png)

---

## 继承

- 继承的本质是对某一批类的抽象，从而实现对现实世界更好的建模。
- **extends**的意思是”扩展“。子类是父类的扩展。
- **Java中类只有单继承，没有多继承！**
- 继承是类和类之间的一种关系，除此之外，类和类之间的关系还有依赖、组合、聚合等。
- 继承关系的两个类，一个为子类（派生类）、一个为父类（基类）。子类继承父类，使用关键字extends来表示。
- object类
- super
- 方法重写

**子类继承了父类，就会拥有父类的全部方法，前提是方法属性为
在Java中所有类都默认直接或间接继承Object；快捷键：ctrl+H**

```java
package com.xiheya.oop.demo05;

/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 12:45
 * @Description
 */
//在Java中所有类都默认直接或间接继承Object
public class Person {
    
    public void speak(){
        System.out.println("say some thing");
    }
}
/*
//子类继承了父类，就会拥有父类的全部方法，前提是方法属性为
//快捷键：ctrl+H
public class Teacher extends Person{

}

public class Application {
    public static void main(String[] args) {
//        Student Tom = new Student();
//        System.out.println(Tom.getName());
//        Tom.setName("Tom");
//        System.out.println(Tom.getName());
//        System.out.println(Tom.getId());
//        Tom.setId(1234);
//        System.out.println(Tom.getId());
        Teacher teacher = new Teacher();
        teacher.speak();

    }
}


 */
```

![继承](https://img30.360buyimg.com/pop/jfs/t1/91867/18/23639/150759/622d908dEc5b63cc7/e997d4b885bc1b6e.png)

可以看到Teacher类没有speak方法，但是实例化的teacher对象却可以调用speak方法，原因就是Teacher继承了Person类，而Person类中定义了speak方法。

---

### super关键字：

super关键字可以在子类调用父类。

#### 代码

```java
package com.xiheya.oop.demo05;

/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 12:45
 * @Description
 */
//在Java中所有类都默认直接或间接继承Object
public class Person {
    public Person() {
        System.out.println("Person类的构造方法执行了");
    }

    public void speak(){
        System.out.println("say some thing");
    }
    public void print(){
        System.out.println("Person");
    }
}
/*
//子类继承了父类，就会拥有父类的全部方法，前提是方法属性为
//快捷键：ctrl+H
public class Teacher extends Person{
    public Teacher() {
        System.out.println("Teache的构造方法执行了");
    }

    public void test(){
        print();
        this.print();
        super.print();
    }
    public void print(){
        System.out.println("Teacher");
    }
}

public class Application {
    public static void main(String[] args) {
//        Student Tom = new Student();
//        System.out.println(Tom.getName());
//        Tom.setName("Tom");
//        System.out.println(Tom.getName());
//        System.out.println(Tom.getId());
//        Tom.setId(1234);
//        System.out.println(Tom.getId());
        Teacher teacher = new Teacher();
        teacher.speak();

    }
}


 */
```

#### 运行结果：

![super](https://img30.360buyimg.com/pop/jfs/t1/216992/40/14624/218209/622d97e7E8a269e74/ba7d7393e715a8e8.png)

![构造方法](https://img30.360buyimg.com/pop/jfs/t1/207922/37/19475/234785/622d98a0E30c65ddb/2813111d970d715e.png)

可以看到，当对象被实例化之后，会调用构造器方法，如果有父类则先调用父类的构造器方法。这是因为子类的构造方法中：默认添加了super();关键字，所以会先调用父类的构造器方法！同时，调用父类的构造器必须放在第一行。

---

#### super注意点

1. super调用父类的构造方法，必须在构造方法的第一个
2. super必须只能出现在子类的方法或者构造方法中
3. super和this不能同时调用构造方法

super与this的区别

1. 代表的对象不同：
   1. this：本身调用者这个对象
   2. super：代表父类对象的应用
2. 前提：
   1. this：没有继承也可以使用
   2. super：只能在继承条件才可以使用
3. 构造方法：
   1. this();本类的构造
   2. super():父类的构造

### 方法重写

**静态方法是类的方法，而非静态方法是对象的方法。** 有static时，对象调用的是自身类的方法，没有static时，对象调用的是自身对象的方法。

**静态方法和非静态方法差别很大**

1. 静态方法：方法的调用只和左边定义的数据类型有关
2. 非静态：非静态方法才存在重写。

**只有非静态的 public属性的方法才能被重写**

```java
package com.xiheya.oop.demo05;

/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 15:27
 * @Description
 */
public class B {
    public void test(){
        System.out.println("b--->test");
    }
}
/*
public class A extends B{
    public void test(){
        System.out.println("a--->test");
    }
}

public class Application {
    public static void main(String[] args) {
        A a = new A();
        B b = new A();
        a.test();
        b.test();
    }
}

 */
```

#### 代码

```java
package com.xiheya.oop.demo05;

/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 15:27
 * @Description
 */
public class B {
    public void test(){
        System.out.println("b--->test");
    }
}
/*
public class A extends B{
    public void test(){
        System.out.println("a--->test");
    }
}

public class Application {
    public static void main(String[] args) {
        A a = new A();
        B b = new A();
        a.test();
        b.test();
    }
}

 */

```

#### 运行结果

![方法重写](https://img30.360buyimg.com/pop/jfs/t1/213987/27/14642/174096/622da1edE609d5ba6/147aba0b6b34adc1.png)

---

#### 笔记

**重写：需要有继承关系，子类重写父类的方法！**

1. 方法名必须相同
2. 参数列表必须相同
3. 修饰符：范围可以扩大. public>protected>default>private
4. 抛出的异常：范围可以被缩小，但不能扩大。eg父类抛出的异常为：Exception，那么子类抛出的异常范围就需要比Exception要小。可以抛出为：ClassNotFoundException

**重写：子类的方法和父类必须要一致，方法体不同。**

**为什么要重写**：

1. 父类的功能，子类不一定需要，或者不一定满足

---

## 多态

- 动态编译：类型”可扩展性更强
- 多态即同一个方法可以根据发送对象的不同而采用多种不同的行为方式。
- 一个对象的实际类型是确定的，但可以指向对象的引用的类型有很多
- 多态存在的条件
  - 有继承关系
  - 子类重写父类方法
  - 父类引用指向子类对象
- **注意：多态是方法的多态，属性没有多态**
- instanceof

### 代码

```java
package com.xiheya.oop.demo06;

/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 15:59
 * @Description
 */
public class Person {
    public void run(){
        System.out.println("Person run");
    }
}
/*
public class Student extends Person{
    @Override
    public void run() {

        System.out.println("Student run");
    }

    public void eat(){
        System.out.println("Student eat");
    }
}


public class Application {
    public static void main(String[] args) {
        //一个对象的实际类型是确定的
        // 可以指向的引用类型就不确了 父类的引用可以指向子类
        // Student 能调用的方法都是自己的，或者继承父类的、
        Student s1 = new Student();
        //Person 父类型，可以指向子类，但是不能调用子类独有的方法
        Person s2 = new Student();
        Object s3 = new Student();
        //对象执行哪些方法，主要看对象左边的类型，和右边关系不大
        // 子类重写了父类的方法，则执行子类的方法。
        s1.run();
        s2.run();
        ((Student)s2).eat();
        s1.eat();


    }
}

 */
```

### 运行结果

![多态](https://img30.360buyimg.com/pop/jfs/t1/147128/13/24014/189407/622daf0dEbda502a0/b1b1cc4e335d0d70.png)

---

### 注意事项

1. 多态是方法的多态；属性没有多态。
2. 父类和子类，有联系 （类型转换异常--ClassCastException）
3. 存在条件：继承关系，方法需要重写，父类引用指向子类对象。Father f1 = new Son();
4. **哪些方法不能重写？**
   1. static方法，static代码块属于类，对象被创建时一同被执行
   2. **final：常量 被final修饰的方法不能被重写，被final修饰的类不能被继承；被final修饰的变量一经赋值后续不能更改。**
   3. private方法： 私有属性不能被继承

### instanceof关键字

#### 代码：

```java
package com.xiheya.oop;

import com.xiheya.oop.demo06.Teacher;
import com.xiheya.oop.demo06.Student;
import com.xiheya.oop.demo06.Person;


/**
 * @Author {xiheya}
 * @Date: 2022/03/13/ 11:59
 * @Description
 */
public class Application {
    public static void main(String[] args) {
        Student student = new Student();
        Person person = new Student();
        Object object = new Student();
        //X instanceof Y:能不能编译通过主要看X和Y是否存在父子关系。
        System.out.println(object instanceof Student);     //true
        System.out.println(object instanceof Person);       //true
        System.out.println(object instanceof Object);       //true
        System.out.println(object instanceof Teacher);      //false
        System.out.println(object instanceof String);       //false
        System.out.println("========================");

        System.out.println(person instanceof Student);      //true
        System.out.println(person instanceof Person);       //true
        System.out.println(person instanceof Object);       //true
        System.out.println(person instanceof Teacher);      //false
        //System.out.println(person instanceof String);     //编译报错
        System.out.println("========================");

        System.out.println(student instanceof Student);     //true
        System.out.println(student instanceof Person);      //true
        System.out.println(student instanceof Object);      //true
        //System.out.println(student instanceof Teacher);   //编译报错
        //System.out.println(student instanceof String);    //编译报错

        //类型之间的转换  父-------》子
        // 高-------------------低（强制转换）。
//        Person obj = new Student();
//        //student.go();报错： 因为Person类中没有go方法。需要将Person类型强制转换为Student类型
//        ((Student) obj).go();
//
//        //类型之间的转换  子-------》父
//        Student student = new Student();
//        //由Student类型转换为他的父类Person类型可以直接转换，但是可能会丢失一些自己本来的方法。
//        Person person = student;

    }
}
/*
1.父类引用指向子类的对象
2.把子类转换为父类：向上转型：（自动转换）
3.把父类转换为子类：向下转型：（强制转换）
4.方便方法的调用，减少代码重复率，简洁
 */
```

#### 运行结果

![instanceof](https://img30.360buyimg.com/pop/jfs/t1/144913/10/23289/20567/622dbb9aE35c557d0/d9d1e32d67c5dbf0.png)

---

### 强制转换

1. 父类引用指向子类的对象

2. 把子类转换为父类：向上转型：（自动转换）

3. 把父类转换为子类：向下转型：（强制转换）

4. 方便方法的调用，减少代码重复率，简洁

5. 由Student类型转换为他的父类Person类型可以直接转换，但是可能会丢失一些自己本来的方法。

6. student.go();报错： 因为Person类中没有go方法。需要将Person类型强制转换为Student类型

7. 类型之间的转换
   高-------------------低（强制转换）

   低-------------------高（自动转换）

代码：

```java
public class Application {
    public static void main(String[] args) {

        //类型之间的转换  父-------》子
        // 高-------------------低（强制转换）。
        Person obj = new Student();
        //student.go();报错： 因为Person类中没有go方法。需要将Person类型强制转换为Student类型
        ((Student) obj).go();

        //类型之间的转换  子-------》父
        Student student = new Student();
        //由Student类型转换为他的父类Person类型可以直接转换，但是可能会丢失一些自己本来的方法。
        Person person = student;

    }
}
/*
1.父类引用指向子类的对象
2.把子类转换为父类：向上转型：（自动转换）
3.把父类转换为子类：向下转型：（强制转换）
4.方便方法的调用，减少代码重复率，简洁
 */
```
