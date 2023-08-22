---
title: 分析Android项目运行
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva1.sinaimg.cn/large/87c01ec7gy1fsnqqsspxqj21kw0w0to4.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/4770439e8367f.jpg
abbrlink: 7d2e59d3
date: 2022-08-26 11:41:59
authorAbout:
authorDesc:
tags:
keywords:
description:
---

## 分析安卓项目如何运行

### AndroidManifest.xml

```xml
<activity
          android:name=".MainActivity"
          android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />
        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</activity>
```

这段话表示对HelloWorldActivity这个活动进行注册，<font color = "red"><strong> 没有 </strong></font> 在AndroidManifest.xml中<font color = "red"><strong> 注册的活动是不能使用的 </strong></font> 。其中<font color = "red"><strong> intent-filter里的两行代码尤为重要 </strong></font> 。

```xml
<action android:name="android.intent.action.MAIN" />
<category android:name="android.intent.category.LAUNCHER" />
```

这两行代码表示HelloWorldActivity是这个项目的主活动。

### MainActivity

活动是应用程序的门面，凡是在应用中看到的东西，都是放在活动中的。，所以我们看到的界面，就是MainActivity这个活动。

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }
}
```

MainActivity 继承自 AppCompatActivity，Activity是一个Android系统提供的活动基类，我们项目中所有活动都必须继承它或者它的子类才能拥有活动的特性（AppCompatActivity是Activity的子类）。

MainActivity有一个onCreate()方法,这个方法是每一个活动被创建时必定要执行的方法，但是也只有两行代码。并且我们并没有看到HelloWorld字样。

我们看不到HelloWorld的原因是因为Android程序设计是<font style="background:green" font color = white><strong> 逻辑与视图分离 </strong></font> ，不推荐直接在活动中直接编写界面。更多的做法是<font style="background:green" font color = white><strong> 在布局文件中编写界面，然后再在活动中引入进来 </strong></font> 。在onCreate()方法的第二行调用了<font style="background:green" font color = white><strong> setContentView(R.layout.activity_main)方法 </strong></font> ，这个方法给当前活动引入了一个布局，所以HelloWorld是在这里定义的。

其中布局文件都是定义在res/layout 目录下的，展开layout目录，可以看到：activity_main.xml文件，代码如下：

```xml
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Hello World!"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

用于在布局中显示文字的是一个TextView控件，而我们可以通过android:text="Hello World!"这句代码定义需要显示什么文字。



