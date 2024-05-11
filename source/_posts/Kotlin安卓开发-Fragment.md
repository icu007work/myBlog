---
title: Kotlin安卓开发-Fragment
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
swiper_index: 1
top_group_index: 1
background: '#fff'
cover: >-
  https://pic2.ziyuan.wang/user/xiheya/2024/05/Android-Fragment_459fb5ae97b91.jpg
ai:
  - >-
    这篇文章是关于Android开发中Fragment的介绍和使用。Fragment是一种可以嵌入在Activity中的UI片段，它能让程序更加合理和充分地利用大屏幕的空间，因此在平板上应用得非常广泛。文章首先讨论了移动设备的发展，特别是手机和平板的屏幕大小差异，这可能会导致同样的界面在视觉效果上有较大的差异。因此，对于Android开发人员，能够兼顾手机和平板的开发是非常重要的。接着，文章介绍了Fragment的概念，它可以让界面在平板上更好地展示。Fragment和Activity非常相似，都能包含布局，都有自己的生命周期。最后，文章通过一个新闻应用的例子，解释了如何使用Fragment充分地利用平板屏幕的空间。在手机中，新闻标题列表和新闻的详细内容可以放在两个不同的Activity中。但在平板上，更好的设计方案是将新闻标题列表界面和新闻详细内容界面分别放在两个Fragment中，然后在同一个Activity里引入这两个Fragment，这样就可以将屏幕空间充分地利用起来。
abbrlink: 14654f7f
date: 2023-11-15 15:06:51
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


当今是移动设备发展非常迅速的时代，不仅手机已经成为了生活必需品，而且平板也变得越来越普及。平板和手机最大的区别就在于屏幕的大小：一般手机屏幕的大小在3英寸到6英寸之间，平板屏幕的大小在7英寸到10英寸之间。屏幕大小差距过大有可能会让同样的界面在视觉效果上有较大的差异，比如一些界面在手机上看起来非常美观，但在平板上看起来可能会有控件被过分拉长、元素之间空隙过大等情况。

对于一名专业的Android开发人员而言，能够兼顾手机和平板的开发是我们尽可能要做到的事情。Android自3.0版本开始引入了Fragment的概念，它可以让界面在平板上更好地展示，下面我们就一起来学习一下。

## 一、Fragment是什么

Fragment是一种可以嵌入在Activity当中的UI片段，它能让程序更加合理和充分地利用大屏幕的空间，因而在平板上应用得非常广泛。虽然Fragment对你来说是个全新的概念，但我相信你学习起来应该毫不费力，因为它和Activity实在是太像了，同样都能包含布局，同样都有自己的生命周期。你甚至可以将Fragment理解成一个迷你型的Activity，虽然这个迷你型的Activity有可能和普通的Activity是一样大的。

那么究竟要如何使用Fragment才能充分地利用平板屏幕的空间呢？想象我们正在开发一个新闻应用，其中一个界面使用RecyclerView展示了一组新闻的标题，当点击其中一个标题时，就打开另一个界面显示新闻的详细内容。如果是在手机中设计，我们可以将新闻标题列表放在一个Activity中，将新闻的详细内容放在另一个Activity中，如图所示。

![1699341239870.png](https://pic.ziyuan.wang/2023/11/07/xiheya_e90b2199c15a9.png)

可是如果在平板上也这么设计，那么新闻标题列表将会被拉长至填充满整个平板的屏幕，而新闻的标题一般不会太长，这样将会导致界面上有大量的空白区域。

![1699341321005.png](https://pic.ziyuan.wang/2023/11/07/xiheya_2563c28792cef.png)

因此，更好的设计方案是将新闻标题列表界面和新闻详细内容界面分别放在两个Fragment中，然后在同一个Activity里引入这两个Fragment，这样就可以将屏幕空间充分地利用起来了。

![1699341356299.png](https://pic.ziyuan.wang/2023/11/07/xiheya_e2a3cc9fdca8c.png)

## 二、Fragment的使用方式

### 2.1 Fragment的简单用法

这里我们准备先写一个最简单的Fragment示例来练练手。在一个Activity当中添加两个Fragment，并让这两个Fragment平分Activity的空间。

新建一个左侧Fragment的布局left_fragment.xml，代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"
        android:text="button"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

这个布局非常简单，只放置了一个按钮，并让它水平居中显示。

然后新建右侧Fragment的布局right_fragment.xml，代码如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/design_default_color_secondary_variant"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"
        android:text="This is right fragment"
        android:textSize="24sp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

可以看到，我们将这个布局的背景色设置成了绿色，并放置了一个TextView用于显示一段文本。

接下来介是在Fragment类中加载布局文件了：例如LeftFragment是这样加载的

```kotlin
    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.left_fragment, container, false)
    }
```

这里仅仅是重写了Fragment的onCreateView()方法，然后在这个方法中通过LayoutInflater的inflate()方法将刚才定义的left_fragment布局动态加载进来，整个方法简单明了。RightFragment也是使用同样的方法，就不贴代码了。

接下来修改activity_main.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <fragment
        android:id="@+id/leftFrag"
        android:name="work.icu007.fragmenttest.LeftFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/rightFrag"/>

    <fragment
        android:id="@+id/rightFrag"
        android:name="work.icu007.fragmenttest.RightFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        app:layout_constraintStart_toEndOf="@+id/leftFrag"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

可以看到，我们使用了<fragment>标签在布局中添加Fragment，只不过这里还需要通过android:name属性来显式声明要添加的Fragment类名，注意一定要将类的包名也加上。

![1699345080151.png](https://pic.ziyuan.wang/2023/11/07/xiheya_69f9e2caaa017.png)

### 2.2 动态添加Fragment

现在我们已经学会了在布局文件中添加Fragment的方法，不过Fragment真正的强大之处在于，它可以在程序运行时动态地添加到Activity当中。根据具体情况来动态地添加Fragment，这样就可以将程序界面定制得更加多样化。

继续新建another_right_fragment.xml，代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/material_dynamic_tertiary80"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="10dp"
        android:text="This is another right fragment"
        android:textSize="24sp"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

这个布局文件的代码和right_fragment.xml中的代码基本相同，只是将背景色改成了黄色，并将显示的文字改了改。然后新建AnotherRightFragment作为另一个右侧Fragment。

准备工作已经完成了，接下来就要实现将Fragment动态加载到Activity当中了。

修改activity_main.xml，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <fragment
        android:id="@+id/leftFrag"
        android:name="work.icu007.fragmenttest.LeftFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/rightLayout"/>

    <FrameLayout
        android:id="@+id/rightLayout"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        app:layout_constraintStart_toEndOf="@+id/leftFrag"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

可以看到，现在将右侧Fragment替换成了一个FrameLayout。这是Android中最简单的一种布局，所有的控件默认都会摆放在布局的左上角。由于这里仅需要在布局里放入一个Fragment，不需要任何定位，因此非常适合使用FrameLayout。

下面我们将在代码中向FrameLayout里添加内容，从而实现动态添加Fragment的功能。修改MainActivity中的代码

```kotlin
package work.icu007.fragmenttest

import android.app.Activity
import android.graphics.Color
import android.os.Build
import android.os.Bundle
import android.view.View
import android.widget.Button
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import work.icu007.fragmenttest.databinding.ActivityMainBinding


class MainActivity : AppCompatActivity() {
    private lateinit var viewBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        viewBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(viewBinding.root)
        val button: Button = findViewById(R.id.button)
        button.setOnClickListener {
            replaceFragment(AnotherRightFragment())
        }
        replaceFragment(RightFragment())
    }

    private fun replaceFragment(fragment: Fragment){
        val fragmentManager = supportFragmentManager
        val transaction = fragmentManager.beginTransaction()
        transaction.replace(R.id.rightLayout, fragment)
        transaction.commit()
    }
}
```

首先我们给左侧Fragment中的按钮注册了一个点击事件，然后调用replaceFragment()方法动态添加了RightFragment。当点击左侧Fragment中的按钮时，又会调用replaceFragment()方法，将右侧Fragment替换成AnotherRightFragment。结合replaceFragment()方法中的代码可以看出，动态添加Fragment主要分为5步。

1. 创建待添加Fragment的实例。
2. 获取FragmentManager，在Activity中可以直接调用getSupportFragmentManager()方法获取。
3. 开启一个事务，通过调用beginTransaction()方法开启。
4. 向容器内添加或替换Fragment，一般使用replace()方法实现，需要传入容器的id和待添加的Fragment实例。
5. 提交事务，调用commit()方法来完成。

### 2.3 在Fragment中实现返回栈

在上一小节中，我们成功实现了向Activity中动态添加Fragment的功能。不过你尝试一下就会发现，通过点击按钮添加了一个Fragment之后，这时按下Back键程序就会直接退出。如果我们想实现类似于返回栈的效果，按下Back键可以回到上一个Fragment，该如何实现呢？其实很简单，FragmentTransaction中提供了一个addToBackStack()方法，可以用于将一个事务添加到返回栈中。修改MainActivity中的代码，如下所示：

```kotlin
class MainActivity : AppCompatActivity() {
    ...

    private fun replaceFragment(fragment: Fragment){
        val fragmentManager = supportFragmentManager
        val transaction = fragmentManager.beginTransaction()
        transaction.replace(R.id.rightLayout, fragment)
        transaction.addToBackStack(null)
        transaction.commit()
    }
}
```

这里我们在事务提交之前调用了FragmentTransaction的addToBackStack()方法，它可以接收一个名字用于描述返回栈的状态，一般传入null即可。现在重新运行程序，并点击按钮将AnotherRightFragment添加到Activity中，然后按下Back键，你会发现程序并没有退出，而是回到了RightFragment界面。继续按下Back键，RightFragment界面也会消失，再次按下Back键，程序才会退出。

### 2.4 Fragment和Activity之间的交互

虽然Fragment是嵌入在Activity中显示的，可是它们的关系并没有那么亲密。实际上，Fragment和Activity是各自存在于一个独立的类当中的，它们之间并没有那么明显的方式来直接进行交互。如果想要在Activity中调用Fragment里的方法，或者在Fragment中调用Activity里的方法，应该如何实现呢？

为了方便Fragment和Activity之间进行交互，FragmentManager提供了一个类似于findViewById()的方法，专门用于从布局文件中获取Fragment的实例，代码如下所示：

```kotlin
val fragment = supportFragmentManager.findFragmentById(R.id.leftFrag) as LeftFragment
```

调用FragmentManager的findFragmentById()方法，可以在Activity中得到相应Fragment的实例，然后就能轻松地调用Fragment里的方法了。

掌握了如何在Activity中调用Fragment里的方法，那么在Fragment中又该怎样调用Activity里的方法呢？这就更简单了，在每个Fragment中都可以通过调用getActivity()方法来得到和当前Fragment相关联的Activity实例，代码如下所示：

```kotlin
if(activity != null){
    val mainActivity = activity as MainActivity
}
```

这里由于getActivity()方法有可能返回null，因此我们需要先进行一个判空处理。有了Activity的实例，在Fragment中调用Activity里的方法就变得轻而易举了。另外当Fragment中需要使用Context对象时，也可以使用getActivity()方法，因为获取到的Activity本身就是一个Context对象。

## 三、Fragment的生命周期

和Activity一样，Fragment也有自己的生命周期，并且它和Activity的生命周期实在是太像了。

### 3.1 Fragment的状态和回调

Activity在其生命周期一共有 **运行状态、暂停状态、停止状态和销毁状态**这4种。类似地，每个Fragment在其生命周期内也可能会经历这几种状态，只不过在一些细小的地方会有部分区别。



1. 运行状态

   当一个Fragment所关联的Activity正处于运行状态时，该Fragment也处于运行状态。

2. 暂停状态

   当一个Activity进入暂停状态时（由于另一个未占满屏幕的Activity被添加到了栈顶），与它相关联的Fragment就会进入暂停状态。

3. 停止状态

   当一个Activity进入停止状态时，与它相关联的Fragment就会进入停止状态，或者通过调用FragmentTransaction的remove()、replace()方法将Fragment从Activity中移除，但在事务提交之前调用了addToBackStack()方法，这时的Fragment也会进入停止状态。总的来说，进入停止状态的Fragment对用户来说是完全不可见的，有可能会被系统回收。

4. 销毁状态

   Fragment总是依附于Activity而存在，因此当Activity被销毁时，与它相关联的Fragment就会进入销毁状态。或者通过调用FragmentTransaction的remove()、replace()方法将Fragment从Activity中移除，但在事务提交之前并没有调用addToBackStack()方法，这时的Fragment也会进入销毁状态。

   同样地，Fragment类中也提供了一系列的回调方法，以覆盖它生命周期的每个环节。其中，Activity中有的回调方法，Fragment中基本上也有，不过Fragment还提供了一些附加的回调方法，下面我们就重点看一下这几个回调。

   - onAttach()：当Fragment和Activity建立关联时调用。
   - onCreateView()：为Fragment创建视图（加载布局）时调用。
   - onActivityCreated()：确保与Fragment相关联的Activity已经创建完毕时调用。
   - onDestroyView()：当与Fragment关联的视图被移除时调用。
   - onDetach()：当Fragment和Activity解除关联时调用。

   ![Fragment完整生命周期](https://pic.ziyuan.wang/2023/11/07/xiheya_553dc0b882a59.png)

### 3.2 体验Fragment的生命周期

为了能够更直观的体验Fragment生命周期，可以重写 RightFragment中的一系列回调方法来打印一些log。

接下来，我们在RightFragment中的每一个回调方法里都加入了打印日志的代码，然后重新运行程序。这时观察Logcat中的打印信息如图所示：

![1699408161113.png](https://pic.ziyuan.wang/2023/11/08/xiheya_b7c78bdee0659.png)

- 当RightFragment第一次被加载到屏幕上时，会依次执行onAttach()、onCreate()、onCreateView()、onActivityCreated()、onStart()和onResume()方法。

- 然后点击LeftFragment中的按钮，由AnotherRightFragment替换了RightFragment，此时的RightFragment进入了停止状态，因此onPause()、onStop()和onDestroyView()方法会得到执行。当然，如果在替换的时候没有调用addToBackStack()方法，此时的RightFragment就会进入销毁状态，onDestroy()和onDetach()方法就会得到执行。

- 接着按下Back键，RightFragment会重新回到屏幕，由于RightFragment重新回到了运行状态，因此onCreateView()、onActivityCreated()、onStart()和onResume()方法会得到执行。注意，此时onCreate()方法并不会执行，因为我们借助了addToBackStack()方法使得RightFragment并没有被销毁。

- 现在再次按下Back键，程序会依次执行onPause()、onStop()、onDestroyView()、onDestroy()和onDetach()方法，最终将Fragment销毁。

在Fragment中你也可以通过onSaveInstanceState()方法来保存数据，因为进入停止状态的Fragment有可能在系统内存不足的时候被回收。保存下来的数据在onCreate()、onCreateView()和onActivityCreated()这3个方法中你都可以重新得到，它们都含有一个Bundle类型的savedInstanceState参数。

## 四、动态加载布局的技巧

虽然动态添加Fragment的功能很强大，可以解决很多实际开发中的问题，但是它毕竟只是在一个布局文件中进行一些添加和替换操作。如果程序能够根据设备的分辨率或屏幕大小，在运行时决定加载哪个布局，那我们可发挥的空间就更多了。因此我们就来探讨一下Android中动态加载布局的技巧。

### 4.1 使用限定符

很多平板应用采用的是双页模式（程序会在左侧的面板上显示一个包含子项的列表，在右侧的面板上显示内容），因为平板的屏幕足够大，完全可以同时显示两页的内容，但手机的屏幕就只能显示一页的内容，因此两个页面需要分开显示。

那么怎样才能在运行时判断程序应该是使用双页模式还是单页模式呢？这就需要借助限定符（qualifier）来实现了。下面我们通过一个例子来学习一下它的用法，修改FragmentTest项目中的activity_main.xml文件，代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <fragment
        android:id="@+id/leftFrag"
        class="work.icu007.fragmenttest.LeftFragment"
        android:layout_width="0dp"
        android:layout_weight="1"
        android:layout_height="match_parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/rightLayout"/>

</LinearLayout>
```

这里将多余的代码删掉，只留下一个左侧Fragment，并让它充满整个父布局。接着在res目录下新建layout-large文件夹，在这个文件夹下新建一个布局，也叫作activity_main.xml，代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <fragment
        android:id="@+id/leftFrag"
        class="work.icu007.fragmenttest.LeftFragment"
        android:layout_width="0dp"
        android:layout_weight="1"
        android:layout_height="match_parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/rightFrag"/>


    <fragment
        android:id="@+id/rightFrag"
        android:name="work.icu007.fragmenttest.RightFragment"
        android:layout_width="0dp"
        android:layout_weight="3"
        android:layout_height="match_parent"
        app:layout_constraintStart_toEndOf="@+id/leftFrag"
        app:layout_constraintEnd_toEndOf="parent"/>

</LinearLayout>
```

可以看到，layout/activity_main布局只包含了一个Fragment，即单页模式，而layoutlarge/activity_main布局包含了两个Fragment，即双页模式。其中，large就是一个限定符，那些屏幕被认为是large的设备就会自动加载layout-large文件夹下的布局，小屏幕的设备则还是会加载layout文件夹下的布局。

这样就实现了在程序运行时动态加载布局的功能，安卓中常见限定符如表所示：

| 屏幕特征 | 限定符 |                     描述                      |
| :------: | :----: | :-------------------------------------------: |
|   大小   | small  |            提供给小屏幕设备的资源             |
|   大小   | normal |           提供给中等屏幕设备的资源            |
|   大小   | large  |            提供给大屏幕设备的资源             |
|   大小   | xlarge |           提供给超大屏幕设备的资源            |
|  分辨率  |  ldpi  |    提供给低分辨率设备的资源（120dpi以下）     |
|  分辨率  |  mdpi  |  提供给中等分辨率设备的资源（120dpi-160dpi）  |
|  分辨率  |  hdpi  |   提供给高分辨率设备的资源（160dpi-240dpi）   |
|  分辨率  | xhdpi  |  提供给超高分辨率设备的资源（240dpi-320dpi）  |
|  分辨率  | xxhdpi | 提供给超超高分辨率设备的资源（320dpi-480dpi） |
|   方向   |  land  |             提供给横屏设备的资源              |
|   方向   |  port  |             提供给竖屏设备的资源              |

### 4.2 使用最小宽度限定符

在上一小节中我们使用large限定符成功解决了单页双页的判断问题，不过很快又有一个新的问题出现了：large到底是指多大呢？有时候我们希望可以更加灵活地为不同设备加载布局，不管它们是不是被系统认定为large，这时就可以使用最小宽度限定符（smallest-width qualifier）。

最小宽度限定符允许我们对屏幕的宽度指定一个最小值（以dp为单位），然后以这个最小值为临界点，屏幕宽度大于这个值的设备就加载一个布局，屏幕宽度小于这个值的设备就加载另一个布局。

在res目录下新建layout-sw600dp文件夹，然后在这个文件夹下新建activity_main.xml布局，代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">


    <fragment
        android:id="@+id/leftFrag"
        class="work.icu007.fragmenttest.LeftFragment"
        android:layout_width="0dp"
        android:layout_weight="1"
        android:layout_height="match_parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toStartOf="@+id/rightFrag"/>


    <fragment
        android:id="@+id/rightFrag"
        android:name="work.icu007.fragmenttest.RightFragment"
        android:layout_width="0dp"
        android:layout_weight="3"
        android:layout_height="match_parent"
        app:layout_constraintStart_toEndOf="@+id/leftFrag"
        app:layout_constraintEnd_toEndOf="parent"/>

</LinearLayout>
```

这就意味着，当程序运行在屏幕宽度大于等于600 dp的设备上时，会加载layoutsw600dp/activity_main布局，当程序运行在屏幕宽度小于600 dp的设备上时，则仍然加载默认的layout/activity_main布局。

## 五、Fragment最佳实践：一个简易版的新闻应用

前面提到过，Fragment很多时候是在平板开发当中使用的，因为它可以解决屏幕空间不能充分利用的问题。那是不是就表明，我们开发的程序都需要提供一个手机版和一个平板版呢？确实有不少公司是这么做的，但是这样会耗费很多的人力物力财力。因为维护两个版本的代码成本很高：每当增加新功能时，需要在两份代码里各写一遍；每当发现一个bug时，需要在两份代码里各修改一次。因此，今天实践内容就是编写兼容手机和平板的应用程序。

首先我们要准备好一个新闻的实体类，新建类News，代码如下所示：

```kotlin
package work.icu007.fragmentbestpractice


/*
 * Author: Charlie_Liao
 * Time: 2023/11/8-14:07
 * E-mail: rookie_l@icu007.work
 */

class News(val title: String, val content: String)
```

News类的代码非常简单，title字段表示新闻标题，content字段表示新闻内容。接着新建NewsContentFragment类并编辑其布局文件fragment_news_content.xml，作为新闻内容的布局：

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".NewsContentFragment">

    <LinearLayout
        android:id="@+id/contentLayout"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        android:visibility="invisible">

        <TextView
            android:id="@+id/newsTitle"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:gravity="center"
            android:padding="10dp"
            android:textSize="25sp"/>

        <View
            android:layout_width="match_parent"
            android:layout_height="1dp"
            android:background="@color/black"/>

        <TextView
            android:layout_width="match_parent"
            android:layout_height="0dp"
            android:layout_weight="1"
            android:padding="15dp"
            android:textSize="18sp"/>

    </LinearLayout>

    <View
        android:layout_width="1dp"
        android:layout_height="match_parent"
        android:layout_alignParentLeft="true"
        android:background="@color/black"/>

</RelativeLayout>
```

新闻内容的布局主要可以分为两个部分：头部部分显示新闻标题，正文部分显示新闻内容，中间使用一条水平方向的细线分隔开。除此之外，这里还使用了一条垂直方向的细线，它的作用是在双页模式时将左侧的新闻列表和右侧的新闻内容分隔开。细线是利用View来实现的，将View的宽或高设置为1 dp，再通过background属性给细线设置一下颜色就可以了，这里我们把细线设置成黑色。

另外，我们还要将新闻内容的布局设置成不可见。因为在双页模式下，如果还没有选中新闻列表中的任何一条新闻，是不应该显示新闻内容布局的。

接下来新建一个NewsContentFragment类，继承自Fragment，代码如下所示：

```kotlin
package work.icu007.fragmentbestpractice

import android.os.Bundle
import android.util.Log
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import work.icu007.fragmentbestpractice.databinding.FragmentNewsContentBinding

// TODO: Rename parameter arguments, choose names that match
// the fragment initialization parameters, e.g. ARG_ITEM_NUMBER
private const val ARG_PARAM1 = "title"
private const val ARG_PARAM2 = "content"

/**
 * A simple [Fragment] subclass.
 * Use the [NewsContentFragment.newInstance] factory method to
 * create an instance of this fragment.
 */
class NewsContentFragment() : Fragment() {
    // TODO: Rename and change types of parameters
    private var param1: String? = null
    private var param2: String? = null

    private var _binding: FragmentNewsContentBinding? = null
    private val binding get() = _binding!!

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        arguments?.let {
            param1 = it.getString(ARG_PARAM1)
            param2 = it.getString(ARG_PARAM2)
            Log.d(TAG, "onCreate: title: $param1, content: $param2")
            Log.d(TAG, "onCreate: title: ${arguments?.getString("title")},content: ${arguments?.getString("content")}")
        }
    }

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        _binding = FragmentNewsContentBinding.inflate(inflater,container,false)
        val bundle = arguments
        val title = bundle?.getString("title")
        val content = bundle?.getString("content")
        Log.d(TAG, "onCreateView: title: $title, content: $content")
        if ( title != null && content!=null ) {
            refresh(title,content)
        }
        return binding.root
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }

    fun refresh(title: String, content: String){
        Log.d(TAG, "refresh: step in")
        binding.contentLayout.visibility = View.VISIBLE
        binding.newsTitle.text = title
        binding.newsContent.text = content
    }

    companion object {
        const val TAG = "NewsContentFragment"
        /**
         * Use this factory method to create a new instance of
         * this fragment using the provided parameters.
         *
         * @param param1 Parameter 1.
         * @param param2 Parameter 2.
         * @return A new instance of fragment NewsContentFragment.
         */
        // TODO: Rename and change types and number of parameters
        @JvmStatic
        fun newInstance(param1: String, param2: String): NewsContentFragment{
            val fragment = NewsContentFragment()
            val bundle = Bundle()
            bundle.putString("title",param1)
            bundle.putString("content",param2)
            Log.d(TAG, "newInstance: title: $param1, content: $param2")
//            fragment.arguments = bundle
            return fragment
        }
    }
}
```

这里首先在onCreateView()方法中加载了我们刚刚创建的news_content_frag布局，接下来又提供了一个refresh()方法，用于将新闻的标题和内容显示在我们刚刚定义的界面上。注意，当调用了refresh()方法时，需要将我们刚才隐藏的新闻内容布局设置成可见。

这样我们就把新闻内容的Fragment和布局都创建好了，但是它们都是在双页模式中使用的，如果想在单页模式中使用的话，我们还需要再创建一个Activity。右击com.example.fragmentbestpractice包→New→Activity→Empty Activity，新建一个NewsContentActivity，布局名就使用默认的activity_news_content即可。然后修改activity_news_content.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".NewsContentActivity">

    <fragment
        android:id="@+id/newsContentFrag"
        class="work.icu007.fragmentbestpractice.NewsContentFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

这里充分发挥了代码的复用性，直接在布局中引入了NewsContentFragment。这样相当于把news_content_frag布局的内容自动加了进来。

然后修改NewsContentActivity中的代码，如下所示：

```kotlin
package work.icu007.fragmentbestpractice

import android.content.Context
import android.content.Intent
import android.os.Bundle
import android.util.Log
import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.FragmentManager
import work.icu007.fragmentbestpractice.databinding.ActivityNewsContentBinding
import work.icu007.fragmentbestpractice.databinding.FragmentNewsContentBinding

class NewsContentActivity : AppCompatActivity() {
    private lateinit var binding: ActivityNewsContentBinding
    private lateinit var contentFragmentBinding: FragmentNewsContentBinding

    companion object{
        const val TAG = "NewsContentActivity"
        fun actionStart(context: Context, title: String, content: String){
            val intent = Intent(context, NewsContentActivity::class.java).apply{
                putExtra("news_title", title)
                putExtra("news_content", content)
                Log.d(TAG, "actionStart: title: $title, content: $content")
            }
            context.startActivity(intent)
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityNewsContentBinding.inflate(layoutInflater)
        setContentView(binding.root)

        val title = intent.getStringExtra("news_title")
        val content = intent.getStringExtra("news_content")
        Log.d("NewsContentActivity", "onCreate: title: $title, content: $content")

        if (title != null && content != null){
            val fragment = supportFragmentManager.findFragmentById(R.id.newsContentFrag) as NewsContentFragment
            fragment.refresh(title,content)
        }
    }
}
```

在onCreate()方法中我们通过Intent获取到了传入的新闻标题和新闻内容，然后使用kotlin-android-extensions插件提供的简洁写法得到了NewsContentFragment的实例，接着调用它的refresh()方法，将新闻的标题和内容传入，就可以把这些数据显示出来了。注意，这里我们还提供了一个actionStart()方法用于外部调用启动activity。

接下来还需要再创建一个用于显示新闻列表的布局，新建news_title_frag.xml

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/newsTitleRecyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        />

</LinearLayout>
```

这个布局的代码就非常简单了，里面只有一个用于显示新闻列表的RecyclerView。既然要用到RecyclerView，那么就必定少不了子项的布局。新建news_item.xml作为RecyclerView子项的布局，代码如下所示：

```xml
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/newsTitle"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:maxLines="1"
    android:ellipsize="end"
    android:textSize="18sp"
    android:paddingLeft="10dp"
    android:paddingRight="10dp"
    android:paddingTop="15dp"
    android:paddingBottom="15dp" />
```

子项的布局也非常简单，只有一个TextView。仔细观察TextView，你会发现其中有几个属性是我们之前没有学过的：android:padding表示给控件的周围加上补白，这样不至于让文本内容紧靠在边缘上；android:maxLines设置为1表示让这个TextView只能单行显示；android:ellipsize用于设定当文本内容超出控件宽度时文本的缩略方式，这里指定成end表示在尾部进行缩略。

既然新闻列表和子项的布局都已经创建好了，那么接下来我们就需要一个用于展示新闻列表的地方。这里新建NewsTitleFragment作为展示新闻列表的Fragment，代码如下所示：

```kotlin
import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import work.icu007.fragmentbestpractice.databinding.FragmentNewsContentBinding
import work.icu007.fragmentbestpractice.databinding.FragmentNewsTitleBinding

// TODO: Rename parameter arguments, choose names that match
// the fragment initialization parameters, e.g. ARG_ITEM_NUMBER
private const val ARG_PARAM1 = "param1"
private const val ARG_PARAM2 = "param2"

/**
 * A simple [Fragment] subclass.
 * Use the [NewsTitleFragment.newInstance] factory method to
 * create an instance of this fragment.
 */
class NewsTitleFragment : Fragment() {
    // TODO: Rename and change types of parameters
    private var param1: String? = null
    private var param2: String? = null
    private var isTwoPane = false
    private var _binding: FragmentNewsTitleBinding? = null
    private val binding get() = _binding!!
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        arguments?.let {
            param1 = it.getString(ARG_PARAM1)
            param2 = it.getString(ARG_PARAM2)
        }
    }

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        _binding = FragmentNewsTitleBinding.inflate(inflater, container, false)
        return binding.root
    }
    
    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)
        isTwoPane = activity?.findViewById<View>(R.id.newsContentLayout) != null
    }
}
```

NewsTitleFragment中并没有多少代码，在onCreateView()方法中加载了news_title_frag布局，这个没什么好说的。我们注意看一下onActivityCreated()方法，这个方法通过在Activity中能否找到一个id为newsContentLayout的View，来判断当前是双页模式还是单页模式，因此我们需要让这个id为newsContentLayout的View只在双页模式中才会出现。注意，由于在Fragment中调用getActivity()方法有可能返回null，所以在上述代码中我们使用了一个?.操作符来保证代码的安全性。

那么怎样才能实现让id为newsContentLayout的View只在双页模式中才会出现呢？其实并不复杂，只需要借助我们刚刚学过的限定符就可以了。首先修改activity_main.xml中的代码，如下所示：

```xml
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.fragment.app.FragmentContainerView
        android:id="@+id/newsTitleFrag"
        android:name="work.icu007.fragmentbestpractice.NewsTitleFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        />

</androidx.constraintlayout.widget.ConstraintLayout>
```

上述代码表示在单页模式下只会加载一个新闻标题的Fragment。

然后新建layout-sw600dp文件夹，在这个文件夹下再新建一个activity_main.xml文件，代码如下所示：

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >
    <fragment
        android:id="@+id/newsTitleFrag"
        android:name="work.icu007.fragmentbestpractice.NewsTitleFragment"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1" />
    <FrameLayout
        android:id="@+id/newsContentLayout"
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="3" >
        <androidx.fragment.app.FragmentContainerView
            android:id="@+id/newsContentFrag"
            android:name="work.icu007.fragmentbestpractice.NewsContentFragment"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />
    </FrameLayout>
</LinearLayout>
```

可以看出，在双页模式下，我们同时引入了两个Fragment，并将新闻内容的Fragment放在了一个FrameLayout布局下，而这个布局的id正是newsContentLayout。因此，能够找到这个id的时候就是双页模式，否则就是单页模式。

现在我们已经将绝大部分的工作完成了，但还剩下至关重要的一点，就是在NewsTitleFragment中通过RecyclerView将新闻列表展示出来。我们在NewsTitleFragment中新建一个内部类NewsAdapter来作为RecyclerView的适配器，如下所示：

```kotlin
package work.icu007.fragmentbestpractice

import android.os.Bundle
import androidx.fragment.app.Fragment
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.RecyclerView
import work.icu007.fragmentbestpractice.databinding.FragmentNewsContentBinding
import work.icu007.fragmentbestpractice.databinding.FragmentNewsTitleBinding

// TODO: Rename parameter arguments, choose names that match
// the fragment initialization parameters, e.g. ARG_ITEM_NUMBER
private const val ARG_PARAM1 = "param1"
private const val ARG_PARAM2 = "param2"

/**
 * A simple [Fragment] subclass.
 * Use the [NewsTitleFragment.newInstance] factory method to
 * create an instance of this fragment.
 */
class NewsTitleFragment : Fragment() {
    // TODO: Rename and change types of parameters
    private var param1: String? = null
    private var param2: String? = null
    private var isTwoPane = false
    private var _binding: FragmentNewsTitleBinding? = null
    private val binding get() = _binding!!


    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        arguments?.let {
            param1 = it.getString(ARG_PARAM1)
            param2 = it.getString(ARG_PARAM2)
        }
    }

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        _binding = FragmentNewsTitleBinding.inflate(inflater, container, false)
        return binding.root
    }

    override fun onActivityCreated(savedInstanceState: Bundle?) {
        super.onActivityCreated(savedInstanceState)
        isTwoPane = activity?.findViewById<View>(R.id.newsContentLayout) != null
        val layoutManager = LinearLayoutManager(activity)
        binding.newsTitleRecyclerView.layoutManager = layoutManager
        val adapter = NewsAdapter(getNews())
        binding.newsTitleRecyclerView.adapter = adapter
    }

    private fun getNews(): List<News> {
        val newsList = ArrayList<News>()
        for (i in 1..50) {
            val news = News("This is news title $i", getRandomLengthString("This is news content $i. "))
                    newsList.add(news)
        }
        return newsList
    }
    private fun getRandomLengthString(str: String): String {
        val n = (1..20).random()
        val builder = StringBuilder()
        repeat(n) {
            builder.append(str)
        }
        return builder.toString()
    }

    inner class NewsAdapter(val newsList: List<News>): RecyclerView.Adapter<NewsAdapter.ViewHolder>(){
        inner class ViewHolder(view: View): RecyclerView.ViewHolder(view){
            val newsTitle: TextView = view.findViewById(R.id.newsTitle)
        }

        override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
            val view = LayoutInflater.from(parent.context).inflate(R.layout.news_item,parent,false)
            val holder = ViewHolder(view)
            holder.itemView.setOnClickListener {
                val news = newsList[holder.bindingAdapterPosition]
                if(isTwoPane) {
                    val fragment = parentFragmentManager.findFragmentById(R.id.newsContentFrag) as NewsContentFragment
                    fragment.refresh(news.title, news.content)
                }else {
                    NewsContentActivity.actionStart(parent.context,news.title,news.content)
                }
            }
            return holder
        }

        override fun getItemCount(): Int {
            return newsList.size
        }

        override fun onBindViewHolder(holder: ViewHolder, position: Int) {
            val news = newsList[position]
            holder.newsTitle.text = news.title
        }
    }

    companion object {
        /**
         * Use this factory method to create a new instance of
         * this fragment using the provided parameters.
         *
         * @param param1 Parameter 1.
         * @param param2 Parameter 2.
         * @return A new instance of fragment NewsTitleFragment.
         */
        // TODO: Rename and change types and number of parameters
        @JvmStatic
        fun newInstance(param1: String, param2: String) =
            NewsTitleFragment().apply {
                arguments = Bundle().apply {
                    putString(ARG_PARAM1, param1)
                    putString(ARG_PARAM2, param2)
                }
            }
    }
}
```

之前我们都是将适配器写成一个独立的类，其实也可以写成内部类。这里写成内部类的好处就是可以直接访问NewsTitleFragment的变量，比如isTwoPane。onCreateViewHolder()方法中注册的点击事件，首先获取了点击项的News实例，然后通过isTwoPane变量判断当前是单页还是双页模式。如果是单页模式，就启动一个新的Activity去显示新闻内容；如果是双页模式，就更新NewsContentFragment里的数据。

，onActivityCreated()方法中添加了RecyclerView标准的使用方法。在Fragment中使用RecyclerView和在Activity中使用几乎是一模一样的。这里调用了getNews()方法来初始化50条模拟新闻数据，同样使用了一个getRandomLengthString()方法来随机生成新闻内容的长度，以保证每条新闻的内容差距比较大。

## 六、知识小结

### 6.1 在Activity中通过ViewBinding获取Fragment的引用

我们此时有一个activity，且布局文件如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".NewsContentActivity">

    <fragment
        android:id="@+id/newsContentFrag"
        class="work.icu007.fragmentbestpractice.NewsContentFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

那如何在Activity中使用 ViewBinding 获取 Fragment 的引用呢？其实很简单：

1. 首先确保你的项目已启用 ViewBinding。你的 build.gradle 文件应该包含以下内容：

   ```ini
   android {
       ...
       viewBinding {
           enabled = true
       }
   }
   ```

2. 在 NewsContentActivity 中使用 ViewBinding。确保你的 Activity 已经设置了 ViewBinding：

   ```kotlin
   import android.os.Bundle
   import androidx.appcompat.app.AppCompatActivity
   import androidx.fragment.app.commit
   import com.example.databinding.databinding.ActivityNewsContentBinding
   
   class NewsContentActivity : AppCompatActivity() {
   
       private lateinit var binding: ActivityNewsContentBinding
   
       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
   
           // 初始化 ViewBinding
           binding = ActivityNewsContentBinding.inflate(layoutInflater)
           val view = binding.root
           setContentView(view)
   
           // 获取 fragment 引用
           val newsContentFragment = supportFragmentManager.findFragmentById(R.id.newsContentFrag) as NewsContentFragment
   
           // 在这里可以使用 newsContentFragment 对象进行其他操作
       }
   }
   
   ```

   确保使用正确的 ViewBinding 类型，类名是根据你的布局文件名生成的，以 `Activity` 类名为前缀，去掉下划线并首字母大写。在这个例子中，假设布局文件名为 `activity_news_content.xml`，生成的类名为 `ActivityNewsContentBinding`。这样，你就可以在 `NewsContentActivity` 中通过 ViewBinding 获取到 `newsContentFrag` 的引用。

### 6.2 在另一个 Fragment 中获取到具有 id 为 `newsContentFrag` 的 Fragment

如果你想在另一个 Fragment 中获取到具有 id 为 `newsContentFrag` 的 Fragment，你可以使用 `parentFragmentManager` 或 `childFragmentManager`，具体取决于你在哪个 Fragment 中执行这个操作。

假设你要在另一个 Fragment 的 `onCreateView` 方法中获取 `newsContentFrag` 的引用，代码可能如下所示：

```kotlin
kotlinCopy codeimport android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import com.example.databinding.databinding.YourFragmentLayoutBinding

class YourOtherFragment : Fragment() {

    private lateinit var binding: YourFragmentLayoutBinding

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // 初始化 ViewBinding
        binding = YourFragmentLayoutBinding.inflate(inflater, container, false)
        val view = binding.root

        // 获取 newsContentFrag 的引用
        val newsContentFragment = parentFragmentManager.findFragmentById(R.id.newsContentFrag) as NewsContentFragment

        // 在这里可以使用 newsContentFragment 对象进行其他操作

        return view
    }
}
```

请注意，这里使用的是 `parentFragmentManager`。如果你的 `YourOtherFragment` 嵌套在另一个 Fragment 中，你可能需要使用 `childFragmentManager`。

确保你的布局文件名和 ViewBinding 类型与你的实际设置一致。在这个例子中，假设你的布局文件名为 `your_fragment_layout.xml`，生成的类名为 `YourFragmentLayoutBinding`。

### 6.3 使用Random函数来随机生成内容

如果我们需要生成一些重复内容，且重复内容次数随机。我们就可以这样写：首先通过`(1..20).random()`来获取一个1-20的随机数。然后再通过实例化一个 `StringBuilder()`对象，使用其 `append`方法重复添加n次相同的内容。

```kotlin
private fun getNews(): List<News> {
        val newsList = ArrayList<News>()
        for (i in 1..50) {
            val news = News("This is news title $i", getRandomLengthString("This is news content $i. "))
                    newsList.add(news)
        }
        return newsList
    }
    private fun getRandomLengthString(str: String): String {
        val n = (1..20).random()
        val builder = StringBuilder()
        repeat(n) {
            builder.append(str)
        }
        return builder.toString()
    }
```

