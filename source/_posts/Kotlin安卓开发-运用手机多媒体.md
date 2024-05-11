---
title: Kotlin安卓开发-运用手机多媒体
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
swiper_index: 1
top_group_index: 1
background: '#fff'
cover: 'https://pic2.ziyuan.wang/user/xiheya/2024/05/Media_539aad6787976.jpg'
ai:
  - >-
    这篇文章是关于Android开发中如何使用通知的教程。通知是Android系统中的一个特色功能，当应用程序希望向用户发出提示信息，而应用程序不在前台运行时，可以使用通知来实现。文章首先介绍了通知的基本概念，然后详细解释了如何在Android
    8.0及以上版本中创建通知渠道。通知渠道是Android
    8.0引入的一个新概念，每条通知都需要属于一个对应的渠道。应用程序可以创建自己的通知道，但用户有权控制这些通知渠道的重要程度，例如是否响铃、是否振动或者是否关闭这个渠道的通知。文章最后提供了一个示例，展示了如何获取NotificationManager的实例，这是创建通知渠道的第一步。
abbrlink: 3c95749
date: 2023-12-26 15:18:10
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


## 一、使用通知

通知（notification）是Android系统中比较有特色的一个功能，当某个应用程序希望向用户发出一些提示信息，而该应用程序又不在前台运行时，就可以借助通知来实现。发出一条通知后，手机最上方的状态栏中会显示一个通知的图标，下拉状态栏后可以看到通知的详细内容。Android的通知功能自推出以来就大获成功，连iOS系统也在5.0版本之后加入了类似的功能。

### 1.1 创建通知渠道

Android 8.0系统引入了通知渠道这个概念。什么是通知渠道呢？顾名思义，就是每条通知都要属于一个对应的渠道。每个应用程序都可以自由地创建当前应用拥有哪些通知渠道，但是这些通知渠道的控制权是掌握在用户手上的。用户可以自由地选择这些通知渠道的重要程度，是否响铃、是否振动或者是否要关闭这个渠道的通知。

拥有了这些控制权之后，用户就再也不用害怕那些垃圾通知的打扰了，因为用户可以自主地选择关心哪些通知、不关心哪些通知。

而我们的应用程序如果想要发出通知，也必须创建自己的通知渠道才行，下面我们就来学习一下创建通知渠道的详细步骤。

首先需要一个`NotificationManager`对通知进行管理，可以通过调用Context的`getSystemService()`方法获取。`getSystemService()`方法接收一个字符串参数用于确定获取系统的哪个服务，这里我们传入`Context.NOTIFICATION_SERVICE`即可。因此，获取`NotificationManager`的实例就可以写成：

```kotlin
val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
```

接下来要使用`NotificationChannel`类构建一个通知渠道，并调用`NotificationManager`的`createNotificationChannel()`方法完成创建。由于`NotificationChannel`类和`createNotificationChannel()`方法都是Android 8.0系统中新增的API，因此我们在使用的时候还需要进行版本判断才可以，写法如下：

```kotlin
if (Build.VERSION.SDK_INT >= Build.VERSION_CODE.O) {
    val channel = NotificationChannel(channelId, channelName, importance)
    manager.createNotificationChannel(channel)
}
```

创建一个通知渠道至少需要**渠道ID**、**渠道名称**以及**重要等级**这**3个参数**，其中**渠道ID可以随便定义，只要保证全局唯一性就可以**。**渠道名称是给用户看的，需要可以清楚地表达这个渠道的用途**。**通知的重要等级主要有IMPORTANCE_HIGH、IMPORTANCE_DEFAULT、IMPORTANCE_LOW、IMPORTANCE_MIN这几种，对应的重要程度依次从高到低**。不同的重要等级会决定通知的不同行为，后面我们会通过具体的例子进行演示。当然这里只是初始状态下的重要等级，用户可以随时手动更改某个通知渠道的重要等级，开发者是无法干预的。

下面来看一下通知的使用方法。通知的用法还是比较灵活的，既可以在`Activity`里创建，也可以在`BroadcastReceiver`里创建，当然还可以在后面我们即将学习的`Service`里创建。相比于`BroadcastReceiver`和`Service`，在`Activity`里创建通知的场景还是比较少的，因为一般只有当程序进入后台的时候才需要使用通知。

### 1.2 通知的基本用法

不过，无论是在哪里创建通知，整体的步骤都是相同的，下面我们就来看一下创建通知的详细步骤。

首先需要使用一个`Builder`构造器来创建`Notification`对象，但问题在于，Android系统的每一个版本都会对通知功能进行或多或少的修改，API不稳定的问题在通知上凸显得尤其严重，比方说刚刚介绍的通知渠道功能在Android 8.0系统之前就是没有的。那么该如何解决这个问题呢？其实解决方案我们之前已经见过好几回了，**就是使用`AndroidX`库中提供的兼容API。`AndroidX`库中提供了一个`NotificationCompat`类，使用这个类的构造器创建`Notification`对象，就可以保证我们的程序在所有Android系统版本上都能正常工作了，代码如下所示**：

```kotlin
val notification = NotificationCompat.Builder(context, channelId).build()
```

`NotificationCompat.Builder`的构造函数中接收两个参数：**第一个参数是context**，这个没什么好说的；**第二个参数是渠道ID**，需要和我们在创建通知渠道时指定的渠道ID相匹配才行。

当然，上述代码只是创建了一个空的Notification对象，并没有什么实际作用，我们可以在最终的`build()`方法之前连缀任意多的设置方法来创建一个丰富的Notification对象，先来看一些最基本的设置：

```kotlin
val notification = NotificationCompat.Builder(context, channelId)
.setContentTitle("this is content title").setContentText("This is content text")
.setSmallIcon(R.drawable.small_icon)
.setLargeIcon(BitmapFactory.decodeResource(getResources(),R.drawable.large_icon))
.build()
```

上述代码中一共调用了4个设置方法，下面我们来一一解析一下。`setContentTitle()`方法用于指定通知的标题内容，下拉系统状态栏就可以看到这部分内容。`setContentText()`方法用于指定通知的正文内容，同样下拉系统状态栏就可以看到这部分内容。`setSmallIcon()`方法用于设置通知的小图标，注意，只能使用纯alpha图层的图片进行设置，小图标会显示在系统状态栏上。`setLargeIcon()`方法用于设置通知的大图标，当下拉系统状态栏时，就可以看到设置的大图标了。

以上工作都完成之后，只需要调用`NotificationManager`的`notify()`方法就可以让通知显示出来了。`notify()`方法接收两个参数：**第一个参数是id**，要保证为每个通知指定的id都是不同的；**第二个参数则是`Notification`对象**，这里直接将我们刚刚创建好的Notification对象传入即可。因此，显示一个通知就可以写成：

```kotlin
manager.notify(1,notification)
```

下面就让我们通过一个具体的例子来看看通知到底是长什么样的。

```kotlin
package com.ubx.notificationtest

import android.app.NotificationChannel
import android.app.NotificationManager
import android.content.Context
import android.content.Intent
import android.graphics.BitmapFactory
import android.net.Uri
import android.os.Build
import android.os.Bundle
import android.provider.Settings
import androidx.appcompat.app.AlertDialog
import androidx.appcompat.app.AppCompatActivity
import androidx.core.app.NotificationCompat
import androidx.core.app.NotificationManagerCompat
import com.ubx.notificationtest.databinding.ActivityMainBinding
import kotlin.random.Random

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        // 获取NotificationManager实例
        val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        // 判断安卓版本是否为Android 8.0+
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            // 安卓版本为 8.0+ 则使用NotificationChannel类构建一个通知渠道，
            // 并调用NotificationManager的createNotificationChannel()方法完成创建
            val channel =
                NotificationChannel("normal", "Normal", NotificationManager.IMPORTANCE_DEFAULT)
            manager.createNotificationChannel(channel)
        }

        mainBinding.sendNotice.setOnClickListener {
            // 检查是否已经有了通知权限
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
                // 已经有权限，可以进行下一步操作
                // AndroidX库中提供了一个NotificationCompat类，使用这个类的构造器创建
                // Notification对象，就可以保证我们的程序在所有Android系统版本上都能正常工作了
                val notification = NotificationCompat.Builder(this, "normal")
                    // setContentTitle()方法用于指定通知的标题内容，下拉系统状态栏就可以看到这部分内容。
                    .setContentTitle("this is content title")
                    // setContentText()方法用于指定通知的正文内容，同样下拉系统状态栏就可以看到这部分内容。
                    .setContentText("this is content text".repeat(Random.nextInt(5, 10)))
                    // setSmallIcon()方法
                    //用于设置通知的小图标，注意，只能使用纯alpha图层的图片进行设置，小图标会显示在系统状态栏上
                    .setSmallIcon(R.drawable.small_icon)
                    // setLargeIcon()方法用于设置通知的大图标，当下拉系统状态栏时，就可以看到设置的大图标了。
                    .setLargeIcon(
                        BitmapFactory.decodeResource(
                            resources,
                            R.drawable.large_icon
                        )
                    )
                    .build()
                // 调用NotificationManager的notify()方法就可以让通知显示出来了。notify()方法接收两个参数：
                // 第一个参数是id，要保证为每个通知指定的id都是不同的；第二个参数则是Notification对象，
                // 这里直接将我们刚刚创建好的Notification对象传入即可。
                manager.notify(1, notification)
            }
        }
    }

    companion object {
        const val TAG = "MainActivity"
    }
}
```

如果你使用过Android手机，此时应该会下意识地认为这条通知是可以点击的。但是当你去点击它的时候，会发现没有任何效果。不对啊，每条通知被点击之后都应该有所反应呀。其实要想实现通知的点击效果，我们还需要在代码中进行相应的设置，这就涉及了一个新的概念——`PendingIntent`。

`PendingIntent`从名字上看起来就和Intent有些类似，它们确实存在不少共同点。比如它们都可以指明某一个“意图”，都可以用于启动Activity、启动Service以及发送广播等。不同的是，`Intent`倾向于立即执行某个动作，而`PendingIntent`倾向于在某个合适的时机执行某个动作。所以，也可以把`PendingIntent`简单地理解为**延迟执行的`Intent`**。

`PendingIntent`的用法同样很简单，它主要提供了几个静态方法用于获取`PendingIntent`的实例，可以根据需求来选择是使用`getActivity()`方法、`getBroadcast()`方法，还是`getService()`方法。这几个方法所接收的参数都是相同的：第一个参数依旧是`Context`，不用多做解释；第二个参数一般用不到，传入0即可；第三个参数是一个`Intent`对象，我们可以通过这个对象构建出`PendingIntent`的“意图”；第四个参数用于确定`PendingIntent`的行为，有`FLAG_ONE_SHOT`、`FLAG_NO_CREATE`、`FLAG_CANCEL_CURRENT`和`FLAG_UPDATE_CURRENT`这4种标志可选.这些标志是用来控制 `PendingIntent` 行为的。下面是每个标志的含义：

1. `FLAG_ONE_SHOT`：这个标志表示返回的 `PendingIntent` 只能使用一次。如果后续还需要执行相同的操作，你需要再次获取一个新的 `PendingIntent`。
2. `FLAG_NO_CREATE`：如果当前的 `PendingIntent` 不存在，那么简单地返回 `null`，而不是创建一个新的 `PendingIntent`。
3. `FLAG_CANCEL_CURRENT`：这个标志表示当前的 `PendingIntent` 会被取消，然后创建一个新的 `PendingIntent`。这意味着旧的 `PendingIntent` 不再有效，所有的等待的 `Intent` 都会被取消。
4. `FLAG_UPDATE_CURRENT`：如果相同的 `PendingIntent` 已经存在，那么保持它不变，但是替换它的 `Intent` 数据。这意味着新的 `Intent` 数据会被用来更新已经存在的 `PendingIntent`。

从 Android 12（API 级别 31）开始，创建 `PendingIntent` 时必须指定 `FLAG_IMMUTABLE` 或 `FLAG_MUTABLE`。这是因为 Android 12 对 `PendingIntent` 的行为进行了更改，以提高应用的安全性。

- `FLAG_IMMUTABLE`：这个标志表示 `PendingIntent` 是不可变的，也就是说，一旦创建，就不能更改。这是推荐的选项，因为它可以防止潜在的安全问题。

- `FLAG_MUTABLE`：这个标志表示 `PendingIntent` 是可变的，也就是说，可以在创建后进行更改。只有在某些功能依赖于 `PendingIntent` 的可变性时，才应该使用这个选项，例如，需要与内联回复或气泡一起使用。

对`PendingIntent`有了一定的了解后，我们再回过头来看一下`NotificationCompat.Builder`。这个构造器还可以连缀一个`setContentIntent()`方法，接收的参数正是一个`PendingIntent`对象。因此，这里就可以通过`PendingIntent`构建一个延迟执行的“意图”，当用户点击这条通知时就会执行相应的逻辑。

现在我们来优化一下`NotificationTest`项目，给刚才的通知加上点击功能，让用户点击它的时候可以启动另一个Activity。

- `FLAG_IMMUTABLE`：这个标志表示 `PendingIntent` 是不可变的，也就是说，一旦创建，就不能更改。这是推荐的选项，因为它可以防止潜在的安全问题。
- `FLAG_MUTABLE`：这个标志表示 `PendingIntent` 是可变的，也就是说，可以在创建后进行更改。只有在某些功能依赖于 `PendingIntent` 的可变性时，才应该使用这个选项，例如，需要与内联回复或气泡一起使用。

```kotlin
package com.ubx.notificationtest

import android.app.NotificationChannel
import android.app.NotificationManager
import android.app.PendingIntent
import android.content.Context
import android.content.Intent
import android.graphics.BitmapFactory
import android.net.Uri
import android.os.Build
import android.os.Bundle
import android.provider.Settings
import androidx.appcompat.app.AlertDialog
import androidx.appcompat.app.AppCompatActivity
import androidx.core.app.NotificationCompat
import androidx.core.app.NotificationManagerCompat
import com.ubx.notificationtest.databinding.ActivityMainBinding
import kotlin.random.Random

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        // 获取NotificationManager实例
        val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        // 判断安卓版本是否为Android 8.0+
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            // 安卓版本为 8.0+ 则使用NotificationChannel类构建一个通知渠道，
            // 并调用NotificationManager的createNotificationChannel()方法完成创建
            val channel =
                NotificationChannel("normal", "Normal", NotificationManager.IMPORTANCE_DEFAULT)
            manager.createNotificationChannel(channel)
        }

        mainBinding.sendNotice.setOnClickListener {
            // 检查是否已经有了通知权限
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
                val intent = Intent(this, NotificationActivity::class.java)
                // 从 Android 12（API 级别 31）开始，创建 PendingIntent 时必须指定 FLAG_IMMUTABLE 或 FLAG_MUTABLE。
                // 这是因为 Android 12 对 PendingIntent 的行为进行了更改，以提高应用的安全性。
                /*
                * 
                * FLAG_IMMUTABLE：这个标志表示 PendingIntent 是不可变的，也就是说，一旦创建，就不能更改。这是推荐的选项，因为它可以防止潜在的安全问题。
                * FLAG_MUTABLE：这个标志表示 PendingIntent 是可变的，也就是说，可以在创建后进行更改。只有在某些功能依赖于 PendingIntent 的可变性时，才应该使用这个选项，例如，需要与内联回复或气泡一起使用。
                * */
                val pi = PendingIntent.getActivity(this, 0, intent, PendingIntent.FLAG_UPDATE_CURRENT or PendingIntent.FLAG_IMMUTABLE)
                // 已经有权限，可以进行下一步操作
                // AndroidX库中提供了一个NotificationCompat类，使用这个类的构造器创建
                // Notification对象，就可以保证我们的程序在所有Android系统版本上都能正常工作了
                val notification = NotificationCompat.Builder(this, "normal")
                    // setContentTitle()方法用于指定通知的标题内容，下拉系统状态栏就可以看到这部分内容。
                    .setContentTitle("this is content title")
                    // setContentText()方法用于指定通知的正文内容，同样下拉系统状态栏就可以看到这部分内容。
                    .setContentText("this is content text".repeat(Random.nextInt(5, 10)))
                    // setSmallIcon()方法
                    //用于设置通知的小图标，注意，只能使用纯alpha图层的图片进行设置，小图标会显示在系统状态栏上
                    .setSmallIcon(R.drawable.small_icon)
                    // setLargeIcon()方法用于设置通知的大图标，当下拉系统状态栏时，就可以看到设置的大图标了。
                    .setLargeIcon(
                        BitmapFactory.decodeResource(
                            resources,
                            R.drawable.large_icon
                        )
                    )
                    .setContentIntent(pi)
                    .build()
                // 调用NotificationManager的notify()方法就可以让通知显示出来了。notify()方法接收两个参数：
                // 第一个参数是id，要保证为每个通知指定的id都是不同的；第二个参数则是Notification对象，
                // 这里直接将我们刚刚创建好的Notification对象传入即可。
                manager.notify(1, notification)
            }
        }
    }

    companion object {
        const val TAG = "MainActivity"
    }
}
```

如果我们没有在代码中对该通知进行取消，它就会一直显示在系统的状态栏上。解决的方法有两种：一种是在`NotificationCompat.Builder`中再连缀一个`setAutoCancel()`方法，一种是显式地调用`NotificationManager`的`cancel()`方法将它取消。两种方法我们都学习一下。

第一种方法写法如下：

```kotlin
val notification = NotificationCompat.Builder(this, "normal")
	...
	.setAutoCancel(true)
	.build()
```

`setAutoCancel()`方法传入true，就表示当点击这个通知的时候，通知会自动取消。

第二种方法写法如下：

```kotlin
package com.ubx.notificationtest

import android.app.NotificationManager
import android.content.Context
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle

class NotificationActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_notification)
        val manager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        manager.cancel(1)
    }
}
```

这里我们在`cancel()`方法中传入了1，当时我们给这条通知设置的id就是1。因此，如果你想取消哪条通知，在`cancel()`方法中传入该通知的id就行了。

### 1.3 通知的进阶技巧

当我们想在通知中显示较长的富文本时会发现，Android会将通知中的长文本折叠起来。那么有没有什么办法把富文本全部显示出来呢？实际上，`NotificationCompat.Builder`中提供了非常丰富的API，以便我们创建出更加多样的通知效果。先来看看`setStyle()`方法，这个方法允许我们构建出富文本的通知内容。也就是说，通知中不光可以有文字和图标，还可以包含更多的东西。`setStyle()`方法接收一个`NotificationCompat.Style`参数，这个参数就是用来构建具体的富文本信息的，如长文字、图片等。

```kotlin
val notification = NotificationCompat.Builder(this, "normal")
	...
	.setStyle(NotificationCompat.BigTextStyle().bigText("Learn how to build notifications, send and sync data, and use voice actions. Get the official Android IDE and developer tools to build apps for Android."))
	.build()
```

这里使用了`setStyle()`方法替代`setContentText()`方法。在`setStyle()`方法中，我们创建了一个`NotificationCompat.BigTextStyle`对象，这个对象就是用于封装长文字信息的，只要调用它的`bigText()`方法并将文字内容传入就可以了。

除了显示长文字之外，通知里还可以显示一张大图片，具体用法是基本相似的：

```kotlin
val notification = NotificationCompat.Builder(this, "normal")
	...
	.setStyle(NotificationCompat.BigPictureStyle().bigPicture(BitmapFactory.decodeResource(resources, R.drawable.big_image)))
	.build()
```

可以看到，这里仍然是调用的`setStyle()`方法，这次我们在参数中创建了一个`NotificationCompat.BigPictureStyle`对象，这个对象就是用于设置大图片的，然后调用它的`bigPicture()`方法并将图片传入。这里使用事先准备好的一张图片，通过`BitmapFactory`的`decodeResource()`方法将图片解析成`Bitmap`对象，再传入`bigPicture()`方法中就可以了。

接下来来学习一下不同重要等级的通知渠道对通知的行为具体有什么影响。其实简单来讲，就是通知渠道的重要等级越高，发出的通知就越容易获得用户的注意。比如高重要等级的通知渠道发出的通知可以弹出横幅、发出声音，而低重要等级的通知渠道发出的通知不仅可能会在某些情况下被隐藏，而且可能会被改变显示的顺序，将其排在更重要的通知之后。但需要注意的是，开发者只能在创建通知渠道的时候为它指定初始的重要等级，如果用户不认可这个重要等级的话，可以随时进行修改，开发者对此无权再进行调整和变更，因为通知渠道一旦创建就不能再通过代码修改了。

虽然无法更改之前创建的通知渠道，但是我们可以创建一个新的通知渠道，如下：

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        ...
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
            ...
            val channel2 = NotificationChannel("important", "Important", NotificationManager.IMPORTANCE_HIGH)
            manager.createNotificationChannel(channel2)
        }
        sendNotice.setOnClickListener {
            val intent = Intent(this, NotificationActivity::class.java)
            val pi = PendingIntent.getActivity(this, 0, intent, 0)
            val notification = NotificationCompat.Builder(this, "important")
            ...
        }
    }
}
```

## 二、调用摄像头和相册

我们平时在使用QQ或微信的时候经常要和别人分享图片，这些图片可以是用手机摄像头拍的，也可以是从相册中选取的。这样的功能实在是太常见了，几乎是应用程序必备的功能，那么本节我们就学习一下调用摄像头和相册方面的知识。

### 2.1 调用摄像头拍照

首先在布局文件中添加两个控件：一个Button和一个ImageView。Button是用于打开摄像头进行拍照的，而ImageView则是用于将拍到的图片显示出来。

开始编写调用摄像头的具体逻辑，修改MainActivity中的代码，如下所示：

```kotlin
package work.icu007.cameraalbumtest

import android.app.Activity
import android.content.Intent
import android.graphics.Bitmap
import android.graphics.BitmapFactory
import android.graphics.BitmapShader
import android.graphics.Canvas
import android.graphics.Matrix
import android.graphics.Paint
import android.graphics.Shader
import android.media.ExifInterface
import android.net.Uri
import android.os.Build
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.provider.MediaStore
import android.util.Log
import androidx.activity.result.ActivityResultLauncher
import androidx.activity.result.contract.ActivityResultContracts
import androidx.core.content.FileProvider
import com.ubx.cameraalbumtest.databinding.ActivityMainBinding
import java.io.File
import kotlin.properties.Delegates

class MainActivity : AppCompatActivity() {
    private lateinit var imageUri: Uri
    private lateinit var outputImage: File
    private lateinit var mainBinding: ActivityMainBinding
    private lateinit var takePhoto: ActivityResultLauncher<Intent>
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)

        takePhoto = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
            if (result.resultCode == Activity.RESULT_OK) {
                // 在这里处理图片捕获成功的情况
                val bitmap = BitmapFactory.decodeStream(contentResolver.openInputStream(imageUri))
                if (bitmap != null){
                    mainBinding.imageView.setImageBitmap(rotateIfRequired(bitmap))
                }
            } else {
                Log.d(TAG, "onCreate: take Photo error")
                // 在这里处理图片捕获失败的情况
            }
        }
        mainBinding.takePhotoBtn.setOnClickListener {
            outputImage = File(externalCacheDir, "output_image.jpg")
            if (outputImage.exists()) {
                outputImage.delete()
            }
            outputImage.createNewFile()
            imageUri = if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
                FileProvider.getUriForFile(this, "work.icu007.cameraalbumtest.fileprovider", outputImage)
            } else {
                Uri.fromFile(outputImage)
            }
            // 已经被弃用
            /*val intent = Intent("android.media.action.IMAGE_CAPTURE")
            intent.putExtra(MediaStore.EXTRA_OUTPUT, imageUri)
            startActivityForResult(intent, takePhoto)*/
            val intent = Intent("android.media.action.IMAGE_CAPTURE")
            intent.putExtra(MediaStore.EXTRA_OUTPUT, imageUri)
            takePhoto.launch(intent)
        }
    }


    private fun rotateIfRequired(bitmap: Bitmap): Bitmap {
        val exif = ExifInterface(outputImage.path)
        val orientation = exif.getAttributeInt(ExifInterface.TAG_ORIENTATION,
            ExifInterface.ORIENTATION_NORMAL)
        return when (orientation) {
            ExifInterface.ORIENTATION_ROTATE_90 -> rotateBitmap(bitmap, 90)
            ExifInterface.ORIENTATION_ROTATE_180 -> rotateBitmap(bitmap, 180)
            ExifInterface.ORIENTATION_ROTATE_270 -> rotateBitmap(bitmap, 270)
            else -> bitmap
        }
    }
    private fun rotateBitmap(bitmap: Bitmap, degree: Int): Bitmap {
        val matrix = Matrix()
        matrix.postRotate(degree.toFloat())
        val rotatedBitmap = Bitmap.createBitmap(bitmap, 0, 0, bitmap.width, bitmap.height,
            matrix, true)
        bitmap.recycle() // 将不再需要的Bitmap对象回收
        return rotatedBitmap
    }

    companion object{
        const val TAG = "MainActivity"
    }
}
```

在`MainActivity`中要做的第一件事自然是给Button注册点击事件，然后就是实例化 `takePhoto` 对象，并且注册返回结果的处理函数。紧接着在点击事件里开始处理调用摄像头的逻辑:

首先这里创建了一个`File`对象，用于存放摄像头拍下的图片，这里我们把图片命名为`output_image.jpg`，并存放在手机SD卡的应用关联缓存目录下。什么叫作应用关联缓存目录呢？就是指SD卡中专门用于存放当前应用缓存数据的位置，调用`getExternalCacheDir()`方法可以得到这个目录，具体的路径是`/sdcard/Android/data/<package name>/cache`。那么为什么要使用应用关联缓存目录来存放图片呢？因为从`Android 6.0`系统开始，读写SD卡被列为了危险权限，如果将图片存放在SD卡的任何其他目录，都要进行运行时权限处理才行，而使用应用关联目录则可以跳过这一步。另外，从Android 10.0系统开始，公有的SD卡目录已经不再允许被应用程序直接访问了，而是要使用作用域存储才行。

接着会进行一个判断，如果运行设备的系统版本低于Android 7.0，就调用Uri的`fromFile()`方法将File对象转换成Uri对象，这个Uri对象标识着`output_image.jpg`这张图片的本地真实路径。否则，就调用`FileProvider`的`getUriForFile()`方法将File对象转换成一个封装过的Uri对象。`getUriForFile()`方法接收3个参数：第一个参数要求传入Context对象，第二个参数可以是任意唯一的字符串，第三个参数则是我们刚刚创建的File对象。之所以要进行这样一层转换，是因为从Android 7.0系统开始，直接使用本地真实路径的Uri被认为是不安全的，会抛出一个`FileUriExposedException`异常。而`FileProvider`则是一种特殊的`ContentProvider`，它使用了和`ContentProvider`类似的机制来对数据进行保护，可以选择性地将封装过的Uri共享给外部，从而提高了应用的安全性。

接下来构建了一个`Intent`对象，并将这个`Intent`的`action`指定为`android.media.action.IMAGE_CAPTURE`，再调用`Intent`的`putExtra()`方法指定图片的输出地址，这里填入刚刚得到的Uri对象，最后调用`ActivityResultLauncher`对象的`launch()`方法来启动相机应用进行捕获图片。

调用照相机程序去拍照有可能会在一些手机上发生照片旋转的情况。这是因为这些手机认为打开摄像头进行拍摄时手机就应该是横屏的，因此回到竖屏的情况下就会发生90度的旋转。为此，这里我们又加上了判断图片方向的代码，如果发现图片需要进行旋转，那么就先将图片旋转相应的角度，然后再显示到界面上。

刚才提到了`ContentProvider`，那么我们自然要在`AndroidManifest.xml`中对它进行注册才行，代码如下所示：

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
        android:theme="@style/Theme.CameraAlbumTest"
        tools:targetApi="31">
        <provider
            android:name="androidx.core.content.FileProvider"
            android:authorities="work.icu007.cameraalbumtest.fileprovider"
            android:exported="false"
            android:grantUriPermissions="true">
            <meta-data
                android:name="android.support.FILE_PROVIDER_PATHS"
                android:resource="@xml/file_paths"/>
        </provider>
        <activity
            android:name=".MainActivity"
            android:exported="true">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

`android:name`属性的值是固定的，而`android:authorities`属性的值必须和刚才`FileProvider.getUriForFile()`方法中的第二个参数一致。另外，这里还在`<provider>`标签的内部使用`<meta-data>`指定Uri的共享路径，并引用了一个`@xml/file_paths`资源。当然，这个资源现在还是不存在的，下面我们就来创建它。

右击res目录→New→Directory，创建一个xml目录，接着右击xml目录→New→File，创建一个`file_paths.xml`文件。然后修改`file_paths.xml`文件中的内容，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<paths>
    <external-path
        name="my_images"
        path="/" />
</paths>
```

`external-path`就是用来指定Uri共享路径的，`name`属性的值可以随便填，`path`属性的值表示共享的具体路径。这里使用一个单斜线表示将**整个SD卡进行共享**，当然你也可以仅共享存放output_image.jpg这张图片的路径。

### 2.2 从相册选择图片

还是在 2.1 项目基础上修改，新增一个`button`用于从相册中选择图片。

修改`mainActivity`代码：

```kotlin
package com.ubx.cameraalbumtest

import android.app.Activity
import android.content.Intent
import android.graphics.Bitmap
import android.graphics.BitmapFactory
import android.graphics.BitmapShader
import android.graphics.Canvas
import android.graphics.Matrix
import android.graphics.Paint
import android.graphics.Shader
import android.media.ExifInterface
import android.net.Uri
import android.os.Build
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.provider.MediaStore
import android.util.Log
import androidx.activity.result.ActivityResultLauncher
import androidx.activity.result.contract.ActivityResultContracts
import androidx.core.content.FileProvider
import com.ubx.cameraalbumtest.databinding.ActivityMainBinding
import java.io.File
import kotlin.properties.Delegates

class MainActivity : AppCompatActivity() {
    private lateinit var imageUri: Uri
    private lateinit var outputImage: File
    private lateinit var mainBinding: ActivityMainBinding
    private lateinit var takePhoto: ActivityResultLauncher<Intent>
    private lateinit var fromAlbum: ActivityResultLauncher<Intent>
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)

        ...
        fromAlbum = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) {result ->
            if (result.resultCode == Activity.RESULT_OK && result.data != null){
                result.data!!.data?.let { uri ->
                    val bitmap = BitmapFactory.decodeStream(contentResolver.openInputStream(uri))
                    mainBinding.imageView.setImageBitmap(bitmap)
                }
            }
        }
        ...
        mainBinding.fromAlbumBtn.setOnClickListener {
            val intent = Intent(Intent.ACTION_OPEN_DOCUMENT)
            intent.addCategory(Intent.CATEGORY_OPENABLE)
            intent.type = "image/*"
            fromAlbum.launch(intent)
        }
    }

    ...

    companion object{
        const val TAG = "MainActivity"
    }
}
```

在“`From Album`”按钮的点击事件里，我们先构建了一个`Intent`对象，并将它的`action`指定为`Intent.ACTION_OPEN_DOCUMENT`，表示打开系统的文件选择器。接着给这个`Intent`对象设置一些条件过滤，只允许可打开的图片文件显示出来，然后调用`fromAlbum.launch(intent)`，打开文件选择器。

1. `registerForActivityResult(ActivityResultContracts.StartActivityForResult()) {result ->...}`：在这段代码中，使用registerForActivityResult方法注册了一个回调函数。这个方法接收两个参数，一个是要启动的Activity的类型描述，另一个是回调函数。此处的ActivityResultContracts.StartActivityForResult()表示启动的Activity是为了返回数据。在回调函数中，参数`result`即代表启动Activity的结果。
2. 在回调函数中，首先检查了`result.resultCode == Activity.RESULT_OK && result.data != null`，这也是常见的模式，意味着返回的结果有效且有返回数据。在此条件满足的情况下，`result.data!!.data?.let { uri ->...}` 这段代码解释了对返回的数据`result.data.data`即图片的URI的处理。使用`let`是Kotlin语言中的函数式编程风格，当`result.data!!.data`不为空时，执行其代码块，此处的uri即代表图片的URI。
3. `val bitmap = BitmapFactory.decodeStream(contentResolver.openInputStream(uri))`这段代码使用BitmapFactory解码stream得到一个bitmap，而stream是使用contentResolver从图片的uri中获取到的。
4. `mainBinding.imageView.setImageBitmap(bitmap)`将解码得到的bitmap设为imageView的图像显示。

## 三、播放多媒体文件

手机上最常见的休闲方式毫无疑问就是听音乐和看电影了，随着移动设备的普及，越来越多的人可以随时享受优美的音乐，观看精彩的电影。Android在播放音频和视频方面做了相当不错的支持，它提供了一套较为完整的API，使得开发者可以很轻松地编写出一个简易的音频或视频播放器，下面我们就来具体地学习一下。

### 3.1 播放音频

在Android中播放音频文件一般是使用`MediaPlayer`类实现的，它对多种格式的音频文件提供了非常全面的控制方法，从而使播放音乐的工作变得十分简单。下表列出了`MediaPlayer`类中一些较为常用的控制方法。

|     方法名      |                      功能描述                       |
| :-------------: | :-------------------------------------------------: |
| setDataSource() |             设置要播放的音频文件的位置              |
|    prepare()    |         在开始播放之前调用，以完成准备工作          |
|     start()     |                 开始或继续播放音频                  |
|     pause()     |                    暂停播放音频                     |
|     reset()     |        将MediaPlayer对象重置到刚刚创建的状态        |
|    seekTo()     |              从指定的位置开始播放音频               |
|     stop()      | 停止播放音频。调用后的MediaPlayer对象无法再播放音频 |
|    release()    |           释放与MediaPlayer对象相关的资源           |
|   isPlaying()   |         判断当前MediaPlayer是否正在播放音频         |
|  getDuration()  |              获取载入的音频文件的时长               |

简单了解了上述方法后，我们再来梳理一下`MediaPlayer`的工作流程。首先需要创建一个`MediaPlayer`对象，然后调用`setDataSource()`方法设置音频文件的路径，再调用`prepare()`方法使`MediaPlayer`进入准备状态，接下来调用`start()`方法就可以开始播放音频，调用`pause()`方法就会暂停播放，调用`reset()`方法就会停止播放。下面就让我们通过一个具体的例子来学习一下，代码如下所示：

```kotlin
package work.icu007.playaudiotest

import android.media.MediaPlayer
import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import work.icu007.playaudiotest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    private val mediaPlayer = MediaPlayer()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        initMediaPlayer()
        mainBinding.play.setOnClickListener {
            if (!mediaPlayer.isPlaying) {
                mediaPlayer.start()
            }
        }
        mainBinding.pause.setOnClickListener {
            if (mediaPlayer.isPlaying) {
                mediaPlayer.pause()
            }
        }
        mainBinding.stop.setOnClickListener {
            if (mediaPlayer.isPlaying) {
                mediaPlayer.reset()
                initMediaPlayer()
            }
        }
    }

    private fun initMediaPlayer() {
        val assetManager = assets
        val fd = assetManager.openFd("人生浪费指南 - 夏日入侵企画.mp3")
        mediaPlayer.setDataSource(fd.fileDescriptor, fd.startOffset, fd.length)
        mediaPlayer.prepare()
    }

    override fun onDestroy() {
        super.onDestroy()
        mediaPlayer.stop()
        mediaPlayer.release()
    }
}
```

在类初始化的时候，我们就先创建了一个`MediaPlayer`的实例，然后在`onCreate()`方法中调用`initMediaPlayer()`方法，为`MediaPlayer`对象进行初始化操作。在`initMediaPlayer()`方法中，首先通过`getAssets()`方法得到了一个`AssetManager`的实例，`AssetManager`可用于读取assets目录下的任何资源。接着我们调用了`openFd()`方法将音频文件句柄打开，后面又依次调用了`setDataSource()`方法和`prepare()`方法，为`MediaPlayer`做好了播放前的准备。

当点击“Play”按钮时会进行判断，如果当前`MediaPlayer`没有正在播放音频，则调用`start()`方法开始播放。当点击“Pause”按钮时会判断，如果当前`MediaPlayer`正在播放音频，则调用`pause()`方法暂停播放。当点击“Stop”按钮时会判断，如果当前`MediaPlayer`正在播放音频，则调用`reset()`方法将`MediaPlayer`重置为刚刚创建的状态，然后重新调用一遍`initMediaPlayer()`方法。最后在`onDestroy()`方法中，我们还需要分别调用`stop()`方法和`release()`方法，将与
`MediaPlayer`相关的资源释放掉。

### 3.2 播放视频

播放视频文件其实并不比播放音频文件复杂，主要是使用`VideoView`类来实现的。这个类将视频的显示和控制集于一身，我们仅仅借助它就可以完成一个简易的视频播放器。`VideoView`的用法和`MediaPlayer`也比较类似，常用方法如下表所示。

|     方法名     |          功能描述          |
| :------------: | :------------------------: |
| setVideoPath() | 设置要播放的视频文件的位置 |
|    start()     |     开始或继续播放视频     |
|    pause()     |        暂停播放视频        |
|    resume()    |     将视频从头开始播放     |
|    seekTo()    |  从指定的位置开始播放视频  |
|  isPlaying()   |  判断当前是否正在播放视频  |
| getDuration()  |  获取载入的视频文件的时长  |
|   suspend()    | 释放ViedoView所占用的资源  |

编辑布局文件如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/play"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Play"
        android:layout_margin="10dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toStartOf="@id/pause"
        app:layout_constraintBottom_toTopOf="@id/videoView"/>

    <Button
        android:id="@+id/pause"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Pause"
        android:layout_margin="10dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toEndOf="@id/play"
        app:layout_constraintEnd_toStartOf="@id/reply"
        app:layout_constraintBottom_toTopOf="@id/videoView"/>

    <Button
        android:id="@+id/reply"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Reply"
        android:layout_margin="10dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toEndOf="@id/pause"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintBottom_toTopOf="@id/videoView"/>

    <VideoView
        android:id="@+id/videoView"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layout_constraintTop_toBottomOf="@id/pause"
        app:layout_constraintBottom_toBottomOf="parent"/>




</androidx.constraintlayout.widget.ConstraintLayout>
```

而后修改MainActivity中的代码，如下：

```kotlin
package work.icu007.playvideotest

import android.net.Uri
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import android.widget.MediaController
import work.icu007.playvideotest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        val uri = Uri.parse("android.resource://$packageName/${R.raw.video}")
        Log.d(TAG, "onCreate: uri: $uri")
        mainBinding.videoView.setVideoURI(uri)
        val mediaController = MediaController(this)
        mainBinding.videoView.setMediaController(mediaController)
        //
        mainBinding.play.setOnClickListener {
            if (!mainBinding.videoView.isPlaying){
                mainBinding.videoView.start()
            }
        }
        mainBinding.pause.setOnClickListener {
            if (mainBinding.videoView.isPlaying){
                mainBinding.videoView.pause()
            }
        }
        mainBinding.reply.setOnClickListener {
            if (mainBinding.videoView.isPlaying){
                mainBinding.videoView.resume()
            }
        }
    }

    override fun onDestroy() {
        super.onDestroy()
        mainBinding.videoView.suspend()
    }
    companion object {
        var TAG = this.javaClass.simpleName
    }
}
```

首先在`onCreate()`方法中调用了`Uri.parse()`方法，将raw目录下的video.mp4文件解析成了一个Uri对象，这里使用的写法是Android要求的固定写法。然后调用`VideoView`的`setVideoURI()`方法将刚才解析出来的Uri对象传入，这样`VideoView`就初始化完成了。

当点击“Play”按钮时会判断，如果当前没有正在播放视频，则调用`start()`方法开始播放。当点击“Pause”按钮时会判断，如果当前视频正在播放，则调用`pause()`方法暂停播放。当点击“Replay”按钮时会判断，如果当前视频正在播放，则调用`resume()`方法从头播放视频。最后在`onDestroy()`方法中，我们还需要调用一下`suspend()`方法，将VideoView所占用的资源释放掉。还添加了一个`MediaController`。`MediaController`可以提供一套控制面板，用户可以用它来播放、暂停和快进视频。

