---
title: Kotlin安卓开发-高级技巧
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
swiper_index: 1
top_group_index: 1
background: '#fff'
cover: 'https://pic2.ziyuan.wang/user/xiheya/2024/05/Advance_7270c77bc7e69.jpg'
ai:
  - >-
    这篇文章主要介绍了在Android开发中如何全局获取Context的技巧。在Android开发过程中，许多操作都需要使用到Context，例如弹出Toast、启动Activity、发送广播、操作数据库、使用通知等。然而，当应用程序的架构变得复杂，许多逻代码将脱离Activity类，此时我们仍需要使用Context，这可能会带来一些困扰。文章提出了一个解决方案，即定制一个自己的Application类，以便于管理程序内一些全局的状态信息，比如全局Context。Android提供了一个Application类，每当应用程启动的时候，系统就会自动将这个类进行初始化。文章最后提供了一个示例，展示了如何创建一个自定义的Application类，并在其中定义一个全局的Context变量。这样，我们就可以在程序的任何地方方便地获取到Contex了。
abbrlink: 6c099795
date: 2024-02-04 15:31:01
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

## 一、全局获取Context的技巧

在Android开发过程中很多地方都需要用到Context，弹出Toast的时候需要，启动Activity的时候需要，发送广播的时候需要，操作数据库的时候需要，使用通知的时候需要……

当应用程序的架构逐渐开始复杂起来的时候，很多逻辑代码将脱离Activity类，但此时我们又恰恰需要使用Context，这个时候就会感到有些伤脑筋了。

Android提供了一个Application类，每当应用程序启动的时候，系统就会自动将这个类进行初始化。而我们可以定制一个自己的Application类，以便于管理程序内一些全局的状态信息，比如全局Context。

定制一个自己的Application其实并不复杂，首先需要创建一个MyApplication类继承自Application，代码如下所示：

```kotlin
class MyApplication : Application() {
    companion object {
        lateinit var context: Context
    }

    override fun onCreate() {
        super.onCreate()
        context = applicationContext
    }
}
```

可以看到，MyApplication中的代码非常简单。这里我们在companion object中定义了一个context变量，然后重写父类的onCreate()方法，并将调用getApplicationContext()方法得到的返回值赋值给context变量，这样我们就可以以静态变量的形式获取Context对象了。

需要注意的是，将Context设置成静态变量很容易会产生内存泄漏的问题，所以这是一种有风险的做法。

但是由于这里获取的不是Activity或Service中的Context，而是Application中的Context，它全局只会存在一份实例，并且在整个应用程序的生命周期内都不会回收，因此是不存在内存泄漏风险的。那么我们可以使用如下注解，让Android Studio忽略上述警告提示：

```kotlin
class MyApplication : Application() {
    companion object {
        @SuppressLint("StaticFieldLeak")
        lateinit var context: Context
    }
    ...
}
```

接下来我们还需要告知系统，当程序启动的时候应该初始化`MyApplication`类，而不是默认的`Application`类。这一步也很简单，在`AndroidManifest.xml`文件的`<application>`标签下进行指定就可以了，代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:name=".MyApplication"		// 这里指定初始化MyApplication类
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.MaterialTest"
        tools:targetApi="31">
        ...
    </application>

</manifest>
```

这样我们就实现了一种全局获取Context的机制，之后不管你想在项目的任何地方使用Context，只需要调用一下`MyApplication.context`就可以了。那么接下来我们再对`showToast()`方法进行优化，代码如下所示：

```kotlin
package work.icu007.materialtest

import android.widget.Toast


/*
 * Author: Charlie_Liam
 * Time: 2024/3/19-10:48
 * E-mail: rookie_l@icu007.work
 */

fun String.showToast(duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(MyApplication.context, this, duration).show()
}

fun Int.showToast(duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(MyApplication.context, this, duration).show()
}
```

可以看到，`showToast()`方法不需要再通过传递参数的方式得到Context对象，而是调用一下`MyApplication.context`就可以了。这样`showToast()`方法的用法也得到了进一步的精简，现在只需要使用如下写法就能弹出一段文字提示：

```kotlin
"toast test".showToast()
```

## 二、使用Intent传递对象

我们可以借助**Intent**来**启动Activity**、**启动Service**、**发送广播**等。在进行上述操作的时候，我们还可以在**Intent**中添加一些附加数据，以达到传值的效果，比如在`FirstActivity`中添加如下代码：

```kotlin
val intent = Intent(this, SecondActivity::class.java)
intent.putExtra("string_data", "hello")
intent.putExtra("int_data", 100)
startActivity(intent)
```

这里调用了**Intent**的`putExtra()`方法来添加要传递的数据，之后在`SecondActivity`中就可以得到这些值了，代码如下所示：

```kotlin
intent.getStringExtra("string_data")
intent.getIntExtra("int_data", 0)
```

`putExtra()`方法中所支持的数据类型是有限的，虽然常用的一些数据类型是支持的，但是当你想去传递一些自定义对象的时候，就会发现无从下手。

### 2.1 Serializable方式

使用Intent来传递对象通常有两种实现方式：`Serializable`和`Parcelable`。

`Serializable`是序列化的意思，表示将一个对象转换成可存储或可传输的状态。序列化后的对象可以在网络上进行传输，也可以存储到本地。至于序列化的方法非常简单，只需要让一个类去实现`Serializable`这个接口就可以了。

比如说有一个Person类，其中包含了name和age这两个字段，如果想要将他序列化，就可以这样写

```kotlin
class Person : Serializable {
    val name = ""
    val age = 0
}
```

这里我们让Person类实现了Serializable接口，这样所有的Person对象都是可序列化的了。

然后在FirstActivity中只需要这样写：

```kotlin
val person = Person()
person.name = "Tom"
person.age = 20
val intent = Intent(this, SecondActivity::class.java)
intent.putExtra("person_data", person)
startActivity(intent)
```

这里我们创建了一个**Person**的实例，并将它直接传入了**Intent**的`putExtra()`方法中。由于Person类实现了`Serializable`接口，所以才可以这样写。

接下来在`SecondActivity`中获取这个对象也很简单，写法如下：

```kotlin
val person = intent.getSerializableExtra("person_data") as Person
```

这里调用了Intent的getSerializableExtra()方法来获取通过参数传递过来的序列化对象，接着再将它向下转型成Person对象，这样我们就成功实现了使用Intent传递对象的功能。需要注意的是，这种传递对象的工作原理是先将一个对象序列化成可存储或可传输的状态，传递给另外一个Activity后再将其反序列化成一个新的对象。虽然这两个对象中存储的数据完全一致，但是它们实际上是不同的对象.

### 2.2 Parcelable方式

除了`Serializable`之外，使用 `Parcelable` 也可以实现相同的效果，不过不同于将对象进行序列化， `Parcelable`方式的实现原理是将一个完整的对象进行分解，而分解后的每一部分都是Intent所支持的数据类型，这样就能实现传递对象的功能了。

下面来看一下 `Parcelable`的实现方式，修改Person中的代码，如下所示：

```kotlin
class Person : Parcelable {
    var name = ""
    var age = 0
    
    override fun writeToParcel(parcel: Parcel, flags: Int) {
        parcel.writeString(name)	// 写出name
        parcel.writeInt(age)		// 写出age
    }
    
    override fun describeContents(): Int {
        return 0
    }
    
    companion object CREATOR : Parcelable.Creator<Person> {
        override fun createFromParcel(parcel: Parcel): Person {
            val person = Person
            person.name = parcel.readString() ?: ""		// 读取name
            person.age = parcel.readInt()				// 读取age
            return person
        }
        
        override fun newArray(size: Int): Array<Person?> {
            return arrayOfNulls)size
        }
    }
}
```

`Parcelable`的实现方式要稍微复杂一些。可以看到，首先我们让Person类实现了`Parcelable`接口，这样就必须重写`describeContents()`和`writeToParcel()`这两个方法。其中`describeContents()`方法直接返回0就可以了，而在`writeToParcel()`方法中，我们需要调用Parcel的`writeXxx()`方法，将Person类中的字段一一写出。**注意，字符串型数据就调用`writeString()`方法，整型数据就调用`writeInt()`方法，以此类推。**

除此之外，**我们还必须在Person类中提供一个名为CREATOR的匿名类实现。**这里创建了`Parcelable.Creator`接口的一个实现，并将泛型指定为Person。接着需要重写`createFromParcel()`和`newArray()`这两个方法，在`createFromParcel()`方法中，我们要创建一个Person对象进行返回，并读取刚才写出的name和age字段。其中name和age都是调用Parcel的`readXxx()`方法读取到的，注意这里读取的顺序一定要和刚才写出的顺序完全相同。而`newArray()`方法中的实现就简单多了，只需要调用`arrayOfNulls()`方法，并使用参数中传入的size作为数组大小，创建一个空的Person数组即可。

接下来，在`FirstActivity`中我们仍然可以使用相同的代码来传递Person对象，只不过在`SecondActivity`中获取对象的时候需要稍加改动，如下所示：

```kotlin
val person = intent.getParcelableExtra("person_data") as Person
```

这里不再是调用`getSerializableExtra()`方法，而是调用`getParcelableExtra()`方法来获取传递过来的对象，其他的地方完全相同。

不过，这种实现方式写起来确实比较复杂，为此Kotlin给我们提供了另外一种更加简便的用法，但前提是要传递的所有数据都必须封装在对象的主构造函数中才行。

修改Person类中的代码，如下所示：

```kotlin
@Parcelize
class Person(var name: String, var age:Int) : Parcelable
```

没错，就是这么简单。将name和age这两个字段移动到主构造函数中，然后给Person类添加一个@Parcelize注解即可.

对比一下，Serializable的方式较为简单，但由于会把整个对象进行序列化，因此效率会比Parcelable方式低一些，所以在通常情况下，还是更加推荐使用Parcelable的方式来实现Intent传递对象的功能。

## 三、定制自己的日志工具

虽然Android中自带的日志工具功能非常强大，但也不能说完全没有缺点，例如在打印日志的控制方面就做得不够好。

打个比方，你正在编写一个比较庞大的项目，期间为了方便调试，在代码的很多地方打印了大量的日志。最近项目已经基本完成了，但是却有一个非常让人头疼的问题，之前用于调试的那些日志，在项目正式上线之后仍然会照常打印，这样不仅会降低程序的运行效率，还有可能将一些机密性的数据泄露出去。

那该怎么办呢？难道要一行一行地把所有打印日志的代码都删掉吗？显然这不是什么好点子，不仅费时费力，而且以后你继续维护这个项目的时候可能还会需要这些日志。因此，最理想的情况是能够自由地控制日志的打印，当程序处于开发阶段时就让日志打印出来，当程序上线之后就把日志屏蔽掉。

看起来好像是挺高级的一个功能，其实并不复杂，我们只需要定制一个自己的日志工具就可以轻松完成了。新建一个LogUtil单例类，代码如下所示：

```kotlin
package work.icu007.materialtest

import android.util.Log


/* 
 * Author: Charlie_Liam
 * Time: 2024/3/19-11:42
 * E-mail: rookie_l@icu007.work
 */

object LogUtil {
    private const val VERBOSE = 1 
    private const val DEBUG = 2 
    private const val INFO= 3 
    private const val WARN = 4 
    private const val ERROR = 5 
    private var level = VERBOSE
    
    fun v(tag: String, msg: String) {
        if (level <= VERBOSE) {
            Log.v(tag,msg)
        }
    }
    fun d(tag: String, msg: String) {
        if (level <= DEBUG) {
            Log.d(tag,msg)
        }
    }
    fun i(tag: String, msg: String) {
        if (level <= INFO) {
            Log.i(tag,msg)
        }
    }
    fun w(tag: String, msg: String) {
        if (level <= WARN) {
            Log.w(tag,msg)
        }
    }
    fun e(tag: String, msg: String) {
        if (level <= ERROR) {
            Log.e(tag,msg)
        }
    }
}
```

我们在LogUtil中首先定义了VERBOSE、DEBUG、INFO、WARN、ERROR这5个整型常量，并且它们对应的值都是递增的。然后又定义了一个静态变量level，可以将它的值指定为上面5个常量中的任意一个。接下来，我们提供了v()、d()、i()、w()、e()这5个自定义的日志方法，在其内部分别调用了**Log.v()、Log.d()、Log.i()、Log.w()、Log.e()这5个方法来打印日志**，只不过在这些自定义的方法中都加入了一个if判断，只有当level的值小于或等于对应日志级别值的时候，才会将日志打印出来。这样就把一个自定义的日志工具创建好了，之后在项目里，我们可以像使用普通的日志工具一样使用LogUtil。比如打印一行DEBUG级别的日志可以这样写：

```kotlin
LogUtil.d("TAG", "debug log")

// 打印一行WARN级别的日志可以这样写：

LogUtil.w("TAG", "warn log")
```

我们只需要通过修改level变量的值，就可以自由地控制日志的打印行为。比如让level等于VERBOSE就可以把所有的日志都打印出来，让level等于ERROR就可以只打印程序的错误日志。

使用了这种方法之后，刚才所说的那个问题也就不复存在了

## 四、深色主题

当我们打开我们自己编写的应用程序，会发现目前界面的风格还是使用的浅色主题模式，这就和系统的主题风格不同了，说明我们需要对此进行适配。

最简单的一种适配方式就是使用`Force Dark`，它是一种能让应用程序快速适配深色主题，并且几乎不用编写额外代码的方式。`Force Dark`的工作原理是系统会分析浅色主题应用下的每一层View，并且在这些View绘制到屏幕之前，自动将它们的颜色转换成更加适合深色主题的颜色。注意，只有原本使用浅色主题的应用才能使用这种方式，如果你的应用原本使用的就是深色主题，`Force Dark`将不会起作用。

现在Meterial 项目都有自动适配深色主题的功能，在 `values--> themes.xml`文件中修改为：

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Base.Theme.MaterialTest" parent="Theme.Material3.DayNight.NoActionBar">
        <!-- Customize your light theme here. -->
        <!-- <item name="colorPrimary">@color/my_light_primary</item> -->
    </style>

    <style name="Theme.MaterialTest" parent="Base.Theme.MaterialTest" />

    <style name="Theme.FruitActivity" parent="Base.Theme.MaterialTest" >
        <item name="android:statusBarColor">@android:color/transparent</item>
    </style>
</resources>
```

`Theme.Material3.DayNight.NoActionBar`这是一种`DayNight`主题。因此，在普通情况下`MaterialTest`项目仍然会使用浅色主题，和之前并没有什么区别，但是一旦用户在系统设置中开启了深色主题，`MaterialTest`项目就会自动使用相应的深色主题。

我们还需要修改 `values-night`目录下的 `colors.xml`文件，接着在这个文件中指定深色主题下的颜色值：

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="colorPrimary">#303030</color>
    <color name="colorPrimaryDark">#232323</color>
    <color name="colorAccent">#008577</color>
</resources>
```

这样的话，在普通情况下，系统仍然会读取`values/colors.xml`文件中的颜色值，而一旦用户开启了深色主题，系统就会去读取`values-night/colors.xml`文件中的颜色值了。

另外，或许你还会有一些特殊的需求，比如要在浅色主题和深色主题下分别执行不同的代码逻辑。对此Android也是支持的，你可以使用如下代码在任何时候判断当前系统是否是深色主题：

```kotlin
fun isDarkTheme(context: Context): Boolean {
    val flag = context.resources.configuration.uiMode and
    	Configuration.UI_MODE_NIGHT_MASK
    return flag == Configuration_UI_MODE_NIGHT_YES
}
```

调用`isDarkTheme ()`方法，判断当前系统是浅色主题还是深色主题，然后根据返回值执行不同的代码逻辑即可。

另外，**由于Kotlin取消了按位运算符的写法，改成了使用英文关键字，因此上述代码中的and关键字其实就对应了Java中的&运算符，而Kotlin中的or关键字对应了Java中的|运算符，xor关键字对应了Java中的^运算符**，非常好理解。