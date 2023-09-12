---
title: Kotlin入门
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
swiper_index: 1
top_group_index: 1
background: '#fff'
cover: 'https://pic.ziyuan.wang/2023/09/11/guest_566671d879c50.jpg'
ai: >-
  本文介绍了从变量与函数、程序的逻辑控制、面向对象编程、Lambda编程、空指针检查以及Kotlin中的小魔术六个章节介绍了Kotlin入门语法。还介绍了Kotlin中与Java不同的变量定义方式和类型推导机制，以及Kotlin中出色的类型推导机制。同时还讲解了Kotlin中函数的定义和使用，包括函数的参数、返回值和可选参数等。此外，本文还介绍了Kotlin中条件语句和循环语句的实现方式，以及Kotlin中强大的when语句。最后，本文通过示例演示了如何使用单行代码函数的语法糖来简化代码。
abbrlink: add4ecd9
date: 2023-09-11 15:56:35
top_img:
tags: 
  - Kotlin
  - 编程入门
keywords:
description:
password:
message:
updated:
comments:
toc:
toc_number:
toc_style_simple:
copyright:
copyright_author:
copyright_author_href:
copyright_url:
copyright_info:
mathjax:
katex:
aplayer:
highlight_shrink:
aside:
---
# Kotlin入门

## 一、变量和函数

### 1.1 变量

- 在**Kotlin**中定义变量的方式和**Java**区别很大，在**Java**中如果想要定义一个变量，需要在变量前面声明这个变量的类型，比如说`int a`表示`a`是一个整型变量，`String b`表示`b`是一个字符串变量。而**Kotlin**中定义一个变量，只允许在变量前声明两种关键字：`val`和 `var`。
  - `val`（**value**的简写）用来声明一个不可变的变量，这种变量在初始赋值之后就再也不能重新赋值，对应**Java**中的`final`变量。
  - `var`（**variable**的简写）用来声明一个可变的变量，这种变量在初始赋值之后仍然可以再被重新赋值，对应**Java**中的**非**`final`变量。
- 仅仅使用`val`或者`var`来声明一个变量，那么编译器怎么能知道这个变量是什么类型呢？这也是Kotlin比较有特色的一点，它拥有出色的类型推导机制。
- 🌰举个栗子

```kotlin
fun main(){
    val a = 10
    println("a = " + a)
}
```

- 在上述代码中，我们使用`val`关键字定义了一个变量`a`，并将它赋值为**10**，这里`a`就会被自动推导成整型变量。因为既然你要把一个整数赋值给`a`，那么`a`就只能是整型变量，而如果你要把一个**字符串**赋值给`a`的话，那么`a`就会被自动推导成**字符串变量**，这就是**Kotlin**的**类型推导机制**。
- 但是**Kotlin**的类型推导机制并不总是可以正常工作的，比如说如果我们对一个变量延迟赋值的话，**Kotlin**就无法自动推导它的类型了。这时候就需要显式地声明变量类型才行，**Kotlin**提供了对这一功能的支持，语法如下所示:

```kotlin
val a: Int = 10
```

- 在上述代码中我们显式的声明了变量 `a` 为 `Int` 类型，这个时候 **Kotlin**就不会再进行类型推导了。如果此时我们再尝试将一个 **字符串**复制给 `a`，那么编译器会抛出一个类型不匹配的异常。
- **Kotlin**中`Int`的首字母是大写的，而**Java**中`int`的首字母是小写的。不要小看这一个字母大小写的差距，这表示**Kotlin**完全抛弃了**Java**中的基本数据类型，全部使用了对象数据类型。在**Java**中`int`是关键字，而在**Kotlin**中`Int`变成了一个类，它拥有自己的方法和继承结构。下表列出了**Java**中的每一个基本数据类型在**Kotlin**中对应的对象数据类型。

| Java基本数据类型 | Kotlin对象数据类型 | 数据类型说明 |
| :--------------: | :----------------: | :----------: |
|       int        |        Int         |     整型     |
|       long       |        Long        |    长整型    |
|      short       |       Short        |    短整型    |
|      float       |       Float        | 单精度浮点型 |
|      double      |       Double       | 双精度浮点型 |
|     boolean      |      Boolean       |    布尔型    |
|       char       |        Char        |    字符型    |
|       byte       |        Byte        |    字节型    |

- 在**Java**中，除非你主动在变量前声明了`final`关键字，否则这个变量就是可变的。然而这并不是一件好事，当项目变得越来越复杂，参与开发的人越来越多时，你永远不知道一个可变的变量会在什么时候被谁给修改了，即使它原本不应该被修改，这就经常会导致出现一些很难排查的问题。因此，一个好的编程习惯是，除非一个变量明确允许被修改，否则都应该给它加上`final`关键字。
- 但是，不是每个人都能养成这种良好的编程习惯。我相信至少有**90%**的**Java**程序员没有主动在变量前加上`final`关键字的意识，仅仅因为**Java**对此是不强制的。因此，**Kotlin**在设计的时候就采用了和**Java**完全不同的方式，提供了`val`和`var`这两个关键字，必须由开发者主动声明该变量是可变的还是不可变的。
- **小诀窍**：就是永远优先使用`val`来声明一个变量，而当`val`没有办法满足你的需求时再使用`var`。这样设计出来的程序会更加健壮，也更加符合高质量的编码规范。

### 1.2 函数

- **函数是用来运行代码的载体**，我们可以在一个函数里编写很多行代码，当运行这个函数时，函数中的所有代码会全部运行。就像`main()`函数就是一个函数，只不过它比较特殊，是程序的入口函数，即程序一旦运行，就是从`main()`函数开始执行的。
- 但是只有一个main()函数的程序显然是很初级的，和其他编程语言一样，`Kotlin`也允许我们自由地定义函数，语法规则如下：

```kotlin
fun methodName (param1: Int, param2: Int): Int{
    return 0
}
```

- 首先`fun`（**function**的简写）是定义函数的关键字，无论你在 `Kotlin` 中定义什么函数，都一定要使用`fun`来声明。
- 其次这里的`methodName`是函数名，我们可以给它取任何名字，但是最好能够做到**见文知意**。良好的编程习惯是函数名最好要有一定的意义，能表达这个函数的作用是什么。
- 函数名后面紧跟着一对括号，里面可以声明该函数接收什么参数，**参数的数量可以是任意多个**，例如上述示例就表示该函数接收两个`Int`类型的参数。**参数的声明格式是“参数名: 参数类型”**，其中**参数名也是可以随便定义**的，这一点和函数名类似。如果不想接收任何参数，那么写一对空括号就可以了。
- **参数括号后面的那部分是可选的，用于声明该函数会返回什么类型的数据**，上述示例就表示该函数会返回一个`Int`类型的数据。**如果你的函数不需要返回任何数据，这部分可以直接不写。**
- 最后**两个大括号之间的内容就是函数体**了，我们可以在这里编写一个函数的具体逻辑。由于上述示例中声明了该函数会返回一个`Int`类型的数据，因此在函数体中我们简单地返回了一个0。

最后我们再来学习一个**Kotlin**函数的语法糖：当一个函数中只有一行代码时，**Kotlin**允许我们不必编写函数体，可以直接将唯一的一行代码写在函数定义的尾部，中间用等号连接即可。

- 🌰举个栗子：我们现在编写一个返回两数之间最大数的函数，我们可以这样写

```kotlin
fun largerNumber(a: Int, b: Int): Int{
    return max(a, b)
}
```

- 在这个函数中，函数体内只有一行代码，此时我们可以不写函数体，简化如下：

```kotlin
fun largerNumber(a: Int, b: Int): Int = max(a, b)
```

- 使用这种语法，`return`关键字也可以省略了，等号足以表达返回值的意思。另外，之前还讲过**Kotlin**出色的类型推导机制，在这里它也可以发挥重要的作用。由于`max()`函数返回的是一个`Int`值，而我们在`largerNumber()`函数的尾部又使用等号连接了`max()`函数，因此**Kotlin**可以推导出`largerNumber()`函数返回的必然也是一个`Int`值，这样就不用再显式地声明返回值类型了，代码可以进一步简化成如下形式：

```kotlin
fun largerNumber(a: Int, b: Int) = max(a, b)
```

---

## 二、程序的逻辑控制

程序的执行语句主要分为3种：顺序语句、条件语句和循环语句。顺序语句很好理解，就是代码一行一行地往下执行就可以了，但是这种“愣头青”的执行方式在很多情况下并不能满足我们的编程需求，这时就需要引入条件语句和循环语句了

**Kotlin**的条件语句主要有两种实现方式：`if` 和 `when`

### 2.1 条件语句

#### 2.1.1 if条件语句

- 首先学习`if`，**Kotlin**中的`if`语句和**Java**中的`if`语句几乎没有任何区别
- 🌰举个栗子：

```kotlin
fun largerNumber(a: Int, b: Int): Int{
    if(a > b){
        return a
    } else {
        return b
    }
}
```

- 也可以这样写：

```kotlin
fun largerNumber(a: Int, b: Int): Int{
    val value: Int = if(a > b){
        a
    } else {
        b
    }
    return value
}
```

- 为什么可以写成上述代码呢？这是因为**Kotlin**中的`if`语句相比于**Java**有一个额外的功能，它是可以有返回值的，返回值就是`if`语句每一个条件中最后一行代码的返回值。
- 结合之前讲的语法糖，我们可以再简化一下：

```kotlin
fun largerNumber(a: Int, b: Int) = if(a > b){
    a
} else {
    b
}
```

- 但是上述代码还不是最精简的，我们甚至可以这样写：

```kotlin
fun largerNumber(a: Int, b: Int) = if (a > b) a else b
```

#### 2.1.2 when条件语句

- 接下来我们开始学习`when`。**Kotlin**中的`when`语句有点类似于**Java**中的`switch`语句，但它又远比`switch`语句强大得多；
- 首先，在**Java**中`switch`只能传入整型或短于整型的变量作为条件，JDK 1.7之后增加了对字符串变量的支持，但如果你的判断逻辑使用的并非是上述几种类型的变量,`switch`就不再适用了；其次，`switch`中的每个`case`条件都要在最后主动加上一个`break`，否则执行完当前`case`之后会依次执行下面的`case`，这一特性曾经导致过无数奇怪的bug，就是因为有人忘记添加`break`；
- 而 **Kotlin**中的 `when`语句不仅解决了上述痛点，还增加了许多更为强大的新特性，有时它比if语句还要简单好用。
- 🌰 举个栗子
- 现在我们编写一个demo,输入一个单词，返回其数字。我们先用if语句来实现:

```kotlin
fun getNumber(words: String) = if( words = "one"){
    1
} else if(words = "two"){
    2
} else if(words = "three"){
    3
} else if(words = "four"){
    4
} else if(words = "five"){
    5
} else{
    -1
}
```

- 虽然上述代码确实可以实现我们想要的功能，但是写了这么多的if和else，我们会发现代码很冗余。没错，当你的判断条件非常多的时候，就是应该考虑使用when语句的时候，现在我们将代码改成如下写法：

```kotlin
fun getNumber(words: String) = when (words) {
    "one" -> 1
    "two" -> 2
    "three" -> 3
    "four" -> 4
    "five" -> 5
    else -> -1
}
```

- 在 **Kotlin **中 `when`语句和 `if` 语句一样都有返回值，所以我们仍然可以使用单行代码函数的语法糖；
- 在 **Kotlin**中 `when`语句允许传入一个任意类型的参数，然后可以在`when`的结构体中定义一系列的条件，格式是：

```tex
匹配值 -> {执行逻辑}
```

- 当你的执行逻辑只有一行代码时，{ }可以省略。除了精确匹配之外，`when`语句还允许进行类型匹配。那么什么是类型匹配呢？
- 🌰 举个栗子

```kotlin
fun checkNumber(num: Number) {
    when(num){
        is Int -> println("number is Int")
        is Double -> println("number is Double")
        else -> println("number not support")
    }
}
```

- 上述代码中，`is`关键字就是类型匹配的核心，它相当于**Java**中的`instanceof`关键字。由于`checkNumber()`函数接收一个`Number`类型的参数，这是**Kotlin**内置的一个抽象类，像`Int`、`Long`、`Float`、`Double`等与数字相关的类都是它的子类，所以这里就可以使用类型匹配来判断传入的参数到底属于什么类型，如果是`Int`型或`Double`型，就将该类型打印出来，否则就打印不支持该参数的类型。
- 哦对了，在 **Kotlin** 中 `when`语句还可以不传参数，🌰 举个栗子

```kotlin
fun getNumber(words: String) = when {
    words == "one" -> 1
    words == "two" -> 2
    words == "three" -> 3
    words == "four" -> 4
    words == "five" -> 5
    else -> -1
}
```

### 2.2 循环语句

学完条件语句后，接下来我们学习 **Kotlin** 中的循环语句。

- 熟悉Java的人应该都知道，**Java**中主要有两种循环语句：`while`循环和`for`循环。而**Kotlin**也提供了`while`循环和`for`循环，其中`while`循环不管是在语法还是使用技巧上都和**Java**中的`while`循环没有任何区别。所以我们直接学习 `for`循环。
- **Kotlin**在**for**循环方面做了很大幅度的修改，**Java**中最常用的**for-i**循环在**Kotlin**中直接被舍弃了，而**Java**中另一种**for-each**循环则被**Kotlin**进行了大幅度的加强，变成了**for-in**循环，所以我们只需要学习**for-in**循环的用法就可以了。
- 在开始学习for-in循环之前，先来学习一个区间的概念，因为这也是Java中没有的东西。我们可以使用如下Kotlin代码来表示一个区间：

```kotlin
val range = 0..10
```

这种语法结构虽然看上去挺奇怪的,但在**Kotlin**中，它是完全合法的。上述代码表示创建了一个0到10的区间，并且**两端都是闭区间**，这意味着0到10这两个端点都是包含在区间中的，用数学的方式表达出来就是**[0, 10]**。

- 其中，`..`是创建两端闭区间的关键字，在`..`的两边指定区间的左右端点就可以创建一个区间了。
- 有了区间之后，我们就可以通过`for-in`循环来遍历这个区间.
- 🌰 举个栗子：

```kotlin
fun main(){
    for (i in 1..10) {
        println(i)
    }
}
```

这就是`for-in`循环最简单的用法了，我们遍历了区间中的每一个元素，并将它打印出来。

但是在很多情况下，双端闭区间却不如单端闭区间好用。为什么这么说呢？相信你一定知道数组的下标都是从0开始的，一个长度为10的数组，它的下标区间范围是0到9，因此左闭右开的区间在程序设计当中更加常用。**Kotlin**中可以使用`until`关键字来创建一个左闭右开的区间，如下所示：

```kotlin
val range = 0 until 10
```

上述代码表示创建了一个0到10的**左闭右开区间**，它的数学表达方式是**[0, 10)**。

当然，我们在 **Kotlin** 中也可以创建一个降序区间，使用 `downTo`关键字，如下所示：

```kotlin
val range = 10 downTo 1
```

默认情况下，`for-in`循环每次执行循环时会在区间范围内递增1，相当于**Java** `for-i`循环中`i++`的效果，而如果你想跳过其中的一些元素，可以使用`step`关键字：🌰 举个栗子：

```kotlin
fun main(){
    for(i in 1..10 step 2){
        println(i)
    }
}
```

上述代码表示在遍历**[1, 10]**这个区间的时候，每次执行循环都会在区间范围内递增2，相当于`for-i`循环中`i = i + 2`的效果。当然 `downTo`关键字创建的降序区间也是可以使用 `step`关键字的。

总的来说，`for-in`循环并没有传统的`for-i`循环那样灵活，但是却比`for-i`循环要简单好用得多，而且足够覆盖大部分的使用场景。如果有一些特殊场景使用`for-in`循环无法实现的话，我们还可以改用`while`循环的方式来进行实现。

----

## 三、面向对象编程

和很多现代高级语言一样，`Kotlin`也是面向对象的，因此理解什么是**面向对象编程**对我们来说就非常重要了。不同于**面向过程**的语言（比如C语言），**面向对象的语言是可以创建类的**。**类就是对事物的一种封装**，比如说人、汽车、房屋、书等任何事物，我们都可以将它封装一个类，**类名通常是名词**。而类中又可以拥有自己的**字段和函数**，字段表示该类所拥有的属性，比如说人可以有姓名和年龄，汽车可以有品牌和价格，这些就属于类中的字段，**字段名通常也是名词**。而函数则表示该类可以有哪些行为，比如
说人可以吃饭和睡觉，汽车可以驾驶和保养等，**函数名通常是动词**。

通过这种类的封装，我们就可以在适当的时候创建该类的对象，然后调用对象中的字段和函数来满足实际编程的需求，这就是面向对象编程最基本的思想。当然，面向对象编程还有很多其他特性，如继承、多态等，但是这些特性都是建立在基本的思想之上的。

### 3.1 类与对象

在 **Kotlin**中 也是使用 `class`关键字来声明一个类的，这一点和**Java**一致；这里创建一个 `Person` 类，并且加入 字段（属性）和 函数（方法）

```kotlin
class Person {
    var name = ""
    var age = 0

    fun eat(){
        println("$name is eating. He is $age years old.")
    }
}
```

这里我们创建了 `name`和 `age` 字段（属性） 然后还定义了一个 `eat()` 函数（方法）并在其中打印了一句话。

接下来我们实例化一个 `Person`对象，**Kotlin**中实例化一个类的方式和**Java**是基本类似的，只是去掉了`new`关键字而已。之所以这么设计，是因为当你调用了某个类的构造函数时，你的意图只可能是对这个类进行实例化，因此即使没有`new`关键字，也能清晰表达出你的意图。**Kotlin**本着最简化的设计原则，将诸如`new`、行尾分号这种不必要的语法结构都取消了。

实例化对象：

```kotlin
val p = Person()
```

我们可以在 `main()`函数中对p对象进行一些操作，比如：

```kotlin
fun main(){
    val p = Person()
    p.name = "Charlie"
    p.age = 24
    p.eat
}
```

这就是面向对象编程最基本的用法了，简单概括一下，就是要**先将事物封装成具体的类**，然后将**事物所拥有的属性和能力**分别**定义成类中的字段和函数**，接下来对类进行实例化，再**根据具体的编程需求调用类中的字段和方法**即可。

### 3.2 继承与构造函数

#### 3.2.1 继承

继承—面向对象编程中另一个极其重要的特性。继承也是基于现实场景总结出来的一个概念，其实非常好理解。比如现在我们要定义一个`Student`类，每个学生都有自己的学号和年级，因此我们可以在`Student`类中加入`sno`和`grade`字段。但同时学生也是人呀，学生也会有姓名和年龄，也需要吃饭，如果我们在`Student`类中重复定义`name`、`age`字段和`eat()`函数的话就显得太过冗余了。这个时候就可以让`Student`类去继承`Person`类，这样`Student`就自动拥有了`Person`中的字段和函数，另外还可以定义自己独有的字段和函数。这就是面向对象编程中的继承思想。

接下来实现让 `Student`类继承 `Person` 类，我们要先做两件事：

第一件事：使 `Person`类可以被继承，对你没看错，就是先让父类可被继承。可能很多人会觉得奇怪，尤其是有**Java**编程经验的人。一个类本身不就是可以被继承的吗？为什么还要使`Person`类可以被继承呢？这就是**Kotlin**不同的地方，在**Kotlin**中任何一个非抽象类默认都是不可以被继承的，相当于**Java**中给类声明了`final`关键字。之所以这么设计，其实和`val`关键字的原因是差不多的，因为类和变量一样，最好都是不可变的，而一个类允许被继承的话，它无法预知子类会如何实现，因此可能就会存在一些未知的风险。**Effective Java**这本书中明确提到，如果一个类不是专门为继承而设计的，那么就应该主动将它加上`final`声明，禁止它可以被继承。

很明显，**Kotlin**在设计的时候遵循了这条编程规范，**默认所有非抽象类都是不可以被继承的**。之所以这里一直在说非抽象类，是因为抽象类本身是无法创建实例的，一定要由子类去继承它才能创建实例，因此抽象类必须可以被继承才行，要不然就没有意义了。

那我们应该如何让 `Person`类可被继承呢？其实很简单！只需要在类前面加上 `open`关键字就可以啦！如下：

```kotlin
open class Person{
    ...
}
```

加上`open`关键字之后，我们就是在主动告诉**Kotlin**编译器，`Person`这个类是专门为继承而设计的，这样`Person`类就允许被继承了。

第二件事： 让 `Student`类继承 `Person`类，在Java中继承的关键字是`extends`，而在**Kotlin**中变成了一个冒号。写法如下：

```kotlin
class Student : Person(){
    var sno = ""
    var grade = 0
}
```

继承的写法如果只是替换一下关键字倒也挺简单的，但是为什么`Person`类的后面要加上一对括号呢？**Java**中继承的时候好像并不需要括号。这对括号还涉及 `Kotlin`中 主构造函数、次构造函数等方面的知识.

#### 3.2.2 构造函数

任何一个面向对象的编程语言都会有构造函数的概念，**Kotlin**中也有，但是**Kotlin**将构造函数分成了两种：**主构造函数**和**次构造函数**。

##### 3.2.2.1 主构造函数

**主构造函数将会是我们最常用的构造函数**，每个类默认都会有一个不带参数的主构造函数，当然也可以显式地给它指明参数。主构造函数的特点是没有函数体，直接定义在类名的后面即可。比如下面这种写法：

```kotlin
class Student (val sno: String, val grade: Int) : Person(){
    ...
}
```

这里我们将学号和年级这两个字段都放到了主构造函数当中，这就表明在对`Student`类进行实例化的时候，**必须**传入构造函数中要求的所有参数。比如：

```kotlin
val student = Student("123", 5)
```

这样我们就创建了一个`Student`的对象，同时指定该学生的学号是`123`，年级是`5`。另外，由于构造函数中的参数是在创建实例的时候传入的，不像之前的写法那样还得重新赋值，因此我们可以将参数全部声明成`val`。

当然啦，我们也可以在主构造函数当中写一些逻辑，**Kotlin**给我们提供了一个 `init`结构体，所有主构造函数的逻辑都可以写在里面：

```kotlin
class Student (val sno: String, val grade: Int) : Person() {
    init{
        println("sno is " + sno)
        println("grade is " + grade)
    }
}
```

上述代码中，我们打印了学号以及年级。到这里为止都还挺好理解的，但是这和那对括号又有什么关系呢？这就涉及了**Java**继承特性中的一个规定，**子类中的构造函数必须调用父类中的构造函数**，这个规定在**Kotlin**中也要遵守。

看一下**Student**类，现在我们声明了一个主构造函数，根据继承特性的规定，子类的构造函数必须调用父类的构造函数，可是主构造函数并没有函数体，我们怎样去调用父类的构造函数呢？你可能会说，在`init`结构体中去调用不就好了。这或许是一种办法，但绝对不是一种好办法，因为在绝大多数的场景下，我们是不需要编写`init`结构体的。
**Kotlin**当然没有采用这种设计，而是用了另外一种简单但是可能不太好理解的设计方式：括号。**子类的主构造函数调用父类中的哪个构造函数，在继承的时候通过括号来指定。**因此再来看一遍这段代码，你应该就能理解了吧。

```kotlin
class Student (val sno: String, val grade: Int) : Person(){
    ...
}
```

在这里，`Person`类后面的一对空括号表示`Student`类的主构造函数在初始化的时候会调用`Person`类的无参数构造函数，即使在无参数的情况下，这对括号也不能省略。

此时如果我们更改一下 `Person`类，将姓名和年龄放到主构造函数中，如下：

```kotlin
open class Person (val name: String, val age: Int)
```

此时我们在`Student`再去使用空括号调用`Person`类的无参构造函数肯定会报错，因为此时`Person`类的主构造函数需要 `name`和 `age`两个参数。

要解决这个问题也很简单，给`Person`类的构造函数传入`name`和`age`字段就好了。可是问题又来了`Student`类中也没有这两个字段啊，其实我们可以在 `Student` 类的主构造函数中 加上 `name` 和 `age` 这两个参数，然后再将这两个参数传给`Person`类的构造函数就行了。如下：

```kotlin
class Student (val sno: String, val grade: Int, name: String, age: Int) : Person(name, age){
    ...
}
```

⭐**注意**，我们在`Student`类的主构造函数中增加`name`和`age`这两个字段时，不能再将它们声明成`val`，因为在主构造函数中声明成`val`或者`var`的参数将自动成为该类的字段，这就会导致和父类中同名的`name`和`age`字段造成冲突。因此，这里的`name`和`age`参数前面我们不用加任何关键字，让它的作用域仅限定在主构造函数当中即可。

现在可以通过如下代码来创建一个`Student`类的实例：

```kotlin
val student = Student("123", 5, "Charlie", 24)
```

##### 3.2.2.2 次构造函数

任何一个类只能有一个主构造函数，但是可以有多个次构造函数。次构造函数也可以用于实例化一个类，这一点和主构造函数没有什么不同，只不过它是有函数体的。其实我们几乎是用不到次构造函数的，Kotlin提供了一个给函数设定参数默认值的功能，基本上可以替代次构造函数的作用。
**Kotlin**规定，当一个类既有主构造函数又有次构造函数时，所有的次构造函数都必须调用主构造函数（包括间接调用）。

🌰举个栗子：

```kotlin
class Student(val sno: String, val grade: Int, name: String, age: Int){
    constructor(name: String, age: Int) : this("", 0, name, age){
    }
    constructor() : this("", 0)
}
```

次构造函数是通过`constructor`关键字来定义的，这里我们定义了两个次构造函数：第一个次构造函数接收`name`和`age`参数，然后它又通过`this`关键字调用了主构造函数，并将`sno`和`grade`这两个参数赋值成初始值；第二个次构造函数不接收任何参数，它通过`this`关键字调用了我们刚才定义的第一个次构造函数，并将`name`和`age`参数也赋值成初始值，由于第二个次构造函数间接调用了主构造函数，因此这仍然是合法的。

那么现在我们就拥有了3种方式来对`Student`类进行实体化，分别是通过**不带参数**的构造函数、通过带**两个参数**的构造函数和通过带**4个参数**的构造函数，对应代码如下所示：

```kotlin
val student1 = Student()
val student2 = Student("Charlie", 24)
val student3 = Student("123", 5, "Charlie", 24)
```

接下来我们就再来看一种非常特殊的情况：类中只有次构造函数，没有主构造函数。这种情况真的十分少见，但在**Kotlin**中是允许的。当一个类没有显式地定义主构造函数且定义了次构造函数时，它就是没有主构造函数的。代码如下：

```kotlin
class Student: Person{
    constructor(name: String, age: Int) : super(name, age){
        
    }
}
```

注意这里的代码变化，首先`Student`类的后面**没有显式地定义主构造函数**，同时又因为定义了次构造函数，所以现在`Student`类是没有主构造函数的。那么既然没有主构造函数，继承`Person`类的时候就**不用继承其主构造函数**，也就不需要再加上括号了。

另外，由于没有主构造函数，次构造函数只能直接调用父类的构造函数，上述代码也是将`this`关键字换成了`super`关键字。

### 3.3 接口

接口是用于实现多态编程的重要组成部分。我们都知道，**Java**是单继承结构的语言，任何一个类最多只能继承一个父类，但是却可以实现任意多个接口，**Kotlin**也是如此。

我们可以在接口中定义一系列的抽象行为，然后由具体的类去实现。下面还是通过具体的代码来学习一下，首先创建一个`Study`接口，并在其中定义几个学习行为。

```kotlin
interface Study {
    fun readBook()
    fun doHomework()
}
```

接下来就可以让`Student`类去实现`Study`接口

```kotlin
class Student(val sno: String, val grade: Int, name: String, age: Int) : Person(name, age), Study {
    fun info(){
        println("$name's sno is $sno he's grade is $grade")
    }

    override fun readBook() {
        println("$name is reading")
    }


    override fun doHomework() {
        println("$name is doing homework")
    }
}
```

熟悉**Java**的人一定知道，**Java**中继承使用的关键字是`extends`，实现接口使用的关键字是`implements`，而**Kotlin**中统一使用冒号，中间用`,`进行分隔。上述代码就表示`Student`类继承了`Person`类，同时还实现了`Study`接口。另外接口的后面不用加上括号，因为它没有构造函数可以去调用。Study接口中定义了`readBooks()`和`doHomework()`这两个待实现函数，因此`Student`类必须实现这两个函数。**Kotlin**中使用`override`关键字来重写父类或者实现接口中的函数，这里我们只是简单地在实现的函数中打印了一行信息。

接下来在 `main()`函数中调用如下代码：

```kotlin
fun main(args: Array<String>) {

    val student = Student("1232", 5, "Charlie", 24)
    student.eat()
    student.info()
    student.readBook()
    student.doHomework()
}
```

`Person`类如下：

```kotlin
open class Person(val name: String, val age: Int) {
    fun eat(){
        println("$name is eating. He is $age years old.")
    }
}
```

为了让接口的功能更加灵活，**Kotlin**还增加了一个额外的功能：允许对接口中定义的函数进行默认实现。其实**Java**在**JDK1.8**之后也开始支持这个功能了，因此总体来说，**Kotlin**和**Java**在接口方面的功能仍然是一模一样的。

对接口中的函数进行默认实现的具体实现如下（修改`Study`接口）：

```kotlin
interface Study {
    fun readBook()
    fun doHomework()
    fun sleep(){
        println("zZZZ sleeping... default implementation")
    }
}
```

在 `Study`接口中，我们新增了一个 `sleep()`函数，并且默认实现了。如果接口中的一个函数拥有了函数体，这个函数体中的内容就是它的默认实现。现在当一个类去实现`Study`接口时，只会强制要求实现`readBooks()`和`doHomework()`函数，而`sleep`函数则可以自由选择实现或者不实现，不实现时就会自动使用默认的实现逻辑。

现在回到`Student`类当中，你会发现如果我们删除了`doHomework()`和`readBooks()`函数，代码是会提示错误的，而删除`sleep()`函数则不会。

以上就是**Kotlin**面向对象编程中最主要的一些内容，接下来我们再学习一个和**Java**相比变化比较大的部分——`函数的可见性修饰`符。

熟悉**Java**的人一定知道，**Java**中有`public`、`private`、`protected`和`default`（什么都不写）这4种函数可见性修饰符。**Kotlin**中也有4种，分别是`public`、`private`、`protected`和`internal`，需要使用哪种修饰符时，直接定义在`fun`关键字的前面即可。接下来详细介绍一下**Java**和**Kotlin**中这些函数可见性修饰符的异同。

首先`private`修饰符在两种语言中的作用是一模一样的，都表示只对当前类内部可见。`public`修饰符的作用虽然也是一致的，表示对所有类都可见，但是在**Kotlin**中`public`修饰符是默认项，而在`Java`中`default`才是默认项。前面我们定义了那么多的函数，都没有加任何的修饰符，所以它们默认都是`public`的。`protected`关键字在**Java**中表示对当前类、子类和同一包路径下的类可见，在`Kotlin`中则表示只对当前类和子类可见。`Kotlin`抛弃了**Java**中的`default`可见性（同一包路径下的类可见），引入了一种新的可见性概念，只对同一模块中的类可见，使用的是`internal`修饰符。比如我们开发了一个模块给别人使用，但是有一些函数只允许在模块内部调用，不想暴露给外部，就可以将这些函数声明成`internal`。

|  修饰符   |                Java                |       kotlin       |
| :-------: | :--------------------------------: | :----------------: |
|  public   |             所有类可见             | 所有类可见（默认） |
|  private  |             当前类可见             |     当前类可见     |
| protected | 当前类、子类、同一包路径下的类可见 |  当前类、子类可见  |
|  default  |    同一包路径下的类可见（默认）    |         无         |
| internal  |                 无                 | 同一模块中的类可见 |

### 3.4 数据类与单例类

#### 3.4.1 数据类

在一个规范的系统架构中，数据类通常占据着非常重要的角色，它们用于将服务器端或数据库中的数据映射到内存中，为编程逻辑提供数据模型的支持。或许你听说过**MVC**、**MVP**、**MVVM**之类的架构模式，不管是哪一种架构模式，其中的**M**指的就是**数据类**。

数据类通常需要**重写**`equals()`、`hashCode()`、`toString()`这几个方法。其中，`equals()`方法用于判断两个数据类是否相等。`hashCode()`方法作为`equals()`的配套方法，也需要一起重写，否则会导致`HashMap`、`HashSet`等hash相关的系统类无法正常工作。`toString()`方法用于提供更清晰的输入日志，否则一个数据类默认打印出来的就是一行内存地址。

比如在 **Java**中 我们要实现一个手机数据类，我们要这样写：

```java
public class Cellphone{
    String brand;
    double price;
    
    public Cellphone(String brand, double price){
        this.brand = brand;
        this.price = price;
    }
    
    @Override
    public boolean equals(Object obj){
        if(obj instanceof Cellphone){
            Cellphone other = (Cellphone) obj;
            return other.brand.equals(brand) && other.price == price;
        }
        return false;
    }
    
    @Override
    public int hashCode(){
        return brand.hashCode() + (int) price;
    }
    
    @Override
    public String toString(){
        return "Cellphone(brand=" + band + ", price=" + price + ")";
    }
}
```

看上去挺复杂的吧？关键是这些代码还是一些没有实际逻辑意义的代码，只是为了让它拥有数据类的功能而已。而同样的功能使用**Kotlin**来实现就会变得极其简单:

```kotlin
data class Cellphone(val brand: String, val price: Double) {
}
```

对！你没有看错，在 **Kotlin**当中，当我们需要一个数据类的时候只需要在这个类前声明了data关键字。当在一个类前面声明了`data`关键字时，就表明你希望这个类是一个**数据类**，**Kotlin**会根据主构造函数中的参数帮你将`equals()`、`hashCode()`、`toString()`等固定且无实际逻辑意义的方法自动生成，从而大大减少了开发的工作量。

#### 3.4.2 单例类

掌握了数据类的使用技巧之后，接下来我们再来看另外一个Kotlin中特有的功能——**单例类**。

想必你一定听说过**单例模式**吧，这是**最常用、最基础的设计模式之一**，它可以用于**避免创建重复的对象**。比如我们希望某个类在全局最多只能拥有一个实例，这时就可以使用单例模式。当然单例模式也有很多种写法，这里演示一种最常见的Java写法：

```java
public class Singleton {
    private static Singleton instance;
    
    private Singleton(){}
    
    public synchronized static Singleton getInstance() {
        if(instance == null){
            intance = new Singleton();
        }
        return instance;
    }
    
    public void singletonTest(){
        System.out.println("singleTon is called");
    }
}
```

先来看下这段代码，为了禁止外部创建`Singleton`的实例，我们使用`private`关键字将 `Singleton`的构造函数私有化，然后给外部提供了一个`getInstance()`静态方法用于获取`Singleton`的实例。在`getInstance()`方法中，我们判断如果当前缓存的`Singleton`实例为`null`，就创建一个新的实例，否则直接返回缓存的实例即可，这就是单例模式的工作机制。

如果我们想调用单例类中的方法，也很简单，比如想调用上述的`singletonTest()`方法，就可以这样写：

```kotlin
Singleton singleton = Singleton.getInstance();
singleton.singletonTest();
```

虽然**Java**中的单例实现并不复杂，但是**Kotlin**明显做得更好，它同样是将一些固定的、重复的逻辑实现隐藏了起来，只暴露给我们最简单方便的用法。

在`Kotlin`中创建一个单例类的方式极其简单，只需要将`class`关键字改成`object`关键字即可。初始化代码如下所示：

```kotlin
object Singleton {
    fun singletonTest(){
        println("singletonTest is called. --kotlin")
    }
}
```

可以看到，在**Kotlin**中我们不需要私有化构造函数，也不需要提供`getInstance()`这样的静态方法，只需要把`class`关键字改成`object`关键字，一个单例类就创建完成了。而调用单例类中的函数也很简单，比较类似于**Java**中静态方法的调用方式：

```kotlin
Singleton.singletonTest()
```

这种写法虽然看上去像是静态方法的调用，但其实Kotlin在背后自动帮我们创建了一个Singleton类的实例，并且保证全局只会存在一个Singleton实例。

---

## 四、Lambda编程

可能很多**Java**程序员对于Lambda编程还比较陌生，但其实这并不是什么新鲜的技术。许多现代高级编程语言在很早之前就开始支持`Lambda`编程了，但是**Java**却直到**JDK 1.8**之后才加入了`Lambda`编程的语法支持。因此，大量早期开发的**Java**和**Android**程序其实并未使用`Lambda`编程的特性。

**Kotlin**从第一个版本开始就支持了`Lambda`编程，并且**Kotlin**中的`Lambda`功能极为强大，我甚至认为`Lambda`才是**Kotlin**的灵魂所在。

### 4.1 集合的创建和遍历

**集合的函数式API**是用来入门`Lambda`编程的绝佳示例，不过在此之前，我们得先学习创建集合的方式才行。

传统意义上的集合主要就是`List`和`Set`，再广泛一点的话，像`Map`这样的键值对数据结构也可以包含进来。`List`、`Set`和`Map`在**Java**中都是接口，`List`的主要实现类是`ArrayList`和`LinkedList`，`Set`的主要实现类是`HashSet`，`Map`的主要实现类是`HashMap`，熟悉**Java**的人对这些集合的实现类一定不会陌生。

现在我们提出一个需求，创建一个包含许多水果名称的集合。如果是在**Java**中你会怎么实现？我们首先想到的是创建一个 `ArrayList`实例，然后再将水果的名称一个个添加到集合中。当然啦，在 `kotlin`中我们也可以这么做。

```kotlin
val list = ArrayList<String>()
list.add("Apple")
list.add("Banana")
list.add("Orange")
list.add("pear")
list.add("grape")
```

但是这种初始化集合的方式比较烦琐，为此**Kotlin**专门提供了一个内置的`listOf()`函数来简化初始化集合的写法，如下所示：

```kotlin
val list = listof("Apple", "Banana", "Orange", "Pear", "Grape")
```

可以看到，这里仅用一行代码就完成了集合的初始化操作。之前在学习循环语句时提到过：`for-in`循环不仅可以用来遍历区间，还可以用来遍历集合。现在我们就尝试一下使用`for-in`循环来遍历这个水果集合：

```kotlin
fun main(){
    val list = listOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")

    for (fruit in list){
        println(fruit)
    }
}
```

不过需要注意的是，`listOf()`函数创建的是一个不可变的集合。你也许不太能理解什么叫作不可变的集合，因为在**Java**中这个概念不太常见。不可变的集合指的就是该集合只能用于读取，我们无法对集合进行添加、修改或删除操作。

至于这么设计的理由，和`val`关键字、类默认不可继承的设计初衷是类似的，可见**Kotlin**在不可变性方面控制得极其严格。那如果我们确实需要创建一个可变的集合呢？也很简单，使用`mutableListOf()`函数就可以了，示例如下：

```kotlin
fun main(){
    val list = listOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
    val listVar = mutableListOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")

    showlist(list)
    showList(listVar)
    listVar.add("Watermelon")
    showList(listVar)
}

fun showList(list: List<String>){
    println("===========List===========")
    for (l in list){
        println(l)
    }
    println("===========List===========")
}
```

前面我们介绍的都是List集合的用法，实际上`Set`集合的用法几乎与此一模一样，只是将创建集合的方式换成了`setOf()`和`mutableSetOf()`函数而已。大致代码如下：

```kotlin
fun main(){
    val set = setOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
    val setVar = mutableSetOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")

    showSet(set)
    showSet(setVar)
    setVar.add("Watermelon")
    showSet(setVar)
}

fun showSet(list: Set<String>){
    println("===========Set===========")
    for (l in list){
        println(l)
    }
    println("===========Set===========")
}
```

⭐**需要注意**，`Set`集合中是**不可以存放重复元素**的，**如果存放了多个相同的元素，只会保留其中一份**，这是和`List`集合最大的不同之处。

最后再来看一下`Map`集合的用法。Map是一种键值对形式的数据结构，因此在用法上和`List`、`Set`集合有较大的不同。传统的`Map`用法是先创建一个`HashMap`的实例，然后将一个个键值对数据添加到`Map`中。比如这里我们给每种水果设置一个对应的编号，就可以这样写：

```kotlin
fun main(){
    val map = HashMap<String, Int>()

    map.put("Apple",1)
    map.put("Banana",2)
    map.put("Orange",3)
    map.put("Pear",4)
    map.put("Grape",5)

    val hashMap = HashMap<String, Int>()
    hashMap["Apple"] = 1
    hashMap["Banana"] = 2
    hashMap["Orange"] = 3
    hashMap["Pear"] = 4
    hashMap["Grape"] = 5

    val map1 = mapOf<String, Int>("Apple" to 1, "Banana" to 2, "Orange" to 3, "Pear" to 4, "Grape" to 5)

    showMap(map)
    showMap(hashMap)
    showMap(map1)
}

fun showMap(map: Map<String, Int>){
    println("===========Map===========")
    for ((fruit, number) in map){
        println("fruit is $fruit, number is $number")
    }
    println("===========Map===========")
}
```

使用第一种写法，是因为这种写法和**Java**语法是最相似的，因此可能最好理解。但其实在**Kotlin**中并不建议使用`put()`和`get()`方法来对`Map`进行添加和读取数据操作，而是更加推荐使用一种类似于数组下标的语法结构，比如向`Map`中添加一条数据就可以像第二种方法这么写：

```kotlin
val hashMap = HashMap<String, Int>()
    hashMap["Apple"] = 1
    hashMap["Banana"] = 2
    hashMap["Orange"] = 3
    hashMap["Pear"] = 4
    hashMap["Grape"] = 5
```

而从Map中读取一条数据就可以这么写：

```kotlin
val number = map["Apple"]
```

当然，这仍然不是最简便的写法，因为**Kotlin**毫无疑问地提供了一对`mapOf()`和`mutableMapOf()`函数来继续简化`Map`的用法。在`mapOf()`函数中，我们可以直接传入初始化的键值对组合来完成对Map集合的创建,如下：

```kotlin
val map1 = mapOf<String, Int>("Apple" to 1, "Banana" to 2, "Orange" to 3, "Pear" to 4, "Grape" to 5)
```

这里的键值对组合看上去好像是使用`to`这个关键字来进行关联的，但其实`to`并不是关键字，而是一个`infix`函数.

最后，遍历`Map`集合中的数据也是使用 `for-in`循环，如下：

```kotlin
fun showMap(map: Map<String, Int>){
    println("===========Map===========")
    for ((fruit, number) in map){
        println("fruit is $fruit, number is $number")
    }
    println("===========Map===========")
}
```

这段代码主要的区别在于，在`for-in`循环中，我们将Map的键值对变量一起声明到了一对括号里面，这样当进行循环遍历时，每次遍历的结果就会赋值给这两个键值对变量，最后将它们的值打印出来。

### 4.2 集合的函数式API

集合的函数式`API`有很多个，这里重点学习函数式`API`的语法结构，也就是`Lambda`表达式的语法结构。

首先实现一个需求：在一个水果集合里面找到单词最长的那个水果，我们可以这样写:

```kotlin
	val listLength = listOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
    var maxLengthFruit = ""
    for (fruit in listLength){
        if (fruit.length > maxLengthFruit.length){
            maxLengthFruit = fruit
        }
    }
    println("maxLengthFruit is $maxLengthFruit")
```

上述代码使用的是打擂台的方法找出单词最长的那个水果，我们还可以使用集合的函数式**API**，这可以让我们的功能变的更加容易:

```kotlin
val listLength = listOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
val maxLength = listLength.maxBy { it.length }
println("maxLength is $maxLength")
```

上面的代码只用了一行就找出了单词最长的那个水果，是怎么做到的呢?一起来学习下！

首先来看一下`Lambda`的定义，如果用最直白的语言来阐述的话，`Lambda`就是一小段可以作为参数传递的代码。从定义上看，这个功能就很厉害了，因为正常情况下，我们向某个函数传参时只能传入变量，而借助`Lambda`却允许传入一小段代码。这里两次使用了“一小段代码”这种描述，那么到底多少代码才算一小段代码呢？**Kotlin**对此并没有进行限制，但是通常不建议在`Lambda`表达式中编写太长的代码，否则可能会影响代码的可读性。

`Lambda`表达式的语法结构如下：

```tex
{参数名1: 参数类型, 参数名2: 参数类型 -> 函数体}
```

这是`Lambda`表达式最完整的语法结构定义。首先最外层是一对大括号，如果有参数传入到`Lambda`表达式中的话，我们还需要声明参数列表，参数列表的结尾使用一个`->`符号，**表示参数列表的结束以及函数体的开始**，函数体中可以编写任意行代码（虽然不建议编写太长的代码），并且最后一行代码会自动作为`Lambda`表达式的返回值。

当然，在很多情况下，我们并不需要使用`Lambda`表达式完整的语法结构，而是有很多种简化的写法。还是回到刚才找出最长单词水果的需求，前面使用的函数式**API**的语法结构看上去好像很特殊，但其实`maxBy`就是一个普通的函数而已，只不过它接收的是一个`Lambda`类型的参数，并且会在遍历集合时将每次遍历的值作为参数传递给`Lambda`表达式。`maxBy`函数的工作原理是根据我们传入的条件来遍历集合，从而找到该条件下的最大值，比如说想要找到单词最长的水果，那么条件自然就应该是单词的长度了。

理解了maxBy函数的工作原理之后，我们就可以开始套用刚才学习的Lambda表达式的语法结构，并将它传入到maxBy函数中了，如下所示：

```kotlin
val listLength = listOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
val lambda = {fruit: String -> fruit.length}
val maxLength = listLength.maxBy(lambda)
```

可以看到，`maxBy`函数实质上就是接收了一个`Lambda`参数而已，并且这个`Lambda`参数是完全按照刚才学习的表达式的语法结构来定义的，因此这段代码应该算是比较好懂的。
这种写法虽然可以正常工作，但是比较啰嗦，可简化的点也非常多，下面我们就开始对这段代码一步步进行简化。
首先，我们不需要专门定义一个`lambda`变量，而是可以直接将`lambda`表达式传入`maxBy`函数当中，因此第一步简化如下所示：

```kotlin
val listLength = listOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
val maxLength = listLength.maxBy({fruit: String -> fruit.length})
```

然后**Kotlin**规定，**当`Lambda`参数是函数的最后一个参数时，可以将`Lambda`表达式移到函数括号的外面**，如下所示：

```kotlin
val listLength = listOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
val maxLength = listLength.maxBy() {fruit: String -> fruit.length}
```

接下来，**如果`Lambda`参数是函数的唯一一个参数的话，还可以将函数的括号省略**：

```kotlin
val listLength = listOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
val maxLength = listLength.maxBy {fruit: String -> fruit.length}
```

由于**Kotlin**拥有出色的类型推导机制，`Lambda`表达式中的参数列表其实在大多数情况下不必声明参数类型，因此代码可以进一步简化成：

```kotlin
val listLength = listOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
val maxLength = listLength.maxBy {fruit -> fruit.length}
```

最后，当`Lambda`表达式的参数列表中只有一个参数时，也不必声明参数名，而是可以使用`it`关键字来代替，那么代码就变成了:

```kotlin
val listLength = listOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
val maxLength = listLength.maxBy {it.length}
```

怎么样？通过一步步推导的方式，我们就得到了和一开始那段函数式**API**一模一样的写法，是不是现在理解起来就非常轻松了呢？

接下来我们就再来学习几个集合中比较常用的函数式**API**,集合中的`map`函数是最常用的一种函数式API，它用于将集合中的每个元素都映射成一个另外的值，映射的规则在`Lambda`表达式中指定，最终生成一个新的集合。比如，这里我们希望让所有的水果名都变成大写模式，就可以这样写：

```kotlin
fun main(){
	val list = mutableListOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
	val newList = list.map { it.toUpperCase() }
    for (fruit in newList){
        println(fruit)
    }
}
```

`map`函数的功能非常强大，它可以按照我们的需求对集合中的元素进行任意的映射转换，上面只是一个简单的示例而已。除此之外，你还可以将水果名全部转换成小写，或者是只取单词的首字母，甚至是转换成单词长度这样一个数字集合，只要在`Lambda`表示式中编写你需要的逻辑即可。

我们再来学习另外一个比较常用的函数式**API**——`filter`函数。顾名思义，`filter`函数是用来过滤集合中的数据的，它可以单独使用，也可以配合刚才的`map`函数一起使用。

```kotlin
fun main(){
    val list = mutableListOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
    val newList = list.filter { it.length < 5 } .map { it.toUpperCase()) }
    for (fruit in newList){
        println(fruit)
    }
}
```

我们继续学习两个比较常用的函数式**API**——`any`和`all`函数。其中`any`函数用于判断集合中是否至少存在一个元素满足指定条件，`all`函数用于判断集合中是否所有元素都满足指定条件。由于这两个函数都很好理解，我们就直接通过代码示例学习了：

```kotlin
fun main(){
    val list = mutableListOf<String>("Apple", "Banana", "Orange", "Pear", "Grape")
    val anyResult = list.any{it.length <= 5}
    val allResult = list.all{it.length <= 5}
    println("anyResult is $anyResult, allResult is $allResult")
}
```

这里还是在`Lambda`表达式中将条件设置为5个字母以内的单词，那么`any`函数就表示集合中**是否存在5个字母以内的单词**，而`all`函数就表示集合中**是否所有单词都在5个字母以内**。

这样我们就将`Lambda`表达式的语法结构和几个常用的函数式**API**的用法都学习完了，虽然集合中还有许多其他函数式**API**，但是只要掌握了基本的语法规则，其他函数式API的用法只要看一看文档就能掌握了.

### 4.3 Java函数式API的使用

现在我们已经学习了**Kotlin**中函数式**API**的用法，但实际上在**Kotlin**中调用**Java**方法时也可以使用函数式**API**，只不过这是有一定条件限制的。具体来讲，如果我们在**Kotlin**代码中调用了一个**Java**方法，并且该方法接收一个**Java**单抽象方法接口参数，就可以使用函数式**API**。**Java**单抽象方法接口指的是接口中只有一个待实现方法，如果接口中有多个待实现方法，则无法使用函数式**API**。

🌰 举个栗子：

**Java**原生**API**中有一个最为常见的单抽象方法接口——`Runnable`接口。这个接口中只有一个待实现的`run()`方法，定义如下：

```java
public interface Runnable {
    void run();
}
```

根据前面的讲解，对于任何一个**Java**方法，只要它接收`Runnable`参数，就可以使用函数式**API**。那么什么**Java**方法接收了`Runnable`参数呢？这就有很多了，不过`Runnable`接口主要还是结合线程来一起使用的，因此这里我们就通过**Java**的线程类`Thread`来学习一下。

`Thread`类的构造方法中接收了一个`Runnable`参数，我们可以使用如下**Java**代码创建并执行一个子线程：

```java
new Thread(new Runnable(){
    @override
    public void run(){
        System.out.println("Thread is running");
    }
}).start();
```

⭐注意：这里使用了匿名类的写法，我们创建了一个`Runnable`接口的匿名类实例，并将它传给了`Thread`类的构造方法，最后调用`Thread`类的`start()`方法执行这个线程。

而如果将这段代码翻译成 `Kotlin` 版本，写法如下：

```kotlin
Thread(object: Runnable {
    override fun run(){
        println("Thread is running")
    }
}).start()
```

**Kotlin**中匿名类的写法和**Java**有一点区别，由于**Kotlin**完全舍弃了`new`关键字，因此创建匿名类实例的时候就不能再使用`new`了，而是改用了`object`关键字。这种写法虽然算不上复杂，但是相比于**Java**的匿名类写法，并没有什么简化之处。
但是别忘了，目前`Thread`类的构造方法是符合**Java**函数式**API**的使用条件的，下面我们就看看如何对代码进行精简，如下所示：

```kotlin
Thread(Runnable {
	println("Thread is running")
}).start()
```

这段代码明显简化了很多，既可以实现同样的功能，又不会造成任何歧义。因为`Runnable`类中只有一个待实现方法，即使这里没有显式地重写`run()`方法，**Kotlin**也能自动明白`Runnable`后面的`Lambda`表达式就是要在`run()`方法中实现的内容。

另外，如果一个**Java**方法的参数列表中有且仅有一个**Java**单抽象方法接口参数，我们还可以将接口名进行省略，这样代码就变得更加精简了：

```kotlin
Thread({
    println("thread is running")
}).start()
```

不过到这里还没有结束，和之前**Kotlin**中函数式**API**的用法类似，当`Lambda`表达式是方法的最后一个参数时，可以将`Lambda`表达式移到方法括号的外面。同时，如果`Lambda`表达式还是方法的唯一一个参数，还可以将方法的括号省略，最终简化结果如下：

```kotlin
Thread{
    println("Thread is running")
}.start()
```

----

## 五、空指针检查

某国外机构做了一个统计，**Android**系统上崩溃率最高的异常类型就是**空指针异常（NullPointerException）**。相信不只是**Android**，其他系统上也面临着相同的问题。若要分析其根本原因的话，我觉得主要**是因为空指针是一种不受编程语言检查的运行时异常，只能由程序员主动通过逻辑判断来避免**，但即使是最出色的程序员，也不可能将所有潜在的空指针异常全部考虑到。

先来看一段代码：

```java
public void doStudy(Study study){
    study.readBook();
    study.doHomework();
}
```

这段代码安全吗？不一定，因为这要取决于调用方传入的参数是什么，如果我们向`doStudy()`方法传入了一个`null`参数，那么毫无疑问这里就会发生空指针异常。因此，更加稳妥的做法是**在调用参数的方法之前先进行一个判空处理**，如下所示:

```java
public void doStudy(Study study){
    if(study != null){
        study.readBook();
        study.doHomework();
    }
}
```

这样就能保证不管传入的参数是什么，这段代码始终都是安全的。

由此可以看出，即使是如此简单的一小段代码，都有产生空指针异常的潜在风险，那么在一个大型项目中，想要完全规避空指针异常几乎是不可能的事情，这也是它高居各类崩溃排行榜首位的原因。

### 5.1 可空类型系统

然而，**Kotlin**却非常科学地解决了这个问题，它利用编译时判空检查的机制几乎杜绝了空指针异常。虽然编译时判空检查的机制有时候会导致代码变得比较难写，但是不用担心，**Kotlin**提供了一系列的辅助工具，让我们能轻松地处理各种判空情况。

是回到刚才的`doStudy()`函数，现在将这个函数再翻译回**Kotlin**版本，代码如下所示：

```kotlin
fun doStudy(study: Study){
    study.readBook()
    study.doHomework()
}
```

这段代码看上去和刚才的**Java**版本并没有什么区别，但实际上它是没有空指针风险的，因为Kotlin默认所有的参数和变量都不可为空，所以这里传入的`Study`参数也一定不会为空，我们可以放心地调用它的任何函数。如果你尝试向`doStudy()`函数传入一个**null**参数,则它会报错： `Null can not be a value of a non-null type Study`

也就是说，Kotlin将空指针异常的检查提前到了编译时期，如果我们的程序存在空指针异常的风险，那么在编译的时候会直接报错，修正之后才能成功运行，这样就可以保证程序在运行时期不会出现空指针异常了。

那如果我们的业务逻辑就是需要某个参数或者变量为空该怎么办呢？不用担心，**Kotlin**提供了另外一套可为空的类型系统，**只不过在使用可为空的类型系统时，我们需要在编译时期就将所有潜在的空指针异常都处理掉**，否则代码将无法编译通过。

那么**可为空的类型系统是什么样的呢**？很简单，就是在类名的后面加上一个问号。比如，**Int**表示**不可为空的整型**，而**Int?**就表示**可为空的整型**；**String**表示**不可为空的字符串**，而**String?**就表示**可为空的字符串**。

回到刚才的`doStudy()`函数，如果我们希望传入的参数可以为空，那么就应该**将参数的类型由`Study`改成`Study?`**

```kotlin
fun doStudy(study: Study?){
    study.readBook()
    study.doHomework()
}
```

可以看到，现在在调用`doStudy()`函数时传入`null`参数，就不会再提示错误了。但是，在`doStudy()`函数中调用参数的`readBooks()`和`doHomework()`方法时，却出现了一个红色下滑线的错误提示，这又是为什么呢？

由于我们将参数改成了可为空的`Study?`类型，此时调用参数的`readBooks()`和`doHomework()`方法都可能造成空指针异常，因此**Kotlin**在这种情况下不允许编译通过。我们只需要把空指针异常都处理掉就可以了，比如做个判断处理，如下所示：

```kotlin
fun doStudy(study: Study?){
    if(study != null){
        study.readBook()
        study.doHomework()
    }
}
```

现在代码就可以正常编译通过了，并且还能保证完全不会出现空指针异常。

为了在编译时期就处理掉所有的空指针异常，通常需要编写很多额外的检查代码才行。如果每处检查代码都使用`if`判断语句，则会让代码变得比较啰嗦，而且`if`判断语句还处理不了全局变量的判空问题。为此，**Kotlin**专门提供了一系列的辅助工具，使开发者能够更轻松地进行判空处理.接下来一一学习！

### 5.2 判空辅助工具

- 首先学习最常用的`?.`操作符。这个操作符的作用非常好理解，就是**当对象不为空时正常调用相应的方法，当对象为空时则什么都不做**。比如以下的判空处理代码：

```kotlin
if (a != null){
    a.doSomething()
}
```

这段代码使用 `?.`操作符就可以简化为：

```kotlin
a?.doSomething()
```

了解了 `?.`操作符后 `doStudy()`函数就可以优化成：

```kotlin
for doStudy(study: Study?){
    study?.readBook()
    study?.doHomework()
}
```

可以看到，这样我们就借助`?.`操作符将`if`判断语句去掉了。可能你会觉得使用`if`语句来进行判空处理也没什么复杂的，那是因为目前的代码还非常简单，当以后我们开发的功能越来越复杂，需要判空的对象也越来越多的时候，你就会觉得`?.`操作符特别好用了。

- 接下来再来学习另外一个非常常用的`?:`操作符。**这个操作符的左右两边都接收一个表达式，如果左边表达式的结果不为空就返回左边表达式的结果，否则就返回右边表达式的结果。**这个操作符和**三目运算符** `a ? b : c` 类似但又有差异.观察如下代码：

```kotlin
val c = if(a != null){
    a
}else{
    b
}
```

这段代码使用了 `?:`操作符就可以简化成：

```kotlin
val c = a ?: b
```

接下来通过一个具体的例子来结合使用 `?.` 和 `?:`这两个操作符，从而加深理解。

比如我们现在编写一个函数用来获得一段文本的长度，传统写法如下：

```kotlin
fun getTextLength(text: String?): Int{
    if(text != null){
        return text.length
    }
    return 0
}
```

由于文本是可能为空的，因此我们需要先进行一次判空操作，如果文本不为空就返回它的长度，如果文本为空就返回0。
这段代码看上去也并不复杂，但是我们却可以借助操作符让它变得更加简单，如下所示：

```kotlin
fun getTextLength(text: String?) = text?.length ?: 0
```

这里我们将`?.`和`?:`操作符结合到了一起使用，首先由于`text`是可能为空的，因此我们在调用它的`length`字段时需要使用`?.`操作符，而当`text`为空时，`text?.length`会返回一个`null`值，这个时候我们再借助`?:`操作符让它返回`0`。

不过**Kotlin**的空指针检查机制也并非总是那么智能，有的时候我们可能从逻辑上已经将空指针异常处理了，但是**Kotlin**的编译器并不知道，这个时候它还是会编译失败.

```kotlin
var content: String? = "hello"

fun main(){
    if(content != null){
        printUpperCase()
    }
}

fun printUpperCase(){
    val upperCase = content.toUpperCase()
    println(upperCase)
}
```

这里我们定义了一个可为空的全局变量`content`，然后在`main()`函数里先进行一次判空操作，当`content`不为空的时候才会调用`printUpperCase()`函数，在`printUpperCase()`函数里，我们将`content`转换为大写模式，最后打印出来。

看上去好像逻辑没什么问题，但是很遗憾，这段代码一定是无法运行的。因为`printUpperCase()`函数并不知道外部已经对`content`变量进行了非空检查，在调用`toUpperCase()`方法时，还认为这里存在空指针风险，从而无法编译通过。

在这种情况下，如果我们想要强行通过编译，可以使用非空断言工具，写法是在对象的后面加上`!!`，如下所示：

```kotlin
fun printUpperCase(){
    val upperCase = content!!.toUpperCase()
    println(upperCase)
}
```

这种写法就是在告知 **Kotlin**，我非常确信这里的对象不会为空，所以不用你来帮我做空指针检查了，如果出现问题，你可以直接抛出空指针异常，后果由我自己承担。虽然这样编写代码确实可以通过编译，但是当你想要使用非空断言工具的时候，最好提醒一下自己，是不是还有更好的实现方式。你最自信这个对象不会为空的时候，其实可能就是一个潜在空指针异常发生的时候。

- 最后我们再来学习一个比较与众不同的辅助工具 — `let`。 `let`既不是操作符，也不是什么关键字，而是一个**函数**。这个函数提供了函数式的 **API**的编程接口，并将原始调用对象作为参数传递到 `Lambda`表达式中。示例代码如下：

```kotlin
obj.let { obj2 ->
         // 具体代码实现
}
```

可以看到，这里调用了`obj`对象的`let`函数，然后`Lambda`表达式中的代码就会立即执行，并且这个`obj`对象本身还会作为参数传递到`Lambda`表达式中。不过，为了防止变量重名，这里我将参数名改成了`obj2`，但**实际上它们是同一个对象**，这就是`let`函数的作用。

`let`函数属于**Kotlin**中的标准函数，那这个`let`函数和空指针检查有什么关系呢？其实`let`函数的特性配合`?.`操作符可以在空指针检查的时候起到很大的作用。

就回到上面的 `doStudy()`函数当中，目前代码如下所示：

```kotlin
for doStudy(study: Study?){
    study?.readBook()
    study?.doHomework()
}
```

虽然这段代码我们通过`?.`操作符优化之后可以正常编译通过，但其实这种表达方式是有点啰嗦的，如果将这段代码准确翻译成使用`if`判断语句的写法，对应的代码如下：

```kotlin
for doStudy(study: Study?){
    if(study != null){
        study.readBook()
    }
    if(study != null){
        study.doHomework()
    }
}
```

也就是说，本来我们进行一次`if`判断就能随意调用`study`对象的任何方法，但受制于`?.`操作符的限制，现在变成了每次调用`study`对象的方法时都要进行一次`if`判断。
这个时候就可以结合使用`?.`操作符和`let`函数来对代码进行优化了，如下所示：

```kotlin
fun doStudy(study: Study?){
    study?.let { stu ->
        stu.readBook()
        stu.doHomeworl()
    }
}
```

上述代码的意思是，`?.`操作符表示对象为空时什么都不做，对象不为空时就调用`let`函数，而`let`函数会将`study`对象本身作为参数传递到`Lambda`表达式中，此时的`study`对象肯定不为空了，我们就能放心地调用它的任意方法了。

外还记得`Lambda`表达式的语法特性吗？**当`Lambda`表达式的参数列表中只有一个参数时**，可以不用声明参数名，直接使用`it`关键字来代替即可，那么代码就可以进一步简化成：

```kotlin
fun doStudy(study: Study?){
    study?.let {
        it.readBook()
        it.doHomework()
    }
}
```

`let`函数是可以处理全局变量的判空问题的，而`if`判断语句则无法做到这一点。比如我们将`doStudy()`函数中的参数变成一个全局变量，使用`let`函数仍然可以正常工作，但使用 `if`判断句则会提示错误。

---

## 六、Kotlin中的小魔术

其实就是一些**Kotlin**小技巧啦~

### 6.1 字符串内嵌表达式

`Kotlin`从一开始就支持了字符串内嵌表达式的功能，可以直接将表达式写在字符串里面，即使是构建非常复杂的字符串，也会变得轻而易举。

首先来看一下`Kotlin`中字符串内嵌表达式的语法规则：

```kotlin
"hello, ${obj.name}. nice to meet u"
```

可以看到，**Kotlin**允许我们在字符串里嵌入`${}`这种语法结构的表达式，并在运行时使用表达式执行的结果替代这一部分内容。另外，当表达式中仅有一个变量的时候，还可以将两边的大括号省略，如下所示：

```kotlin
"hello $name . nice to meet u"
```

### 6.2 函数的参数默认值

其实之前在学习次构造函数用法的时候我就提到过，次构造函数在**Kotlin**中很少用，因为**Kotlin**提供了给函数设定参数默认值的功能，它在很大程度上能够替代次构造函数的作用。
具体来讲，我们可以在定义函数的时候给任意参数设定一个默认值，这样当调用此函数时就不会强制要求调用方为此参数传值，在没有传值的情况下会自动使用参数的默认值。
给参数设定默认值的方式也很简单，观察如下代码：

```kotlin
fun printParams(num: Int, str: String = "hello"){
    println("num is $num, str is $str.")
}
fun main(){
    printParams(123)
    printParams(123,"Charlie")
}
```

可以看到，这里我们给`printParams()`函数的第二个参数设定了一个默认值，这样当调用`printParams()`函数时，可以选择给第二个参数传值，也可以选择不传，在不传的情况下就会自动使用默认值。

而如果我们想要 `printParams()`中的 `num`参数使用默认值应该怎么写呢？

```kotlin
fun printParams(num: Int = 100, str: String){
    println("num is $num, str is $str.")
}
```

函数像上面这样写是没有问题的，那么我们该如何调用呢？模仿刚才的写法肯定是行不通的，因为编译器会认为我们想把字符串赋值给第一个`num`参数，从而报类型不匹配的错误。

不过不用担心，**Kotlin**提供了另外一种神奇的机制，就是可以通过键值对的方式来传参，从而不必像传统写法那样按照参数定义的顺序来传参。比如调用`printParams()`函数，我们还可以这样写：

```kotlin
printParams(str = "world", num = 123)
```

此时哪个参数在前哪个参数在后都无所谓，**Kotlin**可以准确地将参数匹配上。而使用这种键值对的传参方式之后，我们就可以省略`num`参数了.

```kotlin
fun printParams(num: Int = 100, str: String){
    println("num is $num, str is $str.")
}

fun main(){
    printParams(str = "world")
}
```

现在我们已经掌握了如何给函数设定参数默认值，那么为什么说这个功能可以在很大程度上替代次构造函数的作用呢？

回忆一下当初我们学习次构造函数时所编写的代码：

```kotlin
class Student(val sno: String, val grade: Int, name: String, age: Int){
    constructor(name: String, age: Int) : this("", 0, name, age){
    }
    constructor() : this("", 0)
}
```

上述代码中有一个主构造函数和两个次构造函数，次构造函数在这里的作用是提供了使用更少参数来对Student类进行实例化的方式。无参的次构造函数会调用两个参数的次构造函数，并将这两个参数赋值成初始值。两个参数的次构造函数会调用4个参数的主构造函数，并将缺失的两个参数也赋值成初始值。
这种写法在Kotlin中其实是不必要的，因为我们完全可以通过只编写一个主构造函数，然后给参数设定默认值的方式来实现，代码如下所示：

```kotlin
class Student(val sno: String = "", val grade: Int = 0, name: String = "", age: Int = 0)
```

