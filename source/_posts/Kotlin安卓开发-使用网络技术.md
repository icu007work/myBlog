---
title: Kotlin安卓开发-使用网络技术
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
swiper_index: 1
top_group_index: 1
background: '#fff'
cover: 'https://pic2.ziyuan.wang/user/xiheya/2024/05/Network_0d14f49c4bca7.jpg'
ai:
  - >-
    这篇文章主要介绍了在Android开发中如何使用网络技术，特别是如何使用HTTP与服务器进行交互和解析服务器返回的数据。文章首先强调了网络技术在现代应用开发中的重要性，举例说明了许多常见的应用，如QQ、微博、微信等，都大量使用了网络技术。然后，文章介绍了WebView的用法。WebView是Android提供的一个控件，可以在应用程序中嵌入一个浏览器，从而轻松地展示各种网页。这对于一些特殊的需求，比如在应程序中展示网页，但又不允许打开系统浏览器的情况，非常有用。最后，文章提供了一个创建WebView项目的示例，展示了如何在activity_main.xml中添加WebView控件的代码。
abbrlink: 4a483fcb
date: 2024-03-13 15:37:24
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

如果你在玩手机的时候不能上网，那你一定会感到特别地枯燥乏味。没错，现在早已不是玩单机的时代了，无论是PC、手机、平板，还是电视，都具备上网的功能， 21世纪的确是互联网的时代。

当然，Android手机肯定也是可以上网的。作为开发者，我们就需要考虑如何利用网络编写出更加出色的应用程序，像QQ、微博、微信等常见的应用都会大量使用网络技术。本章主要讲述如何在手机端使用HTTP和服务器进行网络交互，并对服务器返回的数据进行解析，这也是Android中最常使用到的网络技术，下面就让我们一起来学习一下吧。

## 一、WebView的用法

有时候我们可能会碰到一些比较特殊的需求，比如说在应用程序里展示一些网页。相信每个人都知道，加载和显示网页通常是浏览器的任务，但是需求里又明确指出，不允许打开系统浏览器，我们当然不可能自己去编写一个浏览器出来，这时应该怎么办呢？不用担心，**Android**早就考虑到了这种需求，并提供了一个**WebView**控件，借助它我们就可以在自己的应用程序里嵌入一个浏览器，从而非常轻松地展示各种各样的网页。**WebView**的用法也相当简单。新建一个**WebViewTest**项目，然后修改`activity_main.xml`中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">
    <WebView
        android:id="@+id/webView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

  </androidx.constraintlayout.widget.ConstraintLayout>
```

我们在布局文件中使用到了一个新的控件：**WebView**。这个控件就是用来显示网页的，这里的写法很简单，给它设置了一个id，并让它充满整个屏幕。然后修改**MainActivity**中的代码，如下所示：

```kotlin
package com.ubx.webviewtest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.webkit.WebViewClient
import androidx.viewbinding.ViewBinding
import com.ubx.webviewtest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        mainBinding.webView.settings.javaScriptEnabled = true
        mainBinding.webView.webViewClient = WebViewClient()
        mainBinding.webView.loadUrl("https://icu007.work")
    }
}
```

`MainActivity`中的代码也很短，通过**WebView**的`getSettings()`方法可以设置一些浏览器的属性，这里我们并没有设置过多的属性，只是调用了`setJavaScriptEnabled()`方法，让WebView支持JavaScript脚本。

接下来是比较重要的一个部分，我们调用了**WebView**的`setWebViewClient()`方法，并传入了一个**WebViewClient**的实例。这段代码的作用是，当需要从一个网页跳转到另一个网页时，我们希望目标网页仍然在当前WebView中显示，而不是打开系统浏览器。

最后一步就非常简单了，调用**WebView**的`loadUrl()`方法，并将网址传入，即可展示相应网页的内容.

另外还需要注意，由于本程序使用到了网络功能，而访问网络是需要声明权限的，因此我们还得修改`AndroidManifest.xml`文件，并加入权限声明，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools" >
    <uses-permission android:name="android.permission.INTERNET" />

    ...

</manifest>
```

![1708236612706.png](https://pic.ziyuan.wang/user/xiheya/2024/02/1708236612706_7c011c4f58fe4.png)

可以看到，WebViewTest这个程序现在已经具备了一个简易浏览器的功能，不仅成功将博客的首页展示了出来，还可以通过点击链接浏览更多的网页。

---

## 二、使用HTTP访问网络

对于**HTTP**，这里只需要稍微了解一些就足够了，它的工作原理特别简单，就是客户端向服务器发出一条**HTTP**请求，服务器收到请求之后会返回一些数据给客户端，然后客户端再对这些数据进行解析和处理就可以了。是不是非常简单？一个浏览器的基本工作原理也就是如此了。**比如说上一节中使用到的WebView控件，其实就是我们向博客的服务器发起了一条HTTP请求，接着服务器分析出我们想要访问的是百度的首页，于是把该网页的HTML代码进行返回，然后WebView再调用手机浏览器的内核对返回的HTML代码进行解析，最终将页面展示出来。**

简单来说，**WebView已经在后台帮我们处理好了发送HTTP请求、接收服务器响应、解析返回数据，以及最终的页面展示这几步工作，只不过它封装得实在是太好了，反而使得我们不能那么直观地看出HTTP到底是如何工作的。因此，接下来就让我们通过手动发送HTTP请求的方式更加深入地理解这个过程。**

### 2.1 使用HttpURLConnection

在过去，**Android**上发送**HTTP**请求一般有两种方式：**HttpURLConnection**和**HttpClient**。不过由于**HttpClient**存在API数量过多、扩展困难等缺点，**Android**团队越来越不建议我们使用这种方式。终于在**Android 6.0**系统中，**HttpClient**的功能被完全移除了，标志着此功能被正式弃用，因此本小节我们就学习一下现在官方建议使用的**HttpURLConnection**的用法。

首先需要获取**HttpURLConnection**的实例，一般只需创建一个**URL**对象，并传入目标的网络地址，然后调用一下`openConnection()`方法即可，如下所示：

```kotlin
val url = URL("https://icu007.work")
val connection = url.openConnection() as HttpURLConnection
```

在得到了**HttpURLConnection**的实例之后，我们可以设置一下**HTTP**请求所使用的方法。常用的方法主要有两个：**GET**和**POST**。**GET**表示希望从服务器那里获取数据，而**POST**则表示希望提交数据给服务器。写法如下：

```kotlin
connection.requestMethod = "GET"
```

接下来就可以进行一些自由的定制了，比如设置连接超时、读取超时的毫秒数，以及服务器希望得到的一些消息头等。这部分内容根据自己的实际情况进行编写，示例写法如下：

```kotlin
connection.connectTimeout = 8000
connection.readTimeout = 8000
```

之后再调用`getInputStream()`方法就可以获取到服务器返回的输入流了，剩下的任务就是对输入流进行读取：

```kotlin
val input = connection.inputStream
```

最后可以调用`disconnect()`方法将这个**HTTP**连接关闭：

```kotlin
connection.disconnect()
```

下面通过一个具体的例子来真正体验一下**HttpURLConnection**的用法。新建一个**NetworkTest**项目，首先修改activity_main.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/sendRequestBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Send Request"
        android:layout_margin="10dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

    <ScrollView
        app:layout_constraintTop_toBottomOf="@+id/sendRequestBtn"
        app:layout_constraintBottom_toBottomOf="parent"
        android:layout_width="match_parent"
        android:layout_height="0dp">
        <TextView
            android:id="@+id/responseText"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"/>
    </ScrollView>

</androidx.constraintlayout.widget.ConstraintLayout>
```

这里我们使用了一个新的控件：**ScrollView**。它是用来做什么的呢？由于手机屏幕的空间一般比较小，有些时候过多的内容一屏是显示不下的，借助**ScrollView**控件，我们就可以以滚动的形式查看屏幕外的内容。另外，布局中还放置了一个**Button**和一个**TextView**，**Button**用于发送**HTTP**请求，**TextView**用于将服务器返回的数据显示出来。

接着修改 **MainActivity**中的代码：

```kotlin
package work.icu007.networktest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import work.icu007.networktest.databinding.ActivityMainBinding
import java.io.BufferedReader
import java.io.InputStreamReader
import java.net.HttpURLConnection
import java.net.URL
import kotlin.concurrent.thread

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        mainBinding.sendRequestBtn.setOnClickListener {
            sendRequestWithHttpURLConnection()
        }
    }
    private fun sendRequestWithHttpURLConnection() {
        // 开启线程发起网络请求
        thread {
            var connection: HttpURLConnection? = null
            try {
                val response = StringBuilder()
                val url = URL("https://icu007.work")
                connection = url.openConnection() as HttpURLConnection
                connection.connectTimeout = 8000
                connection.readTimeout = 8000
                val input = connection.inputStream
                // 下面对获取到的输入流进行读取
                val reader = BufferedReader(InputStreamReader(input))
                reader.use {
                    reader.forEachLine {
                        response.append(it)
                    }
                }
                showResponse(response.toString())
            } catch (e: Exception) {
                e.printStackTrace()
            } finally {
                connection?.disconnect()
            }
        }
    }

    private fun showResponse(response: String) {
        runOnUiThread {
            // 在这里进行UI操作，将结果显示到界面上
            mainBinding.responseText.text = response
        }
    }
}
```

可以看到，我们在“Send Request”按钮的点击事件里调用了`sendRequestWithHttpURLConnection()`方法，在这个方法中先是开启了一个子线程，然后在子线程里使用**HttpURLConnection**发出一条HTTP请求，请求的目标地址就是百度的首页。接着利用**BufferedReader**对服务器返回的流进行读取，并将结果传入`showResponse()`方法中。而在`showResponse()`方法里，则是调用了一个`runOnUiThread()`方法，然后在这个方法的**Lambda**表达式中进行操作，将返回的数据显示到界面上。

那么这里为什么要用这个`runOnUiThread()`方法呢？别忘了，**Android**是不允许在子线程中进行UI操作的。我们之前学习了异步消息处理机制的工作原理，而`runOnUiThread()`方法其实就是对异步消息处理机制进行了一层封装，它背后的工作原理和之前介绍的内容是一模一样的。借助这个方法，我们就可以将服务器返回的数据更新到界面上了。

完整的流程就是这样。不过在开始运行之前，仍然别忘了要声明一下网络权限。修改**AndroidManifest.xml**中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools" >
    <uses-permission android:name="android.permission.INTERNET" />

    ...

</manifest>
```

运行结果如下：

![1708239819655.png](https://pic.ziyuan.wang/user/xiheya/2024/02/1708239819655_f046632467bd0.png)

会将这些代码解析成漂亮的网页后再展示出来。那么如果想要提交数据给服务器应该怎么办呢？其实也不复杂，**只需要将HTTP请求的方法改成POST**，并在获取输入流之前把要提交的数据写出即可。注意，每条数据都要以键值对的形式存在，**数据与数据之间用“&”符号隔开**。比如说我们想要向服务器提交用户名和密码，就可以这样写：

```kotlin
connection.requestMethod = "POST"
val output = DataOutputStream(connection.outputStream)
output.writeBytes("username=admin&password=123456")
```

### 2.2 使用OkHttp

当然我们并不是只能使用**HttpURLConnection**，完全没有任何其他选择，事实上在开源盛行的今天，有许多出色的网络通信库都可以替代原生的**HttpURLConnection**，而其中**OkHttp**无疑是做得最出色的一个。

**OkHttp**是由鼎鼎大名的**Square**公司开发的，这个公司在开源事业上贡献良多，除了**OkHttp**之外，还开发了**Retrofit**、**Picasso**等知名的开源项目。**OkHttp**不仅在接口封装上做得简单易用，就连在底层实现上也是自成一派，比起原生的**HttpURLConnection**，可以说是有过之而无不及，现在已经成了广大**Android**开发者首选的网络通信库。那么本小节我们就来学习一下**OkHttp**的用法。**OkHttp**的项目主页地址是：[OkHttp](https://github.com/square/okhttp)

在使用**OkHttp**之前，我们需要先在项目中添加**OkHttp**库的依赖。编辑**app/build.gradle**文件，在**dependencies**闭包中添加如下内容：

```tex
dependencies {
    ...
    implementation("com.squareup.okhttp3:okhttp:4.12.0")
}
```

添加上述依赖会自动下载两个库：一个是OkHttp库，一个是Okio库.

下面我们来看一下**OkHttp**的具体用法，首先需要创建一个**OkHttpClient**的实例，如下所示：

```kotlin
val client = OkHttpClient()
```

接下来如果想要发起一条HTTP请求，就需要创建一个Request对象：

```kotlin
val request = Request.Builder().build()
```

当然，上述代码只是创建了一个空的**Request**对象，并没有什么实际作用，我们可以在最终的`build()`方法之前连缀很多其他方法来丰富这个**Request**对象。比如可以通过`url()`方法来设置目标的网络地址，如下所示：

```kotlin
val request = Request.Builder()
		.url("https://icu007.work")
		.build()
```

之后调用**OkHttpClient**的`newCall()`方法来创建一个**Call**对象，并调用它的`execute()`方法来发送请求并获取服务器返回的数据，写法如下：

```kotlin
val response = client.newCall(request).execute()
```

**Response对象**就是服务器返回的数据了，我们可以使用如下写法来得到返回的具体内容：

```kotlin
val responseData = response.body?.string()
```

如果是发起一条**POST**请求，会比**GET**请求稍微复杂一点，**我们需要先构建一个`Request Body`对象来存放待提交的参数，如下所示：**

```kotlin
val requestBody = FormBody.Builder()
		.add("username", "admin")
		.add("password", "123456")
		.build()
```

然后在**Request.Builder**中调用一下`post()`方法，并将**RequestBody**对象传入：

```kotlin
val request = Request.Builder()
		.url("https://icu007.work")
		.post(requestBody)
		.build()
```

接下来的操作就和**GET**请求一样了，调用`execute()`方法来发送请求并获取服务器返回的数据即可。

好了，**OkHttp**的基本用法就先学到这里，在本章的稍后部分我们还会学习**OkHttp**结合**Retrofit**的使用方法，到时候再进一步学习。那么现在我们先把**NetworkTest**这个项目改用**OkHttp**的方式再实现一遍吧。

代码如下：

```kotlin
package work.icu007.networktest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import okhttp3.OkHttpClient
import okhttp3.Request
import work.icu007.networktest.databinding.ActivityMainBinding
import java.io.BufferedReader
import java.io.InputStreamReader
import java.net.HttpURLConnection
import java.net.URL
import kotlin.concurrent.thread

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        mainBinding.sendRequestBtn.setOnClickListener {
            sendRequestWithHttpURLConnection()
        }
    }
    private fun sendRequestWithHttpURLConnection() {
        // 开启线程发起网络请求
        thread {
            var connection: HttpURLConnection? = null
            try {
                /*val response = StringBuilder()
                val url = URL("https://icu007.work")
                connection = url.openConnection() as HttpURLConnection
                connection.connectTimeout = 8000
                connection.readTimeout = 8000
                val input = connection.inputStream
                // 下面对获取到的输入流进行读取
                val reader = BufferedReader(InputStreamReader(input))
                reader.use {
                    reader.forEachLine {
                        response.append(it)
                    }
                }
                showResponse(response.toString())*/
                val client = OkHttpClient()
                val request = Request.Builder()
                    .url("https://icu007.work")
                    .build()
                val response = client.newCall(request).execute()
                val responseData = response.body?.string()
                if (responseData != null) {
                    showResponse(responseData)
                }
            } catch (e: Exception) {
                e.printStackTrace()
            } finally {
                connection?.disconnect()
            }
        }
    }

    private fun showResponse(response: String) {
        runOnUiThread {
            // 在这里进行UI操作，将结果显示到界面上
            mainBinding.responseText.text = response
        }
    }
}
```

---

## 三、解析XML格式数据

通常情况下，每个需要访问网络的应用程序都会有一个自己的服务器，我们可以向服务器提交数据，也可以从服务器上获取数据。不过这个时候就出现了一个问题，这些数据到底要以什么样的格式在网络上传输呢？随便传递一段文本肯定是不行的，因为另一方根本就不知道这段文本的用途是什么。因此，一般我们会在网络上传输一些格式化后的数据，这种数据会有一定的结构规则和语义，当另一方收到数据消息之后，就可以按照相同的结构规则进行解析，从而取出想要的那部分内容。

在网络上传输数据时最常用的格式有两种：XML和JSON。下面我们就来一个一个地进行学习。本节首先学习一下如何解析XML格式的数据。

### 3.0 准备工作(使用Apache服务器)

在开始之前，我们还需要先解决一个问题，就是从哪儿才能获取一段XML格式的数据呢？这里我准备教你搭建一个最简单的Web服务器，在这个服务器上提供一段XML文本，然后我们在程序里去访问这个服务器，再对得到的XML文本进行解析。

搭建Web服务器的过程其实非常简单，也有很多种服务器类型可供选择，我们准备使用Apache服务器。另外，这里只会演示Windows系统下的搭建过程，因为Mac和Ubuntu系统都是默认安装好Apache服务器的，只需要启动一下即可。如果你使用的是这两种系统，可以自行搜索一下具体的操作方法。

下面来看Window系统下的搭建过程。首先你需要下载一个Apache服务器的安装包，官方下载地址是：[Apache官网](http://httpd.apache.org)。但是在官网下载时我们会发现 **Apache** 本身已经不提供已编译的安装包了，只提供源码。但是他为我们推荐了第三方提供编译的网站：

[Apache VS17 binaries and modules download (apachelounge.com)](https://www.apachelounge.com/download/)

![1708246917587.png](https://pic.ziyuan.wang/user/xiheya/2024/02/1708246917587_4cbfef9c5a33f.png)

下载后解压打开Apache所在目录 conf中的`httpd.conf`文件有几个地方需要修改

1. `Define SRVROOT`： 需要改成自己Apache所在目录；
2. `Listen`： 监听端口按需修改
3. `ServerName`： 改为 `ServerName localhost:监听的端口号`

而后用 **cmd** 命令在 `Apache/bin/`目录下执行：

```bash
.\httpd -k install
```

出现如下字样且在浏览器打开设置的 ServerName 出现 `It works` 字样即安装成功

![1708247243622.png](https://pic.ziyuan.wang/user/xiheya/2024/02/1708247243622_767268653e96e.png)

![1708247536605.png](https://pic.ziyuan.wang/user/xiheya/2024/02/1708247536605_95af53b42e5b0.png)

后续可以输入指令启动：

```bash
httpd -k start  #启动
httpd -k stop   #停止
```

接下来在`Apache/htdocs`目录下，在这里新建一个名为`get_data.xml`的文件，然后编辑这个文件，并加入如下XML格式的内容。

```xml
<apps>
    <app>
        <id>1</id>
        <name>Google Maps</name>
        <version>1.0</version>
    </app>
    <app>
        <id>2</id>
        <name>Chrome</name>
        <version>2.1</version>
    </app>
    <app>
        <id>3</id>
        <name>Google Play</name>
        <version>2.3</version>
    </app>
</apps>
```

这时在浏览器中访问<http://ServerName/get_data.xml>这个网址，应出现如下所示内容

![1708247707227.png](https://pic.ziyuan.wang/user/xiheya/2024/02/1708247707227_9f71538b612a7.png)

### 3.1 Pull解析方式

解析XML格式的数据其实也有挺多种方式的，本节中我们学习比较常用的两种：**Pull解析**和**SAX解析**。那么简单起见，这里仍然是在**NetworkTest**项目的基础上继续开发，这样我们就可以重用之前网络通信部分的代码，从而把工作的重心放在XML数据解析上。

既然XML格式的数据已经提供好了，现在要做的就是从中解析出我们想要得到的那部分内容。修改**MainActivity**中的代码，如下所示：

```kotlin
package work.icu007.networktest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import okhttp3.OkHttpClient
import okhttp3.Request
import org.xmlpull.v1.XmlPullParser
import org.xmlpull.v1.XmlPullParserFactory
import work.icu007.networktest.databinding.ActivityMainBinding
import java.io.BufferedReader
import java.io.InputStreamReader
import java.io.StringReader
import java.lang.StringBuilder
import java.net.HttpURLConnection
import java.net.URL
import kotlin.concurrent.thread

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        mainBinding.sendRequestBtn.setOnClickListener {
//            sendRequestWithHttpURLConnection()
            sendRequestWithOkHttp()
        }
    }
    private fun sendRequestWithHttpURLConnection() {
        // 开启线程发起网络请求
        thread {
            var connection: HttpURLConnection? = null
            try {
                /*val response = StringBuilder()
                val url = URL("https://icu007.work")
                connection = url.openConnection() as HttpURLConnection
                connection.connectTimeout = 8000
                connection.readTimeout = 8000
                val input = connection.inputStream
                // 下面对获取到的输入流进行读取
                val reader = BufferedReader(InputStreamReader(input))
                reader.use {
                    reader.forEachLine {
                        response.append(it)
                    }
                }
                showResponse(response.toString())*/
                val client = OkHttpClient()
                val request = Request.Builder()
                    .url("https://icu007.work")
                    .build()
                val response = client.newCall(request).execute()
                val responseData = response.body?.string()
                if (responseData != null) {
                    showResponse(responseData)
                }
            } catch (e: Exception) {
                e.printStackTrace()
            } finally {
                connection?.disconnect()
            }
        }
    }
    private fun sendRequestWithOkHttp() {
        thread {
            try {
                val client = OkHttpClient()
                val request = Request.Builder()
                    .url("http:/10.0.2.2:8088/get_data.xml")
                    .build()
                val response = client.newCall(request).execute()
                val responseData = response.body?.string()
                Log.d("MainActivity", "sendRequestWithOkHttp: $responseData")
                if (responseData != null) {
                    parseXMLWithPull(responseData)
                }
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }

    private fun showResponse(response: String) {
        runOnUiThread {
            // 在这里进行UI操作，将结果显示到界面上
            mainBinding.responseText.text = response
        }
    }

    private fun parseXMLWithPull(xmlData: String) {
        try {
            val factory = XmlPullParserFactory.newInstance()
            val xmlPullParser = factory.newPullParser()
            xmlPullParser.setInput(StringReader(xmlData))
            var eventType = xmlPullParser.eventType
            var id = ""
            var name = ""
            var version = ""
            while (eventType != XmlPullParser.END_DOCUMENT) {
                val nodeName = xmlPullParser.name
                when (eventType) {
                    // 开始解析某个节点
                    XmlPullParser.START_TAG -> {
                        when (nodeName) {
                            "id" -> id = xmlPullParser.nextText()
                            "name" -> name = xmlPullParser.nextText()
                            "version" -> version = xmlPullParser.nextText()
                        }
                    }
                    // 完成解析某个节点
                    XmlPullParser.END_TAG -> {
                        if ("app" == nodeName) {
                            Log.d("MainActivity", "id is $id")
                            Log.d("MainActivity", "name is $name")
                            Log.d("MainActivity", "version is $version")
                        }
                    }
                }
                eventType = xmlPullParser.next()
            }
        } catch (e: Exception) {
            e.printStackTrace()
        }
    }
}
```

可以看到，**这里首先将HTTP请求的地址改成了<http://10.0.2.2/get_data.xml，10.0.2.2>对于模拟器来说就是计算机本机的IP地址**。在得到了服务器返回的数据后，我们不再直接将其展示，而是调用了`parseXMLWithPull()`方法来解析服务器返回的数据。

下面就来仔细看下`parseXMLWithPull()`方法中的代码吧。这里首先要创建一个**XmlPullParserFactory**的实例，并借助这个实例得到**XmlPullParser**对象，然后调用**XmlPullParser**的`setInput()`方法将服务器返回的XML数据设置进去，之后就可以开始解析了。解析的过程也非常简单，通过`getEventType()`可以得到当前的解析事件，然后在一个**while**循环中不断地进行解析，如果当前的解析事件不等于`XmlPullParser.END_DOCUMENT`，说明解析工作还没完成，调用`next()`方法后可以获取下一个解析事件。

在**while**循环中，我们通过`getName()`方法得到了当前节点的名字。如果发现节点名等于**id、name或version**，就调用`nextText()`方法来获取节点内具体的内容，每当解析完一个**app**节点，就将获取到的内容打印出来。

好了，整体的过程就是这么简单，不过在程序运行之前还得再进行一项额外的配置。从Android9.0系统开始，应用程序默认只允许使用HTTPS类型的网络请求，HTTP类型的网络请求因为有安全隐患默认不再被支持，而我们搭建的Apache服务器现在使用的就是HTTP。

那么为了能让程序使用HTTP，我们还要进行如下配置才可以。右击**res目录→New→Directory**，创建一个xml目录，接着**右击xml目录→New→File**，创建一个`network_config.xml`文件。然后修改`network_config.xml`文件中的内容，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true">
        <trust-anchors>
            <certificates src="system" />
        </trust-anchors>
    </base-config>
</network-security-config>
```

这段配置文件的意思就是允许我们以明文的方式在网络上传输数据，而HTTP使用的就是明文传输方式。

接下来修改`AndroidManifest.xml`中的代码来启用我们刚才创建的配置文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <uses-permission android:name="android.permission.INTERNET" />

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.NetworkTest"
        android:networkSecurityConfig="@xml/network_config">
        ...
    </application>

</manifest>
```

运行NetworkTest项目，然后点击“Send Request”按钮，观察Logcat中的打印日志.

```tex
2024-02-19 11:21:59.080  7199-7307  MainActivity            work.icu007.networktest              D  id is 1
2024-02-19 11:21:59.080  7199-7307  MainActivity            work.icu007.networktest              D  name is Google Maps
2024-02-19 11:21:59.080  7199-7307  MainActivity            work.icu007.networktest              D  version is 1.0
2024-02-19 11:21:59.080  7199-7307  MainActivity            work.icu007.networktest              D  id is 2
2024-02-19 11:21:59.080  7199-7307  MainActivity            work.icu007.networktest              D  name is Chrome
2024-02-19 11:21:59.080  7199-7307  MainActivity            work.icu007.networktest              D  version is 2.1
2024-02-19 11:21:59.081  7199-7307  MainActivity            work.icu007.networktest              D  id is 3
2024-02-19 11:21:59.081  7199-7307  MainActivity            work.icu007.networktest              D  name is Google Play
2024-02-19 11:21:59.081  7199-7307  MainActivity            work.icu007.networktest              D  version is 2.3
```

可以看到，XML数据中的指定内容已经解析出来了。

### 3.2 SAX解析方式

**Pull**解析方式虽然非常好用，但它并不是我们唯一的选择。**SAX**解析也是一种特别常用的**XML**解析方式，虽然它的用法比**Pull**解析要复杂一些，但在语义方面会更加清楚。要使用**SAX**解析，通常情况下我们会新建一个类继承自`DefaultHandler`，并重写父类的5个方法，如下所示：

```kotlin
package work.icu007.networktest

import org.xml.sax.Attributes
import org.xml.sax.helpers.DefaultHandler


/*
 * Author: Charlie_Liao
 * Time: 2024/2/19-11:33
 * E-mail: rookie_l@icu007.work
 */

class MyHandler : DefaultHandler() {
    override fun startDocument() {
        super.startDocument()
    }

    override fun endDocument() {
        super.endDocument()
    }

    override fun startElement(
        uri: String?,
        localName: String?,
        qName: String?,
        attributes: Attributes?
    ) {
        super.startElement(uri, localName, qName, attributes)
    }

    override fun endElement(uri: String?, localName: String?, qName: String?) {
        super.endElement(uri, localName, qName)
    }

    override fun characters(ch: CharArray?, start: Int, length: Int) {
        super.characters(ch, start, length)
    }
}
```

`startDocument()`方法会在开始**XML**解析的时候调用，`startElement()`方法会在开始解析某个节点的时候调用，`characters()`方法会在获取节点中内容的时候调用，`endElement()`方法会在完成解析某个节点的时候调用，`endDocument()`方法会在完成整个**XML**解析的时候调用。其中，`startElement()`、`characters()`和`endElement()`这3个方法是有参数的，从**XML**中解析出的数据就会以参数的形式传入这些方法中。需要注意的是，在获取节点中的内容时，`characters()`方法可能会被调用多次，一些换行符也被当作内容解析出来，我们需要针对这种情况在代码中做好控制。

那么下面就让我们尝试用SAX解析的方式来实现和上一小节同样的功能吧。新建一个`ContentHandler`类继承自`DefaultHandler`，并重写父类的5个方法

```kotlin
package work.icu007.networktest

import android.util.Log
import org.xml.sax.Attributes
import org.xml.sax.helpers.DefaultHandler
import java.lang.StringBuilder


/*
 * Author: Charlie_Liao
 * Time: 2024/2/19-11:33
 * E-mail: rookie_l@icu007.work
 */

class ContentHandler : DefaultHandler() {

    private var nodeName = ""
    private lateinit var id: StringBuilder
    private lateinit var name: StringBuilder
    private lateinit var version: StringBuilder

    override fun startDocument() {
        id = StringBuilder()
        name = StringBuilder()
        version = StringBuilder()
    }

    override fun endDocument() {
        super.endDocument()
    }

    override fun startElement(
        uri: String?,
        localName: String?,
        qName: String?,
        attributes: Attributes?
    ) {
        if (localName != null){
            nodeName = localName
        }
        Log.d("ContentHandler", "startElement: uri is $uri")
        Log.d("ContentHandler", "startElement: localName is $localName")
        Log.d("ContentHandler", "startElement: qName is $qName")
        Log.d("ContentHandler", "startElement: attributes is $attributes")
    }

    override fun endElement(uri: String?, localName: String?, qName: String?) {
        if ("app" == localName) {
            Log.d("ContentHandler", "endElement: id is ${id.toString().trim()}")
            Log.d("ContentHandler", "endElement: name is ${name.toString().trim()}")
            Log.d("ContentHandler", "endElement: version is ${version.toString().trim()}")
            // 最后将StringBuilder清空
            id.setLength(0)
            name.setLength(0)
            version.setLength(0)
        }
    }

    override fun characters(ch: CharArray?, start: Int, length: Int) {
        when(nodeName) {
            "id" -> id.append(ch, start, length)
            "name" -> name.append(ch, start, length)
            "version" -> version.append(ch, start, length)
        }
    }
}
```

可以看到，我们首先给**id**、**name**和**version**节点分别定义了一个**StringBuilder**对象，并在`startDocument()`方法里对它们进行了初始化。每当开始解析某个节点的时候，`startElement()`方法就会得到调用，其中**localName**参数记录着当前节点的名字，这里我们把它记录下来。接着在解析节点中具体内容的时候就会调用`characters()`方法，我们会根据当前的节点名进行判断，将解析出的内容添加到哪一个**StringBuilder**对象中。最后在`endElement()`方法中进行判断，如果app节点已经解析完成，就打印出id、name和version的内容。需要注意的是，目前**id**、**name**和**version**中都可能是包括回车或换行符的，因此在打印之前我们还需要调用一下`trim()`方法，并且打印完成后要将**StringBuilder**的内容清空，不然的话会影响下一次内容的读取。

接下来的工作就非常简单了，修改**MainActivity**中的代码，如下所示：

```kotlin
package work.icu007.networktest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import okhttp3.OkHttpClient
import okhttp3.Request
import org.xml.sax.InputSource
import org.xmlpull.v1.XmlPullParser
import org.xmlpull.v1.XmlPullParserFactory
import work.icu007.networktest.databinding.ActivityMainBinding
import java.io.BufferedReader
import java.io.InputStreamReader
import java.io.StringReader
import java.lang.StringBuilder
import java.net.HttpURLConnection
import java.net.URL
import javax.xml.parsers.SAXParserFactory
import kotlin.concurrent.thread

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        mainBinding.sendRequestBtn.setOnClickListener {
//            sendRequestWithHttpURLConnection()
            sendRequestWithOkHttp()
        }
    }
    ...
    private fun sendRequestWithOkHttp() {
        thread {
            try {
                val client = OkHttpClient()
                val request = Request.Builder()
                    .url("http:/10.0.2.2:8088/get_data.xml")
                    .build()
                val response = client.newCall(request).execute()
                val responseData = response.body?.string()
                Log.d("MainActivity", "sendRequestWithOkHttp: $responseData")
                if (responseData != null) {
//                    parseXMLWithPull(responseData)
                    parseXMLWithSAX(responseData)
                }
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }

    ...

    private fun parseXMLWithSAX(xmlData: String) {
        try {
            val factory = SAXParserFactory.newInstance()
            val xmlReader = factory.newSAXParser().xmlReader
            val handler = ContentHandler()
            xmlReader.contentHandler = handler
            xmlReader.parse(InputSource(StringReader(xmlData)))
        } catch (e: Exception) {
            e.printStackTrace()
        }
    }
}
```

在得到了服务器返回的数据后，我们这次通过调用`parseXMLWithSAX()`方法来解析XML数据。`parseXMLWithSAX()`方法中先是创建了一个**SAXParserFactory**的对象，然后再获取**XMLReader**对象，接着将我们编写的**ContentHandler**的实例设置到**XMLReader**中，最后调用`parse()`方法开始执行解析。观察log会发现结果和Pull解析是一致的。

---

## 四、解析JSON格式数据

现在你已经掌握了**XML**格式数据的解析方式，那么接下来我们要学习一下如何解析**JSON**格式的数据了。比起**XML**，**JSON**的主要优势在于它的体积更小，在网络上传输的时候更省流量。但缺点在于，它的语义性较差，看起来不如**XML**直观。

在开始之前，我们还需要在`Apache/htdocs`目录中新建一个`get_data.json`的文件，然后编辑这个文件，并加入如下**JSON**格式的内容：

```json
[{"id":"5","version":"5.5","name":"Clash of Clans"},
{"id":"6","version":"7.0","name":"Boom Beach"},
{"id":"7","version":"3.5","name":"Clash Royale"}]
```

### 4.1 使用JSONObject

解析**JSON**数据也有很多种方法，可以使用官方提供的**JSONObject**，也可以使用**Google**的开源库**GSON**。另外，一些第三方的开源库如**Jackson**、**FastJSON**等也非常不错。本节中我们就来学习一下前两种解析方式的用法。

修改**MainActivity**中的代码，如下：

```kotlin
package work.icu007.networktest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import okhttp3.OkHttpClient
import okhttp3.Request
import org.json.JSONArray
import org.xml.sax.InputSource
import org.xmlpull.v1.XmlPullParser
import org.xmlpull.v1.XmlPullParserFactory
import work.icu007.networktest.databinding.ActivityMainBinding
import java.io.BufferedReader
import java.io.InputStreamReader
import java.io.StringReader
import java.lang.StringBuilder
import java.net.HttpURLConnection
import java.net.URL
import javax.xml.parsers.SAXParserFactory
import kotlin.concurrent.thread

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        mainBinding.sendRequestBtn.setOnClickListener {
//            sendRequestWithHttpURLConnection()
            sendRequestWithOkHttp()
        }
    }
    ...
    private fun sendRequestWithOkHttp() {
        thread {
            try {
                val client = OkHttpClient()
                val request = Request.Builder()
                    .url("http:/10.0.2.2:8088/get_data.json")
                    .build()
                val response = client.newCall(request).execute()
                val responseData = response.body?.string()
                Log.d("MainActivity", "sendRequestWithOkHttp: $responseData")
                if (responseData != null) {
//                    parseXMLWithPull(responseData)
//                    parseXMLWithSAX(responseData)
                    parseJSONWithJSONObject(responseData)
                }
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }

    ...

    private fun parseJSONWithJSONObject(jsonData: String) {
        try {
            val jsonArray = JSONArray(jsonData)
            for (i in 0 until jsonArray.length()) {
                val jsonObject = jsonArray.getJSONObject(i)
                val id = jsonObject.getString("id")
                val name = jsonObject.getString("name")
                val version = jsonObject.getString("version")
                Log.d("MainActivity", "parseJSONWithJSONObject: id is $id")
                Log.d("MainActivity", "parseJSONWithJSONObject: name is $name")
                Log.d("MainActivity", "parseJSONWithJSONObject: version is $version")
            }
        } catch (e: Exception) {
            e.printStackTrace()
        }
    }
}
```

首先将HTTP请求的地址改成<http://10.0.2.2/get_data.json>，然后在得到服务器返回的数据后调用`parseJSONWithJSONObject()`方法来解析数据。可以看到，解析**JSON**的代码真的非常简单，由于我们在服务器中定义的是一个**JSON**数组，因此这里首先将服务器返回的数据传入一个**JSONArray**对象中。然后循环遍历这个**JSONArray**，从中取出的每一个元素都是一个**JSONObject**对象，每个**JSONObject**对象中又会包含**id、name和version**这些数据。接下来只需要调用`getString()`方法将这些数据取出，并打印出来即可。

运行结果如下：

![1708323342502.png](https://pic.ziyuan.wang/user/xiheya/2024/02/1708323342502_06812d6513a1c.png)

### 4.2 使用GSON

使用**JSONObject**来解析**JSON**数据已经非常简单了，但是**Google**提供的**GSON**开源库可以让解析**JSON**数据的工作简单到让你不敢想象的地步。[GSON仓库](https://github.com/google/gson)

不过，**GSON**并没有被添加到**Android**官方的**API**中，因此如果想要使用这个功能的话，就必须在项目中添加**GSON**库的依赖。编辑`app/build.gradle`文件，在**dependencies**闭包中添加如下内容：

```tex
dependencies {
    ...
    implementation("com.google.code.gson:gson:2.10.1")
}
```

那么GSON库究竟是神奇在哪里呢？它的强大之处就在于可以将一段JSON格式的字符串自动映射成一个对象，从而不需要我们再手动编写代码进行解析了。比如说一段JSON格式的数据如下所示：

```json
{"name":"Tom","age":20}
```

那我们就可以定义一个**Person**类，并加入**name**和**age**这两个字段，然后只需简单地调用如下代码就可以将**JSON**数据自动解析成一个**Person**对象了：

```kotlin
val gson = Gson()
val person = gson.fromJson(jsonData, Person::class.java)
```

如果需要解析的是一段**JSON**数组，会稍微麻烦一点，比如如下格式的数据：

```json
[{"name":"Tom","age":20}, {"name":"Jack","age":25}, {"name":"Lily","age":22}]
```

这个时候，我们需要借助**TypeToken**将期望解析成的数据类型传入`fromJson()`方法中，如下所示：

```kotlin
val typeOf = object : TypeToken<List<Person>>() {}.type
val people = gson.fromJson<List<Person>>(jsonData, typeOf)
```

好了，基本的用法就是这样，下面就让我们来真正地尝试一下。首先新增一个App类，并加入id、name和version这3个字段，如下所示:

```kotlin
class App(val id: String, val name: String, val version: String)
```

然后修改MainActivity中的代码，如下：

```kotlin
package work.icu007.networktest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import com.google.gson.Gson
import com.google.gson.reflect.TypeToken
import okhttp3.OkHttpClient
import okhttp3.Request
import org.json.JSONArray
import org.xml.sax.InputSource
import org.xmlpull.v1.XmlPullParser
import org.xmlpull.v1.XmlPullParserFactory
import work.icu007.networktest.databinding.ActivityMainBinding
import java.io.BufferedReader
import java.io.InputStreamReader
import java.io.StringReader
import java.lang.StringBuilder
import java.net.HttpURLConnection
import java.net.URL
import javax.xml.parsers.SAXParserFactory
import kotlin.concurrent.thread

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        mainBinding.sendRequestBtn.setOnClickListener {
//            sendRequestWithHttpURLConnection()
            sendRequestWithOkHttp()
        }
    }
    ...
    private fun sendRequestWithOkHttp() {
        thread {
            try {
                val client = OkHttpClient()
                val request = Request.Builder()
                    .url("http:/10.0.2.2:8088/get_data.json")
                    .build()
                val response = client.newCall(request).execute()
                val responseData = response.body?.string()
                Log.d("MainActivity", "sendRequestWithOkHttp: $responseData")
                if (responseData != null) {
//                    parseXMLWithPull(responseData)
//                    parseXMLWithSAX(responseData)
//                    parseJSONWithJSONObject(responseData)
                    parseJSONWithGSON(responseData)
                }
            } catch (e: Exception) {
                e.printStackTrace()
            }
        }
    }

    ...

    private fun parseJSONWithGSON(jsonData: String) {
        val gson = Gson()
        val typeOf = object : TypeToken<List<App>>() {}.type
        val appList = gson.fromJson<List<App>>(jsonData, typeOf)
        for (app in appList) {
            Log.d("MainActivity", "parseJSONWithGSON: id is ${app.id}")
            Log.d("MainActivity", "parseJSONWithGSON: name is ${app.name}")
            Log.d("MainActivity", "parseJSONWithGSON: version is ${app.version}")
        }
    }
}
```

观察打印日志，与使用JSONObject结果一致。

---

## 五、网络请求回调的实现方式

目前我们已经掌握了**HttpURLConnection**和**OkHttp**的用法，知道了如何发起**HTTP**请求，以及解析服务器返回的数据，但之前我们的写法其实是很有问题的。因为一个应用程序很可能会在许多地方都使用到网络功能，而发送**HTTP**请求的代码基本是相同的，如果我们每次都去编写一遍发送**HTTP**请求的代码，这显然是非常差劲的做法。

没错，通常情况下我们应该将这些通用的网络操作提取到一个公共的类里，并提供一个通用方法，当想要发起网络请求的时候，只需简单地调用一下这个方法即可。比如使用如下的写法：

```kotlin
object HttpUtil {
    fun sendHttpRequest (address: String): String {
        var connection: HttpURLConnection? = null
        try {
            val response = StringBuilder()
            val url = URL(address)
            connection = url.openConnection() as HttpURLConnection
            connection.connectTimeout = 8000
            connection.readTimeout = 8000
            val input = connection.inputStream
            val reader = BufferedReader(InputStreamReader(input))
            reader.use {
                reader.forEachLine {
                    response.append(it)
                }
            }
            return response.toString()
        } catch (e: Exception) {
            e.printStackTrace()
            return e.message.toString()
        } finally {
            connection?.disconnect()
        }
    }
}
```

以后每当需要发起一条**HTTP**请求的时候，就可以这样写：

```kotlin
val address = "https://icu007.work"
val response = HttpUtil.sendHttpRequest(address)
```

在获取到服务器响应的数据后，我们就可以对它进行解析和处理了。但是需要注意，网络请求通常属于耗时操作，而`sendHttpRequest()`方法的内部并没有开启线程，这样就有可能导致在调用`sendHttpRequest()`方法的时候主线程被阻塞。

那在`sendHttpRequest()`方法内部开启一个线程，不就解决这个问题了吗？其实没有想象中那么容易，因为如果我们在`sendHttpRequest()`方法中开启一个线程来发起**HTTP**请求，服务器响应的数据是无法进行返回的。这是由于所有的耗时逻辑都是在子线程里进行的，`sendHttpRequest()`方法会在服务器还没来得及响应的时候就执行结束了，当然也就无法返回响应的数据了。

那么在遇到这种情况时应该怎么办呢？其实解决方法并不难，只需要使用编程语言的回调机制就可以了。下面就来学习一下回调机制到底是如何使用的。

首先需要定义一个接口，比如将它命名成`HttpCallbackListener`，代码如下所示：

```kotlin
interface HttpCallbackListener {
    fun onFinish(response: String)
    fun onError(e: Exception)
}
```

可以看到，我们在接口中定义了两个方法：`onFinish()`方法表示当服务器成功响应我们请求的时候调用，`onError()`表示当进行网络操作出现错误的时候调用。这两个方法都带有参数，`onFinish()`方法中的参数代表服务器返回的数据，而`onError()`方法中的参数记录着错误的详细信息。

接着修改`HttpUtil`中的代码：

```kotlin
interface HttpCallbackListener {
    fun onFinish(response: String)
    fun onError(e: Exception)
}
object HttpUtil {
    fun sendHttpRequest (address: String, listener: HttpCallbackListener) {
        thread {
            var connection: HttpURLConnection? = null
            try {
                val response = StringBuilder()
                val url = URL(address)
                connection = url.openConnection() as HttpURLConnection
                connection.connectTimeout = 8000
                connection.readTimeout = 8000
                val input = connection.inputStream
                val reader = BufferedReader(InputStreamReader(input))
                reader.use {
                    reader.forEachLine {
                        response.append(it)
                    }
                }
                listener.onFinish(response.toString())
            } catch (e: Exception) {
                e.printStackTrace()
                listener.onError(e)
            } finally {
                connection?.disconnect()
            }
        }
    }
}
```

我们首先给`sendHttpRequest()`方法添加了一个`HttpCallbackListener`参数，并在方法的内部开启了一个子线程，然后在子线程里执行具体的网络操作。注意，**子线程中是无法通过return语句返回数据的**，因此这里我们将服务器响应的数据传入了`HttpCallbackListener`的`onFinish()`方法中，如果出现了异常，就将异常原因传入`onError()`方法中。

现在`sendHttpRequest()`方法接收两个参数，因此我们在调用它的时候还需要将`HttpCallbackListener`的实例传入，如下所示：

```kotlin
HttpUtil.sendHttpRequest(address, object: HttpCallbackListener{
    override fun onError(e: Exception) {
        // 对异常情况进行处理
        TODO("Not yet implemented")
    }

    override fun onFinish(response: String) {
        // 得到服务器返回的具体内容
        TODO("Not yet implemented")
    }
})
```

这样当服务器成功响应的时候，我们就可以在`onFinish()`方法里对响应数据进行处理了。类似地，如果出现了异常，就可以在`onError()`方法里对异常情况进行处理。如此一来，我们就巧妙地利用回调机制将响应数据成功返回给调用方了。

不过你会发现，上述使用`HttpURLConnection`的写法总体来说还是比较复杂的，那么使用`OkHttp`会变得简单吗？答案是肯定的，而且要简单得多，下面我们来具体看一下。在`HttpUtil`中加入一个`sendOkHttpRequest()`方法，如下所示：

```kotlin
object HttpUtil {
    ...
    fun sendOkHttpRequest(address: String, callback: okhttp3.Callback) {
        val client = OkHttpClient()
        val request = Request.Builder()
            .url(address)
            .build()
        client.newCall(request).enqueue(callback)
    }
}
```

可以看到，`sendOkHttpRequest()`方法中有一个`okhttp3.Callback`参数，这个是`OkHttp`库中自带的回调接口，类似于我们刚才自己编写的`HttpCallbackListener`。然后在`client.newCall()`之后没有像之前那样一直调用`execute()`方法，而是调用了一个`enqueue()`方法，并把`okhttp3.Callback`参数传入。OkHttp`在`enqueue()`方法的内部已经帮我们开好子线程了，然后会在子线程中执行HTTP请求，并将最终的请求结果回调到`okhttp3.Callback`当中。**

那么我们在调用`sendOkHttpRequest()`方法的时候就可以这样写：

```kotlin
HttpUtil.sendOkHttpRequest(address, object : Callback {
    override fun onResponse(call: Call, response: Response) {
        // 得到服务器返回的具体内容
        TODO("Not yet implemented")
    }

    override fun onFailure(call: Call, e: IOException) {
        // 进行异常情况处理
        TODO("Not yet implemented")
    }
})
```

由此可以看出，`OkHttp`的接口设计得确实非常人性化，它将一些常用的功能进行了很好的封装，使得我们只需编写少量的代码就能完成较为复杂的网络操作。

另外，需要注意的是，不管是使用`HttpURLConnection`还是`OkHttp`，最终的回调接口都还是在子线程中运行的，因此我们不可以在这里执行任何的UI操作，除非借助`runOnUiThread()`方法来进行类型转换。

## 六、最好用的网络库： Retrofit

既然学习**Android**网络技术，那么就不得不提到`Retrofit`，因为它实在是太好用了。`Retrofit`同样是一款由**Square**公司开发的网络库，但是它和`OkHttp`的定位完全不同。`OkHttp`侧重的是底层通信的实现，而`Retrofit`侧重的是上层接口的封装。事实上，`Retrofit`就是**Square**公司在`OkHttp`的基础上进一步开发出来的应用层网络通信库，使得我们可以用更加面向对象的思维进行网络操作。`Retrofit`的项目主页地址是：[GitHub仓库](https://github.com/square/retrofit)

### 6.1 Retrofit的基本用法

`Retrofit`的设计基于以下几个事实：

1. 同一款应用程序中所发起的网络请求绝大多数指向的是同一个服务器域名。这个很好理解，因为任何公司的产品，客户端和服务器都是配套的，很难想象一个客户端一会去这个服务器获取数据，一会又要去另外一个服务器获取数据吧？
2. 另外，服务器提供的接口通常是可以根据功能来归类的。比如新增用户、修改用户数据、查询用户数据这几个接口就可以归为一类，上架新书、销售图书、查询可供销售图书这几个接口也可以归为一类。将服务器接口合理归类能够让代码结构变得更加合理，从而提高可阅读性和可维护性。
3. 最后，开发者肯定更加习惯于“调用一个接口，获取它的返回值”这样的编码方式，但当调用的是服务器接口时，却很难想象该如何使用这样的编码方式。其实大多数人并不关心网络的具体通信细节，但是传统网络库的用法却需要编写太多网络相关的代码。

而`Retrofit`的用法就是基于以上几点来设计的，首先我们可以配置好一个根路径，然后在指定服务器接口地址时只需要使用相对路径即可，这样就不用每次都指定完整的**URL**地址了。

另外，`Retrofit`允许我们对服务器接口进行归类，将功能同属一类的服务器接口定义到同一个接口文件当中，从而让代码结构变得更加合理。

最后，我们也完全不用关心网络通信的细节，只需要在接口文件中声明一系列方法和返回值，然后通过注解的方式指定该方法对应哪个服务器接口，以及需要提供哪些参数。当我们在程序中调用该方法时，`Retrofit`会自动向对应的服务器接口发起请求，并将响应的数据解析成返回值声明的类型。这就使得我们可以用更加面向对象的思维来进行网络操作。

`Retrofit`的基本设计思想差不多就是这些，下面就通过一个具体的例子来快速体验一下`Retrofit`的用法。

要想使用`Retrofit`，我们需要先在项目中添加必要的依赖库。编辑`app/build.gradle`文件，在**dependencies**闭包中添加如下内容：

```tex
dependencies {
    ...
    implementation("com.squareup.retrofit2:retrofit:2.9.0")
    implementation("com.squareup.retrofit2:converter-gson:2.9.0")
}
```

由于`Retrofit`是基于`OkHttp`开发的，因此添加上述第一条依赖会自动将`Retrofit`、`OkHttp`和`Okio`这几个库一起下载，我们无须再手动引入`OkHttp`库。另外，`Retrofit`还会将服务器返回的**JSON**数据自动解析成对象，因此上述第二条依赖就是一个`Retrofit`的转换库，它是借助`GSON`来解析**JSON**数据的，所以会自动将`GSON`库一起下载下来，这样我们也不用手动引入`GSON`库了。除了`GSON`之外，`Retrofit`还支持各种其他主流的**JSON**解析库，包括`Jackson`、`Moshi`等，不过毫无疑问`GSON`是最常用的。

我们使用之前提供的**JSON**数据接口。由于`Retrofit`会借助`GSON`将**JSON**数据转换成对象，因此这里同样需要新增一个**App**类，并加入**id**、**name**和**version**这3个字段，如下所示：

```kotlin
package work.icu007.retrofittest


/*
 * Author: Charlie_Liao
 * Time: 2024/2/19-15:23
 * E-mail: rookie_l@icu007.work
 */

class App (val id: String, val name: String, val version: String)
```

接下来，我们可以根据服务器接口的功能进行归类，创建不同种类的接口文件，并在其中定义对应具体服务器接口的方法。不过由于我们的**Apache**服务器上其实只有一个获取**JSON**数据的接口，因此这里只需要定义一个接口文件，并包含一个方法即可。新建`AppService`接口，代码如下所示：

```kotlin
package work.icu007.retrofittest

import retrofit2.Call
import retrofit2.http.GET


/*
 * Author: Charlie_Liao
 * Time: 2024/2/19-15:26
 * E-mail: rookie_l@icu007.work
 */

interface AppService {
    @GET("get_data.json")
    fun getAppData(): Call<List<App>>
}
```

通常`Retrofit`的接口文件建议以具体的功能种类名开头，并以`Service`结尾，这是一种比较好的命名习惯。

上述代码中有两点需要我们注意。第一就是在`getAppData()`方法上面添加的注解，这里使用了一个`@GET`注解，**表示当调用`getAppData()`方法时`Retrofit`会发起一条GET请求，请求的地址就是我们在`@GET`注解中传入的具体参数。**注意，**这里只需要传入请求地址的相对路径即可**，根路径我们会在稍后设置。

第二就是`getAppData()`方法的返回值必须声明成`Retrofit`中内置的**Call**类型，并通过泛型来指定服务器响应的数据应该转换成什么对象。由于服务器响应的是一个包含**App**数据的**JSON**数组，因此这里我们将泛型声明成`List<App>`。当然，`Retrofit`还提供了强大的**Call** **Adapters**功能来允许我们自定义方法返回值的类型，比如`Retrofit`结合`RxJava`使用就可以将返回值声明成`Observable`、`Flowable`等类型。

定义好了`AppService`接口之后，接下来的问题就是该如何使用它。为了方便测试，我们还得在界面上添加一个按钮才行。修改`activity_main.xml`中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <Button
        android:id="@+id/getAppDataBtn"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Get App Data"
        android:layout_margin="10dp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

修改**MainActivity**中的代码，如下所示：

```kotlin
package work.icu007.retrofittest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.util.Log
import retrofit2.Call
import retrofit2.Callback
import retrofit2.Response
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory
import work.icu007.retrofittest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)

        mainBinding.getAppDataBtn.setOnClickListener {
            val retrofit = Retrofit.Builder()
                .baseUrl("http://10.0.2.2:8088/")
                .addConverterFactory(GsonConverterFactory.create())
                .build()
            val appService = retrofit.create(AppService::class.java)
            appService.getAppData().enqueue(object : Callback<List<App>> {
                override fun onResponse(call: Call<List<App>>, response: Response<List<App>>) {
                    val list = response.body()
                    if (list != null) {
                        for (app in list) {
                            Log.d("MainActivity", "onResponse: id is ${app.id}")
                            Log.d("MainActivity", "onResponse: name is ${app.name}")
                            Log.d("MainActivity", "onResponse: version is ${app.version}")
                        }
                    }
                }

                override fun onFailure(call: Call<List<App>>, t: Throwable) {
                    t.printStackTrace()
                }

            })
        }
    }
}
```

在“Get App Data”按钮的点击事件当中，首先使用了`Retrofit.Builder`来构建一个`Retrofit`对象，**其中`baseUrl()`方法用于指定所有`Retrofit`请求的根路径，`addConverterFactory()`方法用于指定`Retrofit`在解析数据时所使用的转换库，这里指定成`GsonConverterFactory`。注意这两个方法都是必须调用的。**

有了`Retrofit`对象之后，我们就可以调用它的`create()`方法，并传入具体**Service**接口所对应的**Class**类型，创建一个该接口的动态代理对象。如果不熟悉什么是动态代理也没有关系，只需要知道**有了动态代理对象之后，我们就可以随意调用接口中定义的所有方法，而`Retrofit`会自动执行具体的处理**就可以了。

对应到上述的代码当中，当调用了`AppService`的`getAppData()`方法时，会返回一个`Call<List<App>>`对象，这时我们再调用一下它的`enqueue()`方法，`Retrofit`就会根据注解中配置的服务器接口地址去进行网络请求了，服务器响应的数据会回调到`enqueue()`方法中传入的**Callback**实现里面。需要注意的是，当发起请求的时候，`Retrofit`会自动在内部开启子线程，当数据回调到**Callback**中之后，`Retrofit`又会自动切换回主线程，整个操作过程中我们都不用考虑线程切换问题。在**Callback**的`onResponse()`方法中，调用`response.body()`方法将会得到`Retrofit`解析后的对象，也就是`List<App>`类型的数据，最后遍历List，将其中的数据打印出来即可。

这里使用的服务器接口仍然是HTTP，因此我们还要进行网络安全配置才行。

新建`network_config.xml`：

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true">
        <trust-anchors>
            <certificates src="system" />
        </trust-anchors>
    </base-config>
</network-security-config>
```

修改 `AndroidManifest.xml`如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">
    <uses-permission android:name="android.permission.INTERNET"/>
    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:networkSecurityConfig="@xml/network_config">
        ...
    </application>
</manifest>
```

### 6.2 处理复杂的接口地址类型

在上一小节中，我们通过示例程序向一个非常简单的服务器接口地址发送请求：<http://10.0.2.2/8088/get_data.json>，然而在真实的开发环境当中，服务器所提供的接口地址不可能一直如此简单。如果你在使用浏览器上网时观察一下浏览器上的网址，你会发现这些网址可能会是千变万化的，那么本小节我们就来学习一下如何使用`Retrofit`来应对这些千变万化的情况。

为了方便举例，这里先定义一个Data类，并包含id和content这两个字段，如下所示：

```kotlin
class Data(val id: String, val content: String)
```

然后我们先从最简单的看起，比如服务器的接口地址如下所示：

```kotlin
GET http://example.com/get_data.json
```

这是最简单的一种情况，接口地址是静态的，永远不会改变。那么对应到`Retrofit`当中，使用如下的写法即可：

```kotlin
interface ExampleService {
    @GET("get_data.json")
    fun getData(): Call<Data>
}
```

这也是我们在上一小节中已经学过的部分，理解起来应该非常简单吧。

但是显然服务器不可能总是给我们提供静态类型的接口，在很多场景下，接口地址中的部分内容可能会是动态变化的，比如如下的接口地址：

```kotlin
GET http://example.com/<page>/get_data.json
```

在这个接口当中，`<page>`部分代表页数，我们传入不同的页数，服务器返回的数据也会不同。这种接口地址对应到`Retrofit`当中应该怎么写呢？其实也很简单，如下所示：

```kotlin
interface ExampleService {
    @GET("{page}/get_data.json")
    fun getData(@Path("page") page: Int): Call<Data>
}
```

在`@GET`注解指定的接口地址当中，这里使用了一个`{page}`的占位符，然后又在`getData()`方法中添加了一个**page**参数，并使用`@Path("page")`注解来声明这个参数。这样当调用`getData()`方法发起请求时，`Retrofit`就会自动将**page**参数的值替换到占位符的位置，从而组成一个合法的请求地址。

另外，很多服务器接口还会要求我们传入一系列的参数，格式如下：

```kotlin
GET http://example.com/get_data.json?u<user>&t=<token>
```

这是一种标准的带参数**GET**请求的格式。**接口地址的最后使用问号来连接参数部分，每个参数都是一个使用等号连接的键值对，多个参数之间使用“&”符号进行分隔。**那么很显然，在上述地址中，服务器要求我们传入**user**和**token**这两个参数的值。对于这种格式的服务器接口，我们可以使用刚才所学的`@Path`注解的方式来解决，但是这样会有些麻烦，`Retrofit`针对这种带参数的**GET**请求，专门提供了一种语法支持：

```kotlin
interface ExampleService {
    @GET("get_data.json")
    fun getData(@Query("u") user: String, @Query("t") token: String): Call<Data>
}
```

这里在`getData()`方法中添加了**user**和**token**这两个参数，并使用`@Query`注解对它们进行声明。这样当发起网络请求的时候，`Retrofit`就会自动按照带参数**GET**请求的格式将这两个参数构建到请求地址当中。

学习了以上内容之后，现在你在一定程度上已经可以应对千变万化的服务器接口地址了。不过**HTTP**并不是只有**GET**请求这一种类型，而是有很多种，其中比较常用的有**GET**、**POST**、**PUT**、**PATCH**、**DELETE**这几种。它们之间的分工也很明确，简单概括的话，**GET**请求用于从服务器获取数据，**POST**请求用于向服务器提交数据，**PUT**和**PATCH**请求用于修改服务器上的数据，**DELETE**请求用于删除服务器上的数据。

而`Retrofit`对所有常用的**HTTP**请求类型都进行了支持，使用`@GET`、`@POST`、`@PUT`、`@PATCH`、`@DELETE`注解，就可以让`Retrofit`发出相应类型的请求了。

比如服务器提供了如下接口地址：

```kotlin
DELETE http://example.com/data/<id>
```

这种接口通常意味着要根据id删除一条指定的数据，而我们在`Retrofit`当中想要发出这种请求就可以这样写：

```kotlin
interface ExampleService {
    @DELETE("data/{id}")
    fun deleteData(@Path("id") id: String): Call<ResponseBody>
}
```

这里使用了`@DELETE`注解来发出**DELETE**类型的请求，并使用了`@Path`注解来动态指定**id**，这些都很好理解。但是在返回值声明的时候，我们将**Call**的泛型指定成了**ResponseBody**，这是什么意思呢？

由于**POST**、**PUT** 、**PATCH**、**DELETE**这几种请求类型与**GET**请求不同，它们更多是用于操作服务器上的数据，而不是获取服务器上的数据，所以通常它们对于服务器响应的数据并不关心。这个时候就可以使用**ResponseBody**，表示**Retrofit**能够接收任意类型的响应数据，并且不会对响应数据进行解析。

那么如果我们需要向服务器提交数据该怎么写呢？比如如下的接口地址：

```kotlin
POST http://example.com/data/create
{"id": 1, "content": "The description for this data."}
```

使用**POST**请求来提交数据，需要将数据放到**HTTP**请求的**body**部分，这个功能在`Retrofit`中可以借助`@Body`注解来完成：

```kotlin
interface ExampleService {
    @POST("data/create")
    fun createData(@Body data: Data): Call<ResponseBody>
}
```

可以看到，这里我们在`createData()`方法中声明了一个**Data**类型的参数，并给它加上了`@Body`注解。这样当`Retrofit`发出**POST**请求时，就会自动将**Data**对象中的数据转换成JSON格式的文本，并放到**HTTP**请求的**body**部分，服务器在收到请求之后只需要从**body**中将这部分数据解析出来即可。这种写法同样也可以用来给**PUT**、**PATCH**、**DELETE**类型的请求提交数据。

最后，有些服务器接口还可能会要求我们在**HTTP**请求的**header**中指定参数，比如：

```kotlin
GET http://example.com/get_data.json
User-Agent: okhttp
Cache-Control: max-age=0
```

这些**header**参数其实就是一个个的键值对，我们可以在`Retrofit`中直接使用`@Headers`注解来对它们进行声明。

```kotlin
interface ExampleService {
    @Header("User-Agent: okhttp", "Cache-Control: max-age=0")
    @GET("get_data.json")
    fun getData(): Call<Data>
}
```

但是这种写法只能进行静态**header**声明，如果想要动态指定**header**的值，则需要使用`@Header`注解，如下所示：

```kotlin
interface ExampleService {
    @GET("get_data.json")
    fun getData(@Header("User-Agent") userAgent: String,
        @Header("Cache-Control") cacheControl: String): Call <Data>
}
```

现在当发起网络请求的时候，`Retrofit`就会自动将参数中传入的值设置到`User-Agent`和`Cache-Control`这两个**header**当中，从而实现了动态指定**header**值的功能。

### 6.3 Retrofit构建器的最佳写法

学到这里，其实还有一个问题我们没有正视过，就是获取Service接口的动态代理对象实在是太麻烦了。先回顾一下之前的写法吧，大致代码如下所示：

```kotlin
val retrofit = Retrofit.Builder()
	.baseUrl("http:10.0.2.2/8088/")
	.addConverterFactory(GsonConverterFactory.create())
	.build()
val appService = retrofit.create(AppService::class.java)
```

我们想要得到`AppService`的动态代理对象，需要先使用`Retrofit.Builder`构建出一个`Retrofit`对象，然后再调用`Retrofit`对象的`create()`方法创建动态代理对象。如果只是写一次还好，每次调用任何服务器接口时都要这样写一遍的话，肯定没有人能受得了。

事实上，确实也没有每次都写一遍的必要，因为构建出的`Retrofit`对象是全局通用的，**只需要在调用`create()`方法时针对不同的Service接口传入相应的Class类型即可。**因此，我们可以将通用的这部分功能封装起来，从而简化获取**Service**接口动态代理对象的过程。

新建一个`ServiceCreator`单例类，代码如下所示：

```kotlin
object ServiceCreator {
    private const val BASE_URL = "http://10.0.2.2/8088"
    private val retrofit = Retrofit.Builder()
    	.baseUrl(BASE_URL)
		.addConverterFactory(GsonConverterFactory.create())
		.build()
    fun <T> create(serviceClass: Class<T>): T = retrofit.create(serviceClass)
}
```

这里我们使用object关键字让`ServiceCreator`成为了一个单例类，并在它的内部定义了一个`BASE_URL`常量，用于指定`Retrofit`的根路径。然后同样是在内部使用`Retrofit.Builder`构建一个`Retrofit`对象，注意这些都是用**private**修饰符来声明的，相当于对于外部而言它们都是不可见的。

最后，我们提供了一个外部可见的`create()`方法，并接收一个**Class**类型的参数。当在外部调用这个方法时，实际上就是调用了`Retrofit`对象的`create()`方法，从而创建出相应**Service**接口的动态代理对象。

经过这样的封装之后，`Retrofit`的用法将会变得异常简单，比如我们想获取一个`AppService`接口的动态代理对象，只需要使用如下写法即可：

```kotlin
val appService = ServiceCreator.create(AppService::class.java)
```

之后就可以随意调用`AppService`接口中定义的任何方法了。

上述代码通过之前学习的学习的泛型实化功能，还可以继续优化。修改`ServiceCreator`中的代码，如下所示：

```kotlin
object ServiceCreator {
    ...
    inline fun <reified T> create(): T = create(T::class.java)
}
```

可以看到，我们又定义了一个不带参数的`create()`方法，并使用**inline**关键字来修饰方法，使用**reified**关键字来修饰泛型，这是泛型实化的两大前提条件。接下来就可以使用`T::class.java`这种语法了，这里调用刚才定义的带有**Class**参数的`create()`方法即可。

那么现在我们就又有了一种新的方式来获取`AppService`接口的动态代理对象，如下所示：

```kotlin
val appService = ServiceCreator.create<AppService>()
```

