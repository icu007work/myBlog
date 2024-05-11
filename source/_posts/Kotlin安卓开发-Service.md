---
title: Kotlin安卓开发-Service
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
swiper_index: 1
top_group_index: 1
background: '#fff'
cover: 'https://pic2.ziyuan.wang/user/xiheya/2024/05/Service_595ef761c9141.jpg'
ai:
  - >-
    这篇文章主要介绍了Android开发中的Service和多线程编程。首先，文章解释了Service的概念，它是Android中实现程序后台运行的解决方案，非常适合执行那些不需要和用户交互而且还要求长期运行的任务。Service的运行不依赖于任何用户界面，即使程序被切换到后台，者用户打开了另外一个应用程序，Service仍然能够保持正常运行。然而，Service并不是运行在一个独立的进程当中的，而是依赖于创建Service时所在的应用程序进程。当某个应用程序进程被杀掉时，所有依赖于进程的Service也会停止运行。此外，Service并不会自动开启线程，所有的代码都是默认运行在主线程当中的。因此，我们需要在Service的内部手动创建子线程，并在这里执行具体的任务，否则就有可能出现主线被阻塞的情况。然后，文章介绍了Android多线程编程的基本用法。当我们需要执行一些耗时操作，比如发起一条网络请求时，如果不将这类操作放在子线程里运行，就会导致主线程被阻塞，从而影响用户对软件的正常使用。Androi多线程编程其实并不比Java多线程编程特殊，基本是使用相同的语法。比如，定义一个线程只需要新建一个类继承自Thread，然后重写父类的run()方法，并在里面编写耗时逻辑即可。
abbrlink: 4928ca7e
date: 2024-01-11 15:23:01
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

## 一、什么是Service

`Service`是Android中实现程序后台运行的解决方案，它非常适合执行那些不需要和用户交互而且还要求长期运行的任务。`Service`的运行不依赖于任何用户界面，即使程序被切换到后台，或者用户打开了另外一个应用程序，`Service`仍然能够保持正常运行。不过需要注意的是，**`Service`并不是运行在一个独立的进程当中的，而是依赖于创建`Service`时所在的应用程序进程。当某个应用程序进程被杀掉时，所有依赖于该进程的`Service`也会停止运行。**

另外，也不要被Service的后台概念所迷惑，实际上**Service并不会自动开启线程，所有的代码都是默认运行在主线程当中的。也就是说，我们需要在Service的内部手动创建子线程，并在这里执行具体的任务，否则就有可能出现主线程被阻塞的情况。**

---

## 二、Android多线程编程

当我们需要执行一些耗时操作，比如发起一条网络请求时，考虑到网速等其他原因，服务器未必能够立刻响应我们的请求，如果不将这类操作放在子线程里运行，就会导致主线程被阻塞，从而影响用户对软件的正常使用。

### 2.1 多线程的基本用法

Android多线程编程其实并不比Java多线程编程特殊，基本是使用相同的语法。比如，定义一个线程只需要新建一个类继承自Thread，然后重写父类的run()方法，并在里面编写耗时逻辑即可，如下所示：

```kotlin
class MyThread: Thread() {
    override fun run() {
        // TODO
    }
}
```

那么该如何启动这个线程呢？其实很简单，只需要创建`MyThread`的实例，然后调用它的`start()`方法即可，这样`run()`方法中的代码就会在子线程当中运行了，如下所示：

```kotlin
MyThread().start()
```

当然，使用继承的方式耦合性有点高，我们会更多地选择使用实现`Runnable`接口的方式来定义一个线程，如下所示：

```kotlin
class MyThread: Runnable() {
    override fun run() {
        // TODO
    }
}
```

如果使用了这种写法，启动线程的方法也需要进行相应的改变，如下所示：

```kotlin
val myThread = MyThread()
Thread(myThread).start()
```

可以看到，**Thread的构造函数接收一个`Runnable`参数**，而我们创建的`MyThread`实例正是一个实现了`Runnable`接口的对象，所以可以直接将它传入`Thread`的构造函数里。接着调用`Thread`的`start()`方法，`run()`方法中的代码就会在子线程当中运行了。

当然，如果你不想专门再定义一个类去实现`Runnable`接口，也可以使用`Lambda`的方式，这种写法更为常见，如下所示：

```kotlin
Thread {
    // TODO
}.start()
```

以上几种线程的使用方式你应该不会感到陌生，因为在Java中创建和启动线程也是使用同样的方式。而Kotlin还给我们提供了一种更加简单的开启线程的方式，写法如下：

```kotlin
thread {
    // TODO
}
```

这里的thread是一个Kotlin内置的顶层函数，我们只需要在Lambda表达式中编写具体的逻辑就可以了，连start()方法都不用调用，thread函数在内部帮我们全部都处理好了。

### 2.2 在子线程中更新UI

和许多其他的GUI库一样，Android的UI也是线程不安全的。也就是说，如果想要更新应用程序里的UI元素，必须在主线程中进行，否则就会出现异常。

举个栗子🌰：

我们增加两个控件，一个`Text View`，一个`Button`。我们在捕获到`Button`的点击事件后开启一个线程，在线程中更新`Text View`的内容。

```kotlin
package work.icu007.androidthread

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import work.icu007.androidthread.databinding.ActivityMainBinding
import kotlin.concurrent.thread

class MainActivity : AppCompatActivity() {
    lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)

        mainBinding.changeTextBtn.setOnClickListener {
            thread {
                // 在子线程中更新UI将会导致crash
                // Only the original thread that created a view hierarchy can touch its views. Expected: main Calling: Thread-3
                mainBinding.textView.text = "Nice to meet u"
            }
        }
    }
}
```

运行程序会发现，程序会崩溃，报错信息为：

![1705904581604.png](https://pic.ziyuan.wang/user/xiheya/2024/01/1705904581604_916aaaa5a848e.png)

可以看出是由于在子线程中更新UI所导致，由此证实了Android确实是不允许在子线程中进行UI操作的。但是有些时候，我们必须在子线程里执行一些耗时任务，然后根据任务的执行结果来更新相应的UI控件，该怎么办呢。

Android提供了一套异步消息处理机制，完美地解决了在子线程中进行UI操作的问题。

代码如下：

```kotlin
package work.icu007.androidthread

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.os.Handler
import android.os.Looper
import android.os.Message
import work.icu007.androidthread.databinding.ActivityMainBinding
import kotlin.concurrent.thread

class MainActivity : AppCompatActivity() {
    private val updateText: Int = 1
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)

        val handler = object : Handler(Looper.getMainLooper()) {
            override fun handleMessage(msg: Message) {
                when(msg.what) {
                    updateText -> mainBinding.textView.text = "Nice to Meet U"
                }
            }
        }

        mainBinding.changeTextBtn.setOnClickListener {
            thread {
                val msg = Message()
                msg.what = updateText
                handler.sendMessage(msg)
            }
        }
    }
}
```

这里我们先是定义了一个整型变量`updateText`，用于表示更新`TextView`这个动作。然后新增一个Handler对象，并重写父类的`handleMessage()`方法，在这里对具体的Message进行处理。如果发现Message的what字段的值等于`updateText`，就将`TextView`显示的内容改成“Nice to meet you”。下面再来看一下“Change Text”按钮的点击事件中的代码。可以看到，这次我们并没有在子线程里直接进行UI操作，而是创建了一个`Message（android.os.Message）`对象，并将它的`what`字段的值指定为`updateText`，然后调用`Handler`的`sendMessage()`方法将这条Message发送出去。很快，`Handler`就会收到这条`Message`，并在`handleMessage()`方法中对它进行处理。**注意此时`handleMessage()`方法中的代码就是在主线程当中运行的了，所以我们可以放心地在这里进行UI操作。**接下来对`Message`携带的`what`字段的值进行判断，**如果等于`updateText`，就将`TextView`显示的内容改成“Nice to meet you”。**

来分析一下`Android`异步消息处理机制到底是如何工作的。

### 2.3 解析异步消息处理机制

Android中的异步消息处理主要由4个部分组成：`Message`、`Handler`、`MessageQueue`和`Looper`。其中`Message`和`Handler`在上一小节中我们已经接触过了，`MessageQueue`和`Looper`相对来说还是全新的概念。

1. **Message**

   **Message是在线程之间传递的消息，它可以在内部携带少量的信息，用于在不同线程之间传递数据。**上一小节中我们使用到了Message的what字段，除此之外还可以使用arg1和arg2字段来携带一些整型数据，使用`obj`字段携带一个`Object`对象。

2. **Handler**

   **Handler顾名思义也就是处理者的意思，它主要是用于发送和处理消息的。**发送消息一般是使用Handler的`sendMessage()`方法、`post()`方法等，而发出的消息经过一系列地辗转处理后，最终会传递到Handler的`handleMessage()`方法中。

3. **MessageQueue**

   **`MessageQueue`是消息队列的意思，它主要用于存放所有通过Handler发送的消息。**这部分消息会一直存在于消息队列中，等待被处理。每个线程中只会有一个`MessageQueue`对象。

4. **Looper**

   **Looper是每个线程中的`MessageQueue`的管家，调用Looper的`loop()`方法后，就会进入一个无限循环当中，然后每当发现`MessageQueue`中存在一条消息时，就会将它取出，并传递到Handler的`handleMessage()`方法中。**每个线程中只会有一个Looper对象。

   了解了**Message、Handler、MessageQueue以及Looper**的基本概念后，我们再来把异步消息处理的整个流程梳理一遍。

   - 首先需要在主线程当中创建一个Handler对象，并重写`handleMessage()`方法。然后当子线程中需要进行UI操作时，就创建一个Message对象，并通过Handler将这条消息发送出去。

   - 之后这条消息会被添加到MessageQueue的队列中等待被处理，而Looper则会一直尝试从MessageQueue中取出待处理消息，最后分发回Handler的handleMessage()方法中。

   - 由于Handler的构造函数中我们传入了`Looper.getMainLooper()`，所以此时`handleMessage()`方法中的代码也会在主线程中运行，于是我们在这里就可以安心地进行UI操作了。

   整个异步消息处理机制的流程如下图所示。

   ![1705905983755.png](https://pic.ziyuan.wang/user/xiheya/2024/01/1705905983755_52e55f9c353f2.png)

   一条Message经过以上流程的辗转调用后，也就从子线程进入了主线程，从不能更新UI变成了可以更新UI，整个异步消息处理的核心思想就是如此。

   > 异步消息处理在 Android 中主要涉及到四个部分：Message, Handler, MessageQueue 和 Looper。
   >
   > 
   >
   > 1. **Message** 当我们需要执行一个新的任务时，首先我们会创建一个 Message 对象。这个对象包含了我们在将来需要处理的内容。
   > 2. **Handler** 接下来我们会通过 Handler 对象来把这个 Message 对象加入到任务队列中。Handler 主要有两个功能：一是添加任务到任务队列，二是处理任务队列中的任务。Handler 依赖于它被创建时所在的线程的 Looper 对象。
   > 3. **MessageQueue** MessageQueue 是一个等待处理的Message对象的队列。它由 Looper 维护，Handler 添加任务就是向 MessageQueue 中添加 Message 对象。
   > 4. **Looper** Looper 是一个循环，它从 MessageQueue 中取出 Message 并通过 Handler 进行处理。每个线程最多有一个 Looper 对象，主线程（UI 线程）会自动创建 Looper 对象，非主线程需要手动创建 Looper 对象。并在异步操作完毕之后，发送消息给 Looper，这时Looper 把要处理的消息交给相应的 Handler 进行处理。
   >
   > 
   >
   > 总结一下流程就是：当我们需要执行一个新的异步任务时，我们创建一个 Message 对象，然后通过 Handler 对象把这个 Message 发送到 MessageQueue。Looper 在循环中从 MessageQueue 中取出 Message，交给相应的 Handler 进行处理。这就是在 Android 中异步消息处理的基本流程。
   >
   > 
   >
   > 在Android系统中，Looper的循环运行、取出MessageQueue中的Message交给对应的Handler处理的过程是在Looper的loop()方法中进行的。这一部分通常被看做是Android系统完成的，开发者负责的是创建Message、Handler以及调用Handler的sendMessage方法将Message放入队列等操作。
   >
   > 
   >
   > 在之前的代码中，handler.sendMessage(msg)将Message放入MessageQueue队列。然后由运行在主线程的Looper自动将MessageQueue中的Message提取出来并交给对应的Handler处理。
   >
   > 
   >
   > Looper.loop()方法的大概运行逻辑如下（这个一般在Thread中被调用）：
   >
   > 
   >
   > ```kotlin
   > while (true) {
   >         val msg = queue.next(); // 从MessageQueue中获取Message
   >         if (msg == null) {
   >             // 如果Message为null，代表MessageQueue已经无消息，退出循环
   >             return;
   >         }
   >         // 调用Handler的handleMessage进行处理
   >         msg.target.handleMessage(msg);
   >         msg.recycleUnchecked();
   > }
   > ```
   >
   > 
   >
   > 在上述代码中，每生成一个Message对象，就调用Handler的handleMessage方法进行处理。在之前代码中，msg.what值为updateText时，就会执行mainBinding.textView.text = "Nice to Meet U"这个操作。

### 2.4 使用AsyncTask

为了更加方便我们在子线程中对UI进行操作，Android还提供了另外一些好用的工具，比如AsyncTask。借助AsyncTask，即使你对异步消息处理机制完全不了解，也可以十分简单地从子线程切换到主线程。当然，AsyncTask背后的实现原理也是基于异步消息处理机制的，只是Android帮我们做了很好的封装而已。

首先来看一下AsyncTask的基本用法。由于AsyncTask是一个抽象类，所以如果我们想使用它，就必须创建一个子类去继承它。在继承时我们可以为AsyncTask类指定3个泛型参数，这3个参数的用途如下。

- Params。在执行AsyncTask时需要传入的参数，可用于在后台任务中使用。
- Progress。在后台任务执行时，如果需要在界面上显示当前的进度，则使用这里指定的泛型作为进度单位。
- Result。当任务执行完毕后，如果需要对结果进行返回，则使用这里指定的泛型作为返回值类型。

因此，一个最简单的自定义AsyncTask就可以写成如下形式：

```kotlin
class DownloadTask : AsyncTask<Unit, Int, Boolean>() {
    ...
}
```

这里我们把AsyncTask的第一个泛型参数指定为Unit，表示在执行AsyncTask的时候不需要传入参数给后台任务。第二个泛型参数指定为Int，表示使用整型数据来作为进度显示单位。第三个泛型参数指定为Boolean，则表示使用布尔型数据来反馈执行结果。

当然，目前我们自定义的DownloadTask还是一个空任务，并不能进行任何实际的操作，我们还需要重写AsyncTask中的几个方法才能完成对任务的定制。经常需要重写的方法有以下4个。

1. **onPreExecute()**

   这个方法会在后台任务开始执行之前调用，用于进行一些界面上的初始化操作，比如显示
   一个进度条对话框等。

2. **doInBackground(Params...)**

   **这个方法中的所有代码都会在子线程中运行，我们应该在这里去处理所有的耗时任务。**任务一旦完成，就可以通过return语句将任务的执行结果返回，如果AsyncTask的第三个泛型参数指定的是Unit，就可以不返回任务执行结果。**注意，在这个方法中是不可以进行UI操作的，如果需要更新UI元素，比如说反馈当前任务的执行进度，可以调用publishProgress (Progress...)方法来完成。**

3. **onProgressUpdate(Progress...)**

   **当在后台任务中调用了publishProgress(Progress...)方法后，onProgressUpdate (Progress...)方法就会很快被调用，该方法中携带的参数就是在后台任务中传递过来的。**在这个方法中可以对UI进行操作，利用参数中的数值就可以对界面元素进行相应的更新。

4. **onPostExecute(Result)**

   **当后台任务执行完毕并通过return语句进行返回时，这个方法就很快会被调用。**返回的数据会作为参数传递到此方法中，可以利用返回的数据进行一些UI操作，比如说提醒任务执行的结果，以及关闭进度条对话框等。

   因此，一个比较完整的自定义AsyncTask就可以写成如下形式：

   ```kotlin
   class DownloadTask : AsyncTask<Unit, Int, Boolean>() {
       override fun onPreExecute() {
           progressDialog.show() // 显示进度对话框
       }
   
       override fun doInBackground(vararg params: Unit?) = try {
           while(true) {
               val downloadPercent = doDownload() // 这是一个虚构的方法
               publishProgress(downloadPercent)
               if (downloadPercent >= 100) {
                   break
               }
           }
           true
       } catch (e: Exception) {
           false
       }
   
       override fun onProgressUpdate(vararg values: Int?) {
           // 在这里更新下载进度
           progressDialog.setMessage("Downloaded ${values[0]}")
       }
   
       override fun onPostExecute(result: Boolean) {
           progressDialog.dismiss() // 关闭对话框
           if (result) {
               // 下载成功后动作
           } else {
               // 下载失败后动作
           }
       }
   }
   ```

在这个DownloadTask中，我们在`doInBackground()`方法里**执行具体的下载任务**。**这个方法里的代码都是在子线程中运行的，因而不会影响主线程的运行**。注意，这里虚构了一个doDownload()方法，用于计算当前的下载进度并返回，我们假设这个方法已经存在了。在得到了当前的下载进度后，下面就该考虑如何把它显示到界面上了，**由于doInBackground()方法是在子线程中运行的，在这里肯定不能进行UI操作，所以我们可以调用publishProgress()方法并传入当前的下载进度，这样onProgressUpdate()方法就会很快被调用，在这里就可以进行UI操作了。**

当下载完成后，doInBackground()方法会返回一个布尔型变量，这样onPostExecute()方法就会很快被调用，这个方法也是在主线程中运行的。然后，在这里我们会根据下载的结果弹出相应的Toast提示，从而完成整个DownloadTask任务。

简单来说，使用AsyncTask的诀窍就是，**在doInBackground()方法中执行具体的耗时任务，在onProgressUpdate()方法中进行UI操作，在onPostExecute()方法中执行一些任务的收尾工作。**

如果想要启动这个任务，只需编写以下代码即可：

```kotlin
DownloadTask().execute()
```

当然，你也可以给`execute()`方法传入任意数量的参数，这些参数将会传递到`DownloadTask`的`doInBackground()`方法当中。

## 三、Service的基本用法

### 3.1 定义一个Service

新建一个`Service`，新建`Service`时的`Exported`属性表示是否将这个`Service`暴露给外部其他程序访问，`Enabled`属性表示是否启用这个`Service`。

观察一下`MyService`的代码：

```kotlin
package work.icu007.servicetest

import android.app.Service
import android.content.Intent
import android.os.IBinder

class MyService : Service() {

    override fun onBind(intent: Intent): IBinder {
        TODO("Return the communication channel to the service.")
    }
}
```

`MyService`是继承自系统的`Service`类的。目前`MyService`中可以算是空空如也，但有一个`onBind()`方法特别醒目。**这个方法是`Service`中唯一的抽象方法，所以必须在子类里实现。**

我们还可以重写Service中的另外一些方法，如下所示：

```kotlin
package work.icu007.servicetest

import android.app.Service
import android.content.Intent
import android.os.IBinder

class MyService : Service() {

    override fun onBind(intent: Intent): IBinder {
        TODO("Return the communication channel to the service.")
    }

    override fun onCreate() {
        super.onCreate()
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        return super.onStartCommand(intent, flags, startId)
    }

    override fun onDestroy() {
        super.onDestroy()
    }
}
```

这里我们又重写了`onCreate()`、`onStartCommand()`和`onDestroy()`这3个方法，它们是每个`Service`中最常用到的3个方法了。其中`onCreate()`方法会在`Service`创建的时候调用，`onStartCommand()`方法会在每次`Service`启动的时候调用，`onDestroy()`方法会在`Service`销毁的时候调用。

通常情况下，如果我们希望`Service`一旦启动就立刻去执行某个动作，就可以将逻辑写在`onStartCommand()`方法里。而当`Service`销毁时，我们又应该在`onDestroy()`方法中回收那些不再使用的资源。

另外需要注意，每一个`Service`都需要在`AndroidManifest.xml`文件中进行注册才能生效，这是Android四大组件共有的特点。

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.ServiceTest"
        tools:targetApi="31">
        <service
            android:name=".MyService"
            android:enabled="true"
            android:exported="true"></service>

        ...
    </application>

</manifest>
```

### 3.2 启动和停止Service

启动和停止Service主要是借助Intent来实现的。下面来尝试一下,在布局文件中新增两个button。分别用于启动和停止Service：

```kotlin
package work.icu007.servicetest

import android.content.Intent
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import work.icu007.servicetest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)

        mainBinding.startServiceBtn.setOnClickListener {
            val intent = Intent(this, MyService::class.java)
            startService(intent)
        }

        mainBinding.stopServiceBtn.setOnClickListener {
            val intent = Intent(this, MyService::class.java)
            stopService(intent)
        }
    }
}
```

在“Start Service”按钮的点击事件里，我们构建了一个Intent对象，并调用`startService()`方法来启动`MyService`。在“Stop Service”按钮的点击事件里，我们同样构建了一个`Intent`对象，并调用`stopService()`方法来停止MyService。`startService()`和`stopService()`方法都是定义在`Context`类中的，所以我们在`Activity`里可以直接调用这两个方法。另外，`Service`也可以自我停止运行，只需要在`Service`内部调用`stopSelf()`方法即可。

### 3.3 Activity和Service进行通信

我们在`Activity`里调用了`startService()`方法来启动`MyService`，然后`MyService`的`onCreate()`和`onStartCommand()`方法就会得到执行。之后`Service`会一直处于运行状态，但具体运行的是什么逻辑，`Activity`就控制不了了。这就类似于`Activity`通知了`Service`一下：“你可以启动了！”然后`Service`就去忙自己的事情了，但`Activity`并不知道`Service`到底做了什么事情，以及完成得如何。

那么可不可以让`Activity`和`Service`的关系更紧密一些呢？例如在`Activity`中指挥`Service`去干什么，`Service`就去干什么。当然可以，这就需要借助我们刚刚忽略的`onBind()`方法了。

比如说，目前我们希望在`MyService`里提供一个下载功能，然后在`Activity`中可以决定何时开始下载，以及随时查看下载进度。实现这个功能的思路是创建一个专门的`Binder`对象来对下载功能进行管理。修改`MyService`中的代码，如下所示：

```kotlin
package work.icu007.servicetest

import android.app.Service
import android.content.Intent
import android.os.Binder
import android.os.IBinder
import android.util.Log

class MyService : Service() {

    private val mBinder = DownloadBinder()

    class DownloadBinder : Binder() {
        fun startDownload() {
            Log.d(TAG, "startDownload: executed")
        }

        fun getProgress() : Int {
            Log.d(TAG, "getProgress: executed")
            return 0
        }
    }

    override fun onBind(intent: Intent): IBinder {
        return mBinder
        TODO("Return the communication channel to the service.")
    }

    override fun onCreate() {
        super.onCreate()
        Log.d(TAG, "onCreate: executed")
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        Log.d(TAG, "onStartCommand: executed")
        return super.onStartCommand(intent, flags, startId)
    }

    override fun onDestroy() {
        Log.d(TAG, "onDestroy: executed")
        super.onDestroy()
    }

    companion object {
        var TAG: String = MyService.javaClass.simpleName
    }
}
```

可以看到，这里我们新建了一个`DownloadBinder`类，并让它继承自`Binder`，然后在它的内部提供了开始下载以及查看下载进度的方法。当然这只是两个模拟方法，并没有实现真正的功能，我们在这两个方法中分别打印了一行日志。

接着，在`MyService`中创建了`DownloadBinder`的实例，然后在`onBind()`方法里返回了这个实例，这样`MyService`中的工作就全部完成了。

下面就要看一看在`Activity`中如何调用`Service`里的这些方法了。修改代码，如下所示：

```kotlin
package work.icu007.servicetest

import android.content.ComponentName
import android.content.Context
import android.content.Intent
import android.content.ServiceConnection
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.os.IBinder
import work.icu007.servicetest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    lateinit var downloadBinder : MyService.DownloadBinder
    private val connection = object : ServiceConnection {
        override fun onServiceConnected(name: ComponentName?, service: IBinder?) {
            downloadBinder = service as MyService.DownloadBinder
            downloadBinder.startDownload()
            downloadBinder.getProgress()
        }

        override fun onServiceDisconnected(name: ComponentName?) {
            // todo
        }
    }
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)


        mainBinding.startServiceBtn.setOnClickListener {
            val intent = Intent(this, MyService::class.java)
            startService(intent)
        }

        mainBinding.stopServiceBtn.setOnClickListener {
            val intent = Intent(this, MyService::class.java)
            stopService(intent)
        }

        mainBinding.bindServiceBtn.setOnClickListener {
            val intent = Intent(this, MyService::class.java)
            bindService(intent, connection, Context.BIND_AUTO_CREATE)
        }

        mainBinding.unbindServiceBtn.setOnClickListener {
            unbindService(connection)
        }
    }
}
```

这里我们首先创建了一个`ServiceConnection`的匿名类实现，并在里面重写了`onServiceConnected()`方法和`onServiceDisconnected()`方法。`onServiceConnected()`方法方法会在`Activity`与`Service`成功绑定的时候调用，而`onServiceDisconnected()`方法只有**在Service的创建进程崩溃或者被杀掉的时候才会调用**，这个方法不太常用。那么在`onServiceConnected()`方法中，我们又通过向下转型得到了`DownloadBinder`的实例，有了这个实例，`Activity`和`Service`之间的关系就变得非常紧密了。**现在我们可以在`Activity`中根据具体的场景来调用`DownloadBinder`中的任何public方法，即实现了指挥`Service`干什么`Service`就去干什么的功能。**这里仍然只是做了个简单的测试，在`onServiceConnected()`方法中调用了`DownloadBinder`的`startDownload()`和`getProgress()`方法。

当然，现在`Activity`和`Service`其实还没进行绑定呢，这个功能是在“`Bind Service`”按钮的点击事件里完成的。可以看到，这里我们仍然构建了一个`Intent`对象，然后调用`bindService()`方法将`MainActivity`和`MyService`进行绑定。`bindService()`方法接收3个参数，**第一个参数就是刚刚构建出的Intent对象，第二个参数是前面创建出的`ServiceConnection`的实例，第三个参数则是一个标志位，这里传入`BIND_AUTO_CREATE`表示在Activity和Service进行绑定后自动创建Service。这会使得MyService中的`onCreate()`方法得到执行，但`onStartCommand()`方法不会执行。**

如果我们想解除Activity和Service之间的绑定该怎么办呢？调用一下`unbindService()`方法就可以了，这也是“Unbind Service”按钮的点击事件里实现的功能。

### 3.4 Service的生命周期

之前有了解过**Activity**以及**Fragment**的生命周期。类似地，**Service**也有自己的生命周期，前面我们使用到的`onCreate()`、`onStartCommand()`、`onBind()`和`onDestroy()`等方法都是在Service的生命周期内可能回调的方法。

一旦在项目的任何位置调用了**Context**的`startService()`方法，相应的**Service**就会启动，并回调`onStartCommand()`方法。如果这个**Service**之前还没有创建过，`onCreate()`方法会先于`onStartCommand()`方法执行。**Service启动了之后会一直保持运行状态，直到`stopService()`或`stopSelf()`方法被调用，或者被系统回收。**注意，**虽然每调用一次`startService()`方法，`onStartCommand()`就会执行一次，但实际上每个Service只会存在一个实例。**所以不管你调用了多少次`startService()`方法，只需调用一次`stopService()`或`stopSelf()`方法，`Service`就会停止。另外，**还可以调用Context的`bindService()`来获取一个Service的持久连接，这时就会回调Service中的`onBind()`方法。类似地，如果这个Service之前还没有创建过，`onCreate()`方法会先于`onBind()`方法执行。之后，调用方可以获取到`onBind()`方法里返回的IBinder对象的实例，这样就能自由地和Service进行通信了。**只要调用方和Service之间的连接没有断开，Service就会一直保持运行状态，直到被系统回收。

当调用了`startService()`方法后，再去调用`stopService()`方法。这时**Service**中的`onDestroy()`方法就会执行，表示**Service**已经销毁了。类似地，当调用了`bindService()`方法后，再去调用`unbindService()`方法，`onDestroy()`方法也会执行，这两种情况都很好理解。但是需要注意，**我们是完全有可能对一个Service既调用了`startService()`方法，又调用了`bindService()`方法的，在这种情况下该如何让Service销毁呢？根据Android系统的机制，一个Service只要被启动或者被绑定了之后，就会处于运行状态，必须要让以上两种条件同时不满足，Service才能被销毁。所以，这种情况下要同时调用`stopService()`和`unbindService()`方法，`onDestroy()`方法才会执行。**

### 3.5 Service的更多技巧

#### 3.5.1 使用前台Service

**从Android 8.0系统开始，只有当应用保持在前台可见状态的情况下，Service才能保证稳定运行，一旦应用进入后台之后，Service随时都有可能被系统回收。**而如果你希望**Service**能够一直保持运行状态，就可以考虑使用前台**Service**。前台**Service**和普通**Service**最大的区别就在于，它一直会有一个正在运行的图标在系统的状态栏显示，下拉状态栏后可以看到更加详细的信息，非常类似于通知的效果。

来看一下如何才能创建一个前台`Service`，其实并不复杂，修改`MyService`中的代码，如下所示：

```kotlin
package work.icu007.servicetest

import android.app.NotificationChannel
import android.app.NotificationManager
import android.app.PendingIntent
import android.app.Service
import android.content.Context
import android.content.Intent
import android.graphics.BitmapFactory
import android.os.Binder
import android.os.Build
import android.os.IBinder
import android.util.Log
import androidx.core.app.NotificationCompat

class MyService : Service() {

    private val mBinder = DownloadBinder()

    class DownloadBinder : Binder() {
        fun startDownload() {
            Log.d(TAG, "startDownload: executed")
        }

        fun getProgress() : Int {
            Log.d(TAG, "getProgress: executed")
            return 0
        }
    }

    override fun onBind(intent: Intent): IBinder {
        return mBinder
        TODO("Return the communication channel to the service.")
    }

    override fun onCreate() {
        super.onCreate()
        Log.d(TAG, "onCreate: executed")
        val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            val channel = NotificationChannel("my_service", "前台Service通知", NotificationManager.IMPORTANCE_DEFAULT)
            manager.createNotificationChannel(channel)
        }
        val intent = Intent(this, MainActivity::class.java)
        // A12 之后必须指定最后一个参数 flags 为 PendingIntent.FLAG_UPDATE_CURRENT or PendingIntent.FLAG_IMMUTABLE
        val pi = PendingIntent.getActivity(this, 0, intent, PendingIntent.FLAG_IMMUTABLE)
        val notification = NotificationCompat.Builder(this, "my_service")
            .setContentTitle("this is content title")
            .setContentText("this is content text")
            .setSmallIcon(R.drawable.ic_launcher_background)
            .setLargeIcon(BitmapFactory.decodeResource(resources, R.drawable.ic_launcher_foreground))
            .setContentIntent(pi)
            .build()
        startForeground(1, notification)

    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        Log.d(TAG, "onStartCommand: executed")
        return super.onStartCommand(intent, flags, startId)
    }

    override fun onDestroy() {
        Log.d(TAG, "onDestroy: executed")
        super.onDestroy()
    }

    companion object {
        var TAG: String = MyService.javaClass.simpleName
    }
}
```

这里在`onCreate()`方法中创建了通知，但是构建**Notification**对象后并没有使用**NotificationManager**将通知显示出来，而是调用了`startForeground()`方法。这个方法接收两个参数：第一个参数是通知的id，类似于`notify()`方法的第一个参数；第二个参数则是构建的**Notification**对象。调用`startForeground()`方法后就会让**MyService**变成一个前台Service，并在系统状态栏显示出来。

另外，从Android 9.0系统开始，使用前台**Service**必须在`AndroidManifest.xml`文件中进行权限声明才行，通知权限也需要申请，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>
    <uses-permission android:name="android.permission.POST_NOTIFICATIONS" />
    <uses-permission
        android:name="android.permission.BIND_NOTIFICATION_LISTENER_SERVICE"
        tools:ignore="ProtectedPermissions" />

    ...

</manifest>
```

还可以在**MainActivity**中申请权限：

```kotlin
package work.icu007.servicetest

import android.content.ComponentName
import android.content.Context
import android.content.Intent
import android.content.ServiceConnection
import android.content.pm.PackageManager
import android.net.Uri
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.os.IBinder
import android.provider.Settings
import androidx.appcompat.app.AlertDialog
import androidx.core.app.ActivityCompat
import androidx.core.app.NotificationManagerCompat
import androidx.core.content.ContextCompat
import work.icu007.servicetest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    ...
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        if (!NotificationManagerCompat.from(this).areNotificationsEnabled()) {
            // 如果没有权限，向用户显示一个对话框，引导他们去设置中开启
            AlertDialog.Builder(this)
                .setTitle("需要通知权限")
                .setMessage("此应用需要您允许通知权限才能正常工作。请点击“设置”按钮，然后在设置中开启通知权限。")
                .setPositiveButton("设置") { _, _ ->
                    val intent = Intent(Settings.ACTION_APPLICATION_DETAILS_SETTINGS)
                    val uri = Uri.fromParts("package", packageName, null)
                    intent.data = uri
                    startActivity(intent)
                }
                .setNegativeButton("取消") { _, _ ->
                    /*这个方法需要两个参数（一个 DialogInterface 对象和一个表示哪个按钮被点击的 int 值），
                    但是我们不关心这两个参数的具体值，所以我们用下划线 _ 来代替它们*/
                    // 用户点击取消，结束应用
                    finish()
                }
                .show()
        } else {
            mainBinding.startServiceBtn.setOnClickListener {
                val intent = Intent(this, MyService::class.java)
                startService(intent)
            }
        }

        ...
    }
}
```

#### 3.5.2 使用IntentService

**Service**中的代码都是默认运行在主线程当中的，如果直接在**Service**里处理一些耗时的逻辑，就很容易出现**ANR**（**Application Not Responding**）的情况。所以这个时候就需要用到**Android**多线程编程的技术了，我们应该在**Service**的每个具体的方法里开启一个子线程，然后在这里处理那些耗时的逻辑。因此，一个比较标准的**Service**就可以写成如下形式：

```kotlin
class MyService : Service() {
    ...
    override fun onStartCommand(intent: Intent, flags: Int, startId: Int): Int {
        thread {
            // 处理具体的逻辑
        }
        return super.onStartCommand(intent, flags, startId)
    }
}
```

但是，这种**Service**一旦启动，就会一直处于运行状态，必须调用`stopService()`或`stopSelf()`方法，或者被系统回收，**Service**才会停止。所以，如果想要实现让一个**Service**在执行完毕后自动停止的功能，就可以这样写：

```kotlin
class MyService : Service() {
    ...
    override fun onStartCommand(intent: Intent, flags : Int, startId: Int): Int {
        thread {
            // 处理具体的逻辑
            stopSelf()
        }
        return super.onStartCommand(intent, flags, startId)
    }
}
```

虽说这种写法并不复杂，但是总会有一些程序员忘记开启线程，或者忘记调用`stopSelf()`方法。为了可以简单地创建一个异步的、会自动停止的**Service**，**Android**专门提供了一个**IntentService**类，这个类就很好地解决了前面所提到的两种尴尬，下面我们就来看一下它的用法。
新建一个**MyIntentService**类继承自**IntentService**，代码如下所示：

```kotlin
class MyIntentService : IntentService("MyIntentService") {
    override fun onHandleIntent(intent: Intent?) {
        // 打印当前线程id
        Log.d("MyIntentService", "Thread id is ${Thread.currentThread().name}")
    }
    
    override fun onDestroy() {
        super.onDestroy()
        Log.d("MyIntentService", "onDestroy executed")
    }
}
```

这里首先要求必须先调用父类的构造函数，并传入一个字符串，这个字符串可以随意指定，只在调试的时候有用。然后要在子类中实现`onHandleIntent()`这个抽象方法，这个方法中可以处理一些耗时的逻辑，而不用担心**ANR**的问题，因为这个方法已经是在子线程中运行的了。这里为了证实一下，我们在`onHandleIntent()`方法中打印了当前线程名。另外，根据**IntentService**的特性，这个**Service**在运行结束后应该是会自动停止的，所以我们又重写了`onDestroy()`方法，在这里也打印了一行日志，以证实**Service**是不是停止了。

新增一个**Button**用于启动**MyIntentService**：

```kotlin
package work.icu007.servicetest

import android.content.ComponentName
import android.content.Context
import android.content.Intent
import android.content.ServiceConnection
import android.content.pm.PackageManager
import android.net.Uri
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.os.IBinder
import android.provider.Settings
import android.util.Log
import androidx.appcompat.app.AlertDialog
import androidx.core.app.ActivityCompat
import androidx.core.app.NotificationManagerCompat
import androidx.core.content.ContextCompat
import work.icu007.servicetest.databinding.ActivityMainBinding
private const val TAG = "MainActivity"
class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    ...
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        ...

        mainBinding.startIntentServiceBtn.setOnClickListener {
            Log.d(TAG, "onCreate: Thread id is ${Thread.currentThread().name}")
            val intent = Intent(this, MyIntentService::class.java)
            startService(intent)
        }
    }
}
```

观察日志打印：

```tex
2024-01-24 09:32:15.416 26117-26117 MainActivity            work.icu007.servicetest              D  onCreate: Thread id is main
2024-01-24 09:32:15.456 26117-26152 MyIntentService         work.icu007.servicetest              D  onHandleIntent: Thread id is IntentService[MyIntentService]
2024-01-24 09:32:15.466 26117-26117 MyIntentService         work.icu007.servicetest              D  onDestroy: executed
```


可以看到，不仅**MyIntentService**和**MainActivity**所在的线程名不一样，而且`onDestroy()`方法也得到了执行，说明**MyIntentService**在运行完毕后确实自动停止了。集开启线程和自动停止于一身，**IntentService**还是博得了不少程序员的喜爱。

