---
title: Kotlin 进阶
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
swiper_index: 1
top_group_index: 1
background: '#fff'
cover: 'https://pic2.ziyuan.wang/user/xiheya/2024/05/Kotlin-Advance_535ea90c69f3c.jpg'
ai:
  - >-
    这篇文章主要介绍了Kotlin编程语言中的标准函数和静态方法。首先，文章介绍了Kotlin中的标准函数，包括with、run和apply。这些函数都是Kotlin的内置函数，用于简化代码和提高代码的可读性。文章详细解释了with函数的用法。with函数接收两个参数：第一个参数可以是一个任意类型的对象，第二个参数是一个Lambda表达式。with函数会在Lambda表达式中提供第个参数对象的上下文，并使用Lambda表达式中的最后一行代码作为返回值返回。文章最后提供了一个示例，展示了如何使用with函数。在这个示例中，我们有一个水果列表，我们需要吃完所有的水果，并将结果打印出来。这个示例展示了如何使用with函来简化这个过程。
abbrlink: 4fa27f9d
date: 2024-05-11 15:40:52
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

## 一、标准函数和静态方法

### 1.1 标准函数with、run和apply

#### 1.1.1 with函数

with函数接收两个参数：第一个参数可以是一个任意类型的对象，第二个参数是一个Lambda表达式。with函数会在Lambda表达式中提供第一个参数对象的上下文，并使用Lambda表达式中的最后一行代码作为返回值返回。示例代码如下：

```kotlin
val result = with(obj){
    // obj的上下文
    "value" //with 函数的返回值
}
```

具体怎么使用呢？举个栗子🌰：我们有个fruit列表，现在我们需要吃完所有水果，并将结果打印出来。我们可以这样写：

```kotlin
val listFruit = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
val builder = StringBuilder()
builder.append("Start eating fruits. \n")
for (fruit in listFruit){
    builder.append(fruit).append("\n")
}
builder.append("Ate all fruits.")
val result = builder.toString()
println(result)
```

这段代码的逻辑很简单，就是使用StringBuilder来构建吃水果的字符串，最后将结果打印出来。我们连续调用了很多次builder对象的方法。其实这个时候就可以考虑使用with函数来让代码变得更加精简。如下所示：

```kotlin
val listWith = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
val resultWith = with(StringBuilder()){
    append("Start eating fruits.\n")
    for (fruit in listWith){
        append(fruit).append("\n")
    }
    append("Ate all fruits.")
    toString()
}
println(resultWith)
```

首先我们给with函数的第一个参数传入了一个StringBuilder对象，那么接下来整个Lambda表达式的上下文就会是这个StringBuilder对象。于是我们在Lambda表达式中就不用再像刚才那样调用builder.append()和builder.toString()方法了，而是可以直接调用append()和toString()方法。Lambda表达式的最后一行代码会作为with函数的返回值返回，最终我们将结果打印出来。

#### 1.1.2 run函数

run函数的用法和使用场景其实和with函数是非常类似的，只是稍微做了一些语法改动而已。首先run函数通常不会直接调用，而是要在某个对象的基础上调用；其次run函数只接收一个Lambda参数，并且会在Lambda表达式中提供调用对象的上下文。其他方面和with函数是一样的，包括也会使用Lambda表达式中的最后一行代码作为返回值返回。

```kotlin
val result = obj.run{
    // obj的上下文
    "value" //run函数的返回值
}
```

接下来使用run函数来改写一下上述代码：

```kotlin
val listWith = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
val resultRun = StringBuilder().run{
    append("Start eating fruits.\n")
    for (fruit in listWith){
        append(fruit).append("\n")
    }
    append("Ate all fruits.")
    toString()
}
println(resultRun)
```

总体来说变化非常小，只是将调用with函数并传入StringBuilder对象改成了调用StringBuilder对象的run方法，其他都没有任何区别，这两段代码最终的执行结果是完全相同的。

#### 1.1.3 apply函数

apply函数和run函数也是极其类似的，都要在某个对象上调用，并且只接收一个Lambda参数，也会在Lambda表达式中提供调用对象的上下文，但是apply函数无法指定返回值，而是会自动返回调用对象本身。

```kotlin
val result = obj.apply{
    // obj上下文
}
// result == obj
```

那么现在我们再使用apply函数来修改一下吃水果的这段代码，如下所示：

```kotlin
val listWith = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
val resultApply = StringBuilder().run{
    append("Start eating fruits.\n")
    for (fruit in listWith){
        append(fruit).append("\n")
    }
    append("Ate all fruits.")
}
println(resulApply.toString())
```

注意这里的代码变化，由于apply函数无法指定返回值，只能返回调用对象本身，因此这里的result实际上是一个StringBuilder对象，所以我们在最后打印的时候还要再调用它的toString()方法才行.

### 1.2 定义静态方法

静态方法在某些编程语言里面又叫作类方法，指的就是那种不需要创建实例就能调用的方法，所有主流的编程语言都会支持静态方法这个特性。

在Java中定义一个静态方法非常简单，只需要在方法上声明一个static关键字就可以了，如下所示：

```java
public class Util {
    public static void doAction(){
        System.out.println("do Action")
    }
}
```

这是一个非常简单的工具类，上述代码中的doAction()方法就是一个静态方法。调用静态方法并不需要创建类的实例，而是可以直接以Util.doAction()这种写法来调用。因而静态方法非常适合用于编写一些工具类的功能，因为工具类通常没有创建实例的必要，基本是全局通用的。

**但是和绝大多数主流编程语言不同的是，Kotlin却极度弱化了静态方法这个概念，想要在Kotlin中定义一个静态方法反倒不是一件容易的事。**

那么Kotlin为什么要这样设计呢？因为Kotlin提供了比静态方法更好用的语法特性，并且我们在上一节中已经学习过了，那就是单例类。

像工具类这种功能，在Kotlin中就非常推荐使用单例类的方式来实现，比如上述的Util工具类，如果使用Kotlin来实现的话就可以这样写：

```kotlin
object Util{
    fun doAction(){
        println("do action")
    }
}
```

虽然这里的doAction()方法并不是静态方法，但是我们仍然可以使用Util.doAction()的方式来调用，这就是单例类所带来的便利性。

不过，使用单例类的写法会将整个类中的所有方法全部变成类似于静态方法的调用方式，而如果我们只是希望让类中的某一个方法变成静态方法的调用方式该怎么办呢？这个时候就可以使用刚刚在最佳实践环节用到的**companion object**了，示例如下：

```kotlin
class Util{
    fun doAction(){
        println("do action")
    }
    companion object{
        fun doAction2() {
            println("do action2")
        }
    }
}
```

这里首先我们将Util从单例类改成了一个普通类，然后在类中直接定义了一个doAction1()方法，又在companion object中定义了一个doAction2()方法。现在这两个方法就有了本质的区别，因为doAction1()方法是一定要先创建Util类的实例才能调用的，而doAction2()方法可以直接使用Util.doAction2()的方式调用。

不过，doAction2()方法其实也并不是静态方法，companion object这个关键字实际上会在Util类的内部创建一个伴生类，而doAction2()方法就是定义在这个伴生类里面的实例方法。只是Kotlin会保证Util类始终只会存在一个伴生类对象，因此调用Util.doAction2()方法实际上就是调用了Util类中伴生对象的doAction2()方法。

由此可以看出，Kotlin确实没有直接定义静态方法的关键字，但是提供了一些语法特性来支持类似于静态方法调用的写法，这些语法特性基本可以满足我们平时的开发需求了。然而如果你确确实实需要定义真正的静态方法， Kotlin仍然提供了两种实现方式：**注解和顶层方法**。下面我们来逐个学习一下。

#### 1.2.1 注解

先来看注解，前面使用的单例类和companion object都只是在语法的形式上模仿了静态方法的调用方式，实际上它们都不是真正的静态方法。因此如果你在Java代码中以静态方法的形式去调用的话，你会发现这些方法并不存在。而如果我们给单例类或companion object中的方法加上@JvmStatic注解，那么Kotlin编译器就会将这些方法编译成真正的静态方法，如下所示：

```kotlin
class Util{
    fun doAction(){
        println("do action")
    }
    companion object{
        @JvmStatic
        fun doAction2() {
            println("do action2")
        }
    }
}
```

注意，@JvmStatic注解只能加在单例类或companion object中的方法上，如果你尝试加在一个普通方法上，会直接提示语法错误。由于doAction2()方法已经成为了真正的静态方法，那么现在不管是在Kotlin中还是在Java中，都可以使用Util.doAction2()的写法来调用了。

#### 1.2.2 顶层方法

再来看顶层方法，顶层方法指的是那些没有定义在任何类中的方法，比如我们在上一节中编写的main()方法。Kotlin编译器会将所有的顶层方法全部编译成静态方法，因此只要你定义了一个顶层方法，那么它就一定是静态方法。

想要定义一个顶层方法，首先需要创建一个Kotlin文件。对着任意包名右击 → New → Kotlin File/Class，在弹出的对话框中输入文件名即可。注意创建类型要选择File

点击“OK”完成创建，这样刚刚的包名路径下就会出现一个Helper.kt文件。现在我们在这个文件中定义的任何方法都会是顶层方法，比如这里我就定义一个doSomething()方法吧，如下所示：

```kotlin
fun doSomething(){
    println("do something")
}
```

刚才已经讲过了，Kotlin编译器会将所有的顶层方法全部编译成静态方法，那么我们要怎么调用这个doSomething()方法呢？如果是在Kotlin代码中调用的话，那就很简单了，所有的顶层方法都可以在任何位置被直接调用，不用管包名路径，也不用创建实例，直接键入doSomething()即可.

但如果是在Java代码中调用，你会发现是找不到doSomething()这个方法的，因为Java中没有顶层方法这个概念，所有的方法必须定义在类中。那么这个doSomething()方法被藏在了哪里呢？我们刚才创建的Kotlin文件名叫作Helper.kt，于是Kotlin编译器会自动创建一个叫作HelperKt的Java类，doSomething()方法就是以静态方法的形式定义在HelperKt类里面的，因此在Java中使用HelperKt.doSomething()的写法来调用就可以了.

---

## 二、延迟初始化和密封类

### 2.1 对变量延迟初始化

前面学习了Kotlin语言的许多特性，包括变量不可变，变量不可为空，等等。这些特性都是为了尽可能地保证程序安全而设计的，但是有的时候会给我们编码带来一些麻烦。

例如如果项目的某个类中存在很多全局变量实例，为了保证它们能够满足Kotlin的空指针检查语法标准，我们不得不做许多的非空判断保护才行，即使非常确定它们不会为空。

们通过一个具体的例子来看一下吧，就使用刚刚的UIBestPractice项目来作为例子。如果仔细观察MainActivity中的代码，会发现这里适配器的写法略微有点特殊：

```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {
    private val msgList = ArrayList<Msg>()
    private var adapter:MsgAdapter? = null
    lateinit var binding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        ...
        adapter = MsgAdapter(msgList)
        ...
    }

    // 监听点击事件
    override fun onClick(v: View?) {
        ...
        // 通知列表有新的数据插入
        adapter?.notifyItemInserted(msgList.size - 1)
        ...
                }
            }
        }
    }
}
```

这里我们将adapter设置为了全局变量，但是它的初始化工作是在onCreate()方法中进行的，因此不得不先将adapter赋值为null，同时把它的类型声明成MsgAdapter?

虽然我们会在onCreate()方法中对adapter进行初始化，同时能确保onClick()方法必然在onCreate()方法之后才会调用，但是我们在onClick()方法中调用adapter的任何方法时仍然要进行判空处理才行，否则编译肯定无法通过。

当我们的代码中有了越来越多的全局变量实例时，这个问题就会变得越来越明显，到时候我们可能需要编写大量额外的判空处理代码，只是为了满足Kotlin编译器的要求。

那么该如何解决呢？其实非常简单，那就是对全局变量进行延迟初始化。

延迟初始化使用的是lateinit关键字，它可以告诉Kotlin编译器，我会在晚些时候对这个变量进行初始化，这样就不用在一开始的时候将它赋值为null了。

接下来使用延迟初始化对上述代码进行优化：

```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {
    private val msgList = ArrayList<Msg>()
    private lateinit var adapter:MsgAdapter
    lateinit var binding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        ...
        adapter = MsgAdapter(msgList)
        ...
    }

    // 监听点击事件
    override fun onClick(v: View?) {
        ...
        // 通知列表有新的数据插入
        adapter.notifyItemInserted(msgList.size - 1)
        ...
                }
            }
        }
    }
}
```

当我们在adapter变量前面加上lateinit关键字后就用一开始就将它赋值为 null ，同时类型声明也就可以改成MsgAdapter了。由于MsgAdapter是不可为空的类型，所以我们在onClick()方法中也就不再需要进行判空处理，直接调用adapter的任何方法就可以了。

当然，使用lateinit关键字也不是没有任何风险，如果我们在adapter变量还没有初始化的情况下就直接使用它，那么程序就一定会崩溃，并且抛出一个UninitializedPropertyAccessException异常。

我们还可以通过代码来判断一个全局变量是否已经完成了初始化，这样在某些时候能够有效地避免重复对某一个变量进行初始化操作，示例代码如下：

```kotlin
if(!::adapter.isInitialized){
    adapter = MsgAdapter(msgList)
}
```

具体语法就是这样，::adapter.isInitialized可用于判断adapter变量是否已经初始化。

### 2.2 使用密封类优化代码

首先来了解一下密封类具体的作用，这里我们来看一个简单的例子。新建一个Kotlin文件，文件名就叫Result.kt好了，然后在这个文件中编写如下代码：

```kotlin
interface Result
class Success(val msg: String) : Result
class Failure(val error: Exception) : Result
```

这里定义了一个Result接口，用于表示某个操作的执行结果，接口中不用编写任何内容。然后定义了两个类去实现Result接口：一个Success类用于表示成功时的结果，一个Failure类用于表示失败时的结果，这样就把准备工作做好了。

接下来再定义一个getResultMsg()方法，用于获取最终执行结果的信息，代码如下：

```kotlin
fun getResultMsg(result: Result) = when(result){
    is Success -> result.msg
    is Failure -> result.error.message
    else -> throw IllegalArgumentException()
}
```

方法中接收一个Result参数。我们通过when语句来判断：如果Result属于Success，那么就返回成功的消息；如果Result属于Failure，那么就返回错误信息。到目前为止，代码都是没有问题的，但比较让人讨厌的是，接下来我们不得不再编写一个else条件，否则Kotlin编译器会认为这里缺少条件分支，代码将无法编译通过。但实际上Result的执行结果只可能是Success或者Failure，这个else条件是永远走不到的，所以我们在这里直接抛出了一个异常，只是为了满足Kotlin编译器的语法检查而已。

另外，编写else条件还有一个潜在的风险。如果我们现在新增了一个Unknown类并实现Result接口，用于表示未知的执行结果，但是忘记在getResultMsg()方法中添加相应的条件分支，编译器在这种情况下是不会提醒我们的，而是会在运行的时候进入else条件里面，从而抛出异常并导致程序崩溃。

当然，这种为了满足编译器的要求而编写无用条件分支的情况不仅在Kotlin当中存在，在Java或者是其他编程语言当中也普遍存在。

不过好消息是，Kotlin的密封类可以很好地解决这个问题，下面我们就来学习一下。密封类的关键字是sealed class，它的用法同样非常简单，我们可以轻松地将Result接口改造成密封类的写法：

```kotlin
sealed class Result
class Success(val msg: String) : Result
class Failure(val error: Exception) : Result
```

可以看到，代码并没有什么太大的变化，只是将interface关键字改成了sealed class。另外，由于密封类是一个可继承的类，因此在继承它的时候需要在后面加上一对括号.

那么改成密封类之后有什么好处呢？你会发现现在getResultMsg()方法中的else条件已经不再需要了，如下所示：

```kotlin
fun getResultMsg(result: Result) = when(result){
    is Success -> result.msg
    is Failure -> result.error.message
}
```

为什么这里去掉了else条件仍然能编译通过呢？这是因为当在when语句中传入一个密封类变量作为条件时，Kotlin编译器会自动检查该密封类有哪些子类，并强制要求你将每一个子类所对应的条件全部处理。这样就可以保证，即使没有编写else条件，也不可能会出现漏写条件分支的情况。而如果我们现在新增一个Unknown类，并也让它继承自Result，此时getResultMsg()方法就一定会报错，必须增加一个Unknown的条件分支才能让代码编译通过。

这就是密封类主要的作用和使用方法了。另外再多说一句，密封类及其所有子类只能定义在同一个文件的顶层位置，不能嵌套在其他类中，这是被密封类底层的实现机制所限制的。

了解完理论知识，接下来尝试结合 MsgAdapter中的ViewHolder一起使用顺便优化下MsgAdapter中的代码。

观看MsgAdapter现在的代码，你会发现onBindViewHolder()方法中就存在一个没有实际作用的else条件，只是抛出了一个异常而已。对于这部分代码，我们就可以借助密封类的特性来进行优化。首先删除MsgAdapter中的LeftViewHolder和RightViewHolder，然后新建一个MsgViewHolder.kt文件，在其中加入如下代码：

```kotlin
package work.icu007.uibestpractice

import android.view.View
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView


/*
 * Author: Charlie_Liao
 * Time: 2023/11/7-14:46
 * E-mail: rookie_l@icu007.work
 */

sealed class MsgViewHolder(view: View) : RecyclerView.ViewHolder(view)

class LeftViewHolder(view: View) : MsgViewHolder(view) {
    val leftMsg: TextView = view.findViewById(R.id.leftMsg)
}
class RightViewHolder(view: View) : MsgViewHolder(view) {
    val rightMsg: TextView = view.findViewById(R.id.rightMsg)
}

```

这里我们定义了一个密封类MsgViewHolder，并让它继承自RecyclerView.ViewHolder，然后让LeftViewHolder和RightViewHolder继承自MsgViewHolder。这样就相当于密封类MsgViewHolder只有两个已知子类，因此在when语句中只要处理这两种情况的条件分支即可。

现在修改MsgAdapter中的代码，如下所示：

```kotlin
package work.icu007.uibestpractice

import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView


/*
 * Author: Charlie_Liao
 * Time: 2023/11/2-14:34
 * E-mail: rookie_l@icu007.work
 */

class MsgAdapter(private val msgList: List<Msg>) : RecyclerView.Adapter<MsgViewHolder>() /*RecyclerView.Adapter<RecyclerView.ViewHolder>()*/ {
    /*inner class LeftViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val leftMsg: TextView = view.findViewById(R.id.leftMsg)
    }
    inner class RightViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val rightMsg: TextView = view.findViewById(R.id.rightMsg)
    }*/

    override fun getItemViewType(position: Int): Int {
        val msg = msgList[position]
        return  msg.type
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MsgViewHolder {
        return if (viewType == Msg.TYPE_RECEIVED){
            val view = LayoutInflater.from(parent.context).inflate(R.layout.msg_left_item,parent,false)
            LeftViewHolder(view)
        } else {
            val view = LayoutInflater.from(parent.context).inflate(R.layout.msg_right_item,parent,false)
            RightViewHolder(view)
        }
    }

    override fun getItemCount(): Int {
        return msgList.size
    }

    override fun onBindViewHolder(holder: MsgViewHolder /*RecyclerView.ViewHolder*/, position: Int) {
        val msg = msgList[position]
        when(holder){
            is LeftViewHolder -> holder.leftMsg.text = msg.content
            is RightViewHolder -> holder.rightMsg.text = msg.content
            //else -> throw IllegalArgumentException()
        }
    }
}
```

这里我们将RecyclerView.Adapter的泛型指定成刚刚定义的密封类MsgViewHolder，这样onBindViewHolder()方法传入的参数就变成了MsgViewHolder。然后我们只要在when语句当中处理LeftViewHolder和RightViewHolder这两种情况就可以了，那个讨厌的else终于不再需要了，这种RecyclerView适配器的写法更加规范也更加推荐。

---

## 三、扩展函数和运算符重载

### 3.1 大有用途的扩展函数

不少现代高级编程语言中有扩展函数这个概念，Java却一直以来都不支持这个非常有用的功能，这多少会让人有些遗憾。但值得高兴的是，Kotlin对扩展函数进行了很好的支持，因此这个知识点是我们无论如何都不能错过的。

首先看一下什么是扩展函数。扩展函数表示即使在不修改某个类的源码的情况下，仍然可以打开这个类，向该类添加新的函数。

为了帮助你更好地理解，我们先来思考一个功能。一段字符串中可能包含字母、数字和特殊符号等字符，现在我们希望统计字符串中字母的数量，你要怎么实现这个功能呢？如果按照一般的编程思维，可能大多数人会很自然地写出如下函数：

```kotlin
fun main(args: Array<String>) {
    val str = "Hello World!123"
    val count = StringUtil.lettersCount(str)
    println("string is: $str, letterCount is : $count")
}

object StringUtil {
    fun lettersCount(str: String): Int {
        var count = 0
        for (char in str) {
            if (char.isLetter()) {
                count++
            }
        }
        return count
    }
}
```

这里先定义了一个StringUtil单例类，然后在这个单例类中定义了一个lettersCount()函数，该函数接收一个字符串参数。在lettersCount()方法中，我们使用for-in循环去遍历字符串中的每一个字符。如果该字符是一个字母的话，那么就将计数器加1，最终返回计数器的值。

这种写法绝对可以正常工作，并且这也是Java编程中最标准的实现思维。但是有了扩展函数之后就不一样了，我们可以使用一种更加面向对象的思维来实现这个功能，比如说将lettersCount()函数添加到String类当中。

下面我们先来学习一下定义扩展函数的语法结构，其实非常简单，如下所示：

```kotlin
fun ClassName.methodName(param1: Int, param2: Int): Int {
    return 0
}
```

相比于定义一个普通的函数，定义扩展函数只需要在函数名的前面加上一个ClassName.的语法结构，就表示将该函数添加到指定类当中了。

了解了定义扩展函数的语法结构，接下来我们就尝试使用扩展函数的方式来优化刚才的统计功
能。

由于我们希望向String类中添加一个扩展函数，因此需要先创建一个String.kt文件。文件名虽然并没有固定的要求，但是我建议向哪个类中添加扩展函数，就定义一个同名的Kotlin文件，这样便于你以后查找。当然，扩展函数也是可以定义在任何一个现有类当中的，并不一定非要创建新文件。不过通常来说，最好将它定义成顶层方法，这样可以让扩展函数拥有全局的访问域。

现在我们可以这样写：

```kotlin
fun main(args: Array<String>) {
    val str = "Hello World!123"
    println("string is: $str, letterCount is : ${str.lettersCount()}")
}

fun String.lettersCount(): Int{
    var count = 0
    for (char in this){
        if (char.isLetter()){
            count++
        }
    }
    return count
}
```

注意这里的代码变化，现在我们将lettersCount()方法定义成了String类的扩展函数，那么函数中就自动拥有了String实例的上下文。因此lettersCount()函数就不再需要接收一个字符串参数了，而是直接遍历this即可，因为现在this就代表着字符串本身。

定义好了扩展函数之后，统计某个字符串中的字母数量只需要这样写即可：

```kotlin
val str = "Hello World!123"
val count = str.lettersCount()
// or
"hello world!123".lettersCount()
```

是不是很神奇？看上去就好像是String类中自带了lettersCount()方法一样。扩展函数在很多情况下可以让API变得更加简洁、丰富，更加面向对象。我们再次以String类为例，这是一个final类，任何一个类都不可以继承它，也就是说它的API只有固定的那些而已，至少在Java中就是如此。然而到了Kotlin中就不一样了，我们可以向String类中扩展任何函数，使它的API变得更加丰富。比如，你会发现Kotlin中的String甚至还有reverse()函数用于反转字符串，capitalize()函数用于对首字母进行大写，等等，这都是Kotlin语言自带的一些扩展函数。这个特性使我们的编程工作可以变得更加简便。

### 3.2 有趣的运算符重载

运算符重载是Kotlin提供的一个比较有趣的语法糖。我们知道，Java中有许多语言内置的运算符关键字，如`+ - * / % ++ --`。而Kotlin允许我们将所有的运算符甚至其他的关键字进行重载，从而拓展这些运算符和关键字的用法。

我们先来回顾一下运算符的基本用法。相信每个人都使用过加减乘除这种四则运算符。在编程语言里面，两个数字相加表示求这两个数字之和，两个字符串相加表示对这两个字符串进行拼接，这种基本用法相信接触过编程的人都明白。但是Kotlin的运算符重载却允许我们让任意两个对象进行相加，或者是进行更多其他的运算操作。

当然，虽然Kotlin赋予了我们这种能力，在实际编程的时候也要考虑逻辑的合理性。比如说，让两个Student对象相加好像并没有什么意义，但是让两个Money对象相加就变得有意义了，因为钱是可以相加的。

那么接下来，我们首先学习一下运算符重载的基本语法，然后再来实现让两个Money对象相加的功能。

运算符重载使用的是operator关键字，只要在指定函数的前面加上operator关键字，就可以实现运算符重载的功能了。但问题在于这个指定函数是什么？这是运算符重载里面比较复杂的一个问题，因为不同的运算符对应的重载函数也是不同的。比如说加号运算符对应的是plus()函数，减号运算符对应的是minus()函数。

这里还是以加号运算符为例，如果想要实现让两个对象相加的功能，那么它的语法结构如下：

```kotlin
class Obj {
operator fun plus(obj: Obj): Obj {
    // 处理相加的逻辑
}
}
```

在上述语法结构中，关键字operator和函数名plus都是固定不变的，而接收的参数和函数返回值可以根据你的逻辑自行设定。那么上述代码就表示一个Obj对象可以与另一个Obj对象相加，最终返回一个新的Obj对象。对应的调用方式如下：

```kotlin
val obj1 = Obj()
val obj2 = Obj()
val obj3 = obj1 + obj2
```

这种obj1 + obj2的语法看上去好像很神奇，但其实这就是Kotlin给我们提供的一种语法糖，它会在编译的时候被转换成obj1.plus(obj2)的调用方式。

了解了运算符重载的基本语法之后，下面我们开始实现一个更加有意义功能：让两个Money对象相加。

首先定义Money类的结构，这里我准备让Money的主构造函数接收一个value参数，用于表示钱的金额。创建Money.kt文件，代码如下所示：

```kotlin
class Money(val value: Int){

    operator fun plus(money: Money): Money {
        val sum = value + money.value
        return Money(sum)
    }
}
```

可以看到，这里使用了operator关键字来修饰plus()函数，这是必不可少的。在plus()函数中，我们将当前Money对象的value和参数传入的Money对象的value相加，然后将得到的和传给一个新的Money对象并将该对象返回。这样两个Money对象就可以相加了，就是这么简单。

```kotlin
fun main(args: Array<String>) {
    val money1 = Money(6)
    val money2 = Money(7)
    val money = money1 + money2
    println("Money(6) + Money(7) is ${money.value}")
}

class Money(val value: Int){

    operator fun plus(money: Money): Money {
        val sum = value + money.value
        return Money(sum)
    }
}
```

但是，Money对象只允许和另一个Money对象相加，有没有觉得这样不够方便呢？或许你会觉得，如果Money对象能够直接和数字相加的话，就更好了。这个功能当然也是可以实现的，因为Kotlin允许我们对同一个运算符进行多重重载，代码如下所示：

```kotlin
fun main(args: Array<String>) {
    val  money1 = Money(6)
    val  money2 = Money(7)
    val money = money1 + money2
    println("Money(6) + Money(7) is ${money.value}. Money(6) + 6 is ${(money1 + 6).value}.")
}

class Money(val value: Int){

    operator fun plus(money: Money): Money {
        val sum = value + money.value
        return Money(sum)
    }
    operator fun plus(money: Int): Money {
        val sum = value + money
        return Money(sum)
    }
    
}
```

当然，我们还可以对这个例子进一步扩展，比如加上汇率转换的功能。让1人民币的Money对象和1美元的Money对象相加，然后根据实时汇率进行转换，从而返回一个新的Money对象。这类功能都是非常有趣的，运算符重载如果运用得好的话，可以玩出很多花样。

```kotlin
fun main(args: Array<String>) {
    val  rmb = Money(7.0,"¥",1.0)
    val  dollar = Money(6.0,"$",7.1264772)
    Money.printMoneyOperation(dollar,dollar,"+")
    Money.printMoneyOperation(dollar,dollar,"-")
    Money.printMoneyOperation(dollar,rmb,"+")
    Money.printMoneyOperation(dollar,rmb,"-")
    Money.printMoneyOperation(rmb,dollar,"+")
    Money.printMoneyOperation(rmb,dollar,"-")
    Money.printMoneyOperation(rmb,rmb,"+")
    Money.printMoneyOperation(rmb,rmb,"-")
    println("Dollar(6.0) + RMB(7.0) is ${(Dollar(6.0) + RMB(7.0)).value}${(Dollar(6.0) + RMB(7.0)).getCurrency()}.\n" +
            "RMB(7.0) + Dollar(6.0) is ${(RMB(7.0) + Dollar(6.0)).value}${(RMB(7.0) + Dollar(6.0)).getCurrency()}.")
}

class Money(val value: Double, val currency: String, val exchangeRate: Double = 1.0){

    operator fun plus(money: Money): Money {
        val convertedValue = money.value * (money.exchangeRate / this.exchangeRate)
        return Money(this.value + convertedValue, this.currency, this.exchangeRate)
    }
    operator fun minus(money: Money): Money {
        val convertedValue = money.value * (money.exchangeRate / this.exchangeRate)
        return Money(this.value - convertedValue, this.currency, this.exchangeRate)
    }
    companion object {
        fun printMoneyOperation(money1: Money, money2: Money, operation: String): Money {
            val result = when (operation) {
                "+" -> money1 + money2
                "-" -> money1 - money2
                else -> throw IllegalArgumentException("Unsupported operation: $operation")
            }
            println("${money1.value}${money1.currency} $operation ${money2.value}${money2.currency} is ${result.value}${result.currency}.")
            return result
        }
    }

}
class RMB(val value: Double){

    operator fun plus(dollar: Dollar): RMB {
        val sum = value + (dollar.value * 7.1264772)
        return RMB(sum)
    }
    operator fun plus(rmb: RMB): RMB {
        val sum = value + rmb.value
        return RMB(sum)
    }

    fun getCurrency(): String{
        return "¥"
    }

}
class Dollar(val value: Double){

    operator fun plus(rmb: RMB): Dollar {
        val sum = value + (rmb.value * 0.140322)
        return Dollar(sum)
    }
    operator fun plus(dollar: Dollar): Dollar {
        val sum = value + dollar.value
        return Dollar(sum)
    }
    fun getCurrency(): String{
        return "$"
    }

}
```

例如上述代码，我们就使用运算符重载实现了对Money类的加减运算并且加上了汇率转换。

实际上Kotlin允许我们重载的运算符和关键字多达十几个。，因此下表列出了所有常用的可重载运算符和关键字对应的语法糖表达式，以及它们会被转换成的实际调用函数。如果你想重载其中某一种运算符或关键字，只要参考刚才加号运算符重载的写法去实现就可以了。

| 语法糖表达式 |  实际调用函数  |
| :----------: | :------------: |
|    a + b     |   a.plus(b)    |
|    a - b     |   a.minus(b)   |
|    a * b     |   a.times(b)   |
|    a / b     |    a.div(b)    |
|    a % b     |    a.rem(b)    |
|     a++      |     a.ic()     |
|     a--      |    a.dec()     |
|      +a      | a.unaryPlus()  |
|      -a      | a.unaryMinus() |
|      !a      |    a.not()     |
|    a == b    |  a.equals(b)   |
|    a > b     |  a.equals(b)   |
|    a < b     |  a.equals(b)   |
|    a >= b    |  a.equals(b)   |
|    a <= b    | a.compareTo(b) |
|     a..b     |  a.rangeTo(b)  |
|     a[b]     |    a.get(b)    |
|   a[b] = c   |  a.set(b, c)   |
|    a in b    | b.contains(b)  |

最后一个a in b的语法糖表达式对应的实际调用函数是b.contains(a)，a、b对象的顺序是反过来的。这在语义上很好理解，因为a in b表示判断a是否在b当中，而b.contains(a)表示判断b是否包含a，因此这两种表达方式是等价的。

例如我们判断 hello中是否包含 he，我们可以这样写：

```kotlin
if ("hello".contains("he")){
    // do something
}
if ("he" in "hello"){
    // do something
}
```

这两个写法的效果其实是一致的。

实践一下，假如我们有一个用于生成随机字符串长度的函数：

```kotlin
fun getRandomLengthString(str: String): String {
    val n = (1..20).random()
    val builder = StringBuilder()
    repeat(n) {
        builder.append(str)
    }
    return builder.toString()
}
```

这个函数的核心思想就是将传入的字符串重复n次，如果我们能够使用str * n这种写法来表示让str字符串重复n次，这种语法体验就非常通俗易懂了，而在Kotlin中这又是很容易实现的。

要让一个字符串可以乘以一个数字，那么肯定要在String类中重载乘号运算符才行，但是String类是系统提供的类，我们无法修改这个类的代码。这个时候就可以借助扩展函数功能向String类中添加新函数了。

这个时候我们加入代码：

```kotlin
operator fun String.times(n: Int): String{
    val builder = StringBuilder()
    repeat(n){
        builder.append(this)
    }
    return builder.toString()
}
```

这段代码应该不难理解，这里只讲几个关键的点。首先，operator关键字肯定是必不可少的；然后既然是要重载乘号运算符，参考上表可知，函数名必须是times；最后，由于是定义扩展函数，因此还要在方向名前面加上String.的语法结构。

另外，必须说明的是，其实Kotlin的String类中已经提供了一个用于将字符串重复n遍的repeat()函数，因此times()函数还可以进一步精简成如下形式：

```kotlin
operator fun String.times(n: Int): String{
    return repeat(n)
}
```

进而简化成：

```kotlin
operator fun String.times(n: Int) = repeat(n)
```

---

## 四、高阶函数详解

### 4.1 定义高阶函数

高阶函数和Lambda的关系是密不可分的。之前几章掌握了一些与集合相关的函数式API的用法，如map、filter函数等。另外，我们之前还学习了Kotlin的标准函数，如run、apply函数等。

这几个函数有一个共同的特点：它们都会要求我们传入一个Lambda表达式作为参数。像这种接收Lambda参数的函数就可以称为具有函数式编程风格的API，而如果你想要定义自己的函数式API，那就得借助高阶函数来实现了

首先来看一下高阶函数的定义：**如果一个函数接收另一个函数作为参数，或者返回值的类型是另一个函数，那么该函数就称为高阶函数。**

一个函数怎么能接收另一个函数作为参数呢？这就涉及另外一个概念了：函数类型。我们知道，编程语言中有整型、布尔型等字段类型，而Kotlin又增加了一个函数类型的概念。如果我们将这种函数类型添加到一个函数的参数声明或者返回值声明当中，那么这就是一个高阶函数了。

接下来学习一下如何定义一个函数类型。不同于定义一个普通的字段类型，函数类型的语法规则是有点特殊的，基本规则如下：

```kotlin
(String, Int)-> Unit
```

看到这个语法规则是不是很懵？不用着急，给大家解释一下：既然是定义一个函数类型，那么最关键的就是要声明该函数接收什么参数，以及它的返回值是什么。因此，->左边的部分就是用来声明该函数接收什么参数的，多个参数之间使用逗号隔开，如果不接收任何参数，写一对空括号就可以了。而->右边的部分用于声明该函数的返回值是什么类型，如果没有返回值就使用Unit，它大致相当于Java中的void。

现在将上述函数类型添加到某个函数的参数声明或者返回值声明上，那么这个函数就是一个高阶函数了，如下所示：

```kotlin
fun example(func: (String, Int) -> Unit){
    func("hello", 123)
}
```

可以看到，这里的example()函数接收了一个函数类型的参数，因此example()函数就是一个高阶函数。而调用一个函数类型的参数，它的语法类似于调用一个普通的函数，只需要在参数名的后面加上一对括号，并在括号中传入必要的参数即可。

现在已经了解了高阶函数的定义方式，但是这种函数具体有什么用途呢？由于高阶函数的用途实在是太广泛了，这里如果要简单概括一下的话，那就是高阶函数允许让函数类型的参数来决定函数的执行逻辑。即使是同一个高阶函数，只要传入不同的函数类型参数，那么它的执行逻辑和最终的返回结果就可能是完全不同的。为了详细说明这一点，下面我们来举一个具体的例子。

这里我准备定义一个叫作num1AndNum2()的高阶函数，并让它接收两个整型和一个函数类型的参数。我们会在num1AndNum2()函数中对传入的两个整型参数进行某种运算，并返回最终的运算结果，但是具体进行什么运算是由传入的函数类型参数决定的。

新建一个HigherOrderFunction.kt文件，然后在这个文件中编写如下代码：

```kotlin
fun num1AndNum2(num1: Int, num2: Int, operation: (Int, Int) -> Int): Int {
    val result = operation(num1, num2)
    return result
}

fun plus(num1: Int, num2: Int): Int {
    return num1 + num2
}
fun minus(num1: Int, num2: Int): Int {
    return num1 - num2
}
```

这里定义了两个函数，并且这两个函数的参数声明和返回值声明都和num1AndNum2()函数中的函数类型参数是完全匹配的。其中，plus()函数将两个参数相加并返回，minus()函数将两个参数相减并返回，分别对应了两种不同的运算操作。有了上述函数之后，我们就可以调用num1AndNum2()函数了，在main()函数中编写如下代码：

```kotlin
fun main(){
    val num1 = 100
    val num2 = 80
    val result1 = num1AndNum2(num1, num2, ::plus)
    val result2 = num1AndNum2(num1, num2, ::minus)
    println("result is $result1")
    println("result is $result2")
}
```

注意这里调用num1AndNum2()函数的方式，第三个参数使用了::plus和::minus这种写法。这是一种函数引用方式的写法，表示将plus()和minus()函数作为参数传递给num1AndNum2()函数。而由于num1AndNum2()函数中使用了传入的函数类型参数来决定具体的运算逻辑，因此这里实际上就是分别使用了plus()和minus()函数来对两个数字进行运算。

使用这种函数引用的写法虽然能够正常工作，但是如果每次调用任何高阶函数的时候都还得先定义一个与其函数类型参数相匹配的函数，这是不是有些太复杂了？没错，因此Kotlin还支持其他多种方式来调用高阶函数，比如Lambda表达式、匿名函数、成员引用等。其中，Lambda表达式是最常见也是最普遍的高阶函数调用方式，接下来要重点使用lambda表达式进行调用。

上述代码如果使用Lambda表达式的写法来实现的话，代码如下所示：

> ⭐注意：
>
> 在 Kotlin 中，如果函数的最后一个参数是一个函数，那么我们可以在函数调用的括号外部传递这个函数。这就是为什么下面例子可以在 `num1AndNum2(num1, num2)` 后面直接写 `{n1, n2 -> n1 % n2}`，而不需要把它放在括号里。
>
> 这种语法叫做**尾随 lambda**，它可以让你的代码更加简洁和易读。当你的 lambda 表达式比较长或者包含多行代码时，尾随 lambda 就显得特别有用。

```kotlin
class HighOrderFunction {

}

fun num1AndNum2(num1: Int, num2: Int, operation: (Int, Int) -> Int): Int {
    val result = operation(num1, num2)
    return result
}

fun plus(num1: Int, num2: Int): Int {
    return num1 + num2
}
fun minus(num1: Int, num2: Int): Int {
    return num1 - num2
}

fun main(){
    val num1 = 100
    val num2 = 80
    val result1 = num1AndNum2(num1, num2, ::plus)
    val result2 = num1AndNum2(num1, num2, ::minus)
    val result3 = num1AndNum2(num1, num2) {n1, n2 ->
        n1 % n2
    }
    val result4 = num1AndNum2(num1, num2) {n1, n2 ->
        n1 * n2
    }
    println("result is $result1")
    println("result is $result2")
    println("result is $result3")
    println("result is $result4")
}
```

> ⭐
>
> 在 Kotlin 中，还可以使用 lambda 表达式来定义一个匿名函数。lambda 表达式的语法格式是 `{ 参数列表 -> 函数体 }`。
>
> 在上面例子中，`n1, n2 -> n1 % n2` 就是一个 lambda 表达式。它接受两个参数 `n1` 和 `n2`，并返回它们的余数。这个 lambda 表达式定义了一个函数，这个函数的功能是计算两个数的余数。
>
> 当我们把这个 lambda 表达式作为参数传递给 `num1AndNum2` 函数时，`num1AndNum2` 函数会在内部调用这个 lambda 表达式，就像调用一个普通的函数一样。
>
> 所以，我们可以直接写 `n1, n2 -> n1 % n2`，因为它就是一个函数，只不过这个函数没有名字，我们称之为匿名函数或者 lambda 函数。

下面我们继续对高阶函数进行探究。回顾之前学习的apply函数，它可以用于给Lambda表达式提供一个指定的上下文，当需要连续调用同一个对象的多个方法时，apply函数可以让代码变得更加精简，比如StringBuilder就是一个典型的例子。接下来我们就使用高阶函数模仿实现一个类似的功能。

首先我们给StringBuilder加上一个build扩展函数：这个扩展函数接收一个函数类型参数，并且返回值类型也是StringBuilder。

```kotlin
fun StringBuilder.build(block: StringBuilder.() -> Unit): StringBuilder{
    block()
    return this
}
```

> ⭐注意：
>
> 上述代码使用了 Kotlin 的**扩展函数**和**带接收者的 lambda**。
>
> 首先，`StringBuilder.build` 是一个扩展函数，它为 `StringBuilder` 类添加了一个新的方法。这个方法接受一个带接收者的 lambda 作为参数，然后在 `StringBuilder` 的实例上执行这个 lambda。
>
> 
>
> 带接收者的 lambda 是一种特殊的 lambda，它的语法格式是 `接收者类型.() -> 返回类型`。在上述代码中，`StringBuilder.() -> Unit` 就是一个带接收者的 lambda。这个 lambda 可以在 `StringBuilder` 的实例上调用方法，就像在 `StringBuilder` 的内部一样。
>
> 当我们调用 `StringBuilder().build` 时，可以传递一个带接收者的 lambda。在这个 lambda 中，可以直接调用 `StringBuilder` 的方法，例如 `append`。

注意，这个函数类型参数的声明方式和我们前面num1AndNum2的语法有所不同：它在函数类型的前面加上了一StringBuilder. 的语法结构。这是什么意思呢？其实这才是定义高阶函数完整的语法规则，在函数类型的前面加ClassName. 就表示这个函数类型是定义在哪个类当中的。这里将函数类型定义到StringBuilder类当中，那这样有什么好处呢？好处就是当我们调用build函数时传入的Lambda表达式将会自动拥有StringBuilder的上下文，也就是说我们可以直接调用StringBuilder里面的函数，同时这也是apply函数的实现方式。

接下来尝试调用这个方法

```kotlin
fun main(){
    val word = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
    val result = StringBuilder().build{
        append("start eating fruits.\n")
        for (fruit in word){
            append(fruit).append("\n")
        }
        append("ate all fruits.")
    }
    println(result)
}
```

在这个 lambda 表达式中：

- `StringBuilder.` 是接收者类型。这意味着这个 lambda 可以在 `StringBuilder` 的实例上调用方法，就像在 `StringBuilder` 的内部一样。这就是为什么可以直接调用 `append` 方法。
- `()` 表示这个 lambda 不接受任何参数。这就是为什么上述 lambda 表达式中没有参数列表。
- `-> Unit` 是这个 lambda 的返回类型。但是，在实际的 lambda 表达式中，不需要写 `-> Unit`。因为 Kotlin 可以根据代码自动推断出返回类型是 `Unit`。

所以，虽然带接收者的 lambda 的类型声明是 `StringBuilder.() -> Unit`，但在实际的 lambda 表达式中，只需要写函数体，不需要写 `-> Unit`。

可以看到，build函数的用法和apply函数基本上是一模一样的，只不过我们编写的build函数目前只能作用在StringBuilder类上面，而apply函数是可以作用在所有类上面的。如果想实现apply函数的这个功能，需要借助于Kotlin的泛型才行

### 4.2 内联函数的作用

为了接下来可以更好地理解内联函数这个知识点，这里简单分析一下高阶函数的实现原理。

这里使用刚刚编写的num1Andnum2()函数来举例，代码如下所示：

```kotlin
fun num1AndNum2(num1: Int, num2: Int, operation: (Int, Int) -> Int): Int {
    val result = operation(num1, num2)
    return result
}
fun main() {
    val num1 = 100
    val num2 = 80
    val result = num1AndNum2(num1, num2) { n1, n2
        n1 + n2
    }
}
```

可以看到，上述代码中调用了num1AndNum2()函数，并通过Lambda表达式指定对传入的两个整型参数进行求和。这段代码在Kotlin中非常好理解，因为这是高阶函数最基本的用法。可是我们都知道，Kotlin的代码最终还是要编译成Java字节码的，但Java中并没有高阶函数的概念。

那么Kotlin究竟使用了什么魔法来让Java支持这种高阶函数的语法呢？这就要归功于Kotlin强大的编译器了。Kotlin的编译器会将这些高阶函数的语法转换成Java支持的语法结构，上述的Kotlin代码大致会被转换成如下Java代码：

```java
public static int num1Andnum2(int num1, int num2, Function operation) {
    int result = (int) operation.invoke(num1, num2);
    return result;
}

public static void main(){
    int num1 = 100;
    int num2 = 80;
    int result = num1Andnum2(num1, num2, new Function(){
        @Override
        public Integer invoke(Integer n1, Integer n2) {
            return n1 + n2;
        }
    });
}
```

考虑到可读性，我对这段代码进行了些许调整，并不是严格对应了Kotlin转换成的Java代码。可以看到，在这里num1AndNum2()函数的第三个参数变成了一个Function接口，这是一种Kotlin内置的接口，里面有一个待实现的invoke()函数。而num1AndNum2()函数其实就是调用了Function接口的invoke()函数，并把num1和num2参数传了进去。

在调用num1AndNum2()函数的时候，之前的Lambda表达式在这里变成了Function接口的匿名类实现，然后在invoke()函数中实现了n1 + n2的逻辑，并将结果返回。

这就是Kotlin高阶函数背后的实现原理。你会发现，原来我们一直使用的Lambda表达式在底层被转换成了匿名类的实现方式。这就表明，我们每调用一次Lambda表达式，都会创建一个新的匿名类实例，当然也会造成额外的内存和性能开销。

为了解决这个问题，Kotlin提供了内联函数的功能，它可以将使用Lambda表达式带来的运行时开销完全消除。

内联函数的用法非常简单，只需要在定义高阶函数时加上inline关键字的声明即可，如下所示：

```kotlin
inline fun num1AndNum2(num1: Int, num2: Int, operation: (Int, Int) -> Int): Int {
    return operation(num1, num2)
}
```

那么内联函数的工作原理又是什么呢？其实并不复杂，就是Kotlin编译器会将内联函数中的代码在编译的时候自动替换到调用它的地方，这样也就不存在运行时的开销了。

首先，Kotlin编译器会将Lambda表达式中的代码替换到函数类型参数调用的地方，如图所示。

![1700895120471.png](https://pic.ziyuan.wang/2023/11/25/xiheya_b3196f3ebad9a.png)

接下来，再将内联函数中的全部代码替换到函数调用的地方，如图所示。

![1700895298709.png](https://pic.ziyuan.wang/2023/11/25/xiheya_b528a7d70d9a5.png)

最后代码其实就变成了：

```kotlin
fun main() {
    val num1 = 100
    val num2 = 80
    val result3 = num1 % num2
}
```

正是如此，内联函数才能完全消除Lambda表达式所带来的运行时开销。

### 4.3 noinline 与 crossinline

接下来我们要讨论一些更加特殊的情况。比如，一个高阶函数中如果接收了两个或者更多函数类型的参数，这时我们给函数加上了inline关键字，那么Kotlin编译器会自动将所有引用的Lambda表达式全部进行内联。

但是，如果我们只想内联其中的一个Lambda表达式该怎么办呢？这时就可以使用noinline关键字了，如下所示：

```kotlin
inline fun inlineTest(block1: () -> Unit, noinline block2: () -> Unit) {
    // do someting
}
```

可以看到，这里使用inline关键字声明了inlineTest()函数，原本block1和block2这两个函数类型参数所引用的Lambda表达式都会被内联。但是我们在block2参数的前面又加上了一个noinline关键字，那么现在就只会对block1参数所引用的Lambda表达式进行内联了。这就是noinline关键字的作用。

前面我们已经解释了内联函数的好处，那么为什么Kotlin还要提供一个noinline关键字来排除内联功能呢？**这是因为内联的函数类型参数在编译的时候会被进行代码替换，因此它没有真正的参数属性。非内联的函数类型参数可以自由地传递给其他任何函数，因为它就是一个真实的参数，而内联的函数类型参数只允许传递给另外一个内联函数，这也是它最大的局限性。**

另外，内联函数和非内联函数还有一个重要的区别，那就是内联函数所引用的Lambda表达式中是可以使用return关键字来进行函数返回的，而非内联函数只能进行局部返回。为了说明这个问题，我们来看下面的例子。

```kotlin
fun printString(str: String, block: (String) -> Unit) {
    println("printString begin")
    block(str)
    println("printString end")
}
fun main() {
    println("main start")
    val str = ""
    printString(str) {s ->
        println("lambda start")
        if (s.isEmpty()) return@printString
        println(s)
        println("lambda end")
    }
    println("main end")
}
```

这里定义了一个叫作printString()的高阶函数，用于在Lambda表达式中打印传入的字符串参数。但是如果字符串参数为空，那么就不进行打印。注意，Lambda表达式中是不允许直接使用return关键字的，这里使用了return@printString的写法，表示进行局部返回，并且不再执行Lambda表达式的剩余部分代码。

现在我们就刚好传入一个空的字符串参数，运行程序，打印结果如图所示。

![1700897776694.png](https://pic.ziyuan.wang/2023/11/25/xiheya_5a0c911452403.png)

可以看到，除了Lambda表达式中return@printString语句之后的代码没有打印，其他的日志是正常打印的，说明return@printString确实只能进行局部返回。

但是如果我们将printString()函数声明成一个内联函数，那么情况就不一样了，如下所示：

```kotlin
inline fun printString(str: String, block: (String) -> Unit) {
    println("printString begin")
    block(str)
    println("printString end")
}
fun main() {
    println("main start")
    val str = ""
    printString(str) {s ->
        println("lambda start")
        if (s.isEmpty()) return
        println(s)
        println("lambda end")
    }
    println("main end")
}
```

现在printString()函数变成了内联函数，我们就可以在Lambda表达式中使用return关键字了。此时的return代表的是返回外层的调用函数，也就是main()函数，如果想不通为什么的话，可以回顾一下之前讲的内联函数的代码替换过程。

打印结果如下：

![1700897878240.png](https://pic.ziyuan.wang/2023/11/25/xiheya_eaeb52005d73e.png)

可以看到，不管是main()函数还是printString()函数，确实都在return关键字之后停止执行了，和我们所预期的结果一致。

将高阶函数声明成内联函数是一种良好的编程习惯，事实上，绝大多数高阶函数是可以直接声明成内联函数的，但是也有少部分例外的情况。观察下面的代码示例：

```kotlin
inline fun runRunnable(block: () -> Unit) {
    val runnable = Runnable {
        block()
    }
    runnable.run()
}
```

这段代码在没有加上inline关键字声明的时候绝对是可以正常工作的，但是在加上inline关键字之后就会提示如图所示的错误。

![1700898102099.png](https://pic.ziyuan.wang/2023/11/25/xiheya_5551909e6c42e.png)

首先，在runRunnable()函数中，我们创建了一个Runnable对象，并在Runnable的Lambda表达式中调用了传入的函数类型参数。而Lambda表达式在编译的时候会被转换成匿名类的实现方式，也就是说，上述代码实际上是在匿名类中调用了传入的函数类型参数。

而内联函数所引用的Lambda表达式允许使用return关键字进行函数返回，但是由于我们是在匿名类中调用的函数类型参数，此时是不可能进行外层调用函数返回的，最多只能对匿名类中的函数调用进行返回，因此这里就提示了上述错误。

也就是说，如果我们在高阶函数中创建了另外的Lambda或者匿名类的实现，并且在这些实现中调用函数类型参数，此时再将高阶函数声明成内联函数，就一定会提示错误。

那在这种情况下该如何使用内联函数呢？其实很简单，借助crossinline关键字就可以很好地解决这个问题：

```kotlin
inline fun runRunnable(crossinline block: () -> Unit) {
    val runnable = Runnable {
        block()
    }
    runnable.run()
}
```

那么这个crossinline关键字又是什么呢？前面我们已经分析过，之所以会提示图示的错误，就是因为内联函数的Lambda表达式中允许使用return关键字，和高阶函数的匿名类实现中不允许使用return关键字之间造成了冲突。而crossinline关键字就像一个契约，它用于保证在内联函数的Lambda表达式中一定不会使用return关键字，这样冲突就不存在了，问题也就巧妙地解决了。

声明了crossinline之后，我们就无法在调用runRunnable函数时的Lambda表达式中使用return关键字进行函数返回了，但是仍然可以使用return@runRunnable的写法进行局部返回。总体来说，除了在return关键字的使用上有所区别之外，crossinline保留了内联函数的其他所有特性。

---

## 五、高阶函数的应用

高阶函数非常适用于简化各种API的调用，一些API的原有用法在使用高阶函数简化之后，不管是在易用性还是可读性方面，都可能会有很大的提升。

### 5.1 简化SharedPreferences的用法

首先来看SharedPreferences的用法。向SharedPreferences中存储数据的过程大致可以分为以下3步：

1. 调用SharedPreferences的edit()方法获取SharedPreferences.Editor对象；
2. 向SharedPreferences.Editor对象中添加数据；
3. 调用apply()方法将添加的数据提交，完成数据存储操作。

对应代码如下所示：

```kotlin
val editor = getSharePreferences("data", Context.MODE_PRIVATE).edit()
editor.putString("name", Tom)
editor.putInt("age", 28)
editor.putBoolean("married", false)
editor.apply()
```

其实我们可以通过扩展函数的方式向SharedPreference类当中添加一个open函数，并且让他接收一个函数类型的参数，此时open函数就是一个高阶函数了。

```kotlin
fun SharedPreferences.open(block: SharedPreferences.Editor.() -> Unit) {
    val editor = edit()
    editor.block()
    editor.apply()
}
```

首先，我们通过扩展函数的方式向SharedPreferences类中添加了一个open函数，并且它还接收一个函数类型的参数，因此open函数自然就是一个高阶函数了。

> ⭐注意：
>
> 上述代码使用了Kotlin中的**扩展函数**和 **带接收者的lambda**
>
> 其中 我们为 `SharedPreference`类添加了一个新的方法—SharedPreference.open,这个方法接收一个 **带接收者的lambda**作为参数。在上述代码中: `SharedPreferences.Editor.() -> Unit`是一个 **带接收者的lambda**。在这种 lambda 中，你可以直接调用接收者的方法，就像在接收者的内部一样。这个 lambda可以在 `SharedPreference.Editor`的实例上调用方法，当我们调用`SharedPreferences.open`时，可以传递一个带接收者的 lambda。在这个 lambda 中，你可以直接调用 `SharedPreferences.Editor` 的方法，例如`putString`。
>
> 📕解析：
>
> - `SharedPreferences.Editor.` 为接收者类型。这意味着这个 lambda 可以在 `SharedPreferences.Editor` 的实例上调用方法，就像在 `SharedPreferences.Editor` 的内部一样；
> - `()` 表示这个 lambda 不接受任何参数；
> - `-> Unit` 表示这个 lambda 的返回类型是 `Unit`。`Unit` 在 Kotlin 中类似于 Java 中的 `void`，表示这个函数没有有意义的返回值。

由于`open`函数内拥有`SharedPreferences`的上下文，因此这里可以直接调用`edit()`方法来获取`SharedPreferences.Editor`对象。另外`open`函数接收的是一个`SharedPreferences.Editor`的函数类型参数，因此这里需要调用`editor.block()`对函数类型参数进行调用，我们就可以在函数类型参数的具体实现中添加数据了。最后还要调用`editor.apply()`方法来提交数据，从而完成数据存储操作。

定义好了open函数之后，我们以后在项目中使用SharedPreferences存储数据（Android）就会更加方便了，写法如下所示：

```kotlin
getSharedPreferences("data", Context.MODE_PRIVATE).open {
    putString("name", "Tom")
    putInt("age", 28)
    putBoolean("married", false)
}
```

在Android中，其实Google提供的KTX扩展库中已经包含了上述SharedPreferences的简化用法，这个扩展库会在Android Studio创建项目的时候自动引入build.gradle的dependencies中。 `implementation 'androidx.core:core-ktx:版本号'`。

因此，我们实际上可以直接在项目中使用如下写法来向`SharedPreferences`存储数据：

```kotlin
getSharedPreferences("data", Context.MODE_PRIVATE).edit {
    putString("name", "Tom")
    putInt("age", 28)
    putBoolean("married", false)
}
```

### 5.2 简化ContentValues的用法

ContentValues主要用于结合SQLiteDatabase的API存储和修改数据库中的数据，具体的用法示例如下：

```kotlin
val values = ContentValues()
values.put("name", "剑来")
values.put("author", "烽火戏诸侯")
values.put("pages", 60000)
values.put("price", 60.99)
db.insert("book", null, valuse)
```

这段代码可以使用`apply`函数进行简化。这当然没有错，只是我们其实还可以做到更好。不过在正式开始我们的简化之旅之前，我还得向你介绍一个额外的知识点。之前学过的`mapOf()`函数的用法。它允许我们使用`"Apple" to 1`这样的语法结构快速创建一个键值对。**在Kotlin中使用A to B这样的语法结构会创建一个Pair对象。**

有了这个知识前提之后，就可以进行下一步了。

```kotlin
fun cvOf(vararg pairs: Pair<String, Any?>): ContentValues {
}
```

这个方法的作用是构建一个`ContentValues`对象。首先，`cvOf()`方法接收了一个`Pair`参数，也就是使用`A to B`语法结构创建出来的**参数类型**，但是我们在参数前面加上了一个`vararg`关键字，这是什么意思呢？其实`vararg`对应的就是Java中的可变参数列表，我们允许向这个方法传入0个、1个、2个甚至任意多个Pair类型的参数，这些参数都会被赋值到使用`vararg`声明的这一个变量上面，然后使用`for-in`循环可以将传入的所有参数遍历出来。

再来看声明的`Pair`类型。由于`Pair`是一种键值对的数据结构，因此需要通过泛型来指定它的键和值分别对应什么类型的数据。值得庆幸的是，`ContentValues`的所有键都是字符串类型的，这里可以直接将`Pair`键的泛型指定成`String`。但`ContentValues`的值却可以有多种类型（字符串型、整型、浮点型，甚至是null），所以我们需要将`Pair`值的泛型指定成`Any?`。这是因为`Any`是`Kotlin`中所有类的共同基类，相当于`Java`中的`Object`，而`Any?`则表示允许传入空值。

> vararg 关键字是用来表示一个函数的参数可以接受可变数量的值，也就是说，你可以传递任意个数的同类型的值给这个参数。例如，如果你定义了一个函数：
>
> ```kotlin
> fun sum(vararg numbers: Int): Int {
>  var total = 0
>  for (n in numbers) {
>      total += n
>  }
>  return total
> }
> ```
>
> 那么你可以用不同的方式调用这个函数，例如：
>
> ```kotlin
> sum(1, 2, 3) // 传递三个值
> sum(4, 5) // 传递两个值
> sum() // 不传递任何值
> sum(*arrayOf(6, 7, 8)) // 传递一个数组，需要用 * 号展开
> ```
>
> vararg 关键字可以让你的函数更灵活，更方便地处理不确定数量的输入。
>
> Pair 类是用来表示一对值的，它有两个属性：first 和 second，分别表示第一个值和第二个值。你可以用 Pair 类来存储或返回两个相关的值，而不需要创建一个新的类。例如，如果你想要返回一个函数的最大值和最小值，你可以这样写：
>
> ```kotlin
> fun minMax(numbers: IntArray): Pair<Int, Int> {
>  var min = Int.MAX_VALUE
>  var max = Int.MIN_VALUE
>  for (n in numbers) {
>      if (n < min) min = n
>      if (n > max) max = n
>  }
>  return Pair(min, max) // 返回一个 Pair 对象
> }
> ```
>
> 然后你可以这样调用这个函数，并解构 Pair 对象：
>
> ```kotlin
> val (min, max) = minMax(intArrayOf(1, 2, 3, 4, 5)) // 解构 Pair 对象，得到 min 和 max
> println("The minimum is $min and the maximum is $max")
> ```
>
> Pair 类可以让你的代码更简洁，更易于阅读.

接下来我们开始为`cvOf()`方法实现功能逻辑，核心思路就是先创建一个`ContentValues`对象，然后遍历`pairs`参数列表，取出其中的数据并填入`ContentValues`中，最终将`ContentValues`对象返回即可。思路并不复杂，但是存在一个问题：`Pair`参数的值是`Any?`类型的，我们怎样让它和`ContentValues`所支持的数据类型对应起来呢？这个确实没有什么好的办法，只能使用`when`语句一一进行条件判断，并覆盖`ContentValues`所支持的所有数据类型。结合下面的代码来理解应该更加清楚一些：

```kotlin
fun cvOf(vararg pairs: Pair<String, Any?>): ContentValues {
    val cv = ContentValues()
    for (pair in pairs) {
        val key = pair.first
        val value = pair.second
        when (value) {
            is Int, Long, Short, Float, Double, Boolean, String, Byte, ByteArray -> cv.put(key, value)
            else -> cv.putNull(key)
        }
    }
    return cv
}
```

可以看到，上述代码基本就是按照刚才所说的思路进行实现的。我们使用`for-in`循环遍历了`pairs`参数列表，在循环中取出了`key`和`value`，并使用`when`语句来判断`value`的类型。注意，这里将`ContentValues`所支持的所有数据类型全部覆盖了进去，然后将参数中传入的键值对逐个添加到`ContentValues`中，最终将`ContentValues`返回。

另外，这里还使用了`Kotlin`中的`Smart Cast`功能。比如`when`语句进入`Int`条件分支后，这个条件下面的`value`会被自动转换成`Int`类型，而不再是`Any?`类型，这样我们就不需要像`Java`中那样再额外进行一次向下转型了，这个功能在`if`语句中也同样适用。

有了这个`cvOf()`方法之后，我们使用`ContentValues`时就会变得更加简单了，比如向数据库中插入一条数据就可以这样写：

```kotlin
val values = cvOf("name" to "剑来", "author" to "烽火戏诸侯", "pages" to 60000, "price" to 60.99)
db.insert("book", null, valuse)
```

虽然`cvOf()`方法已经非常好用了，但是它和高阶函数却一点关系也没有。因为`cvOf()`方法接收的参数是Pair类型的可变参数列表，返回值是`ContentValues`对象，完全没有用到函数类型，这和高阶函数的定义不符。

从功能性方面，`cvOf()`方法好像确实用不到高阶函数的知识，但是从代码实现方面，却可以结合高阶函数来进行进一步的优化。比如借助`apply`函数，`cvOf()`方法的实现将会变得更加优雅：

```kotlin
fun ContentValues.cvOf(vararg pairs: Pair<String, Any?>) = apply {
    for (pair in pairs) {
        val key = pair.first
        val value = pair.second
        when (value) {
            is Int, Long, Short, Float, Double, Boolean, String, Byte, ByteArray -> put(key, value)
            else -> putNull(key)
        }
    }
}
```

由于`apply`函数的返回值就是它的调用对象本身，因此这里我们可以使用单行代码函数的语法糖，用等号替代返回值的声明。另外，`apply`函数的Lambda表达式中会自动拥有`ContentValues`的上下文，所以这里可以直接调用`ContentValues`的各种`put`方法。

其实KTX库中也提供了一个具有同样功能的contentValuesOf()方法，用法如下所示：

```kotlin
val values = contentValuesOf("name" to "剑来", "author" to "烽火戏诸侯", "pages" to 60000, "price" to 60.99)
db.insert("book", null, valuse)
```

---

## 六、泛型和委托

### 6.1 泛型的基本用法

准确来讲，泛型并不是什么新鲜的事物。Java早在1.5版本中就引入了泛型的机制，Kotlin自然也就支持了泛型功能。但是Kotlin中的泛型和Java中的泛型有同有异。

首先解释一下什么是泛型。在一般的编程模式下，我们需要给任何一个变量指定一个具体的类型，而**泛型允许我们在不指定具体类型的情况下进行编程，这样编写出来的代码将会拥有更好的扩展性。**

举个栗子🌰：List是一个可以存放数据的列表，但是List并没有限制我们只能存放整型数据或字符串数据，**因为它没有指定一个具体的类型，而是使用泛型来实现的**。也正是如此，我们才可以使用`List<Int>`、`List<String>`之类的语法来构建具体类型的列表。

那么要怎样才能定义自己的泛型实现呢？这里先来学习一下基本的语法。

泛型主要有两种定义方式：**一种是定义泛型类**，**另一种是定义泛型方法**，使用的语法结构都是`<T>`。**当然括号内的T并不是固定要求的，事实上你使用任何英文字母或单词都可以，但是通常情况下，T是一种约定俗成的泛型写法。**

如果我们要定义一个泛型类，就可以这么写：

```kotlin
class MyClass<T> {
    fun method(param: T): T{
        return param
    }
}
```

此时的MyClass就是一个泛型类，MyClass中的方法允许使用T类型的参数和返回值。

我们在调用MyClass类和method()方法的时候，就可以将泛型指定成具体的类型，如下所示：

```kotlin
val myClass = MyClass<Int>()
val result = myClass.method(123)
```

这里我们将MyClass类的泛型指定成Int类型，于是`method()`方法就可以接收一个Int类型的参数，并且它的返回值也变成了Int类型。

而如果我们不想定义一个泛型类，只是想定义一个泛型方法，应该要怎么写呢？也很简单，只需要将定义泛型的语法结构写在方法上面就可以了，如下所示：

```kotlin
class MyClass {
    fun <T> method(param: T): T {
        return param
    }
}
```

此时的调用方式也需要进行相应的调整：

```kotlin
val myClass = MyClass()
val result = myClass.method<Int>(123)
```

可以看到，现在是在调用method()方法的时候指定泛型类型了。另外，Kotlin还拥有非常出色的**类型推导机制**，例如**我们传入了一个Int类型的参数，它能够自动推导出泛型的类型就是Int型**，因此这里也可以直接省略泛型的指定：

```kotlin
val myClass = MyClass()
val result = myClass.method(123)
```

Kotlin**还允许我们对泛型的类型进行限制**。目前你可以将`method()`方法的泛型指定成任意类型，但是如果这并不是你想要的话，还**可以通过指定上界的方式来对泛型的类型进行约束**，**比如这里将`method()`方法的泛型上界设置为Number类型**，如下所示：

```kotlin
class MyClass {
    fun <T: Number> method(param: T): T{
        return param
    }
}
```

这种写法就表明，**我们只能将method()方法的泛型指定成数字类型**，比如`Int`、`Float`、`Double`等。但是**如果你指定成字符串类型，就肯定会报错，因为它不是一个数字。**

另外，**在默认情况下，所有的泛型都是可以指定成可空类型的**，这是因为在**不手动指定上界的时候**，**泛型的上界默认是`Any?`**。而如果**想要让泛型的类型不可为空**，只需要将泛型的**上界**手动**指定成**`Any`就可以了。

之前高阶函数那节有编写一个 build函数代码如下：

```kotlin
fun StringBuilder.build(block: StringBuilder.() -> Unit): StringBuilder {
    block()
    return this
}
```

这个函数的作用和`apply`函数基本是一样的，只是`build`函数只能作用在`StringBuilder`类上面，而`apply`函数是可以作用在**所有类**上面的。现在我们就通过本小节所学的泛型知识对build函数进行扩展，让它实现和apply函数完全一样的功能。

思考一下，其实并不复杂，只需要使用<T>将build函数定义成泛型函数，再将原来所有强制指定StringBuilder的地方都替换成T就可以了。新建一个build.kt文件，并编写如下代码：

```kotlin
fun <T> T.build(block: T.() -> Unit): T{
    block()
    return this
}
```

### 6.2 类委托和委托属性

**委托是一种设计模式，它的基本理念是：操作对象自己不会去处理某段逻辑，而是会把工作委托给另外一个辅助对象去处理。**这个概念对于Java程序员来讲可能相对比较陌生，因为Java对于委托并没有语言层级的实现，而像C#等语言就对委托进行了原生的支持。Kotlin中也是支持委托功能的，并且将**委托功能分为了两种：类委托和委托属性。**下面我们逐个进行学习。

#### 6.2.1 类委托

首先来看**类委托**，**它的核心思想在于将一个类的具体实现委托给另一个类去完成**。我们曾经使用过Set这种数据结构，它和**List**有点类似，只是它所存储的数据是无序的，并且不能存储重复的数据。**Set**是一个接口，如果要使用它的话，需要使用它具体的实现类，比如**HashSet**。而借助于委托模式，我们可以轻松实现一个自己的实现类。比如这里定义一个**MySet**，并让它实现**Set**接口，代码如下所示：

```kotlin
class MySet<T>(val helperSet: HashSet<T>) : Set<T>{
    override val size: Int
        get() = helperSet.size

    override fun isEmpty() = helperSet.isEmpty()

    override fun iterator(): Iterator<T> {
        return helperSet.iterator()
    }

    override fun containsAll(elements: Collection<T>): Boolean {
        return helperSet.containsAll(elements)
    }

    override fun contains(element: T): Boolean {
        return helperSet.contains(element)
    }
}
```

可以看到，`MySet`的构造函数中接收了一个`HashSet`参数，这就相当于一个辅助对象。然后在`Set`接口所有的方法实现中，我们都没有进行自己的实现，而是调用了辅助对象中相应的方法实现，这其实就是一种委托模式。

那么，这种写法的**好处**是什么呢？既然都是调用辅助对象的方法实现，那还不如直接使用辅助对象得了。这么说确实没错，但如果我们只是让大部分的方法实现调用辅助对象中的方法，少部分的方法实现由自己来重写，甚至加入一些自己独有的方法，那么`MySet`就会成为一个全新的数据结构类，这就是委托模式的意义所在。

但是这种写法也有一定的弊端，如果接口中的待实现方法比较少还好，要是有几十甚至上百个方法的话，每个都去这样调用辅助对象中的相应方法实现，那可真是要写哭了。那么这个问题有没有什么解决方案呢？在`Java`中确实没有，但是在`Kotlin`中可以通过类委托的功能来解决。

**Kotlin中委托使用的关键字是by**，我们只需要在接口声明的后面使用by关键字，再接上受委托的辅助对象，就可以免去之前所写的一大堆模板式的代码了，如下所示：使用类委派机制：

```kotlin
class MySet<T>(val helperSet: HashSet<T>) : Set<T> by helperSet{
    
}
```

这两段代码在功能上是一样的。它们都定义了一个名为 `MySet` 的类，这个类实现了 `Set` 接口，并且使用 `HashSet` 作为辅助工具来提供 `Set` 接口的实现。

然而，它们在实现方式上有所不同：

- 第一段代码中，`MySet` 类显式地实现了 `Set` 接口的每一个方法。每一个方法的实现都是通过调用 `helperSet` 的对应方法来完成的。
- 第二段代码中，`MySet` 类使用了 Kotlin 的类委托特性，将 `Set` 接口的所有方法的实现委托给了 `helperSet` 对象。这意味着 `MySet` 类会自动拥有 `Set` 的所有方法，并且这些方法的实现会直接使用 `helperSet` 的对应方法。

所以，虽然这两段代码在功能上是一样的，但是第二段代码更简洁，因为它利用了 Kotlin 的类委托特性，避免了手动实现每一个方法的需要。

在第二段代码中，如果我们要对某个方法进行重新实现，只需要单独重写那一个方法就可以了，其他的方法仍然可以享受类委托所带来的便利。

#### 6.2.2 委托属性

掌握了类委托之后，接下来我们开始学习委托属性。它的基本理念也非常容易理解，真正的难点在于如何灵活地进行应用。

类委托的核心思想是将一个类的具体实现委托给另一个类去完成，而委托属性的核心思想是将一个属性（字段）的具体实现委托给另一个类去完成。

我们看一下委托属性的语法结构，如下所示：

```kotlin
class MyClass {
    var p by Delegate()
}
```

可以看到，这里使用by关键字连接了左边的p属性和右边的`Delegate`实例，这是什么意思呢？这种写法就代表着将p属性的具体实现委托给了`Delegate`类去完成。当调用p属性的时候会自动调用`Delegate`类的`getValue()`方法，当给p属性赋值的时候会自动调用`Delegate`类的`setValue()`方法。

因此，我们还得对Delegate类进行具体的实现才行，代码如下所示：

```kotlin
class Delegate {
    var propValue : Any? = null
    operator fun getValue(myClass: MyClass, prop: KProperty<*>): Any? {
        return propValue
    }
    
    operator fun setValue(myClass: MyClass, prop: KProperty<*>, value: Any?) {
        propValue = value
    }
}
```

这是一种标准的代码实现模板，在Delegate类中我们必须实现`getValue()`和`setValue()`这两个方法，并且都要使用operator关键字进行声明。

`getValue()`方法要接收两个参数：第一个参数用于声明该Delegate类的委托功能可以在什么类中使用，这里写成MyClass表示仅可在MyClass类中使用；第二个参数`KProperty<*>`是Kotlin中的一个属性操作类，可用于获取各种属性相关的值，在当前场景下用不着，但是必须在方法参数上进行声明。另外，`<*>`这种泛型的写法表示你不知道或者不关心泛型的具体类型，只是为了通过语法编译而已，有点类似于Java中`<?>`的写法。至于返回值可以声明成任何类型，根据具体的实现逻辑去写就行了，上述代码只是一种示例写法。

`setValue()`方法也是相似的，只不过它要接收3个参数。前两个参数和`getValue()`方法是相同的，最后一个参数表示具体要赋值给委托属性的值，这个参数的类型必须和`getValue()`方法返回值的类型保持一致。

整个委托属性的工作流程就是这样实现的，现在当我们给`MyClass`的`p`属性赋值时，就会调用`Delegate`类的`setValue()`方法，当获取`MyClass`中`p`属性的值时，就会调用`Delegate`类的`getValue()`方法。

不过，其实还存在一种情况可以不用在`Delegate`类中实现`setValue()`方法，那就是`MyClass`中的p属性是使用`val`关键字声明的。这一点也很好理解，如果`p`属性是使用`val`关键字声明的，那么就意味着`p`属性是无法在初始化之后被重新赋值的，因此也就没有必要实现`setValue()`方法，只需要实现`getValue()`方法就可以了。

#### 6.2.3 实现一个自己的lazy函数

我们**初始化变量**时可以把想要延迟执行的代码放到by lazy代码块中，这样代码块中的代码在一开始的时候就不会执行，只有当**变量**首次被调用的时候，代码块中的代码才会执行。

学习了`Kotlin`的委托功能之后，我们就可以对`by lazy`的工作原理进行解密了，它的基本语法结构如下：

```kotlin
val p by lazy { ... }
```

实际上，`by lazy`并不是连在一起的关键字，只有`by`才是Kotlin中的关键字，`lazy`在这里只是一个高阶函数而已。在`lazy`函数中会创建并返回一个`Delegate`对象，当我们调用p属性的时候，其实调用的是`Delegate`对象的`getValue()`方法，然后`getValue()`方法中又会调用`lazy`函数传入的Lambda表达式，这样表达式中的代码就可以得到执行了，并且调用p属性后得到的值就是Lambda表达式中最后一行代码的返回值。

这样看来，Kotlin的懒加载技术也并没有那么神秘，掌握了它的实现原理之后，我们也可以实现一个自己的`lazy`函数。

```kotlin
import kotlin.reflect.KProperty
fun <T> later(block: () -> T) = Later(block)
class Later<T>(val block: () -> T) {
    private var value: Any? = null
    operator fun getValue(any: Any?, prop: KProperty<*>) : T {
        if (value == null){
            value = block()
        }
        println("value: $value")
        return value as T
    }
}

val heavyObject by later { HeavyObject() }

class HeavyObject {
    init {
        println("HeavyObject is being created.")
    }
    fun use(){
        println("HeavyObject is being used.")
    }
}

fun main() {
    println("main step in")
    // 在这里，heavyObject 还没有被创建
    if (needHeavyObject()) {
        println("if step in")
        // 在这里，heavyObject 被创建
        heavyObject.use()
    }
}

fun needHeavyObject(): Boolean {
    return true
}
```

这段代码演示了如何使用延迟初始化来创建 `heavyObject`。只有在 `heavyObject` 第一次被使用时，它才会被创建。这是一种很好的做法，特别是在对象创建代价高昂，且不总是需要该对象的情况下，可以帮助节省资源，提高应用程序的性能。

---

## 七、使用infix函数构建更可读的语法

在Kotlin中我们可以使用`A to B`这样的语法结构构建键值对，比如在Kotlin自带的`mapOf()`函数，这种语法结构的优点是可读性高，相比于调用一个函数，它更接近于使用英语的语法来编写程序。可能你会好奇，这种功能是怎么实现的呢？to是不是Kotlin语言中的一个关键字？

首先，to并不是Kotlin语言中的一个关键字，之所以我们能够使用`A to B`这样的语法结构，是因为Kotlin提供了一种高级语法糖特性：`infix`函数。当然，`infix`函数也并不是什么难理解的事物，它只是把编程语言函数调用的语法规则调整了一下而已，比如`A to B`这样的写法，实际上等价于`A.to(B)`的写法。

举个栗子🌰：

String类中有一个`startsWith()`函数，你一定使用过，它可以用于判断一个字符串是否是以某个指定参数开头的。比如说下面这段代码的判断结果一定会是true：

```kotlin
if("Hello Kotlin".startsWith("Hello")) {
    // TODO
}
```

startsWith()函数的用法虽然非常简单，但是借助infix函数，我们可以使用一种更具可读性的语法来表达这段代码。新建一个infix.kt文件，然后编写如下代码：

```kotlin
infix fun String.beginsWith(prefix: String) = startsWith(prefix)
```

除去最前面的infix关键字不谈，这是一个String类的扩展函数。我们给String类添加了一个`beginsWith()`函数，它也是用于判断一个字符串是否是以某个指定参数开头的，并且它的内部实现就是调用的String类的`startsWith()`函数。

但是加上了infix关键字之后，`beginsWith()`函数就变成了一个infix函数，这样除了传统的函数调用方式之外，我们还可以用一种特殊的语法糖格式调用`beginsWith()`函数，如下所示：

```kotlin
if("Hello Kotlin" beginsWith "Hello") {
    // TODO
}
```

从这个例子就能看出，infix函数的语法规则并不复杂，上述代码其实就是调用的" Hello Kotlin "这个字符串的`beginsWith()`函数，并传入了一个"Hello"字符串作为参数。但是infix函数允许我们将函数调用时的小数点、括号等计算机相关的语法去掉，从而使用一种更接近英语的语法来编写程序，让代码看起来更加具有可读性。

另外，infix函数由于其语法糖格式的特殊性，有两个比较严格的限制：首先，**infix函数是不能定义成顶层函数的，它必须是某个类的成员函数，可以使用扩展函数的方式将它定义到某个类当中**；其次，**infix函数必须接收且只能接收一个参数，至于参数类型是没有限制的**。只有同时满足这两点，infix函数的语法糖才具备使用的条件.

再举个栗子🌰：

比如这里有一个集合，如果想要判断集合中是否包括某个指定元素，一般可以这样写：

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
if (list.contains("Banana")) {
    // TODO
}
```

很简单对吗？但我们仍然可以借助infix函数让这段代码变得更加具有可读性。在infix.kt文件中添加如下代码：

```kotlin
infix fun <T> Collection<T>.has(element: T) = contains(element)
```

可以看到，我们给`Collection`接口添加了一个扩展函数，这是因为**Collection是Java以及Kotlin所有集合的总接口**，因此给Collection添加一个**has()**函数，那么所有集合的子类就都可以使用这个函数了。

另外，这里还使用了上一章中学习的泛型函数的定义方法，从而使得`has()`函数可以接收任意具体类型的参数。而这个函数内部的实现逻辑就相当简单了，只是调用了`Collection`接口中的`contains()`函数而已。也就是说，`has()`函数和`contains()`函数的功能实际上是一模一样的，只是它多了一个infix关键字，从而拥有了infix函数的语法糖功能。

现在我们就可以使用如下的语法来判断集合中是否包括某个指定的元素：

```kotlin
val list = listOf("Apple", "Banana", "Orange", "Pear", "Grape")
if (list has "Banana") {
    // TODO
}
```



解析一下 `A to B`中 中缀函数 `to` 的实现，其实就只有一段代码：

```kotlin
public infix fun <A, B> A.to(that: B): Pair<A, B> = Pair(this, that)
```

这是Kotlin编程语言中的一种函数声明。我们先分解一下：

1. `public`：这是函数的访问修饰符，表示这个函数可以在任何位置被访问。
2. `infix`：这是一个在Kotlin中表征中缀函数的关键字。所谓中缀函数，就是可以使用更自然的语言风格调用，并且该函数要满足“它们必须是成员函数或扩展函数、它们必须有一个参数、它们的参数不能接受可变数量的参数且不能有默认值”。
3. `<A, B>`：这是泛型参数列表，表明这个函数可以对任意类型的对象进行操作。
4. `A.to(that: B)`：这是函数的声明，函数名为`to`，参数为名为`that`的`B`类型对象。
5. `Pair<A, B>`：这是函数的返回类型，表示函数返回一个包含两个元素，类型分别为A和B的Pair对象。
6. `Pair(this, that)`：这是函数的实现，创建一个新的Pair对象，第一个元素是调用`to`函数的对象，第二个元素是函数的参数。

整个函数可以这样理解：对任意类型A的对象，我们定义了一个函数`to`，它接受一个任意类型B的对象作为参数，并返回一个Pair<A, B>对象。

举个例子🌰：假设有两个变量a和b：

```kotlin
val a = 1
val b = 2
```

那么，我们可以使用中缀函数简洁地构造Pair对象：a to b，代表一对值：1和2

```kotlin
val pair = a to b
// pair is Pair<Int, Int>(1, 2)
```

---

## 八、泛型的高级特性

之前在[6.1](###6.1 泛型的基本用法)中学习了**Kotlin**泛型的基本用法。这些基本用法其实和**Java**中泛型的用法是大致相同的，因此也相对比较好理解。然而实际上，**Kotlin**在泛型方面还提供了不少特有的功能，掌握了这些功能，你将可以更好玩转**Kotlin**，同时还能实现一些不可思议的语法特性。

### 8.1 对泛型进行实化

泛型实化这个功能对于绝大多数**Java**程序员来讲是非常陌生的，因为**Java**中完全没有这个概念。而如果我们想要深刻地理解泛型实化，就要先解释一下**Java**的泛型擦除机制才行。

在**JDK 1.5**之前，**Java**是没有泛型功能的，那个时候诸如**List**之类的数据结构可以存储任意类型的数据，取出数据的时候也需要手动向下转型才行，这不仅麻烦，而且很危险。比如说我们在同一个**List**中存储了字符串和整型这两种数据，但是在取出数据的时候却无法区分具体的数据类型，如果手动将它们强制转成同一种类型，那么就会抛出类型转换异常。

于是在**JDK 1.5**中，**Java**终于引入了泛型功能。这不仅让诸如**List**之类的数据结构变得简单好用，也让我们的代码变得更加安全。

但是实际上，**Java**的泛型功能是通过类型擦除机制来实现的。什么意思呢？就是说泛型对于类型的约束只在编译时期存在，运行的时候仍然会按照**JDK 1.5**之前的机制来运行，JVM是识别不出来我们在代码中指定的泛型类型的。例如，假设我们创建了一个`List<String>`集合，虽然在编译时期只能向集合中添加字符串类型的元素，但是在运行时期**JVM**并不能知道它本来只打算包含哪种类型的元素，只能识别出来它是个**List**。

所有基于**JVM**的语言，它们的泛型功能都是通过类型擦除机制来实现的，其中当然也包括了**Kotlin**。这种机制使得我们不可能使用`a is T`或者`T::class.java`这样的语法，因为T的实际类型在运行的时候已经被擦除了。

然而不同的是，**Kotlin**提供了一个内联函数的概念，我们在第[4.2](###4.2 内联函数的作用)中已经学过了这个知识点。内联函数中的代码会在编译的时候自动被替换到调用它的地方，这样的话也就不存在什么泛型擦除的问题了，因为代码在编译之后会直接使用实际的类型来替代内联函数中的泛型声明，其工作原理如下图所示。

![1706065068503.png](https://pic.ziyuan.wang/user/xiheya/2024/01/1706065068503_a6941c17468b5.png)

最终代码会被替换成如下所示：

```kotlin
fun foo() {
    // do something with String type
}
```

可以看到，`bar()`是一个带有泛型类型的内联函数，`foo()`函数调用了`bar()`函数，在代码编译之后，`bar()`函数中的代码将可以获得泛型的实际类型。**这就意味着，Kotlin中是可以将内联函数中的泛型进行实化的。**

那么具体该怎么写才能将泛型实化呢？**首先，该函数必须是内联函数才行，也就是要用`inline`关键字来修饰该函数。其次，在声明泛型的地方必须加上`reified`关键字来表示该泛型要进行实化。**示例代码如下：

```kotlin
inline fun <reified T> getGenericType() {
    // todo
}
```

上述函数中的泛型**T**就是一个被实化的泛型，因为它满足了内联函数和**reified**关键字这两个前提条件。那么借助泛型实化，到底可以实现什么样的效果呢？从函数名就可以看出来了，这里我们准备实现一个获取泛型实际类型的功能，代码如下所示：

```kotlin
inline fun <reified T> getGenericType() = T::class.java
```

虽然只有一行代码，但是这里却实现了一个**Java**中完全不可能实现的功能：`getGenericType()`函数直接返回了当前指定泛型的实际类型。`T.class`这样的语法在**Java**中是不合法的，而在**Kotlin**中，借助泛型实化功能就可以使用`T::class.java`这样的语法了。

```kotlin
fun main(args: Array<String>) {
    val result1 = getGenericType<String>()
    val result2 = getGenericType<Int>()
    println("result1 is $result1")
    println("result2 is $result2")
}

inline fun <reified T> getGenericType() = T::class.java
```

运行结果打印如下：

```tex
result1 is class java.lang.String
result2 is class java.lang.Integer
```

如果将泛型指定成了**String**，那么就可以得到`java.lang.String`的类型；如果将泛型指定了**Int**，就可以得到`java.lang.Integer`的类型。

接下来学习泛型实化的应用

### 8.2 泛型实化的应用

泛型实化功能允许我们在泛型函数当中获得泛型的实际类型，这也就使得类似于`a is T`、`T::class.java`这样的语法成为了可能。而灵活运用这一特性将可以实现一些不可思议的语法结构。在**Android**四大组件当中，除了**ContentProvider**之外，**Activity**、**Service**还有**BroadcastReceiver**都需要结合 **Intent**一起使用。

就拿启动Activity来说：

我们可以这么写：

```kotlin
val intent = Intent(context, TestActivity::class.java)
context.startActivity(intent)
```

有没有觉得`TestActivity::class.java`这样的语法很难受呢？当然，如果在没有更好选择的情况下，这种写法也是可以忍受的，但是**Kotlin**的泛型实化功能使得我们拥有了更好的选择。

新建一个`reified.kt`文件，然后在里面编写如下代码：

```kotlin
inline fun <reified T> startActivity(context: Context) {
    val intent = Intent(context, T::class.java)
    context.startActivity(intent)
}
```

这里我们定义了一个`startActivity()`函数，该函数接收一个**Context**参数，并同时使用**inline**和**reified**关键字让泛型T成为了一个被实化的泛型。接下来就是神奇的地方了，**Intent**接收的第二个参数本来应该是一个具体**Activity**的**Class**类型，但由于现在**T**已经是一个被实化的泛型了，因此这里我们可以直接传入`T::class.java`。最后调用**Context**的`startActivity()`方法来完成**Activity**的启动。

现在，如果我们要启动**TestActivity**，只需要这样写：

```kotlin
startActivity<TestActivity>(context)
```

不过，现在的`startActivity()`函数其实还是有问题的，因为通常在启用**Activity**的时候还可能会使用**Intent**附带一些参数，比如下面的写法：

```kotlin
val intent = Intent(context, TestActivity::class.java)
intent.putExtra("param1", "data")
intent.putExtra("param2", 123)
context.startActivity(intent)
```

而经过刚才的封装之后，我们就无法进行传参了。这个问题也不难解决，只需要借助之前学习的高阶函数就可以轻松搞定。回到`reified.kt`文件当中，这里添加一个新的`startActivity()`函数重载，如下所示:

```kotlin
inline fun <reified T> startActivity(context: Context, block: Intent.() -> Unit) {
    val intent = Intent(context, T::class.java)
    intent.block()
    context.startActivity(intent)
}
```

可以看到，这次的`startActivity()`函数中增加了一个函数类型参数，并且它的函数类型是定义在**Intent**类当中的。在创建完**Intent**的实例之后，随即调用该函数类型参数，并把**Intent**的实例传入，这样调用`startActivity()`函数的时候就可以在**Lambda**表达式中为**Intent**传递参数了，如下所示：

```kotlin
startActivity<TestActivity>(context) {
    putExtra("param1", "data")
    putExtra("param2", 123)
}
```

### 8.3 泛型的协变

在开始学习协变和逆变之前，我们还得先了解一个约定。一个泛型类或者泛型接口中的方法，它的参数列表是接收数据的地方，因此可以称它为**in**位置，而它的返回值是输出数据的地方，因此可以称它为**out**位置，如下图所示。

![1706256361291.png](https://pic.ziyuan.wang/user/xiheya/2024/01/1706256361291_697ba5b7d4ec0.png)

有了这个约定前提，接下来继续学习，首先定义如下三个类：

```kotlin
open class Person(val name: String, val age: Int)
class Student(name: String, age: Int) : Person(name, age)
class Teacher(name: String, age: Int) : Person(name, age)
```

这里先定义了一个**Person**类，类中包含**name**和**age**这两个字段。然后又定义了**Student**和**Teacher**这两个类，让它们成为**Person**类的子类。

如果某个方法接收一个**Person**类型的参数，而我们传入一个**Student**的实例，这样合不合法呢？很显然，因为**Student**是**Person**的子类，学生也是人呀，因此这是一定合法的。

如果某个方法接收一个`List<Person>`类型的参数，而我们传入一个`List<Student>`的实例，这样合不合法呢？看上去好像也挺正确的，但是Java中是不允许这么做的，因为`List<Student>`不能成为`List<Person>`的子类，**否则将可能存在类型转换的安全隐患。**

为什么会存在类型转换的安全隐患呢？下面我们通过一个具体的例子进行说明。自定义一个`SimpleData`类以及一个`handleSimpleData(data: SimpleData<Person>)`方法

```kotlin
class SimpleData<T> {
    private var data: T? = null
    fun set(t: T?) {
        data = t
    }
    fun get() : T? {
        return data
    }
}

fun handleSimpleData(data: SimpleData<Person>) {
    val teacher = Teacher("Jack", 35)
    data.set(teacher)
}
```

**SimpleData**是一个泛型类，它的内部封装了一个泛型**data**字段，调用`set()`方法可以给**data**字段赋值，调用`get()`方法可以获取**data**字段的值。

接着我们假设，如果编程语言允许向某个接收`SimpleData<Person>`参数的方法传入`SimpleData<Student>`的实例，那么如下代码就会是合法的：

```kotlin
 fun main() {
    val student = Student("Tom", 19)
    val data = SimpleData<Student>()
    data.set(student)
    handleSimpleData(data) //这里会报错
    val studentData = data.get()
}
```

> Type mismatch.
>
> 
>
> Required:
> SimpleData<Person>
> Found:
> SimpleData<Student>

可以看到报错信息是类型不匹配，这里需要一个 `SimpleData<Person>` 而我们却传给了他 `SimpleData<Student>`

在`main()`方法中，我们创建了一个**Student**的实例，并将它封装到`SimpleData<Student>`当中，然后将`SimpleData<Student>`作为参数传递给`handleSimpleData()`方法。但是`handleSimpleData()`方法接收的是一个**SimpleData<Person>**参数（这里假设可以编译通过），那么在`handleSimpleData()`方法中，我们就可以创建一个**Teacher**的实例，并用它来替换`SimpleData<Person>`参数中的原有数据。**这种操作肯定是合法的，因为Teacher也是Person的子类，所以可以很安全地将Teacher的实例设置进去。**

但是问题马上来了，回到`main()`方法当中，我们调用`SimpleData<Student>`的`get()`方法来获取它内部封装的**Student**数据，**可现在`SimpleData<Student>`中实际包含的却是一个Teacher的实例，那么此时必然会产生类型转换异常。**

所以，为了杜绝这种安全隐患，**Java**是不允许使用这种方式来传递参数的。换句话说，即使**Student**是**Person**的子类，`SimpleData<Student>`并不是`SimpleData<Person>`的子类。

不过，回顾一下刚才的代码，你会发现问题发生的主要原因是我们在`handleSimpleData()`方法中向`SimpleData<Person>`里设置了一个**Teacher**的实例。如果**SimpleData**在泛型T上是只读的话，肯定就没有类型转换的安全隐患了，那么这个时候`SimpleData<Student>`可不可以成为`SimpleData<Person>`的子类呢？

讲到这里，我们终于要引出泛型协变的定义了。**假如定义了一个`MyClass<T>`的泛型类，其中A是B的子类型，同时`MyClass<A>`又是`MyClass<B>`的子类型，那么我们就可以称`MyClass`在T这个泛型上是协变的。**

但是如何才能让`MyClass<A>`成为`MyClass<B>`的子类型呢？刚才已经讲了，**如果一个泛型类在其泛型类型的数据上是只读的话，那么它是没有类型转换安全隐患的。**而要实现这一点，则需要让`MyClass<T>`类中的**所有方法都不能接收T类型的参数**。换句话说，**T只能出现在out位置上，而不能出现在in位置上。**

现在修改**SimpleData**类的代码，如下所示：

```kotlin
class SimpleData<out T> (private val data: T?) {
    /*private var data: T? = null
    fun set(t: T?) {
        data = t
    }*/
    fun get() : T? {
        return data
    }
}
```

这里我们对**SimpleData**类进行了改造，在泛型T的声明前面加上了一个**out**关键字。这就意味着现在T只能出现在**out**位置上，而不能出现在**in**位置上，同时也意味着**SimpleData**在**泛型T**上是协变的。

由于**泛型T**不能出现在**in**位置上，因此我们也就不能使用`set()`方法为**data**参数赋值了，所以这里改成了使用构造函数的方式来赋值。你可能会说，构造函数中的泛型T不也是在**in**位置上的吗？没错，但是由于这里我们使用了**val**关键字，所以构造函数中的**泛型T**仍然是只读的，因此这样写是合法且安全的。另外，即使我们使用了**var**关键字，但只要给它加上**private**修饰符，保证这个泛型T对于外部而言是不可修改的，那么就都是合法的写法。

经过了这样的修改之后，下面的代码就可以完美编译通过且没有任何安全隐患了：

```kotlin
fun main() {
    val student = Student("Tom", 19)
    val data = SimpleData<Student>(student)
    handleSimpleData(data)
    val studentData = data.get()
}
fun handleSimpleData(data: SimpleData<Person>) {
    val personData = data.get()
}
```

由于**SimpleData**类已经进行了协变声明，那么`SimpleData<Student>`自然就是`SimpleData<Person>`的子类了，所以这里可以安全地向`handleSimpleData()`方法中传递参数。

然后在`handleSimpleData()`方法中去获取**SimpleData**封装的数据，虽然这里泛型声明的是**Person**类型，实际获得的会是一个**Student**的实例，**但由于Person是Student的父类，向上转型是完全安全的**，所以这段代码没有任何问题。

### 8.4 泛型的逆变

仅从定义上来看，逆变与协变却完全相反。那么这里先引出定义吧，**假如定义了一个MyClass<T>的泛型类，其中A是B的子类型，同时MyClass<B>又是MyClass<A>的子类型，那么我们就可以称MyClass在T这个泛型上是逆变的。**协变和逆变的区别如下图所示：

![1706497381453.png](https://pic.ziyuan.wang/user/xiheya/2024/01/1706497381453_2f65e27d0d37d.png)

从直观的角度上来思考，逆变的规则好像挺奇怪的，原本**A是B的子类型**，怎么**MyClass<B>**能反过来成为**MyClass<A>**的子类型了呢？

举个栗子🌰：

先定义一个 `Transformer` 接口，用于一些转换操作，代码如下：

```kotlin
interface Transformer<T> {
    fun transform(t: T): String
}
```

可以看到，**Transformer**接口中声明了一个`transform()`方法，它接收一个T类型的参数，并且返回一个**String**类型的数据，这意味着参数T在经过`transform()`方法的转换之后将会变成一个字符串。至于具体的转换逻辑是什么样的，则由子类去实现，**Transformer**接口对此并不关心。

那么现在我们就尝试对**Transformer**接口进行实现，代码如下所示:

```kotlin
fun main() {
    val trans = object : Transformer<Person> {
        override fun transform(t: Person) : String {
            return "${t.name} ${t.age}"
        }
    }
    handleTransformer(trans)  // error
}
fun handleTransformer(trans: Transformer<Student>) {
    val student = Student("Tom", 19)
    val result = trans.transform(student)
}
```

首先我们在`main()`方法中编写了一个`Transformer<Person>`的匿名类实现，并通过`transform()`方法将传入的**Person**对象转换成了一个“姓名+年龄”拼接的字符串。而`handleTransformer()`方法接收的是一个`Transformer<Student>`类型的参数，这里在`handleTransformer()`方法中创建了一个**Student**对象，并调用参数的`transform()`方法将**Student**对象转换成一个字符串。

这段代码从安全的角度来分析是没有任何问题的，因为**Student**是**Person**的子类，使用`Transformer<Person>`的匿名类实现将**Student**对象转换成一个字符串也是绝对安全的，并不存在类型转换的安全隐患。但是实际上，在调用`handleTransformer()`方法的时候却会提示语法错误，原因也很简单，`Transformer<Person>`并不是`Transformer<Student>`的子类型。

那么这个时候逆变就可以派上用场了，它就是专门用于处理这种情况的。修改**Transformer**接口中的代码，如下所示：

```kotlin
interface Transformer<in T> {
    fun transform(t: T): String
}
```

这里我们在泛型T的声明前面加上了一个**in**关键字。这就意味着现在**T**只能出现在**in**位置上，而不能出现在**out**位置上，同时也意味着**Transformer**在泛型**T**上是逆变的。

没错，只要做了这样一点修改，刚才的代码就可以编译通过且正常运行了，因为此时`Transformer<Person>`已经成为了`Transformer<Student>`的子类型。

逆变的用法大概就是这样了，如果你还想再深入思考一下的话，可以想一想为什么逆变的时候泛型**T**不能出现在**out**位置上？为了解释这个问题，我们先假设逆变是允许让泛型T出现在out位置上的，然后看一看可能会产生什么样的安全隐患。修改**Transformer**中的代码，如下所示：

```kotlin
interface Transformer<in T> {
    fun transform(name: String, age: Int): @UnsafeVariance T
}
```

可以看到，我们将`transform()`方法改成了接收**name**和**age**这两个参数，并把返回值类型改成了泛型**T**。由于逆变是不允许泛型**T**出现在**out**位置上的，这里为了能让编译器正常编译通过，所以加上了`@UnsafeVariance`注解，这和**List**源码中使用的技巧是一样的。

这个时候会产生什么安全隐患呢？来看看代码：

```kotlin
fun main() {
    val trans = object : Transformer<Person> {
        override fun transform(name: String, age: Int): Person {
            return Teacher(name, age)
        }
    }
    handleTransformer(trans)
}

fun handleTransformer(trans: Transformer<Student>) {
    val result = trans.transform("Tom", 19)
}
```

上述代码就是一个典型的违反逆变规则而造成类型转换异常的例子。在`Transformer<Person>`的匿名类实现中，我们使用`transform()`方法中传入的**name**和**age**参数构建了一个**Teacher**对象，并把这个对象直接返回。由于`transform()`方法的返回值要求是一个**Person**对象，而**Teacher**是**Person**的子类，因此这种写法肯定是合法的。

但在`handleTransformer()`方法当中，我们调用了`Transformer<Student>`的`transform()`方法，并传入了**name**和**age**这两个参数，期望得到的是一个**Student**对象的返回，然而实际上`transform()`方法返回的却是一个**Teacher**对象，因此这里必然会造成类型转换异常。

异常信息如下：

![1708223607170.png](https://pic.ziyuan.wang/user/xiheya/2024/02/1708223607170_02ba4f0d6b5cd.png)

可以看到，提示我们**Teacher**类型是无法转换成**Student**类型的。

也就是说，**Kotlin**在提供协变和逆变功能时，就已经把各种潜在的类型转换安全隐患全部考虑进去了。只要我们严格按照其语法规则，让泛型在协变时只出现在**out**位置上，逆变时只出现在**in**位置上，就不会存在类型转换异常的情况。虽然`@UnsafeVariance`注解可以打破这一语法规则，但同时也会带来额外的风险，所以你在使用`@UnsafeVariance`注解时，必须很清楚自己在干什么才行。

最后我们再来介绍一下逆变功能在**Kotlin**内置**API**中的应用，比较典型的例子就是**Comparable**的使用。**Comparable**是一个用于比较两个对象大小的接口，其源码定义如下：

```kotlin
interface Comparable<in T> {
    operator fun compareTo(other: T): Int
}
```

可以看到，**Comparable**在**T**这个泛型上就是逆变的，`compareTo()`方法则用于实现具体的比较逻辑。那么这里为什么要让**Comparable**接口是逆变的呢？想象如下场景，如果我们使用`Comparable<Person>`实现了让两个**Person**对象比较大小的逻辑，那么用这段逻辑去比较两个**Student**对象的大小也一定是成立的，因此让`Comparable<Person>`成为`Comparable<Student>`的子类合情合理，这也是逆变非常典型的应用。

---

## 九、使用协程编写高效的并发程序

**协程属于Kotlin中非常有特色的一项技术**，因为大部分编程语言中是没有协程这个概念的。

**那么什么是协程呢？它其实和线程是有点类似的，可以简单地将它理解成一种轻量级的线程。**要知道，我们之前所学习的线程是非常重量级的，它需要依靠操作系统的调度才能实现不同线程之间的切换。而**使用协程却可以仅在编程语言的层面就能实现不同协程之间的切换，从而大大提升了并发编程的运行效率。**

举一个具体点的例子，比如我们有如下**foo()**和**bar()**两个方法：

```kotlin
fun foo() {
    a()
    b()
    c()
}

fun bar() {
    x()
    y()
    z()
}
```

在没有开启线程的情况下，先后调用`foo()`和`bar()`这两个方法，那么理论上结果一定是`a()`、`b()`、`c()`执行完了以后，`x()`、`y()`、`z()`才能够得到执行。而如果使用了协程，在协程A中去调用`foo()`方法，协程B中去调用`bar()`方法，虽然它们仍然会运行在同一个线程当中，但是在执行`foo()`方法时随时都有可能被挂起转而去执行`bar()`方法，执行`bar()`方法时也随时都有可能被挂起转而继续执行`foo()`方法，最终的输出结果也就变得不确定了。

可以看出，**协程允许我们在单线程模式下模拟多线程编程的效果，代码执行时的挂起与恢复完全是由编程语言来控制的，和操作系统无关。**这种特性使得高并发程序的运行效率得到了极大的提升，试想一下，开启10万个线程完全是不可想象的事吧？而开启10万个协程就是完全可行的。

### 9.1 协程的基本用法

Kotlin并没有将协程纳入标准库的API当中，而是以依赖库的形式提供的。所以如果我们想要使用协程功能，需要先在app/build.gradle文件当中添加如下依赖库：

```kotlin
dependencies {
    ...
    implementation "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.8.0"
	implementation "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.8.0"
}
```

首先我们要面临的第一个问题就是，如何开启一个协程？最简单的方式就是使用`Global.launch`函数，如下所示：

```kotlin
fun main() {
    GlobalScope.launch {
        println("codes run in coroutine scope")
    }
}
```

`GlobalScope.launch`函数可以创建一个协程的作用域，这样传递给**launch**函数的代码块（Lambda表达式）就是在协程中运行的了，这里我们只是在代码块中打印了一行日志。那么现在运行`main()`函数，日志能成功打印出来吗？如果你尝试一下，会发现没有任何日志输出。**这是因为，Global.launch函数每次创建的都是一个顶层协程，这种协程当应用程序运行结束时也会跟着一起结束。**刚才的日志之所以无法打印出来，就是因为代码块中的代码还没来得及运行，应用程序就结束了。

要解决这个问题也很简单，我们让程序延迟一段时间再结束就行了，如下所示：

```kotlin
fun main() {
    GlobalScope.launch {
        println("codes run in coroutine scope")
    }
    Thread.sleep(1000)
}
```

这里使用Thread.sleep()方法让主线程阻塞1秒钟，现在重新运行程序，日志就可以打印出来了。

可是这种写法还是存在问题，如果代码块中的代码在1秒钟之内不能运行结束，那么就会被强制
中断。观察如下代码：

```kotlin
fun main() {
    GlobalScope.launch {
        println("codes run in coroutine scope")
        delay(1500)
        println("codes run in coroutine scope finished")
    }
    Thread.sleep(1000)
}
```

我们在代码块中加入了一个`delay()`函数，并在之后又打印了一行日志。`delay()`函数可以让当前协程延迟指定时间后再运行，但它和`Thread.sleep()`方法不同。`delay()`函数是一个非阻塞式的挂起函数，它只会挂起当前协程，并不会影响其他协程的运行。而`Thread.sleep()`方法会阻塞当前的线程，这样运行在该线程下的所有协程都会被阻塞。注意，`delay()`函数只能在协程的作用域或其他挂起函数中调用。

这里我们让协程挂起1.5秒，但是主线程却只阻塞了1秒，最终会是什么结果呢？重新运行程序，你会发现代码块中新增的一条日志并没有打印出来，因为它还没能来得及运行，应用程序就已经结束了。

那么有没有什么办法能让应用程序在协程中所有代码都运行完了之后再结束呢？当然也是有的，借助**runBlocking**函数就可以实现这个功能：

```kotlin
fun main() {
    runBlocking {
        println("codes run in coroutine scope")
        delay(1500)
        println("codes run in coroutine scope finished")
    }
}
```

**runBlocking**函数同样会创建一个协程的作用域，但是它可以保证在协程作用域内的所有代码和子协程没有全部执行完之前一直阻塞当前线程。需要注意的是，**runBlocking**函数通常只应该在测试环境下使用，在正式环境中使用容易产生一些性能上的问题。

虽说现在我们已经能够让代码在协程中运行了，可是好像并没有体会到什么特别的好处。这是因为目前所有的代码都是运行在同一个协程当中的，而一旦涉及高并发的应用场景，协程相比于线程的优势就能体现出来了。

那么如何才能创建多个协程呢？很简单，使用**launch**函数就可以了，如下所示：

```kotlin
fun main() {
    runBlocking {
        launch {
            println("launch1")
            delay(500)
            println("launch1 finished")
        }
        launch {
            println("launch2 scope")
            delay(500)
            println("launch2 finished")
        }
    }
}
```

注意这里的**launch**函数和我们刚才所使用的`GlobalScope.launch`函数不同。首先它必须在协程的作用域中才能调用，其次它会在当前协程的作用域下创建子协程。子协程的特点是如果外层作用域的协程结束了，该作用域下的所有子协程也会一同结束。相比而言，`GlobalScope.launch`函数创建的永远是顶层协程，这一点和线程比较像，因为线程也没有层级这一说，永远都是顶层的。

这里我们调用了两次**launch**函数，也就是创建了两个子协程。打印结果如下

![1709102783783.png](https://pic.ziyuan.wang/user/guest/2024/02/1709102783783_31d25f330b374.png)

可以看到，两个子协程中的日志是交替打印的，说明它们确实是像多线程那样并发运行的。然而这两个子协程实际却运行在同一个线程当中，只是由编程语言来决定如何在多个协程之间进行调度，让谁运行，让谁挂起。调度的过程完全不需要操作系统参与，这也就使得协程的并发效率会出奇得高。

那么具体会有多高呢？接下来做个实验。

```kotlin
fun main() {
    val start = System.currentTimeMillis()
    runBlocking {
        repeat(100000) {
            launch {
                println(".")
            }
        }
    }
    val end = System.currentTimeMillis()
    println(end - start)
}
```

这里使用**repeat**函数循环创建了10万个协程，不过在协程当中并没有进行什么有意义的操作，只是象征性地打印了一个点，然后记录一下整个操作的运行耗时。现在重新运行一下程序，结果如下：

![1709103029256.png](https://pic.ziyuan.wang/user/guest/2024/02/1709103029256_9a71690c55802.png)

可以看到，这里仅仅耗时了541毫秒，这足以证明协程有多么高效。试想一下，如果开启的是10万个线程，程序或许已经出现OOM异常了。

不过，随着**launch**函数中的逻辑越来越复杂，可能你需要将部分代码提取到一个单独的函数中。这个时候就产生了一个问题：我们在**launch**函数中编写的代码是拥有协程作用域的，但是提取到一个单独的函数中就没有协程作用域了，那么我们该如何调用像`delay()`这样的挂起函数呢？

为此**Kotlin**提供了一个**suspend**关键字，**使用它可以将任意函数声明成挂起函数，而挂起函数之间都是可以互相调用的**，如下所示：

```kotlin
suspend fun printDot() {
    println(".")
    delay(1000)
}
```

这样就可以在`printDot()`函数中调用`delay()`函数了。

但是，**suspend**关键字只能将一个函数声明成挂起函数，是无法给它提供协程作用域的。比如你现在尝试在`printDot()`函数中调用**launch**函数，一定是无法调用成功的，因为**launch**函数要求必须在协程作用域当中才能调用。

这个问题可以借助**coroutineScope**函数来解决。**coroutineScope**函数也是一个挂起函数，因此可以在任何其他挂起函数中调用。它的特点是会继承外部的协程的作用域并创建一个子协程，借助这个特性，我们就可以给任意挂起函数提供协程作用域了。示例写法如下：

```kotlin
suspend fun printDot() = coroutineScope {
    launch {
        println(".")
        delay(1000)
    }
}
```

可以看到，现在我们就可以在`printDot()`这个挂起函数中调用**launch**函数了。另外，**coroutineScope**函数和**runBlocking**函数还有点类似，它可以保证其作用域内的所有代码和子协程在全部执行完之前，外部的协程会一直被挂起。我们来看如下示例代码：

```kotlin
fun main() {
    runBlocking {
        coroutineScope {
            launch {
                for (i in 1..10) {
                    println(i)
                    delay(200)
                }
            }
        }
        println("coroutineScope finished")
    }
    println("runBlocking finished")
}
```

这里先使用**runBlocking**函数创建了一个协程作用域，然后调用**coroutineScope**函数创建了一个子协程。在**coroutineScope**的作用域中，我们又调用**launch**函数创建了一个子协程，并通过for循环依次打印数字1到10，每次打印间隔一秒钟。最后在**runBlocking**和**coroutineScope**函数的结尾，分别又打印了一行日志。现在重新运行一下程序，结果如图:

![1709104874406.png](https://pic.ziyuan.wang/user/guest/2024/02/1709104874406_707690775e9a7.png)

你会看到，控制台会以200ms的间隔依次输出数字1到10，然后才会打印**coroutineScope**函数结尾的日志，最后打印**runBlocking**函数结尾的日志。

由此可见，**coroutineScope**函数确实是将外部协程挂起了，只有当它作用域内的所有代码和子协程都执行完毕之后，**coroutineScope**函数之后的代码才能得到运行。

虽然看上去**coroutineScope**函数和**runBlocking**函数的作用是有点类似的，但是**coroutineScope**函数只会阻塞当前协程，既不影响其他协程，也不影响任何线程，因此是不会造成任何性能上的问题的。而**runBlocking函数由于会挂起外部线程，如果你恰好又在主线程中当中调用它的话，那么就有可能会导致界面卡死的情况，所以不太推荐在实际项目中使用。**

### 9.2 更多的作用域构建器

我们学习了`GlobalScope.launch`、`runBlocking`、`launch`、`coroutineScope`这几种作用域构建器，它们都可以用于创建一个新的协程作用域。**不过`GlobalScope.launch`和`runBlocking`函数是可以在任意地方调用的，`coroutineScope`函数可以在协程作用域或挂起函数中调用，而`launch`函数只能在协程作用域中调用。**

前面已经说了，`runBlocking`由于会阻塞线程，因此只建议在测试环境下使用。而`GlobalScope.launch`由于每次创建的都是顶层协程，一般也不太建议使用，除非你非常明确就是要创建顶层协程。

为什么说不太建议使用顶层协程呢？主要还是因为它管理起来成本太高了。举个例子，比如我们在某个**Activity**中使用协程发起了一条网络请求，由于网络请求是耗时的，用户在服务器还没来得及响应的情况下就关闭了当前**Activity**，此时按理说应该取消这条网络请求，或者至少不应该进行回调，因为**Activity**已经不存在了，回调了也没有意义。

那么协程要怎样取消呢？不管是`GlobalScope.launch`函数还是`launch`函数，它们都会返回一个**Job**对象，只需要调用**Job**对象的`cancel()`方法就可以取消协程了，如下所示：

```kotlin
val job = GlobalScope.launch {
    // TODO
}
job.cancel()
```

但是如果我们每次创建的都是顶层协程，那么当**Activity**关闭时，就需要逐个调用所有已创建协程的`cancel()`方法，试想一下，这样的代码是不是根本无法维护？

因此，`GlobalScope.launch`这种协程作用域构建器，在实际项目中也是不太常用的。下面演示一下实际项目中比较常用的写法：

```kotlin
val job = Job()
val scope = CoroutineScope(job)
scope.launch {
    // TODO
}
job.cancel()
```

可以看到，我们先创建了一个**Job**对象，然后把它传入`CoroutineScope()`函数当中，注意这里的`CoroutineScope()`是个函数，虽然它的命名更像是一个类。`CoroutineScope()`函数会返回一个**CoroutineScope**对象，这种语法结构的设计更像是我们创建了一个**CoroutineScope**的实例，可能也是**Kotlin**有意为之的。有了**CoroutineScope**对象之后，就可以随时调用它的**launch**函数来创建一个协程了。

现在所有调用**CoroutineScope**的**launch**函数所创建的协程，都会被关联在**Job**对象的作用域下面。这样只需要调用一次`cancel()`方法，就可以将同一作用域内的所有协程全部取消，从而大大降低了协程管理的成本。

**不过相比之下，`CoroutineScope()`函数更适合用于实际项目当中，如果只是在`main()`函数中编写一些学习测试用的代码，还是使用runBlocking函数最为方便。**

我们已经知道了调用**launch**函数可以创建一个新的协程，但是**launch**函数只能用于执行一段逻辑，却不能获取执行的结果，因为它的返回值永远是一个**Job**对象。**那么有没有什么办法能够创建一个协程并获取它的执行结果呢？当然有，使用async函数就可以实现。**

**async**函数必须在协程作用域当中才能调用，它会创建一个新的子协程并返回一个**Deferred**对象，如果我们想要获取**async**函数代码块的执行结果，只需要调用**Deferred**对象的`await()`方法即可，代码如下所示：

```kotlin
fun main() {
	runBlocking {
		val result = async {
			5 + 5
		}.await()
		println(result)
	}
}
```

这里我们在**async**函数的代码块中进行了一个简单的数学运算，然后调用`await()`方法获取运算结果，最终将结果打印出来。重新运行一下代码，发现控制台会打印一个10。

不过**async**函数的奥秘还不止于此。事实上，在调用了**async**函数之后，代码块中的代码就会立刻开始执行。**当调用`await()`方法时，如果代码块中的代码还没执行完，那么`await()`方法会将当前协程阻塞住，直到可以获得async函数的执行结果。**

做个小实验：

```kotlin
fun main() {
    runBlocking {
        val start = System.currentTimeMillis()
        val result1 = async {
            delay(1000)
            5+5
        }.await()
        val result2 = async {
            delay(1000)
            4+6
        }.await()
        println("result is ${result1 + result2}.")
        val end = System.currentTimeMillis()
        println("cost ${end - start}ms.")
    }
}
```

这里连续使用了两个**async**函数来执行任务，并在代码块中调用`delay()`方法进行1秒的延迟。按照刚才的理论，`await()`方法在**async**函数代码块中的代码执行完之前会一直将当前协程阻塞住，那么为了便于验证，我们记录了代码的运行耗时。现在重新运行程序，结果如下：

![1709106260693.png](https://pic.ziyuan.wang/user/xiheya/2024/02/1709106260693_4f1cc4e6044bf.png)

可以看到，整段代码的运行耗时是2033毫秒，说明这里的两个async函数确实是一种串行的关系，前一个执行完了后一个才能执行。

但是这种写法明显是非常低效的，因为两个**async**函数完全可以同时执行从而提高运行效率。现在对上述代码使用如下的写法进行修改：

```kotlin
fun main() {
    runBlocking {
        val start = System.currentTimeMillis()
        val result1 = async {
            delay(1000)
            5+5
        }
        val result2 = async {
            delay(1000)
            4+6
        }
        println("result is ${result1.await() + result2.await()}.")
        val end = System.currentTimeMillis()
        println("cost ${end - start}ms.")
    }
}
```

现在我们不在每次调用**async**函数之后就立刻使用`await()`方法获取结果了，而是仅在需要用到**async**函数的执行结果时才调用`await()`方法进行获取，这样两个**async**函数就变成一种并行关系了。重新运行程序，结果如下所示。

![1709106396465.png](https://pic.ziyuan.wang/user/xiheya/2024/02/1709106396465_060e075f6b533.png)

可以看到，现在整段代码的运行耗时变成了1013毫秒，运行效率的提升显而易见。

最后，再来学习一个比较特殊的作用域构建器：`withContext()`函数。`withContext()`函数是一个挂起函数，大体可以将它理解成**async**函数的一种简化版写法，示例写法如下：

```kotlin
fun main() {
    runBlocking { 
        val result = withContext(Dispatchers.Default) {
            5+5
        }
        println(result)
    }
}
```

调用`withContext()`函数之后，会立即执行代码块中的代码，同时将外部协程挂起。当代码块中的代码全部执行完之后，会将最后一行的执行结果作为`withContext()`函数的返回值返回，因此基本上相当于`val result = async{ 5 + 5}.await()`的写法。唯一不同的是，`withContext()`函数强制要求我们指定一个线程参数。

协程是一种轻量级的线程的概念，因此很多传统编程情况下需要开启多线程执行的并发任务，现在只需要在一个线程下开启多个协程来执行就可以了。但是这并不意味着我们就永远不需要开启线程了，**比如说Android中要求网络请求必须在子线程中进行，即使你开启了协程去执行网络请求，假如它是主线程当中的协程，那么程序仍然会出错。这个时候我们就应该通过线程参数给协程指定一个具体的运行线程。**

线程参数主要有以下3种值可选：`Dispatchers.Default`、`Dispatchers.IO`和`Dispatchers.Main`。`Dispatchers.Default`表示会使用一种**默认低并发的线程策略**，当你要执行的代码属于**计算密集型任务**时，开启过高的并发反而可能会影响任务的运行效率，此时就可以使用`Dispatchers.Default`。`Dispatchers.IO`表示会使用一种**较高并发的线程策略**，当你要执行的代码**大多数时间是在阻塞和等待**中，比如说**执行网络请求**时，为了**能够支持更高的并发数量**，此时就可以使用`Dispatchers.IO`。**`Dispatchers.Main`则表示不会开启子线程**，而是**在Android主线程中执行代码**，但是**这个值只能在Android项目中使用，纯Kotlin程序使用这种类型的线程参数会出现错误。**

事实上，在我们刚才所学的协程作用域构建器中，**除了`coroutineScope`函数之外，其他所有的函数都是可以指定这样一个线程参数的，只不过`withContext()`函数是强制要求指定的，而其他函数则是可选的**。

### 9.3 使用协程简化回调写法

之前学习了编程语言的回调机制，并使用这个机制实现了获取异步网络请求数据响应的功能。回调机制基本上是依靠匿名类来实现的，但是匿名类的写法通常比较烦琐，比如如下代码：

```kotlin
HttpUtil.sendHttpRequest(address, object : HttpCallbackListener {
    override fun onFinish(response: String) {
        // TODO
    }
    
    override fun onError(e: Exception) {
        // TODO
    }
})
```

在多少个地方发起网络请求，就需要编写多少次这样的匿名类实现。还有没有更加简单一点的写法呢？

在过去，可能确实没有什么更加简单的写法了。不过现在，Kotlin的协程使我们的这种设想成为了可能，只需要借助`suspendCoroutine`函数就能将传统回调机制的写法大幅简化，下面我们就来具体学习一下。

`suspendCoroutine`函数必须在协程作用域或挂起函数中才能调用，它接收一个**Lambda**表达式参数，主要作用是将当前协程立即挂起，然后在一个普通的线程中执行**Lambda**表达式中的代码。**Lambda**表达式的参数列表上会传入一个**Continuation**参数，调用它的`resume()`方法或`resumeWithException()`可以让协程恢复执行。

了解了`suspendCoroutine`函数的作用之后，接下来我们就可以借助这个函数来对传统的回调写法进行优化。首先定义一个`request()`函数，代码如下所示：

```kotlin
suspend fun request(address: String): String {
    return suspendCoroutine { continuation ->
        HttpUtil.sendHttpRequest(address, object : HttpCallbackListener {
    		override fun onFinish(response: String) {
        		// TODO
	    	}
    
    		override fun onError(e: Exception) {
        		// TODO
    		}
		})
    }
}
```

可以看到，`request()`函数是一个挂起函数，并且接收一个**address**参数。在`request()`函数的内部，我们调用了刚刚介绍的`suspendCoroutine`函数，这样当前协程就会被立刻挂起，而**Lambda**表达式中的代码则会在普通线程中执行。接着我们在**Lambda**表达式中调用`HttpUtil.sendHttpRequest()`方法发起网络请求，并通过传统回调的方式监听请求结果。如果请求成功就调用`Continuation`的`resume()`方法恢复被挂起的协程，并传入服务器响应的数据，该值会成为`suspendCoroutine`函数的返回值。如果请求失败，就调用`Continuation`的`resumeWithException()`恢复被挂起的协程，并传入具体的异常原因。

可是这里不是仍然使用了传统回调的写法吗？代码怎么就变得更加简化了？这是因为，不管之后我们要发起多少次网络请求，都不需要再重复进行回调实现了。比如说获取百度首页的响应数据，就可以这样写：

```kotlin
suspend fun getBaiduResponse() {
    try {
        val response = request("https://www.baidu.com/")
    } catch (e: Exception) {
        //TODO
    }
}
```

由于 `getBaiduResponse()`是一个挂起函数，因此当它调用了`request()`函数时，当前的协程就会被立刻挂起，然后一直等待网络请求成功或失败后，当前协程才能恢复运行。这样即使不使用回调的写法，我们也能够获得异步网络请求的响应数据，而如果请求失败，则会直接进入**catch**语句当中。

可是`getBaiduResponse()`函数被声明成了挂起函数，这样它也只能在协程作用域或其他挂起函数中调用了，使用起来是不是非常有局限性？确实如此，因为`suspendCoroutine`函数本身就是要结合协程一起使用的。不过通过合理的项目架构设计，我们可以轻松地将各种协程的代码应用到一个普通的项目当中。

事实上，`suspendCoroutine`函数几乎可以用于简化任何回调的写法，比如使用`Retrofit`来发起网络请求需要这样写：

```kotlin
val appService = ServiceCreator.create<AppService>()
appService.getAppData().enqueue(object : Callback<List<App>> {
    override fun onResponse(call: Call<List<App>>, response: Response<List<App>>) {
        //TODO
    }
    
    override fun onFailure(call: Call<List<App>>, t: Throwable) {
        //TODO
    }
})
```

使用`suspendCoroutine`函数，我们马上就能对上述写法进行大幅度的简化。由于不同的`Service`接口返回的数据类型也不同，所以这次我们不能像刚才那样针对具体的类型进行编程了，而是要使用泛型的方式。定义一个`await()`函数，代码如下所示：

```kotlin
suspend fun <T> Call<T>.await(): T {
    return suspendCoroutine {
        enqueue(object : Callback<T> { continuation ->
            override fun onResponse(call: Call<T>, response: Response<T>) {
                val body = response.body()
                if (body != null) continuation.resume(body)
                else continuation.resumeWithException(RuntimeException("reponse body is null"))
            }
            override fun onFailure(call: Call<T>, t: Throwable) {
                continuation.resumeWithException(t)
            }
        })
    }
}
```

这段代码相比于刚才的`request()`函数又复杂了一点。首先`await()`函数仍然是一个挂起函数，然后我们给它声明了一个泛型T，并将`await()`函数定义成了`Call<T>`的扩展函数，这样所有返回值是**Call**类型的`Retrofit`网络请求接口就都可以直接调用`await()`函数了。

接着，`await()`函数中使用了`suspendCoroutine`函数来挂起当前协程，并且由于扩展函数的原因，我们现在拥有了**Call**对象的上下文，那么这里就可以直接调用`enqueue()`方法让`Retrofit`发起网络请求。接下来，使用同样的方式对`Retrofit`响应的数据或者网络请求失败的情况进行处理就可以了。另外还有一点需要注意，在`onResponse()`回调当中，我们调用`body()`方法解析出来的对象是可能为空的。如果为空的话，这里的做法是手动抛出一个异常，你也可以根据自己的逻辑进行更加合适的处理。

有了`await()`函数之后，我们调用所有`Retrofit`的**Service**接口都会变得极其简单，比如刚才同样的功能就可以使用如下写法进行实现：

```kotlin
suspend fun getAppData() {
    try {
        val appList = ServiceCreator.create<Appservice>().getAppdata().await()
        // 对服务器响应的数据进行处理
    } catch (e: Exception) {
        // 对异常情况进行处理
    }
}
```

没有了冗长的匿名类实现，只需要简单调用一下await()函数就可以让Retrofit发起网络请求，并直接获得服务器响应的数据，有没有觉得代码变得极其简单？当然你可能会觉得，每次发起网络请求都要进行一次try catch处理也比较麻烦，其实这里我们也可以选择不处理。在不处理的情况下，如果发生了异常就会一层层向上抛出，一直到被某一层的函数处理了为止。因此，我们也可以在某个统一的入口函数中只进行一次try catch，从而让代码变得更加精简。

---

## 十、编写好用的工具方法

到目前为止，我们已经将**Kotlin**大部分系统性的知识点学习完了。掌握了如此多的**Kotlin**特性，还需要知道该如何对它们进行灵活运用。

事实上，**Kotlin**提供的丰富语法特性给我们提供了无限扩展的可能，各种复杂的**API**经过特殊的封装处理之后都能变得简单易用。但是最重要的还是我们要能养成对**Kotlin**的各种特性进行灵活运用的意识。

### 10.1 求N个数的最大最小值

两个数比大小这个功能，相信每一位开发者都遇到过。如果我想要获取两个数中较大的那个数，除了使用最基本的if语句之外，还可以借助**Kotlin**内置的`max()`函数，如下所示：

```kotlin
val a = 10
val b = 15
val larger = max(a,b)
```

这种代码看上去简单直观，也很容易理解，因此好像并没有什么优化的必要。

可是现在如果我们想要在3个数中获取最大的那个数，应该怎么写呢？由于max()函数只能接收两个参数，因此需要先比较前两个数的大小，然后再拿较大的那个数和剩余的数进行比较，写法如下：

```kotlin
val a = 10
val b = 15
val c = 5
val larger = max(max(a,b),c)
```

有没有觉得代码开始变得复杂了呢？3个数中获取最大值就需要使用这种嵌套max()函数的写法
了，那如果是4个数、5个数呢？没错，这个时候你就应该意识到，我们是可以对max()函数的
用法进行简化的。

回顾一下，我们之前学过`vararg`关键字，它允许方法接收任意多个同等类型的参数，正好满足我们这里的需求。那么我们就可以新建一个`Max.kt`文件，并在其中自定义一个`max()`函数，如下所示：

```kotlin
fun max(vararg nums: Int): Int {
    var maxNum = Int.MIN_VALUE
    for(num in nums) {
        maxNum = kotlin.math.max(maxNum, num)
    }
    return maxNum
}
```

可以看到，这里`max()`函数的参数声明中使用了`vararg`关键字，也就是说现在它可以接收任意多个整型参数。接着我们使用了一个`maxNum`变量来记录所有数的最大值，并在一开始将它赋值成了整型范围的最小值。然后使用`for-in`循环遍历**nums**参数列表，如果发现当前遍历的数字比`maxNum`更大，就将`maxNum`的值更新成这个数，最终将`maxNum`返回即可。

仅仅经过这样的一层封装之后，我们在使用`max()`函数时就会有翻天覆地的变化，比如刚才同样的功能，现在就可以使用如下的写法来实现：

```kotlin
val a = 10
val b = 15
val c = 5
val largest = max(a, b, c)
```

这样我们就彻底摆脱了嵌套函数调用的写法，现在不管是求3个数的最大值还是求N个数的最大值，只需要不断地给`max()`函数传入参数就可以了。

不过，目前我们自定义的`max()`函数还有一个缺点，就是它只能求N个整型数字的最大值，如果我还想求N个浮点型或长整型数字的最大值，该怎么办呢？当然你可以定义很多个`max()`函数的重载，来接收不同类型的参数，因为**Kotlin**中内置的`max()`函数也是这么做的。但是这种方案实现起来过于烦琐，而且还会产生大量的重复代码，因此这里我准备使用一种更加巧妙的做法。

Java中规定，所有类型的数字都是可比较的，因此必须实现**Comparable**接口，这个规则在**Kotlin**中也同样成立。那么我们就可以借助泛型，将`max()`函数修改成接收任意多个实现**Comparable**接口的参数，代码如下所示：

```kotlin
fun <T: Comparable<T>> max(vararg nums: T): T {
    if (nums.isEmpty() throw RuntimeException("Params can not be empty"))
    var maxNum = nums[0]
    for (num in nums) {
        if (num > maxNum) {
            maxNum = num
        }
    }
    return maxNum
}
```

在这个函数中，`<T: Comparable<T>>` 是一个泛型约束。这表示类型 `T` 必须实现 `Comparable` 接口，这样我们才能在函数体内部使用 `>` 操作符来比较 `T` 类型的对象。

在 Kotlin 中，所有的比较操作符（如 `>`, `<`, `>=`, `<=`）都定义在 `Comparable` 接口中。因此，如果我们想在一个泛型函数中使用这些操作符，我们就需要确保泛型类型 `T` 实现了 `Comparable` 接口。这就是为什么我们需要写 `<T: Comparable<T>>` 的原因。这样，无论 `T` 是什么类型，只要它实现了 `Comparable` 接口，我们就可以在函数中使用比较操作符。这使得我们的函数更加灵活和通用。例如，我们可以使用这个函数来找出一组整数、浮点数或者字符串中的最大值，因为这些类型都实现了 `Comparable` 接口。

这里将泛型`T`的上界指定成了`Comparable<T>`，**那么参数`T`就必然是`Comparable<T>`的子类型了**。接下来，我们判断nums参数列表是否为空，如果为空的话就主动抛出一个异常，提醒调用者`max()`函数必须传入参数。紧接着将`maxNum`的值赋值成nums参数列表中第一个参数的值，然后同样是遍历参数列表，如果发现了更大的值就对`maxNum`进行更新。

经过这样的修改之后，我们就可以更加灵活地使用`max()`函数了，比如说求3个浮点型数字的最大值，同样也变得轻而易举：

```kotlin
val a = 3.5
val b = 3.8
val c = 4.1
val largest = max(a, b, c)
```

而且现在不管是双精度浮点型、单精度浮点型，还是短整型、整型、长整型，只要是实现Comparable接口的子类型，max()函数全部支持获取它们的最大值，是一种一劳永逸的做法。

而如果你想获取N个数的最小值，实现的方式也是类似的，只需要定义一个min()函数就可以了:

```kotlin
fun <T: Comparable<T>> min(vararg nums: T): T {
    if (nums.isEmpty() throw RuntimeException("Params can not be empty"))
    var minNum = nums[0]
    for (num in nums) {
        if (num < minNum) {
            minNum = num
        }
    }
    return minNum
}
```

### 10.2 简化Toast的用法

首先回顾一下Toast的标准用法吧，如果想要在界面上弹出一段文字提示需要这样写：

```kotlin
Toast.makeText(context, "this is Toast", Toast.LENGTH_SHORT).show()
```

是不是很长的一段代码？而且曾经不知道有多少人因为忘记调用最后的`show()`方法，导致Toast无法弹出，从而产生一些千奇百怪的bug。

由于Toast是非常常用的功能，每次都需要编写这么长的一段代码确实让人很头疼，这个时候你就应该考虑对Toast的用法进行简化了。

我们来分析一下，Toast的`makeText()`方法接收3个参数：第一个参数是Toast显示的上下文环境，必不可少；第二个参数是Toast显示的内容，需要由调用方进行指定，可以传入字符串和字符串资源id两种类型；第三个参数是Toast显示的时长，只支持`Toast.LENGTH_SHORT`和`Toast.LENGTH_LONG`这两种值，相对来说变化不大。

那么我们就可以给String类和Int类各添加一个扩展函数，并在里面封装弹出Toast的具体逻辑。这样以后每次想要弹出Toast提示时，只需要调用它们的扩展函数就可以了。

新建一个Toast.kt文件，并在其中编写如下代码：

```kotlin
fun String.showToast(context: Context) {
    Toast.makeText(context, "this is Toast", Toast.LENGTH_SHORT).show()
}

fun Int.showToast(context: Context) {
    Toast.makeText(context, "this is Toast", Toast.LENGTH_SHORT).show()
}
```

这里分别给String类和Int类新增了一个`showToast()`函数，并让它们都接收一个Context参数。然后在函数的内部，我们仍然使用了Toast原生API用法，只是将弹出的内容改成了this，另外将Toast的显示时长固定设置成`Toast.LENGTH_SHORT`。

以后如果要弹出一段文字提醒，可以这样写：

```kotlin
"This is Toast".showToast(context)
```

另外，这只是直接弹出一段字符串文本的写法，如果你想弹出一个定义在`strings.xml`中的字符串资源，也非常简单，写法如下：

```kotlin
R.string.app_name.showToast(context)
```

这两种写法分别调用的就是我们刚才在String类和Int类中添加的`showToast()`扩展函数。当然，这种写法其实还存在一个问题，就是Toast的显示时长被固定了，如果我现在想要使用`Toast.LENGTH_LONG`类型的显示时长该怎么办呢？要解决这个问题，其实最简单的做法就是在`showToast()`函数中再声明一个显示时长参数，但是这样每次调用`showToast()`函数时都要额外多传入一个参数，无疑增加了使用复杂度。使用给函数设定参数默认值的功能，我们就可以在不增加`showToast()`函数使用复杂度的情况下，又让它可以支持动态指定显示时长了。修改`Toast.kt`中的代码，如下所示：

```kotlin
fun String.showToast(context: Context, duration: Int = Toast.LENGTH_SHORT) {
	Toast.makeText(context, this, duration).show()
}
fun Int.showToast(context: Context, duration: Int = Toast.LENGTH_SHORT) {
	Toast.makeText(context, this, duration).show()
}
```

可以看到，我们给`showToast()`函数增加了一个显示时长参数，但同时也给它指定了一个参数默认值。这样我们之前所使用的`showToast()`函数的写法将完全不受影响，默认会使用`Toast.LENGTH_SHORT`类型的显示时长。而如果你想要使用`Toast.LENGTH_LONG`的显示时长，只需要这样写就可以了：

```kotlin
"This is Toast".showToast(context, Toast.LENGTH_LONG)
```

### 10.3 简化Snackbar的用法

先来回顾一下Snackbar的常规用法吧，如下所示：

```kotlin
Snackbar.make(view, "This is Snackbar", Snackbar.LENGTH_SHORT)
		.setAction("Action") {
            // TODO
        }
		.show()
```

可以看到，**Snackbar**中`make()`方法的第一个参数变成了**View**，而**Toast**中`makeText()`方法的第一个参数是**Context**，另外**Snackbar还可以调用`setAction()`方法来设置一个额外的点击事件**。除了这些区别之外，**Snackbar**和**Toast**的其他用法都是相似的。

那么对于这种结构的API，我们该如何进行简化呢？其实简化的方式并不固定。

由于`make()`方法接收一个View参数，**Snackbar**会使用这个**View**自动查找最外层的布局，用于展示**Snackbar**。因此，我们就可以给**View**类添加一个扩展函数，并在里面封装显示**Snackbar**的具体逻辑。新建一个`Snackbar.kt`文件，并编写如下代码：

```kotlin
fun View.showSnackbar(text: String, duration: Int = Snackbar.LENGTH_SHORT) {
    Snackbar.make(this, text, duration).show()
}

fun View.showSnackbar(resId: Int, duration: Int = Snackbar.LENGTH_SHORT) {
    Snackbar.make(this, resId, duration).show()
}
```

这段代码应该还是很好理解的，和刚才的showToast()函数比较相似。只是我们将扩展函数添加到了View类当中，并在参数列表上声明了Snackbar要显示的内容以及显示的时长。另外，Snackbar和Toast类似，显示的内容也是支持传入字符串和字符串资源id两种类型的，因此这里我们给showSnackbar()函数进行了两种参数类型的函数重载。

现在想要使用Snackbar显示一段文本提示，只需要这样写就可以了：

```kotlin
view.showSnackbar("This is Snackbar")
```

假如**Snackbar**没有`setAction()`方法，那么我们的简化工作到这里就可以结束了。但是`setAction()`方法作为**Snackbar**最大的特色之一，如果不能支持的话，我们编写的`showSnackbar()`函数也就变得毫无意义了。

这个时候，神通广大的高阶函数又能派上用场了，我们可以让`showSnackbar()`函数再额外接收一个函数类型参数，以此来实现**Snackbar**的完整功能支持。修改`Snackbar.kt`中的代码，如下所示：

```kotlin
fun View.showSnackbar(
    text: String, actionText: String? = null,
    duration: Int = Snackbar.LENGTH_SHORT, block: (() -> Unit)? = null
) {
    val snackbar = Snackbar.make(this, text, duration)

    if (actionText != null && block != null) {
        snackbar.setAction(actionText) {
            block()
        }
    }
    snackbar.show()
}

fun View.showSnackbar(
    resId: Int, actionText: String? = null,
    duration: Int = Snackbar.LENGTH_SHORT, block: (() -> Unit)? = null
) {
    val snackbar = Snackbar.make(this, resId, duration)

    if (actionText != null && block != null) {
        snackbar.setAction(actionText) {
            block()
        }
    }
    snackbar.show()
}
```

在这个函数中，`block()` 是一个可调用的 lambda 函数。当你调用 `showSnackbar` 方法并传入一个 lambda 函数作为 `block` 参数时，这个 lambda 函数就会替换 `block()`。

例如，如果你这样调用 `showSnackbar` 方法:

```kotlin
view.showSnackbar("Hello, World!", "Action") {
    println("Action clicked!")
}
```

那么在 `snackbar.setAction(actionText) { block() }` 这行代码中，`block()` 就会被替换为 `println("Action clicked!")`。所以，当用户点击 Snackbar 的操作文本时，控制台就会打印出 “Action clicked!”。



可以看到，这里我们给两个`showSnackbar()`函数都增加了一个函数类型参数，并且还增加了一个用于传递给`setAction()`方法的字符串或字符串资源id。这里我们需要将新增的两个参数都设置成可为空的类型，并将默认值都设置成空，然后只有当两个参数都不为空的时候，我们才去调用**Snackbar**的`setAction()`方法来设置额外的点击事件。如果触发了点击事件，只需要调用函数类型参数将事件传递给外部的Lambda表达式即可。

这样`showSnackbar()`函数就拥有比较完整的**Snackbar**功能了，比如本小节最开始的那段示例代码，现在就可以使用如下写法进行实现：

```kotlin
view.showSnackbar("This is Snackbar", "Action") {
    // TODO
}
```

---

## 十一、使用DSL构建专有的语法结构

**DSL**的全称是领域特定语言（**Domain Specific Language**），它是编程语言赋予开发者的一种特殊能力，通过它我们可以编写出一些看似脱离其原始语法结构的代码，从而构建出一种专有的语法结构。

毫无疑问，Kotlin也是支持DSL的，并且在Kotlin中实现DSL的实现方式并不固定，比如我们之前使用infix函数构建出的特有语法结构就属于DSL。不过本节课我们的主要学习目标是通过高阶函数的方式来实现DSL，这也是Kotlin中实现DSL最常见的方式。

当我们想要在项目中添加一些依赖库，需要在build.gradle文件中添加依赖时其实就使用了DSL，例如：

```tex
dependencies {
	implementation 'com.squareup.retrofit2:retrofit:2.6.1'
	implementation 'com.squareup.retrofit2:converter-gson:2.6.1'
}
```

Gradle是一种基于Groovy语言的构建工具，因此上述的语法结构其实就是Groovy提供的DSL功能。有没有觉得很神奇？不用吃惊，借助Kotlin的DSL，我们也可以实现类似的语法结构，下面就来具体看一下吧。

首先新建一个DSL.kt文件，然后在里面定义一个Dependency类，代码如下所示：

```kotlin
class Dependency {
    val libraries = ArrayList<String>()

    fun implementation(lib: String) {
        libraries.add(lib)
    }
}
```

这里我们使用了一个List集合来保存所有的依赖库，然后又提供了一个`implementation()`方法，用于向List集合中添加依赖库，代码非常简单。

接下来再定义一个`dependencies`高阶函数，代码如下所示：

```kotlin
fun dependencies(block: Dependency.() -> Unit): List<String> {
    val dependency = Dependency()
    dependency.block()
    return dependency.libraries
}
```

可以看到，`dependencies`函数接收一个函数类型参数，并且该参数是定义到Dependency类中的，因此调用它的时候需要先创建一个Dependency的实例，然后再通过该实例调用函数类型参数，这样传入的Lambda表达式就能得到执行了。最后，我们将Dependency类中保存的依赖库集合返回。

没错，经过这样的DSL设计之后，我们就可以在项目中使用如下的语法结构了：

```kotlin
dependencies {
	implementation 'com.squareup.retrofit2:retrofit:2.6.1'
	implementation 'com.squareup.retrofit2:converter-gson:2.6.1'
}
```

由于`dependencies`函数接收一个函数类型参数，因此这里我们可以传入一个Lambda表达式。而此时的Lambda表达式中拥有Dependency类的上下文，因此当然就可以直接调用Dependency类中的`implementation()`方法来添加依赖库了。

当然，这种语法结构和我们在`build.gradle`文件中使用的语法结构并不完全相同，这主要是因为Kotlin和Groovy在语法层面还是有一定差别的。

另外，我们也可以通过`dependencies`函数的返回值来获取所有添加的依赖库，代码如下所示：

```kotlin
fun main() {
    val libraries = dependencies {
        implementation("com.squareup.retrofit2:retrofit:2.6.1")
        implementation("com.squareup.retrofit2:converter-gson:2.6.1")
    }

    for (lib in libraries) {
        println(lib)
    }
}
```

这里用一个libraries变量接收`dependencies`函数的返回值，然后使用for-in循环将集合中的依赖库全部打印出来。

这种语法结构比起直接调用Dependency对象的`implementation()`方法要更直观一些，而且你会发现，需要添加的依赖库越多，使用DSL写法的优势就会越明显。在实现了一个较为简单的DSL之后，接下来我们再尝试编写一个复杂一点的DSL。

网页的展示都是由浏览器解析HTML代码来实现的。HTML中定义了很多标签，其中<table>标签用于创建一个表格，<tr>标签用于创建表格的行，<td>标签用于创建单元格。将这3种标签嵌套使用，就可以定制出包含任意行列的表格了。

那么如果现在有一个需求，要求我们在Kotlin中动态生成表格所对应的HTML代码，你会怎么做呢？最简单直接的方式就是字符串拼接了，但是这种做法显然十分烦琐，而且字符串拼接的代码也难以阅读。

这个时候DSL又可以大显身手了，借助DSL，我们可以以一种不可思议的语法结构来动态生成表格所对应的HTML代码，下面就来看一下具体应该如何实现吧。

仍然是在DSL.kt文件中进行编写，首先定义一个Td类，代码如下所示：

```kotlin
class Td {
    var content = ""
    fun html() = "\n\t\t<td>$content</td>"
}
```

由于<td>标签表示一个单元格，其中必然是要包含内容的，因此这里我们使用了一个content字段来存储单元格中显示的内容。另外，还提供了一个html()方法，当调用这个方法时就返回一段<td>标签的HTML代码，并将content中存储的内容拼接进去。注意，为了让最终输出的结果更加直观，我使用了\n和\t转义符来进行换行和缩进，当然可以不加这些转义符，因为浏览器在解析HTML代码时是忽略换行和缩进的。

完成了Td类，接下来我们再定义一个Tr类，代码如下所示：

```kotlin
class Tr {
    private val children = ArrayList<Td>()

    fun td(block: Td.() -> String) {
        val td = Td()
        td.content = td.block()
        children.add(td)
    }

    fun html(): String {
        val builder = StringBuilder()
        builder.append("\n\t<tr>")
        for (childTag in children) {
            builder.append(childTag.html())
        }
        builder.append("\n\t</tr>")
        return builder.toString()
    }
}
```

Tr类相比于Td类就要复杂一些了。由于<tr>标签表示表格的行，它是可以包含多个<td>标签的，因此我们首先创建了一个children集合，用于存储当前Tr所包含的Td对象。接下来提供了一个td()函数，它接收一个定义到Td类中并且返回值是String的函数类型参数。当调用td()函数时，会先创建一个Td对象，接着调用函数类型参数并获取它的返回值，然后赋值到Td类的content字段当中，这样就可以将调用td()函数时传入的Lambda表达式的返回值赋值给content字段了。当然，这里既然创建了一个Td对象，就一定要记得将它添加到children集合当中。

另外，Tr类中也定义了一个html()方法，它的作用和刚才Td类中的html()方法一致。只是由于每个Tr都可能会包含很多个Td，因此我们需要使用循环来遍历children集合，将所有的子Td都拼接到<tr>标签当中，从而返回一段嵌套的HTML代码。

定义好了Tr类之后，我们现在就可以使用如下的语法格式来构建表格中的一行数据：

```kotlin
val tr = Tr()
tr.td { "Apple" }
tr.td { "Grape" }
tr.td { "Orange" }
```

那么接下来继续对DSL进行完善，再定义一个Table类，代码如下所示：

```kotlin
class Table {
    private val children = ArrayList<Tr>()
    
    fun tr(block: Tr.() -> Unit) {
        val tr = Tr()
        tr.block()
        children.add(tr)
    }
    
    fun html(): String {
        val builder = StringBuilder()
        builder.append("<table>")
        for (childTag in children) {
            builder.append(childTag)
        }
        builder.append("</table>")
        return builder.toString()
    }
}
```

这段代码相对就好理解多了，因为和刚才Tr类中的代码是比较相似的。Table类中同样创建了一个children集合，用于存储当前Table所包含的Tr对象。然后定义了一个tr()函数，它接收一个定义到Tr类中的函数类型参数。当调用tr()函数时，会先创建一个Tr对象，接着调用函数类型参数，这样Lambda表达式中的代码就能得到执行。最后，仍然要记得将创建的Tr对象添加到children集合当中。

除此之外，html()方法中的代码也都是类似的，这里遍历了children集合，将所有的子Tr对象都拼接到了<table>标签当中。

那么现在，我们就可以使用如下的语法结构来构建一个表格了：

```kotlin
val table = Table()
table.tr {
    td { "Apple" }
    td { "Grape" }
    td { "Orange" }
}
table.tr {
    td { "Pear" }
    td { "Banana" }
    td { "Watermelon" }
}
```

这段代码看上去已经相当不错了，不过这仍然不是最终版本，我们还可以再进一步对语法结构进行精简。定义一个table()函数，代码如下所示：

```kotlin
fun table(block: Table.() -> Unit): String {
    val table = Table()
    table.block()
    return table.html()
}
```

这里的`table()`函数接收一个定义到Table类中的函数类型参数，当调用`table()`函数时，会先创建一个Table对象，接着调用函数类型参数，这样Lambda表达式中的代码就能得到执行。最后调用Table的`html()`方法获取生成的HTML代码，并作为最终的返回值返回。

编写了这么多代码之后，我们就可以使用如下神奇的语法结构来动态生成一个表格所对应的HTML代码了：

```kotlin
package work.icu007.jetpacktest


/*
 * Author: Charlie_Liam
 * Time: 2024/3/7-15:31
 * E-mail: rookie_l@icu007.work
 */

fun main() {
    val html = table {
        tr {
            td { "Apple" }
            td { "Grape" }
            td { "Orange" }
        }
        tr {
            td { "Pear" }
            td { "Banana" }
            td { "Watermelon" }
        }
    }
    println(html)
    println(html.toString())
    }
}
```

另外，在DSL中也可以使用Kotlin的其他语法特性，比如通过循环来批量生成<tr>和<td>标签：

```kotlin
fun main() {
    val html1 = table {
        repeat(2) {
            tr {
                val fruits = listOf("Apple", "Grape", "Orange")
                for (fruit in fruits) {
                    td { fruit }
                }
            }
        }
    }
    println(html1.toString())
}
```

这里使用了`repeat()`函数来为表格生成两行数据，每行数据中又使用了`for-in`循环来遍历List集合，为表格填充具体的单元格数据。