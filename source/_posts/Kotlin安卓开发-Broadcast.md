---
title: Kotlin安卓开发-Broadcast
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
swiper_index: 1
top_group_index: 1
background: '#fff'
cover: >-
  https://pic2.ziyuan.wang/user/xiheya/2024/05/Android-Broadcast_e002cd5cd46cc.jpg
ai:
  - >-
    这篇文章是关于Android开发中广播机制的介绍。在Android中，每个应用程序都可以注册自己感兴趣的广播，这些广播可能来自系统，也可能来自其他应用程序。Android提供了一套完整的API，允许应用程序自由地发送和接收广播。文章主要介绍了两种类型的广播：标准广播和有序广播。标准广播是一种完全异步执行的广播，所有的BroadcastReceiver几乎会在同一时刻收到这条广播消息，因此它们之间没有任何先后顺序可言。这种广播的效率会比较高，但同时也意味着它是无法被截断的。有序广播则是一种同步执行的广播，同一时刻只会有一个BroadcastReceiver能够收到这条广播消息，当这个BroadcastReceiver中的逻辑执行完毕后，广播才会继续传递。所以此时的BroadcastReceiver是有先后序的，优先级高的BroadcastReceiver就可以先收到广播消息，并且前面的BroadcastReceiver还可以截断正在传递的广播，这样后面的BroadcastReceiver就无法收到广播消息了。文章的下一部分将介绍如何接收系统广播。
abbrlink: f2287651
date: 2023-11-28 15:11:36
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


## 一、广播机制简介

Android中的每个应用程序都可以对自己感兴趣的广播进行注册，这样该程序就只会收到自己所关心的广播内容，这些广播可能是来自于系统的，也可能是来自于其他应用程序的。Android提供了一套完整的API，允许应用程序自由地发送和接收广播。发送广播可以借助之前学习过的Intent，而接收广播的方法则需要引入一个新的概念——BroadcastReceiver。

这里我们先来了解一下广播的类型，Android中的广播主要可以分为两种类型：标准广播和有序广播。

- 标准广播（normal broadcasts）：是一种完全异步执行的广播，在广播发出之后，所有的BroadcastReceiver几乎会在同一时刻收到这条广播消息，因此它们之间没有任何先后顺序可言。这种广播的效率会比较高，但同时也意味着它是无法被截断的。

![1700625229306.png](https://pic.ziyuan.wang/2023/11/22/xiheya_6318f129cb33c.png)

- 有序广播（ordered broadcasts）则是一种同步执行的广播，在广播发出之后，同一时刻只会有一个BroadcastReceiver能够收到这条广播消息，当这个BroadcastReceiver中的逻辑执行完毕后，广播才会继续传递。所以此时的BroadcastReceiver是有先后顺序的，优先级高的BroadcastReceiver就可以先收到广播消息，并且前面的BroadcastReceiver还可以截断正在传递的广播，这样后面的BroadcastReceiver就无法收到广播消息了。

![1700625787026.png](https://pic.ziyuan.wang/2023/11/22/xiheya_ffd533f3040a9.png)

## 二、接收系统广播

Android内置了很多系统级别的广播，我们可以在应用程序中通过监听这些广播来得到各种系统的状态信息。比如手机开机完成后会发出一条广播，电池的电量发生变化会发出一条广播，系统时间发生改变也会发出一条广播，等等。如果想要接收这些广播，就需要使用BroadcastReceiver，下面我们就来看一下它的具体用法。

### 2.1 动态注册监听时间变化

我们可以根据自己感兴趣的广播，自由地注册BroadcastReceiver，这样当有相应的广播发出时，相应的BroadcastReceiver就能够收到该广播，并可以在内部进行逻辑处理。注册BroadcastReceiver的方式一般有两种：在代码中注册（**动态注册**）和在AndroidManifest.xml中注册（**静态注册**）。其中前者也被称为动态注册，后者也被称为静态注册。

该如何创建一个BroadcastReceiver呢？其实只需新建一个类，让它继承自BroadcastReceiver，并重写父类的onReceive()方法就行了。这样当有广播到来时，onReceive()方法就会得到执行，具体的逻辑就可以在这个方法中处理。

```kotlin
package work.icu007.broadcasttest

import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.content.IntentFilter
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Toast
import work.icu007.broadcasttest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var activityMainBinding: ActivityMainBinding
    private lateinit var timeChangeReceiver: TimeChangeReceiver
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        activityMainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(activityMainBinding.root)

        val intentFilter = IntentFilter()
        intentFilter.addAction("android.intent.action.TIME_TICK")
        timeChangeReceiver = TimeChangeReceiver()
        registerReceiver(timeChangeReceiver,intentFilter)
    }

    override fun onDestroy() {
        super.onDestroy()
        unregisterReceiver(timeChangeReceiver)
    }

    inner class TimeChangeReceiver: BroadcastReceiver(){
        override fun onReceive(context: Context?, intent: Intent?) {
            Toast.makeText(context, "Time has changed", Toast.LENGTH_SHORT).show()
        }

    }
}
```

我们在MainActivity中定义了一个内部类TimeChangeReceiver，这个类是继承自BroadcastReceiver的，并重写了父类的onReceive()方法。这样每当系统时间发生变化时，onReceive()方法就会得到执行，这里只是简单地使用Toast提示了一段文本信息。然后观察onCreate()方法，首先我们创建了一个IntentFilter的实例，并给它添加了一个值为android.intent.action.TIME_TICK的action，为什么要添加这个值呢？因为当系统时间发生变化时，系统发出的正是一条值为android.intent.action.TIME_TICK的广播，也就是说我们的BroadcastReceiver想要监听什么广播，就在这里添加相应的action。接下来创建了一个TimeChangeReceiver的实例，然后调用registerReceiver()方法进行注册，将TimeChangeReceiver的实例和IntentFilter的实例都传了进去，这样TimeChangeReceiver就会收到所有值为android.intent.action.TIME_TICK的广播，也就实现了监听系统时间变化的功能。

动态注册的BroadcastReceiver一定要取消注册才行，一般会选择在onDestroy()方法中通过调用unregisterReceiver()方法来实现取消注册。

系统每隔一分钟就会发出一条android.intent.action.TIME_TICK的广播，因此我们最多只需要等待一分钟就可以收到这条广播了。

查看完整的系统广播列表，可以到如下的路径中去查看：

> `<Android SDK>/platforms/<任意android api版本>/data/broadcast_actions.txt`

### 2.2 静态实现开机启动

动态注册的BroadcastReceiver可以自由地控制注册与注销，在灵活性方面有很大的优势。但是它存在着一个缺点，即必须在程序启动之后才能接收广播，因为注册的逻辑是写在onCreate()方法中的。如果我们需要实现在程序未启动时也接收广播，我们就需要使用静态注册的方式。

其实从理论上来说，动态注册能监听到的系统广播，静态注册也应该能监听到，在过去的Android系统中确实是这样的。但是由于大量恶意的应用程序利用这个机制在程序未启动的情况下监听系统广播，从而使任何应用都可以频繁地从后台被唤醒，严重影响了用户手机的电量和性能，因此Android系统几乎每个版本都在削减静态注册BroadcastReceiver的功能。

在Android 8.0系统之后，所有隐式广播都不允许使用静态注册的方式来接收了。隐式广播指的是那些没有具体指定发送给哪个应用程序的广播，大多数系统广播属于隐式广播，但是少数特殊的系统广播目前仍然允许使用静态注册的方式来接收。这些特殊的系统广播列表详见： [允许使用静态注册的广播](https://developer.android.google.cn/guide/components/broadcastexceptions.)。

在这些特殊的系统广播当中，有一条值为android.intent.action.BOOT_COMPLETED的广播，这是一条开机广播。

这里我们准备实现一个开机启动的功能。在开机的时候，我们的应用程序肯定是没有启动的，因此这个功能显然不能使用动态注册的方式来实现，而应该使用静态注册的方式来接收开机广播，然后在onReceive()方法里执行相应的逻辑，这样就可以实现开机启动的功能了。

右击work.icu007.broadcasttest包→New→Other→Broadcast Receiver.将创建的类命名为BootCompleteReceiver. Exported属性表示是否允许这个BroadcastReceiver接收本程序以外的广播，Enabled属性表示是否启用这个BroadcastReceiver。勾选这两个属性，点击“Finish”完成创建。

修改BootCompleteReceiver中的代码，如下所示：

```kotlin
package work.icu007.broadcasttest

import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.util.Log
import android.widget.Toast

class BootCompleteReceiver : BroadcastReceiver() {

    override fun onReceive(context: Context, intent: Intent) {
        // This method is called when the BroadcastReceiver is receiving an Intent broadcast.
        TODO("BootCompleteReceiver.onReceive() is not implemented")
        val TAG = "BootCompleteReceiver"
        Log.d(TAG, "onReceive: boot complete")
        Toast.makeText(context, "Boot Complete", Toast.LENGTH_SHORT).show()
    }
}
```

另外，静态的BroadcastReceiver一定要在AndroidManifest.xml文件中注册才可以使用。不过，由于我们是使用Android Studio的快捷方式创建的BroadcastReceiver，因此注册这一步已经自动完成了。

目前的BootCompleteReceiver是无法收到开机广播的，因为我们还需要对AndroidManifest.xml文件进行修改，如下所示：

```xml
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />

<receiver
    android:name=".BootCompleteReceiver"
    android:enabled="true"
    android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.BOOT_COMPLETED"/>
    </intent-filter>
</receiver>
```

由于Android系统启动完成后会发出一条值为android.intent.action.BOOT_COMPLETED的广播，因此我们在<receiver>标签中又添加了一个<intent-filter>标签，并在里面声明了相应的action。

另外，这里有非常重要的一点需要说明。Android 系统为了保护用户设备的安全和隐私，做了严格的规定：如果程序需要进行一些对用户来说比较敏感的操作，必须在AndroidManifest.xml文件中进行权限声明，否则程序将会直接崩溃。比如这里接收系统的开机广播就是需要进行权限声明的，所以我们在上述代码中使用<uses-permission>标签声明了android.permission.RECEIVE_BOOT_COMPLETED权限。

到目前为止，我们在BroadcastReceiver的onReceive()方法中只是简单地使用Toast提示了一段文本信息，当你真正在项目中使用它的时候，可以在里面编写自己的逻辑。需要注意的是，不要在onReceive()方法中添加过多的逻辑或者进行任何的耗时操作，因为BroadcastReceiver中是不允许开启线程的，当onReceive()方法运行了较长时间而没有结束时，程序就会出现错误。

### 2.3 发送自定义广播

广播主要分为两种类型：标准广播和有序广播。现在通过实践的方式来看一下这两种广播具体的区别。

#### 2.3.1 发送标准广播

在发送广播之前，我们还是需要先定义一个BroadcastReceiver来准备接收此广播，不然发出去也是白发。因此新建一个MyBroadcastReceiver，并在onReceive()方法中加入如下代码：

```kotlin
package work.icu007.broadcasttest

import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.widget.Toast

class MyBroadcastReceiver : BroadcastReceiver() {

    override fun onReceive(context: Context, intent: Intent) {
        Toast.makeText(context, "received in MyBroadcastReceiver", Toast.LENGTH_SHORT).show()
        // This method is called when the BroadcastReceiver is receiving an Intent broadcast.
        TODO("BroadcastReceiver.onReceive() is not implemented")
    }
}
```

当MyBroadcastReceiver收到自定义的广播时，就会弹出“received in MyBroadcastReceiver”的提示。然后在AndroidManifest.xml中对这个BroadcastReceiver进行修改：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    ...
    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.BroadcastTest"
        tools:targetApi="31">
        <receiver
            android:name=".MyBroadcastReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
                <action android:name="work.icu007.broadcasttest.MY_BROADCAST"/>
            </intent-filter>
        </receiver>
        ...
    </application>

</manifest>
```

可以看到，这里让MyBroadcastReceiver接收一条值为work.icu007.broadcasttest.MY_BROADCAST的广播，因此待会儿在发送广播的时候，我们就需要发出这样的一条广播。

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var activityMainBinding: ActivityMainBinding
    private lateinit var timeChangeReceiver: TimeChangeReceiver
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        activityMainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(activityMainBinding.root)

        val intentFilter = IntentFilter()
        intentFilter.addAction("android.intent.action.TIME_TICK")
        timeChangeReceiver = TimeChangeReceiver()
        registerReceiver(timeChangeReceiver,intentFilter)
        activityMainBinding.btnSendBroadcast.setOnClickListener {
            val intent = Intent("work.icu007.broadcasttest.MY_BROADCAST")
            intent.setPackage(packageName)
            sendBroadcast(intent)
        }
    }
}
```

我们在按钮的点击事件里面加入了发送自定义广播的逻辑。首先构建了一个Intent对象，并把要发送的广播的值传入。然后调用Intent的setPackage()方法，并传入当前应用程序的包名。packageName是getPackageName()的语法糖写法，用于获取当前应用程序的包名。最后调用sendBroadcast()方法将广播发送出去，这样所有监听work.icu007.broadcasttest.MY_BROADCAST这条广播的BroadcastReceiver就会收到消息了。此时发出去的广播就是一条标准广播。

在Android8.0系统之后，静态注册的BroadcastReceiver是无法接收隐式广播的，而默认情况下我们发出的自定义广播恰恰都是隐式广播。因此这里一定要调用setPackage()方法，指定这条广播是发送给哪个应用程序的，从而让它变成一条显式广播，否则静态注册的BroadcastReceiver将无法接收到这条广播。

由于广播是使用Intent来发送的，因此我们还可以在Intent中携带一些数据传递给相应的BroadcastReceiver，这一点和Activity的用法是比较相似的。

#### 2.3.2 发送有序广播

和标准广播不同，有序广播是一种同步执行的广播，并且是可以被截断的。为了验证这一点，我们需要再创建一个新的BroadcastReceiver。新建AnotherBroadcastReceiver，代码如下所示：

```kotlin
package work.icu007.broadcasttest

import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.util.Log
import android.widget.Toast

class AnotherBroadcastReceiver : BroadcastReceiver() {

    override fun onReceive(context: Context, intent: Intent) {
        Toast.makeText(context, "received broadcast in AnotherBroadcastReceiver", Toast.LENGTH_SHORT).show()
        Log.d(TAG, "onReceive: ")
    }

    companion object{
        const val TAG = "AnotherBroadcastReceiver"
    }
}
```

然后在AndroidManifest.xml中对这个BroadcastReceiver进行修改：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    ...
    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.BroadcastTest"
        tools:targetApi="31">
        <receiver
            android:name=".AnotherBroadcastReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter android:priority="50">
                <action android:name="work.icu007.broadcasttest.MY_BROADCAST" />
            </intent-filter>
        </receiver>
        <receiver
            android:name=".MyBroadcastReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter>
                <action android:name="work.icu007.broadcasttest.MY_BROADCAST"/>
            </intent-filter>
        </receiver>
        ...
    </application>

</manifest>
```

AnotherBroadcastReceiver同样接收的是work.icu007.broadcasttest.MY_BROADCAST这条广播。

现在查岗你是发送广播的时候选择发送有序广播，如下：

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var activityMainBinding: ActivityMainBinding
    private lateinit var timeChangeReceiver: TimeChangeReceiver
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        activityMainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(activityMainBinding.root)

        val intentFilter = IntentFilter()
        intentFilter.addAction("android.intent.action.TIME_TICK")
        timeChangeReceiver = TimeChangeReceiver()
        registerReceiver(timeChangeReceiver,intentFilter)
        activityMainBinding.btnSendBroadcast.setOnClickListener {
            val intent = Intent("work.icu007.broadcasttest.MY_BROADCAST")
            intent.setPackage(packageName)
            sendOrderedBroadcast(intent,null)
//            sendBroadcast(intent)
        }
    }
}
```

发送有序广播只需要改动一行代码，即将sendBroadcast()方法改成sendOrderedBroadcast()方法。sendOrderedBroadcast()方法接收两个参数：第一个参数仍然是Intent；第二个参数是一个与权限相关的字符串，这里传入null就行了。现在重新运行程序，并点击“Send Broadcast”按钮，你会发现，两个BroadcastReceiver仍然都可以收到这条广播。

看上去好像和标准广播并没有什么区别嘛。不过别忘了，这个时候的BroadcastReceiver是有先后顺序的，而且前面的BroadcastReceiver还可以将广播截断，以阻止其继续传播。

该如何设定BroadcastReceiver的先后顺序呢？当然是在注册的时候进行设定了，修改AndroidManifest.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
    ...
    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.BroadcastTest"
        tools:targetApi="31">
        <receiver
            android:name=".AnotherBroadcastReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter android:priority="50">
                <action android:name="work.icu007.broadcasttest.MY_BROADCAST" />
            </intent-filter>
        </receiver>
        <receiver
            android:name=".MyBroadcastReceiver"
            android:enabled="true"
            android:exported="true">
            <intent-filter android:priority="50">
                <action android:name="work.icu007.broadcasttest.MY_BROADCAST"/>
            </intent-filter>
        </receiver>
        ...
    </application>

</manifest>
```

我们通过android:priority属性给BroadcastReceiver设置了优先级，优先级比较高的BroadcastReceiver就可以先收到广播。这里将MyBroadcastReceiver的优先级设成了100，以保证它一定会在AnotherBroadcastReceiver之前收到广播。

既然已经获得了接收广播的优先权，那么MyBroadcastReceiver就可以选择是否允许广播继续传递了。修改MyBroadcastReceiver中的代码，如下所示:

```kotlin
class MyBroadcastReceiver : BroadcastReceiver() {

    override fun onReceive(context: Context, intent: Intent) {
        Toast.makeText(context, "received in MyBroadcastReceiver", Toast.LENGTH_SHORT).show()
        Log.d(TAG, "onReceive: ")
        abortBroadcast()
    }

    companion object{
        const val TAG = "MyBroadcastReceiver"
    }
}
```

如果在onReceive()方法中调用了abortBroadcast()方法，就表示将这条广播截断，后面的BroadcastReceiver将无法再接收到这条广播。

## 三、实践-实现强制下线功能

我们完全可以借刚刚学的广播知识，非常轻松地实现强制下线这一功能。

强制下线功能需要先关闭所有的Activity，然后回到登录界面。如果你的反应足够快，应该会想到我们在第3章的最佳实践部分已经实现过关闭所有Activity的功能了，因此这里使用同样的方案即可。先创建一个ActivityCollector类用于管理所有的Activity，代码如下所示：

```kotlin
package work.icu007.broadcastbestpractice

import android.app.Activity


/*
 * Author: Charlie_Liao
 * Time: 2023/11/23-17:35
 * E-mail: rookie_l@icu007.work
 * manage all activity
 */

object ActivityCollector {
    private val activites = ArrayList<Activity>()

    // add activities to ArrayList
    fun addActivity(activity: Activity){
        activites.add(activity)
    }

    // remove activities from ArrayList
    fun removeActivity(activity: Activity){
        activites.remove(activity)
    }

    // finish all activities
    fun finishAll(){
        for (activity in activites){
            if (!activity.isFinishing){
                activity.finish()
            }
        }
        activites.clear()
    }
}
```

然后创建BaseActivity类作为所有Activity的父类，代码如下所示：

```kotlin
package work.icu007.broadcastbestpractice

import android.content.BroadcastReceiver
import android.content.Context
import android.content.Intent
import android.content.IntentFilter
import android.os.Bundle
import android.util.Log
import androidx.appcompat.app.AlertDialog
import androidx.appcompat.app.AppCompatActivity


/*
 * Author: Charlie_Liao
 * Time: 2023/11/23-17:40
 * E-mail: rookie_l@icu007.work
 */

open class BaseActivity : AppCompatActivity() {
    lateinit var receiver: ForceOfflineReceiver
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        ActivityCollector.addActivity(this)
    }

    override fun onDestroy() {
        super.onDestroy()
        ActivityCollector.removeActivity(this)
    }

    override fun onResume() {
        super.onResume()
        val intentFilter = IntentFilter()
        intentFilter.addAction("work.icu007.broadcastbestpractice.FORCE_OFFLINE")
        receiver = ForceOfflineReceiver()
        registerReceiver(receiver, intentFilter)
    }

    override fun onPause() {
        super.onPause()
        unregisterReceiver(receiver)
    }

    inner class ForceOfflineReceiver : BroadcastReceiver(){
        
        override fun onReceive(context: Context?, intent: Intent?) {
            Log.d(TAG, "onReceive: receive broadcast context == null: ${context == null}")
            if (context != null) {
                AlertDialog.Builder(context).apply {
                    setTitle("Warning")
                    setMessage("U are forced to be offline. please try to login again.")
                    setCancelable(false)
                    setPositiveButton("OK"){_,_ ->
                        ActivityCollector.finishAll()
                        val i = Intent(context,LoginActivity::class.java)
                        context.startActivity(i)
                    }
                    show()
                }
            }
        }

    }
    companion object{
        const val TAG = "BaseActivity"
    }
}
```

首先需要创建一个LoginActivity来作为登录界面，登录界面需要一个横向的LinearLayout，用于输入账号信息；还需要一个横向的LinearLayout，用于输入密码信息；最后创建一个登录按钮。

最后监听登录button：

```kotlin
package work.icu007.broadcastbestpractice

import android.content.Intent
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Toast
import work.icu007.broadcastbestpractice.databinding.ActivityLoginBinding

class LoginActivity : BaseActivity() {
    lateinit var activityLoginBinding: ActivityLoginBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        activityLoginBinding = ActivityLoginBinding.inflate(layoutInflater)
        setContentView(activityLoginBinding.root)

        activityLoginBinding.login.setOnClickListener {
            val account = activityLoginBinding.accountEdit.text.toString()
            val password = activityLoginBinding.passwordEdit.text.toString()
            if (account == "admin" && password == "123456"){
                val intent = Intent(this, MainActivity::class.java)
                startActivity(intent)
                finish()
            } else {
                Toast.makeText(this, "account or password is invalid", Toast.LENGTH_SHORT).show()
            }
        }
    }
}
```

首先将LoginActivity的继承结构改成继承自BaseActivity，然后在登录按钮的点击事件里对输入的账号和密码进行判断：如果账号是admin并且密码是123456，就认为登录成功并跳转到MainActivity，否则就提示用户账号或密码错误。

因此MainActivity就是登录成功后进入的程序主界面，我们只需要加入强制下线功能就可以了。修改activity_main.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/btnForceOffline"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Send force offline broadcast"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

只有一个按钮用于触发强制下线功能。然后修改MainActivity中的代码监听下线button

```kotlin
package work.icu007.broadcastbestpractice

import android.content.Intent
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import work.icu007.broadcastbestpractice.databinding.ActivityMainBinding

class MainActivity : BaseActivity() {
    lateinit var activityMainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        activityMainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(activityMainBinding.root)
        activityMainBinding.btnForceOffline.setOnClickListener {
            val intent = Intent("work.icu007.broadcastbestpractice.FORCE_OFFLINE")
            Log.d(TAG, "onCreate: sendBroadcast")
            sendBroadcast(intent)
        }
    }
    companion object{
        const val TAG = "MainActivity"
    }
}
```

我们在按钮的点击事件里发送了一条广播，广播的值为work.icu007.broadcastbestpractice.FORCE_OFFLINE，这条广播就是用于通知程序强制用户下线的。也就是说，强制用户下线的逻辑并不是写在MainActivity里的，而是应该写在接收这条广播的BroadcastReceiver里。这样强制下线的功能就不会依附于任何界面了，不管是在程序的任何地方，只要发出这样一条广播，就可以完成强制下线的操作了。那么毫无疑问，接下来我们就需要创建一个BroadcastReceiver来接收这条强制下线广播。唯一的问题就是，应该在哪里创建呢？由于BroadcastReceiver中需要弹出一个对话框来阻塞用户的正常操作，但如果创建的是一个静态注册的BroadcastReceiver，是没有办法在onReceive()方法里弹出对话框这样的UI控件的，而我们显然也不可能在每个Activity中都注册一个动态的BroadcastReceiver。

那么到底应该怎么办呢？答案其实很明显，只需要在BaseActivity中动态注册一个BroadcastReceiver就可以了

先来看一下ForceOfflineReceiver中的代码，这次onReceive()方法里可不再是仅仅弹出一个Toast了，而是加入了较多的代码，那我们就来仔细看看吧。首先是使用AlertDialog.Builder构建一个对话框。注意，这里一定要调用setCancelable()方法将对话框设为不可取消，否则用户按一下Back键就可以关闭对话框继续使用程序了。然后使用setPositiveButton()方法给对话框注册确定按钮，当用户点击了“OK”按钮时，就调用ActivityCollector的finishAll()方法销毁所有Activity，并重新启动LoginActivity。

再来看一下我们是怎么注册ForceOfflineReceiver这个BroadcastReceiver的。可以看到，这里重写了onResume()和onPause()这两个生命周期方法，然后分别在这两个方法里注册和取消注册了ForceOfflineReceiver。

为什么要这样写呢？之前不都是在onCreate()和onDestroy()方法里注册和取消注册BroadcastReceiver的吗？这是因为我们始终需要保证只有处于栈顶的Activity才能接收到这条强制下线广播，非栈顶的Activity不应该也没必要接收这条广播，所以写在onResume()和onPause()方法里就可以很好地解决这个问题，当一个Activity失去栈顶位置时就会自动取消BroadcastReceiver的注册。

最后还需要对AndroidManifest.xml文件进行修改，是将主Activity设置为LoginActivity，而不再是MainActivity，因为肯定不能在用户没登录的情况下就直接进入程序主界面。 