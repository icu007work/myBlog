---
title: Kotlin安卓开发入门
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
swiper_index: 1
top_group_index: 1
background: '#fff'
cover: 'https://pic2.ziyuan.wang/user/xiheya/2024/05/安卓开发入门_2838f20538180.jpg'
ai:
  - >-
    这篇文章是关于Android开发中ViewBinding的入门教程。ViewBinding是一个功能，其主要目的是避免编写findViewById，使代码更简洁。文章中提到，与其复杂的兄弟DataBinding相比，ViewBinding更为简单。要使用ViewBinding，需要注意两点：首先，确保你的Android
    Studio版本是3.6或更高。其次，需要在项目工程模块的build.gradle中加入特定的配置。请注意，这是对文章部分内容的总结，可能还有其他重要信息未被包含在内。
abbrlink: 3ba91e1b
date: 2023-10-11 11:37:21
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

## 一、ViewBinding

### 1.1 什么是ViewBinding

**ViewBinding**总体来说其实非常简单，它的目的只有一个，就是为了避免编写`findViewById`，这和它另外一个非常复杂的兄弟**DataBinding**相比有明显的区别。

要想使用**ViewBinding**需要注意两件事。第一，确保你的**Android Studio是3.6或更高**的版本。第二，在你项目工程模块的`build.gradle`中加入以下配置：

```tex
android {
    ...
    buildFeatures {
        viewBinding true
    }
}
```

### 1.2 在Activity中使用ViewBinding

一旦启动了**ViewBinding**功能之后，Android Studio会自动为我们所编写的每一个布局文件都生成一个对应的`Binding`类.`Binding`类的命名规则是将布局文件按驼峰方式重命名后，再加上`Binding`作为结尾。比如说，前面我们定义了一个`activity_main.xml`布局，那么与它对应的`Binding`类就是`ActivityMainBinding`。当然，如果有些布局文件你不希望为它生成对应的`Binding`类，可以在该布局文件的根元素位置加入如下声明：

```xml
<LinearLayout
    xmlns:tools="http://schemas.android.com/tools"
    ...
    tools:viewBindingIgnore="true">
    ...
</LinearLayout>
```

接下来我们看一下如何使用ViewBinding来实现在MainActivity中去设置TextView内容的功能，代码如下所示：

```kotlin
class MainActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        binding.textView.text = "Hello"
    }

}
```

**ViewBinding**的用法可以说就是这么简单。首先我们要调用`activity_main.xml`布局文件对应的`Binding`类，也就是`ActivityMainBinding`的`inflate()`函数去加载该布局，`inflate()`函数接收一个`LayoutInflater`参数，在`Activity`中是可以直接获取到的。

接下来就更加简单了，调用`Binding`类的`getRoot()`函数可以得到`activity_main.xml`中根元素的实例，调用`getTextView()`函数可以获得`id`为`textView`的元素实例。

那么很明显，我们应该把根元素的实例传入到`setContentView()`函数当中，这样`Activity`就可以成功显示`activity_main.xml`这个布局的内容了。然后获取`TextView`控件的实例，并给它设置要显示的文字即可。

当然，如果你需要在`onCreate()`函数之外的地方对控件进行操作，那么就得将`binding`变量声明成全局变量，写法如下：

```kotlin
class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        binding.textView.text = "Hello"
    }

}

```

Kotlin声明的变量都必须在声明的同时对其进行初始化。而这里我们显然无法在声明全局binding变量的同时对它进行初始化，所以这里又使用了lateinit关键字对binding变量进行了延迟初始化。

## 二、Intent

**Intent**大致可以分为两种：**显式Intent**和隐式Intent。我们先来看一下**显式Intent**如何使用。**Intent**有多个构造函数的重载，其中一个是`Intent(Context packageContext, Class<?>cls)`。这个构造函数接收两个参数：第一个参数`Context`要求提供一个启动`Activity`的**上下文**；第二个参数`Class`用于**指定想要启动的目标`Activity`**，通过**这个构造函数就可以构建出`Intent`的“意图**”。那么接下来我们应该怎么使用这个`Intent`呢？`Activity`类中提供了一个`startActivity()`方法，专门用于启动`Activity`，它接收一个`Intent`参数，**这里我们将构建好的Intent传入startActivity()方法就可以启动目标Activity了。**

### 2.1 显式Intent

Intent是Android程序中各组件之间进行交互的一种重要方式，它不仅可以指明当前组件想要执行的动作，还可以在不同组件之间传递数据。Intent一般可用于启动Activity、启动Service以及发送广播等场景。

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = FirstLayoutBinding.inflate(layoutInflater)
        setContentView(binding.root)

//        val button1: Button = findViewById(R.id.button1)
        binding.button1.text = "显式INTENT"
        binding.button1.setOnClickListener{
            val intent = Intent(this, SecondActivity::class.java)
            startActivity(intent)
            Toast.makeText(this, "hello , u clicked button1: ${binding.button1.text}", Toast.LENGTH_SHORT).show()
        }
    }
```

我们首先构建了一个**Intent**对象，第一个参数传入`this`也就是`FirstActivity`作为上下文，第二个参数传入`SecondActivity::class.java`作为目标`Activity`，这样我们的“意图”就非常明显了，即在`FirstActivity`的基础上打开`SecondActivity`。注意，**Kotlin**中`SecondActivity::class.java`的写法就相当于**Java**中`SecondActivity.class`的写法。接下来再通过`startActivity()`方法执行这个**Intent**就可以了。

### 2.2 隐式Intent

相比于显式**Intent**，隐式**Intent**则含蓄了许多，它并不明确指出想要启动哪一个`Activity`，而是指定了一系列更为抽象的`action`和`category`等信息，然后交由系统去分析这个**Intent**，并帮我们找出合适的`Activity`去启动。

通过在`<activity>`标签下配置`<intent-filter>`的内容，可以指定当前`Activity`能够响应的`action`和`category`，打开`AndroidManifest.xml`，添加如下代码：

```xml
        <activity
            android:name=".SecondActivity"
            android:exported="false" >
            <intent-filter>
                <action android:name="com.icu007.activitytest.ACTION_START"/>
                <category android:name="android.intent.category.DEFAULT"/>
                <category android:name="com.icu007.activitytest.MY_CATEGORY"/>
            </intent-filter>
        </activity>
```

在`<action>`标签中我们指明了当前`Activity`可以响应`com.example.activitytest.ACTION_START`这个`action`，而`<category>`标签则包含了一些附加信息，更精确地指明了当前`Activity`能够响应的`Intent`中还可能带有的`category`。只有`<action>`和`<category>`中的内容同时匹配`Intent`中指定的`action`和`category`时，这个`Activity`才能响应该`Intent`。

```kotlin
        binding.button2.setOnClickListener {
            val intent = Intent("com.icu007.activitytest.ACTION_START")
            intent.addCategory("com.icu007.activitytest.MY_CATEGORY")
            startActivity(intent)
        }
```

#### 2.2.1 更多隐式Intent的用法

使用隐式**Intent**，不仅可以启动自己程序内的`Activity`，还可以启动其他程序的`Activity`，这就使多个应用程序之间的功能共享成为了可能。比如你的应用程序中需要展示一个网页，这时你没有必要自己去实现一个浏览器（事实上也不太可能），只需要调用系统的浏览器来打开这个网页就行了。

```kotlin
        binding.button2.setOnClickListener {
            val intent = Intent(Intent.ACTION_VIEW)
            intent.data = Uri.parse("http://icu007.work")
            startActivity(intent)
        }
```

首先指定了**Intent**的`action`是`Intent.ACTION_VIEW`，这是一个`Android`系统内置的动作，其常量值为`android.intent.action.VIEW`。然后通过`Uri.parse()`方法将一个网址字符串解析成一个`Uri`对象，再调用**Intent**的`setData()`方法将这个Uri对象传递进去。当然，这里再次使用了前面学习的语法糖，看上去像是给**Intent**的`data`属性赋值一样。

我们还可以在`<intent-filter>`标签中再配置一个`<data>`标签，用于更精确地指定当前`Activity`能够响应的数据。`<data>`标签中主要可以配置以下内容。

- `android:scheme`：用于指定数据的协议部分，如上例中的https部分；
- `android:host`：用于指定数据的主机名部分，如上例中的www.baidu.com部分；
- `android:port`：用于指定数据的端口部分，一般紧随在主机名之后；
- `android:path`：用于指定主机名和端口之后的部分，如一段网址中跟在域名之后的内容；
- `android:mimeType`：用于指定可以处理的数据类型，允许使用通配符的方式进行指定。

只有当`<data>`标签中指定的内容和**Intent**中携带的`Data`完全一致时，当前`Activity`才能够响应该**Intent**。不过，在`<data>`标签中一般不会指定过多的内容。例如在上面的浏览器示例中，其实只需要指定`android:scheme`为**https**，就可以响应所有**https**协议的**Intent**了。

除了**https**协议外，我们还可以指定很多其他协议，比如**geo**表示显示地理位置、**tel**表示拨打电话。下面的代码展示了如何在我们的程序中调用系统拨号界面。

```kotlin
       binding.button2.setOnClickListener {
            val intent1 = Intent(Intent.ACTION_DIAL)
            intent1.data = Uri.parse("tel:10086")
            startActivity(intent1)
        }
```

### 2.3 向下一个Activity传递数据

到目前为止，我们只是简单地使用**Intent**来启动一个`Activity`，其实**Intent**在启动`Activity`的时候还可以传递数据。

在启动`Activity`时传递数据的思路很简单，**Intent**中提供了一系列`putExtra()`方法的重载，可以把我们想要传递的数据暂存在**Intent**中，在启动另一个`Activity`后，只需要把这些数据从**Intent**中取出就可以了。比如说`FirstActivity`中有一个字符串，现在想把这个字符串传递到`SecondActivity`中，你就可以这样编写：

```kotlin
        binding.button1.setOnClickListener{
            val intent = Intent(this, SecondActivity::class.java)
            val data = "hello SecondActivity"
            intent.putExtra("extra_data",data)
            startActivity(intent)
        }
```

这里我们还是使用显式**Intent**的方式来启动`SecondActivity`，并通过`putExtra()`方法传递了一个字符串。注意，这里`putExtra()`方法接收两个参数，第一个参数是键，用于之后从**Intent**中取值，第二个参数才是真正要传递的数据。然后在`SecondActivity`中将传递的数据取出，并打印出来，代码如下所示：

```kotlin
class SecondActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val bindings = ActivitySecondBinding.inflate(layoutInflater)
        setContentView(bindings.root)
        val extraData = intent.getStringExtra("extra_data")
        Toast.makeText(this, extraData, Toast.LENGTH_SHORT).show()
        bindings.button2.text = "前往FirstActivity"
        bindings.button2.setOnClickListener{
            val intent = Intent(this, FirstActivity::class.java)
            startActivity(intent)
        }
    }
}
```

上述代码中的**intent**实际上调用的是父类的`getIntent()`方法，该方法会获取用于启动`SecondActivity`的**Intent**，然后调用`getStringExtra()`方法并传入相应的键值，就可以得到传递的数据了。这里由于我们传递的是字符串，所以使用`getStringExtra()`方法来获取传递的数据。如果传递的是整型数据，则使用`getIntExtra()`方法；如果传递的是布尔型数据，则使用`getBooleanExtra()`方法，以此类推。

### 2.4 返回数据给上一个Activity

既然可以传递数据给下一个`Activity`，那么能不能够返回数据给上一个`Activity`呢？答案是肯定的。不过不同的是，返回上一个`Activity`只需要按一下**Back**键就可以了，并没有一个用于启动`Activity`的**Intent**来传递数据，这该怎么办呢？其实`Activity`类中还有一个用于启动`Activity`的`startActivityForResult()`方法，但它期望在`Activity`销毁的时候能够返回一个结果给上一个`Activity`。毫无疑问，这就是我们所需要的。

`startActivityForResult()`方法接收两个参数：第一个参数还是**Intent**；第二个参数是**请求码**，用于在之后的回调中判断数据的来源。我们还是来实战一下，修改`FirstActivity`中按钮的点击事件，代码如下所示：

```kotlin
    companion object {
        private const val REQUEST_CODE = 1024
    }

        binding.button3.setOnClickListener {
            val intent = Intent(this, SecondActivity::class.java)
            startActivityForResult(intent, REQUEST_CODE)
        }

    override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        when (requestCode){
            REQUEST_CODE ->{
                val code = requestCode
                val data = data?.getStringExtra("data_return")
                Toast.makeText(this, "code: $code, data: $data", Toast.LENGTH_SHORT).show()
                Log.d(TAG, "onActivityResult: code: $code, data: $data")
            }
        }
    }
```

这里我们使用了`startActivityForResult()`方法来启动`SecondActivity`，请求码只要是一个唯一值即可，这里传入了`1024`。接下来我们在`SecondActivity`中给按钮注册点击事件，并在点击事件中添加返回数据的逻辑，代码如下所示：

```kotlin
        bindings.button1.setOnClickListener {
            val intent = Intent()
            intent.putExtra("data_return", "Hello FirstActivity")
            setResult(1024, intent)
            finish()
        }
```

上述方式在在``androidx.activity-1.2.0-alpha04`时开始就已经被弃用了.Android中这位被调用过无数次的`startActivityForResult`和`onActivityResult`，已经被官方标记为弃用了，继而推出了名为`Activity Result API`的组件。

```kotlin
class FirstActivity : AppCompatActivity() {
    private val TAG: String? = "FirstActivity"
    private lateinit var resultLauncher: ActivityResultLauncher<Intent>
    private val launcherCallback = ActivityResultCallback<ActivityResult> { result ->
        val code = result.resultCode
        val data = result.data
        Toast.makeText(this, "code: $code, data: ${data?.getStringExtra("data_return")}", Toast.LENGTH_SHORT).show()
    }

    companion object {
        private const val REQUEST_CODE = 1024
    }
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = FirstLayoutBinding.inflate(layoutInflater)
        setContentView(binding.root)

        resultLauncher = registerForActivityResult(
            ActivityResultContracts.StartActivityForResult(),
            launcherCallback
        )
        binding.button3.setOnClickListener {
            val intent = Intent(this, SecondActivity::class.java)
//            startActivityForResult(intent, REQUEST_CODE)
            resultLauncher.launch(intent)
        }
    }
}
```

这里其实分为三个部分：对载体、定义协定、回调3个类分别定义写出来。

```kotlin
    private lateinit var resultLauncher: ActivityResultLauncher<Intent>

    private val launcherCallback = ActivityResultCallback<ActivityResult> { result ->
        val code = result.resultCode
        val data = result.data
        Toast.makeText(this, "code: $code, data: ${data?.getStringExtra("data_return")}", Toast.LENGTH_SHORT).show()
    }


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding = FirstLayoutBinding.inflate(layoutInflater)
        setContentView(binding.root)
        resultLauncher = registerForActivityResult(
            ActivityResultContracts.StartActivityForResult(),
            launcherCallback
        )
        resultLauncher.launch(Intent(this,SecondActivity::class.java))
    }
```

其实大部分情况下，我们可以这样写：

```kotlin
private val launcherActivity = registerForActivityResult(
    ActivityResultContracts.StartActivityForResult()) {
    val code = it.resultCode
    val data = it.data
}

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    launcherActivity.launch(Intent(this, SecondActivity::class.java))
}
```

是不是瞬间清爽了许多，但是…你还是觉得比使用`startActivityForResult`更复杂？其实不然，因为上面代码的需求是一个单一的回调，所以看着似乎`startActivityForResult`更便于维护和使用。但倘若编写一个稍复杂的页面，需要同时请求相册、需要在其它**Activity**选择数据并回调、需要判断权限等等时，继续使用`startActivityForResult`，会导致`onActivityResult`里掺杂各种嵌套及判断，导致代码难以维护。而使用`registerForActivityResult()`可以多次调用以注册多个 `ActivityResultLauncher` 实例，用来处理不同的**Activity**结果，让代码更便于维护。

优势了解到了，但既然需要使用新的功能，那么我们就必须要先了解以下，刚说到的`ActivityResultLauncher`、`ActivityResultContract`、`ActivityResultCallback`到底是些什么东西

- **ActivityResultLauncher** 从字面意思其实就能很好理解，可以理解它就是一个Activity的启动器，它的作用就是承载启动对象与返回对象，通过`registerForActivityResult`返回该对象，这时并不会立即启动另一个Activity。
- **ActivityResultContract** 是用来协定所需的输入类型以及结果的输出类型，Android默认提供了一些常用的定义，例如上面所使用到到`ActivityResultContracts.StartActivityForResult()`。当然这里你也可以通过继承`ActivityResultContract`实现自己的定义。
- **ActivityResultCallback** 通过名字就可以了解到这是启动Activity并返回到当前Activity时的结果回调。

对于这3个类，其实只需重点了解`ActivityResultContract`，就能很轻松的理解并使用好`Activity Result API`了。

>官方文档的警告
>
>**注意**：虽然在 fragment 或 activity 创建完毕之前可安全地调用 `registerForActivityResult()`，但在 fragment 或 activity 的 `Lifecycle` 变为 `CREATED` 状态之前，您无法启动 `ActivityResultLauncher`。

## 三、Activity的生命周期

掌握**Activity**的生命周期对任何**Android**开发者来说都非常重要，当你深入理解**Activity**的生命周期之后，就可以写出更加连贯流畅的程序，并在如何合理管理应用资源方面发挥得游刃有余。你的应用程序也将会拥有更好的用户体验。

### 3.1 返回栈

经过前面几节的学习，相信你已经发现了**Android**中的**Activity**是可以层叠的。我们每启动一个新的**Activity**，就会覆盖在原**Activity**之上，然后点击**Back**键会销毁最上面的**Activity**，下面的一个**Activity**就会重新显示出来。

其实**Android**是使用任务（**task**）来管理**Activity**的，一个任务就是一组存放在栈里的**Activity**的集合，这个栈也被称作返回栈（**back stack**）。栈是一种**后进先出**的数据结构，**在默认情况下，每当我们启动了一个新的Activity，它就会在返回栈中入栈，并处于栈顶的位置**。而每当我们按下**Back**键或调用`finish()`方法去销毁一个**Activity**时，处于栈顶的**Activity**就会出栈，前一个入栈的**Activity**就会重新处于栈顶的位置。系统总是会显示处于栈顶的**Activity**给用户。

![返回栈管理Activity入栈出栈操作](https://pic.ziyuan.wang/2023/09/22/guest_8e608b6d30eef_IP210.22.23.7_UPTIME1695350133.png)

### 3.2 Activity状态

每个**Activity**在其生命周期中最多可能会有4种状态。

#### 3.2.1 运行状态

当一个**Activity**位于返回栈的栈顶时，**Activity**就处于运行状态。系统最不愿意回收的就是处于运行状态的**Activity**，因为这会带来非常差的用户体验。

#### 3.2.2 暂停状态

当一个**Activity不再处于栈顶位置，但仍然可见时**，**Activity**就进入了**暂停**状态。你可能会觉得，既然Activity已经不在栈顶了，怎么会可见呢？这是因为并不是每一个Activity都会占满整个屏幕，比如对话框形式的Activity只会占用屏幕中间的部分区域。处于暂停状态的Activity仍然是完全存活着的，系统也不愿意回收这种Activity（因为它还是可见的，回收可见的东西都会在用户体验方面有不好的影响），只有在内存极低的情况下，系统才会去考虑回收这种Activity。

#### 3.2.3 停止状态

**当一个Activity不再处于栈顶位置，并且完全不可见的时候，就进入了停止状态。**系统仍然会为这种Activity保存相应的状态和成员变量，但是这并不是完全可靠的，当其他地方需要内存时，处于停止状态的Activity有可能会被系统回收。

#### 3.2.4 销毁状态

**一个Activity从返回栈中移除后就变成了销毁状态**。**系统最倾向于回收处于这种状态的Activity**，以保证手机的内存充足。

### 3.3 Activity的生存期

Activity类中定义了7个回调方法，覆盖了Activity生命周期的每一个环节，下面就来一一介绍这7个方法。

- `onCreate()`:这个方法你已经看到过很多次了，我们在每个Activity中都重写了这个方法，它会在Activity第一次被创建的时候调用。你应该在这个方法中完成Activity的初始化操作，比如加载布局、绑定事件等。
- `onStart()`:这个方法在Activity由不可见变为可见的时候调用。
- `onResume()`:这个方法在Activity准备好和用户进行交互的时候调用。此时的Activity一定位于返回栈的栈顶，并且处于运行状态。
- `onPause()`:这个方法在系统准备去启动或者恢复另一个Activity的时候调用。我们通常会在这个方法中将一些消耗CPU的资源释放掉，以及保存一些关键数据，但这个方法的执行速度一定要快，不然会影响到新的栈顶Activity的使用。
- `onStop()`:这个方法在Activity完全不可见的时候调用。它和onPause()方法的主要区别在于，如果启动的新Activity是一个对话框式的Activity，那么onPause()方法会得到执行，而onStop()方法并不会执行。
- `onDestroy()`:这个方法在Activity被销毁之前调用，之后Activity的状态将变为销毁状态。
- `onRestart()`:这个方法在Activity由停止状态变为运行状态之前调用，也就是Activity被重新启动了。

以上7个方法中除了`onRestart()`方法，其他都是两两相对的，从而又可以将**Activity**分为以下3种生存期。

- **完整生存期**：Activity在`onCreate()`方法和`onDestroy()`方法之间所经历的就是完整生存期。一般情况下，一个Activity会在onCreate()方法中完成各种初始化操作，而在onDestroy()方法中完成释放内存的操作。
- **可见生存期**：Activity在`onStart()`方法和`onStop()`方法之间所经历的就是可见生存期。在可见生存期内，Activity对于用户总是可见的，即便有可能无法和用户进行交互。我们可以通过这两个方法合理地管理那些对用户可见的资源。比如在`onStart()`方法中对资源进行加载，而在`onStop()`方法中对资源进行释放，从而保证处于停止状态的Activity不会占用过多内存。
- **前台生存期**：Activity在`onResume()`方法和`onPause()`方法之间所经历的就是前台生存期。在前台生存期内，Activity总是处于运行状态，此时的Activity是可以和用户进行交互的，我们平时看到和接触最多的就是这个状态下的Activity。

为了帮助我们理解，Android官方提供了一张Activity生命周期的示意图，如图所示：

![1695438122586.png](https://pic.ziyuan.wang/2023/09/23/guest_4aec46f2fe334_IP210.22.23.7_UPTIME1695438124.png)

### 3.4 体验Activity的生命周期

如果我们重写Activity中的：`onCreate()`、`onStart()`、`onResume()`、`onPause()`、`onStop()`、`onDestroy()`和`onRestart()`方法并打印log，我们就可以发现：Activity在第一次被创建时会依次执行`onCreate()`、`onStart()`和onResume()方法。如下所示：

![1695459210334.png](https://pic.ziyuan.wang/2023/09/23/guest_7a524a4f102bd_IP210.22.23.7_UPTIME1695459211.png)

Activity启动后 我们按下Home键：由于MainAtivity已经不再可见，因此`onPause()`和`onStop()`方法都会得到执行。

![1695459058606.png](https://pic.ziyuan.wang/2023/09/23/guest_950a90c3af688_IP210.22.23.7_UPTIME1695459060.png)

接下来我们再次进入应用，由于之前MainActivity已经进入了停止状态，所以`onRestart()`方法会得到执行，之后会依次执行`onStart()`和`onResume()`方法。注意，**此时`onCreate()`方法不会执行，因为MainActivity并没有重新创建。**

![image-20230923165303676](C:\Users\Charlie\AppData\Roaming\Typora\typora-user-images\image-20230923165303676.png)

假如我们在MainActivity当中启动了一个Dialog，此时我们再去观察打印信息就会发现：

![1695459418512.png](https://pic.ziyuan.wang/2023/09/23/guest_908b26616516a_IP210.22.23.7_UPTIME1695459420.png)

只有`onPause()`方法得到了执行，`onStop()`方法并没有执行，这是因为DialogActivity并没有完全遮挡住MainActivity，**此时MainActivity只是进入了暂停状态，并没有进入停止状态**。相应地，按下Back键返回MainActivity也应该只有`onResume()`方法会得到执行

![1695459806957.png](https://pic.ziyuan.wang/2023/09/23/guest_1752fd92f9fa7_IP210.22.23.7_UPTIME1695459808.png)

当调用了 `finish()`方法结束Activity时，依次会执行`onPause()`、`onStop()`和`onDestroy()`方法，最终销毁MainActivity。

![1695459613638.png](https://pic.ziyuan.wang/2023/09/23/guest_46ea5f1413bda_IP210.22.23.7_UPTIME1695459615.png)

### 3.5 Activity被回收了怎么办

前面我们说过，当一个Activity进入了停止状态，是有可能被系统回收的。那么想象以下场景：应用中有一个Activity A，用户在Activity A的基础上启动了Activity B，Activity A就进入了停止状态，这个时候由于系统内存不足，将Activity A回收掉了，然后用户按下Back键返回Activity A，会出现什么情况呢？其实还是会正常显示Activity A的，只不过这时并不会执行onRestart()方法，而是会执行Activity A的onCreate()方法，因为Activity A在这种情况下会被重新创建一次。

这样看上去好像一切正常，可是别忽略了一个重要问题：Activity A中是可能存在临时数据和状态的。打个比方，MainActivity中如果有一个文本输入框，现在你输入了一段文字，然后启动NormalActivity，这时MainActivity由于系统内存不足被回收掉，过了一会你又点击了Back键回到MainActivity，你会发现刚刚输入的文字都没了，因为MainActivity被重新创建了。

如果我们的应用出现了这种情况，是会比较影响用户体验的，所以得想想办法解决这个问题。其实，Activity中还提供了一个`onSaveInstanceState()`回调方法，这个方法可以保证在Activity被回收之前一定会被调用，因此我们可以通过这个方法来解决问题。

`onSaveInstanceState()`方法会携带一个Bundle类型的参数，Bundle提供了一系列的方法用于保存数据，比如可以使用putString()方法保存字符串，使用putInt()方法保存整型数据，以此类推。每个保存方法需要传入两个参数，第一个参数是键，用于后面从Bundle中取值，第二个参数是真正要保存的内容。

在MainActivity当中添加如下代码就可以将临时数据进行保存了：

```kotlin
	override fun onSaveInstanceState(outState: Bundle) {
        super.onSaveInstanceState(outState)
        val tempData = "Something u just typed"
        outState.putString("data_key", tempData)
    }
```

数据是已经保存下来了，那么我们应该在哪里进行恢复呢？细心的你也许早就发现，我们一直使用的onCreate()方法其实也有一个Bundle类型的参数。这个参数在一般情况下都是null，但是如果在Activity被系统回收之前，你通过onSaveInstanceState()方法保存数据，这个参数就会带有之前保存的全部数据，我们只需要再通过相应的取值方法将数据取出即可。

修改MainActivity方法：

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        Log.d(tag, "onCreate: Activity第一次被创建的时候调用")
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        if(savedInstanceState != null){
            val tempData = savedInstanceState.getString("data_key",null)
            Log.d(tag, "onCreate: ")
        }
    }
```

取出值之后再做相应的恢复操作就可以了，比如将文本内容重新赋值到文本输入框上.

不知道你有没有察觉，使用Bundle保存和取出数据是不是有些似曾相识呢？没错！我们在使用Intent传递数据时也用的类似的方法。这里提醒一点，Intent还可以结合Bundle一起用于传递数据。首先我们可以把需要传递的数据都保存在Bundle对象中，然后再将Bundle对象存放在Intent里。到了目标Activity之后，先从Intent中取出Bundle，再从Bundle中一一取出数据。

---

## 四、Activity的启动模式

在实际项目中我们应该根据特定的需求为每个**Activity**指定恰当的启动模式。启动模式一共有4种，分别是`standard`、`singleTop`、`singleTask`和`singleInstance`，可以在**AndroidManifest.xml**中通过给**`<activity>`**标签指定`android:launchMode`属性来选择启动模式。

### 4.1 standard

standard是Activity默认的启动模式，在不进行显式指定的情况下，所有Activity都会自动使用这种启动模式。Android是使用返回栈来管理Activity的，**在standard模式下，每当启动一个新的Activity，它就会在返回栈中入栈，并处于栈顶的位置。**对于使用standard模式的Activity，系统不会在乎这个Activity是否已经在返回栈中存在，每次启动都会创建一个该Activity的新实例。

standard模式的原理：

![1695630787163.png](https://pic.ziyuan.wang/2023/09/25/guest_6d3de3b7b8990_IP210.22.23.7_UPTIME1695630789.png)

### 4.2 singleTop

可能在有些情况下，**standard**模式不太合理。Activity明明已经在栈顶了，为什么再次启动的时候还要创建一个新的Activity实例呢？

别着急，这只是系统默认的一种启动模式而已，你完全可以根据自己的需要进行修改，比如使用**singleTop模式**。当Activity的启动模式指定为singleTop，在启动Activity时如果发现返回栈的栈顶已经是该Activity，则认为可以直接使用它，不会再创建新的Activity实例。

在 **AndroidManifest.xml**中可以如下设置：`android:launchMode="singleTop"`

```xml
        <activity
            android:name=".MainActivity"
            android:launchMode="singleTop"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```

![1695716279930.png](https://pic.ziyuan.wang/2023/09/26/guest_1dc0c06bc5b27_IP210.22.23.7_UPTIME1695716282.png)

有这么一种情况：从 **FirstActivity**启动 **SecondActivity** 再从 **SecondActivity**启动 **FirstActivity**。

此时系统就创建了两个不同的FirstActivity实例，这是由于在SecondActivity中再次启动FirstActivity时，栈顶Activity已经变成了SecondActivity，因此会创建一个新的FirstActivity实例。

### 4.3 singleTask

**使用singleTop模式可以很好地解决重复创建栈顶Activity的问题**，如果该Activity并没有处于栈顶的位置，还是可能会创建多个Activity实例的。那么有没有什么办法可以让某个Activity在整个应用程序的上下文中只存在一个实例呢？这就要借助singleTask模式来实现了。**当Activity的启动模式指定为singleTask，每次启动该Activity时，系统首先会在返回栈中检查是否存在该Activity的实例，如果发现已经存在则直接使用该实例，并把在这个Activity之上的所有其他Activity统统出栈，如果没有发现就会创建一个新的Activity实例。**

通过代码更直观的感受一下

首先从 **FirstActivity**启动 **SecondActivity** 再从 **SecondActivity**启动 **FirstActivity**。在相应的生存周期加上日志打印：

![1696648430077.png](https://pic.ziyuan.wang/2023/10/07/guest_a37c3b91912f5_IP38.207.142.167_UPTIME1696648431.png)

其实从打印信息中就可以明显看出，在**SecondActivity**中启动**FirstActivity**时，会发现返回栈中已经存在一个**FirstActivity**的实例，并且是在**SecondActivity**的下面，于是**SecondActivity**会从返回栈中出栈，而**FirstActivity重新成为了栈顶Activity，因此FirstActivity的`onRestart()`方法和SecondActivity的`onDestroy()`方法会得到执行**。现在返回栈中只剩下一个FirstActivity的实例了，按一下Back键就可以退出程序。

SingleTask 模式原理

![1696649398154.png](https://pic.ziyuan.wang/2023/10/07/guest_430ee7fa2a0dc_IP38.207.142.167_UPTIME1696649401.png)

### 4.3 singleInstance

**singleInstance**模式应该算是4种启动模式中最特殊也最复杂的一个了。不同于以上3种启动模式，**指定为singleInstance模式的Activity会启用一个新的返回栈来管理这个Activity（其实如果singleTask模式指定了不同的taskAffinity，也会启动一个新的返回栈）。**那么这样做有什么意义呢？想象以下场景，假设我们的程序中有一个**Activity**是允许其他程序调用的，如果想实现其他程序和我们的程序可以共享这个**Activity**的实例，应该如何实现呢？使用前面3种启动模式肯定是做不到的，因为每个应用程序都会有自己的返回栈，同一个**Activity**在不同的返回栈中入栈时必然创建了新的实例。而使用**singleInstance**模式就可以解决这个问题，在这种模式下，会有一个单独的返回栈来管理这个**Activity**，不管是哪个应用程序来访问这个**Activity**，都共用同一个返回栈，也就解决了共享**Activity**实例的问题。

为了更好理解这种启动模式，接下来实践一下：在`AndroidManifest.xml`中修改启动模式为 **singleInstance**

```xml
        <activity
            android:name=".MainActivity"
            android:exported="true"
            android:launchMode="singleInstance">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
```

然后从 **MainActivity**启动 **NormalActivity**,再从**NormalActivity**启动**ThirdActivity**。并在这几个**Activity**中分别打印出**TaskID**。

**通过打印我们可以发现 MainActivity是单独在一个返回栈的，NormalActivity和ThirdActivity在另外一个返回栈。**

![1696732586522.png](https://pic.ziyuan.wang/2023/10/08/guest_5704819fa6887_IP210.22.23.7_UPTIME1696732588.png)

**singleInstance**模式的原理如图所示

![1696734776879.png](https://pic.ziyuan.wang/2023/10/08/guest_aaeb91b4fdf1e_IP210.22.23.7_UPTIME1696734778.png)

---

## 五、Activity的最佳实践

### 5.1 知晓当前是在哪一个Activity

我们还是在ActivityTest项目的基础上修改，首先需要新建一个BaseActivity类。右击com.example.activitytest包→New→Kotlin File/Class，在弹出的窗口中输入BaseActivity，创建类型选择Class。让BaseActivity继承自AppCompatActivity，并重写onCreate()方法，如下所示：

```kotlin
open class BaseActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        Log.d("LCR BaseActivity", javaClass.simpleName)
        super.onCreate(savedInstanceState)

    }
}
```

我们在onCreate()方法中加了一行日志，用于打印当前实例的类名。这里我要额外说明一下，Kotlin中的javaClass表示获取当前实例的Class对象，相当于在Java中调用getClass()方法；而Kotlin中的BaseActivity::class.java表示获取BaseActivity类的Class对象，相当于在Java中调用BaseActivity.class。在上述代码中，我们先是获取了当前实例的Class对象，然后再调用simpleName获取当前实例的类名。

下来我们需要让BaseActivity成为ActivityTest项目中所有Activity的父类，为了使BaseActivity可以被继承，我已经提前在类名的前面加上了open关键字。然后修改FirstActivity、SecondActivity和ThirdActivity的继承结构，让它们不再继承自AppCompatActivity，而是继承自BaseActivity。而由于BaseActivity又是继承自AppCompatActivity的，所以项目中所有Activity的现有功能并不受影响，它们仍然继承了Activity中的所有特性。

通过日志可以看到，启动的Activity都被打印出来了。现在每当我们进入一个Activity的界面，该Activity的类名就会被打印出来，这样我们就可以时刻知晓当前界面对应的是哪一个Activity了。

![1696756270348.png](https://pic.ziyuan.wang/2023/10/08/guest_35b81189e5066_IP210.22.23.7_UPTIME1696756272.png)

### 5.2 随时随地退出程序

如果手机的界面停留在ThirdActivity，我们会发现当前想退出程序是非常不方便的，需要连按3次Back键才行。按Home键只是把程序挂起，并没有退出程序。如果我们的程序需要注销或者退出的功能该怎么办呢？看来要有一个随时随地都能退出程序的方案才行。

其实解决思路也很简单，只需要用一个专门的集合对所有的Activity进行管理就可以了。下面我们就来实现一下。

新建一个单例类ActivityCollector作为Activity的集合，代码如下所示：

```kotlin
package work.icu007.activitylifecycletest

import android.app.Activity
import android.util.Log


/*
 * Author: Charlie_Liao
 * Time: 2023/10/11-11:09
 * E-mail: rookie_l@icu007.work
 */

object ActivityCollector {
    private val activites = ArrayList<Activity>()

    fun addActivity(activity: Activity){
        Log.d("LCR ActivityCollector", "addActivity: ${activity.componentName} add")
        activites.add(activity)
    }

    fun removeActivity(activity: Activity){
        Log.d("LCR ActivityCollector", "removeActivity: ${activity.componentName} remove")
        activites.remove(activity)
    }

    fun finishAll(){
        for (activity in activites){
            if (!activity.isFinishing){
                activity.finish()
            }
        }
        activites.clear()
        android.os.Process.killProcess(android.os.Process.myPid())
        Log.d("LCR ActivityCollector", "finishAll: ")
    }
}
```

这里使用了单例类，是因为全局只需要一个Activity集合。在集合中，我们通过一个ArrayList来暂存Activity，然后提供了一个addActivity()方法，用于向ArrayList中添加Activity；提供了一个removeActivity()方法，用于从ArrayList中移除Activity；最后提供了一个finishAll()方法，用于将ArrayList中存储的Activity全部销毁。注意在销毁Activity之前，我们需要先调用activity.isFinishing来判断Activity是否正在销毁中，因为Activity还可能通过按下Back键等方式被销毁，如果该Activity没有正在销毁中，我们再去调用它的finish()方法来销毁它。

接下来修改 BaseActivity中的代码

```kotlin
open class BaseActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        Log.d("LCR BaseActivity", javaClass.simpleName)
        super.onCreate(savedInstanceState)
        ActivityCollector.addActivity(this)
//        Log.d("LCR BaseActivity", "onCreate: ${javaClass.simpleName} added")
    }

    override fun onDestroy() {
        super.onDestroy()
        ActivityCollector.removeActivity(this)
//        Log.d("LCR BaseActivity", "onDestroy: ${javaClass.simpleName} removed")
    }
}
```

在BaseActivity的onCreate()方法中调用了ActivityCollector的addActivity()方法，表明将当前正在创建的Activity添加到集合里。然后在BaseActivity中重写onDestroy()方法，并调用了ActivityCollector的removeActivity()方法，表明从集合里移除一个马上要销毁的Activity。

从此以后，不管你想在什么地方退出程序，只需要调用ActivityCollector.finishAll()方法就可以了。例如在ThirdActivity界面想通过点击按钮直接退出程序，只需将代码改成如下形式：

```kotlin
class ThirdActivity : BaseActivity() {
    private lateinit var binding: ActivityThirdBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        Log.d("LCR", "onCreate: TaskId is $taskId")
        binding = ActivityThirdBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.btnFinish.setOnClickListener {
            ActivityCollector.finishAll()
//            android.os.Process.killProcess(android.os.Process.myPid())
        }
    }
}
```

### 5.3 启动Activity的最佳写法

启动Activity的方法相信我们已经非常熟悉了，首先通过Intent构建出当前的“意图”，然后调用startActivity()或startActivityForResult()方法将Activity启动起来，如果有数据需要在Activity之间传递，也可以借助Intent来完成。

假设SecondActivity中需要用到两个非常重要的字符串参数，在启动SecondActivity的时候必须传递过来，那么我们很容易会写出如下代码：

```kotlin
val intent = Intent(this, SecondActivity::class.java)
intent.putExtra("param1", "data1")
intent.putExtra("param1", "data1")
startActivity(intent)
```

虽然这样写是完全正确的，但是在真正的项目开发中经常会出现对接的问题。比如SecondActivity并不是由你开发的，但现在你负责开发的部分需要启动SecondActivity，而你却不清楚启动SecondActivity需要传递哪些数据。这时无非就有两个办法：一个是你自己去阅读SecondActivity中的代码，另一个是询问负责编写SecondActivity的同事。你会不会觉得很麻烦呢？其实只需要换一种写法，就可以轻松解决上面的窘境。

```kotlin
    companion object : BaseActivity() {
        fun actionStart(context: Context, data1: String, data2: String){
            val intent = Intent(context, ThirdActivity::class.java)
            intent.putExtra("param1", data1)
            intent.putExtra("param2", data2)
            context.startActivity(intent)
        }
    }
```

在这里我们使用了一个新的语法结构companion object，并在companion object中定义了一个actionStart()方法。之所以要这样写，是因为Kotlin规定，所有定义在companionobject中的方法都可以使用类似于Java静态方法的形式调用。

接下来我们重点看actionStart()方法，在这个方法中完成了Intent的构建，另外所有SecondActivity中需要的数据都是通过actionStart()方法的参数传递过来的，然后把它们存储到Intent中，最后调用startActivity()方法启动SecondActivity。

---



