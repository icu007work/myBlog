---
title: Kotlin安卓开发-Material
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
swiper_index: 1
top_group_index: 1
background: '#fff'
cover: 'https://pic2.ziyuan.wang/user/xiheya/2024/05/Material_c668a539b4551.jpg'
ai:
  - >-
    这篇文章主要介绍了Material Design的概念和在Android开发中的应用。Material
    Design是Google的设计工程师们基于传统优秀的设计原则，结合丰富的创意和科学技术所开发的一套全新的界面设计语言，包含了视觉、运动、互动效果等特性然而，Material
    Design的普及程度并不理想，因为它主要面向UI设计人员，而不是开发者。许多开发者可能对Material
    Design的界面和效果并不清楚，而且实现起来也困难。为了解决这个问题，Google在2015年的Google I/O大会上推出了Design
    Support库，这个库将Material Design中最具代表性的一些控件和效果进行了封装，使得开发者即使在不了解Material
    Design的情况下，也能非常轻松地将自己的应用Material化。后来，Design
    Support库改名为Material库，用于给Google全平台类的产品提Material
    Design的支持。文章接下来介绍了Toolbar，这是AndroidX库提供的一个控件，是Material
    Design的一部分。Toolbar是一个比ActionBar更加灵活的控件，因为ActionBar由于设计的因，只能位于Activity的顶部，不能实现一些Material
    Design的效果，因此官方现在已经不再建议使用ActionBar，而是推荐使用Toolbar。
abbrlink: a3762f06
date: 2024-02-27 15:34:17
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

## 一、什么是Material Design

**Material Design**是由**Google**的设计工程师们基于传统优秀的设计原则，结合丰富的创意和科学技术所开发的一套全新的界面设计语言，包含了视觉、运动、互动效果等特性。

不过，在重磅推出之后，**Material Design**的普及程度却不是特别理想。因为这只是一个推荐的设计规范，主要是面向UI设计人员的，而不是面向开发者的。很多开发者可能根本就搞不清楚什么样的界面和效果才叫**Material Design**，就算搞清楚了，实现起来也会很费劲，因为不少**Material Design**的效果是很难实现的，而**Android**中几乎没有提供相应的**API**支持，基本需要靠开发者自己从零写起。

**Google**当然意识到了这个问题，于是在**2015年的Google I/O大会上推出了一个DesignSupport库**，这个库将**Material Design**中最具代表性的一些控件和效果进行了封装，使得开发者即使在不了解**Material Design**的情况下，也能非常轻松地将自己的应用**Material**化。后来**Design Support**库又改名成了**Material**库，用于给**Google**全平台类的产品提供**Material Design**的支持。这里我们就将对**Material**库进行深入的学习，并且配合**AndroidX**库中的一些控件来完成一个优秀的**Material Design**应用。

## 二、Toolbar

Toolbar将会是我们本章接触的第一个控件，是由AndroidX库提供的。虽说对于Toolbar暂时比较陌生的，但是对于它的另一个相关控件ActionBar，肯定再熟悉不过了。不过ActionBar由于其设计的原因，被限定只能位于Activity的顶部，从而不能实现一些Material Design的效果，因此官方现在已经不再建议使用ActionBar了。接下来也不再介绍ActionBar的用法了，而是直接讲解现在更加推荐使用的Toolbar。

Toolbar的强大之处在于，它不仅继承了ActionBar的所有功能，而且灵活性很高，可以配合其他控件完成一些Material Design的效果，下面我们就来具体学习一下。

首先，任何一个新建的项目，默认都是会显示ActionBar的。那么这个ActionBar到底是从哪里来的呢？其实这是根据项目中指定的主题来显示的。打开AndroidManifest.xml文件看一下，如下所示：

```xml
<application
    android:allowBackup="true"
    android:dataExtractionRules="@xml/data_extraction_rules"
    android:fullBackupContent="@xml/backup_rules"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true"
    android:networkSecurityConfig="@xml/network_config"
    android:theme="@style/Theme.RetrofitTest"
    tools:targetApi="31">
    ...
</application>
```

可以看到，这里使用`android:theme`属性指定了一个**AppTheme**的主题。那么这个AppTheme又是在哪里定义的呢？打开`res/values/theme.xml`文件，代码如下所示：

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->
    <style name="Base.Theme.RetrofitTest" parent="Theme.AppCompat.Light.DarkActionBar">
        <!-- Customize your light theme here. -->
        <!-- <item name="colorPrimary">@color/my_light_primary</item> -->
    </style>

    <style name="Theme.RetrofitTest" parent="Base.Theme.RetrofitTest" />
</resources>
```

这里定义了一个叫 `Theme.RetrofitTest`的主题，其 **parent**主题是 `Base.Theme.RetrofitTest`，而 `Base.Theme.RetrofitTest`的 **parent**主题是 `Theme.AppCompat.Light.DarkActionBar` 这个DarkActionBar是一个深色的ActionBar主题，我们之前所有的项目中自带的ActionBar就是因为指定了这个主题才出现的。

而现在我们准备使用Toolbar来替代ActionBar，因此需要指定一个不带ActionBar的主题，通常有`Theme.AppCompat.NoActionBar` 和`Theme.AppCompat.Light.NoActionBar`这两种主题可选。**其中`Theme.AppCompat.NoActionBar`表示深色主题，它会将界面的主体颜色设成深色，陪衬颜色设成浅色。而`Theme.AppCompat.Light.NoActionBar`表示浅色主题，它会将界面的主体颜色设成浅色，陪衬颜色设成深色。**

接下来看一看如何使用Toolbar来替代ActionBar。修改activity_main.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.appcompat.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize"
        android:background="@color/colorPrimary"
        app:layout_constraintTop_toTopOf="parent"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
        app:popupTheme="@style/Theme.AppCompat.Light"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

虽然这段代码不长，但是里面着实有不少技术点是需要我们仔细琢磨一下的。首先看一下第4行，这里使用`xmlns:app`指定了一个新的命名空间。思考一下，正是由于每个布局文件都会使用`xmlns:android`来指定一个命名空间，我们才能一直使用`android:id`、`android:layout_width`等写法。这里指定了`xmlns:app`，也就是说现在可以使用`app:attribute`这样的写法了。但是为什么这里要指定一个`xmlns:app`的命名空间呢？这是由于许多**Material**属性是在新系统中新增的，老系统中并不存在，那么为了能够兼容老系统，我们就不能使用`android:attribute`这样的写法了，而是应该使用`app:attribute`。

接下来定义了一个**Toolbar**控件，这个控件是由**appcompat**库提供的。这里我们给**Toolbar**指定了一个**id**，将它的宽度设置为**match_parent**，高度设置为**actionBar**的高度，背景色设置为**colorPrimary**。不过下面的部分就稍微有点难理解了，**如果在`styles.xml`中将程序的主题指定成了浅色主题，因此Toolbar现在也是浅色主题，那么Toolbar上面的各种元素就会自动使用深色系，从而和主体颜色区别开。但是之前使用ActionBar时文字都是白色的，现在变成黑色的会很难看。那么为了能让Toolbar单独使用深色主题，这里我们使用了`android:theme`属性，将Toolbar的主题指定成了`ThemeOverlay.AppCompat.Dark.ActionBar`。但是这样指定之后又会出现新的问题，如果Toolbar中有菜单按钮，那么弹出的菜单项也会变成深色主题，这样就再次变得十分难看了，于是这里又使用了`app:popupTheme`属性，单独将弹出的菜单项指定成了浅色主题。**

写完了布局，接下来我们修改MainActivity，代码如下所示：

```kotlin
package work.icu007.materialtes

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import work.icu007.materialtes.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        setSupportActionBar(mainBinding.toolbar)
    }
}
```

这里关键的代码只有一句，调用`setSupportActionBar()`方法并将**Toolbar**的实例传入，这样我们就做到既使用了**Toolbar**，又让它的外观与功能都和**ActionBar**一致了。

接下来我们再学习一些**Toolbar**比较常用的功能吧，比如修改标题栏上显示的文字内容。这段文字内容是在`AndroidManifest.xml`中指定的，如下所示：

```xml
<application
    android:allowBackup="true"
    android:dataExtractionRules="@xml/data_extraction_rules"
    android:fullBackupContent="@xml/backup_rules"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true"
    android:theme="@style/Theme.MaterialTes"
    tools:targetApi="31">
    <activity
        android:name=".MainActivity"
        android:label="Fruits">
        ...
    </activity>
</application>
```

这里给**activity**增加了一个`android:label`属性，用于指定在**Toolbar**中显示的文字内容，如果没有指定的话，会默认使用**application**中指定的**label**内容，也就是我们的应用名称。

不过只有一个标题的**Toolbar**看起来太单调了，我们还可以再添加一些**action**按钮来让**Toolbar**更加丰富一些。。现在`右击res目录→New→Directory`，创建一个**menu**文件夹。然后`右击menu文件夹→New→Menu resource file`，创建一个`toolbar.xml`文件，并编写如下代码：

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/backup"
        android:icon="@drawable/ic_backup"
        android:title="Backup"
        app:showAsAction="always"/>
    <item
        android:id="@+id/delete"
        android:icon="@drawable/ic_delete"
        android:title="Delete"
        app:showAsAction="always"/>
    <item
        android:id="@+id/settings"
        android:icon="@drawable/ic_settings"
        android:title="Settings"
        app:showAsAction="never"/>

</menu>
```

可以看到，我们通过`<item>`标签来定义**action**按钮，`android:id`用于指定按钮的**id**，`android:icon`用于指定按钮的**图标**，`android:title`用于指定按钮的**文字**。

接着使用`app:showAsAction`来指定按钮的显示位置，这里之所以再次使用了app命名空间，同样是为了能够兼容低版本的系统。**showAsAction**主要有以下几种值可选：**always表示永远显示在Toolbar中，如果屏幕空间不够则不显示；ifRoom表示屏幕空间足够的情况下显示在Toolbar中，不够的话就显示在菜单当中；never则表示永远显示在菜单当中。**注意，**Toolbar中的action按钮只会显示图标，菜单中的action按钮只会显示文字。**

```kotlin
package work.icu007.materialtes

class MainActivity : AppCompatActivity() {
    ...

    override fun onCreateOptionsMenu(menu: Menu?): Boolean {
        menuInflater.inflate(R.menu.toolbar,menu)
        return true
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when (item.itemId) {
            R.id.backup -> Toast.makeText(this, "u clicked buck up", Toast.LENGTH_SHORT).show()
            R.id.delete -> Toast.makeText(this, "u clicked delete", Toast.LENGTH_SHORT).show()
            R.id.settings -> Toast.makeText(this, "u clicked settings", Toast.LENGTH_SHORT).show()
        }
        return true
    }
}
```

## 三、滑动菜单

### 3.1 DrawerLayout

所谓的滑动菜单，就是将一些菜单选项隐藏起来，而不是放置在主屏幕上，然后可以通过滑动的方式将菜单显示出来。这种方式既节省了屏幕空间，又实现了非常好的动画效果，是`Material Design`中推荐的做法。

不过，如果我们全靠自己去实现上述功能的话，难度恐怕就很大了。幸运的是，Google在AndroidX库中提供了一个DrawerLayout控件，借助这个控件，实现滑动菜单简单又方便。

先来简单介绍一下DrawerLayout的用法吧。首先它是一个布局，在布局中允许放入两个直接子控件：第一个子控件是主屏幕中显示的内容，第二个子控件是滑动菜单中显示的内容。因此，我们就可以对activity_main.xml中的代码做如下修改：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="@color/colorPrimary"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
    </FrameLayout>

    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:background="#FFF"
        android:text="This is menu"
        android:textSize="30sp" />


</androidx.drawerlayout.widget.DrawerLayout>
```

可以看到，这里最外层的控件使用了`DrawerLayout`。`DrawerLayout`中放置了两个直接子控件：第一个子控件是`FrameLayout`，用于作为主屏幕中显示的内容，当然里面还有我们刚刚定义的Toolbar；第二个子控件是一个TextView，用于作为滑动菜单中显示的内容，其实使用什么都可以，`DrawerLayout`并没有限制只能使用固定的控件。

但是关于第二个子控件有一点需要注意，`layout_gravity`这个属性是必须指定的，因为我们需要告诉`DrawerLayout`滑动菜单是在屏幕的左边还是右边，指定left表示滑动菜单在左边，指定right表示滑动菜单在右边。这里我指定了start，表示会根据系统语言进行判断，如果系统语言是从左往右的，比如英语、汉语，滑动菜单就在左边，如果系统语言是从右往左的，比如阿拉伯语，滑动菜单就在右边。

向左滑动菜单，或者点击一下菜单以外的区域，都可以让滑动菜单关闭，从而回到主界面。无论是展示还是隐藏滑动菜单，都有非常流畅的动画过渡。可以看到，我们只是稍微改动了一下布局文件，就能实现如此炫酷的效果，是不是觉得挺激动呢？ 不过现在的滑动菜单还有点问题，因为只有在屏幕的左侧边缘进行拖动时才能将菜单拖出来，而很多用户可能根本就不知道有这个功能，那么该怎么提示他们呢？

`Material Design`建议的做法是在**Toolbar**的最左边加入一个导航按钮，点击按钮也会将滑动菜单的内容展示出来。这样就相当于给用户提供了两种打开滑动菜单的方式，防止一些用户不知道屏幕的左侧边缘是可以拖动的。

下面我们来实现这个功能。修改MainActivity中的代码，如下所示：

```kotlin
package work.icu007.materialtes

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        setSupportActionBar(mainBinding.toolbar)
        supportActionBar?.let {
            it.setDisplayHomeAsUpEnabled(true)
            it.setHomeAsUpIndicator(R.drawable.ic_menu)
        }
    }

    ...
    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when (item.itemId) {
            ...
            android.R.id.home -> mainBinding.drawerLayout.openDrawer(GravityCompat.START)
        }
        return true
    }
}
```

首先调用`getSupportActionBar()`方法得到了**ActionBar**的实例，虽然这个**ActionBar**的具体实现是由**Toolbar**来完成的。接着在**ActionBar**不为空的情况下调用`setDisplayHomeAsUpEnabled()`方法让导航按钮显示出来，调用`setHomeAsUpIndicator()`方法来设置一个导航按钮图标。实际上，**Toolbar**最左侧的这个按钮就叫作**Home**按钮，它默认的图标是一个返回的箭头，含义是返回上一个**Activity**。很明显，这里我们将它默认的样式和作用都进行了修改。

接下来，在`onOptionsItemSelected()`方法中对**Home**按钮的点击事件进行处理，**Home**按钮的id永远都是`android.R.id.home`。然后调用DrawerLayout的`openDrawer()`方法将滑动菜单展示出来，注意，`openDrawer()`方法要求传入一个**Gravity**参数，为了保证这里的行为和XML中定义的一致，我们传入了`GravityCompat.START`。

### 3.2 NavigationView

我们可以在滑动菜单页面定制任意的布局，不过Google给我们提供了一种更好的方法——使用`NavigationView`。`NavigationView`是**Material**库中提供的一个控件，它不仅是严格按照`Material Design`的要求来设计的，而且可以将滑动菜单页面的实现变得非常简单。接下来我们就学习一下`NavigationView`的用法。

首先，既然这个控件是**Material**库中提供的，那么我们就需要将这个库引入项目中才行。打开`app/build.gradle`文件，在**dependencies**闭包中添加如下内容：

```tex
dependencies {
    ...
    implementation("com.google.android.material:material:1.11.0")
    implementation("de.hdodenhof:circleimageview:3.1.0")
}
```

这里添加了两行依赖关系：第一行就是**Material**库，第二行是一个开源项目`CircleImageView`，它可以用来轻松实现图片圆形化的功能，我们待会就会用到它。

需要注意的是，当你引入了**Material**库之后，还需要将res/values/styles.xml文件中AppTheme的parent主题改成`Theme.MaterialComponents.Light.NoActionBar`，否则在使用接下来的一些控件时可能会遇到崩溃问题。

在开始使用`NavigationView`之前，我们还需要准备好两个东西：**menu和headerLayout。menu是用来在NavigationView中显示具体的菜单项的，headerLayout则是用来在NavigationView中显示头部布局的。**

先来准备menu。`右击menu文件夹→New→Menu resource file`，创建一个`nav_menu.xml`文件，并编写如下代码：

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
    <group android:checkableBehavior="single">
        <item
            android:id="@+id/navCall"
            android:icon="@drawable/nav_call"
            android:title="Call" />
        <item
            android:id="@+id/navFriends"
            android:icon="@drawable/nav_friends"
            android:title="Friends" />
        <item
            android:id="@+id/navLocation"
            android:icon="@drawable/nav_location"
            android:title="Location" />
        <item
            android:id="@+id/navMail"
            android:icon="@drawable/nav_mail"
            android:title="Mail" />
        <item
            android:id="@+id/navTask"
            android:icon="@drawable/nav_task"
            android:title="Tasks" />
    </group>
</menu>
```

我们首先在`<menu>`中嵌套了一个`<group>`标签，然后将group的`checkableBehavior`属性指定为**single**。**group表示一个组，`checkableBehavior`指定为single表示组中的所有菜单项只能单选。**

下面我们来看一下这些菜单项吧。这里一共定义了5个item，分别使用`android:id`属性指定菜单项的id，`android:icon`属性指定菜单项的图标，`android:title`属性指定菜单项显示的文字。就是这么简单，现在我们已经把menu准备好了。接下来应该准备`headerLayout`了，这是一个可以随意定制的布局，不过我并不想将它做得太复杂。这里简单起见，我们就在`headerLayout`中放置头像、用户名、邮箱地址这3项内容吧。

然后`右击layout文件夹→New→Layout resource file`，创建一个`nav_header.xml`文件。修改其中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="180dp"
    android:padding="10dp"
    android:background="@color/colorPrimary">

    <de.hdodenhof.circleimageview.CircleImageView
        android:id="@+id/iconImage"
        android:layout_width="70dp"
        android:layout_height="70dp"
        android:src="@drawable/nav_icon"
        android:layout_centerInParent="true" />
    <TextView
        android:id="@+id/mailText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:text="tonygreendev@gmail.com"
        android:textColor="#FFF"
        android:textSize="14sp" />
    <TextView
        android:id="@+id/userText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_above="@id/mailText"
        android:text="Tony Green"
        android:textColor="#FFF"
        android:textSize="14sp" />

</RelativeLayout>
```

可以看到，布局文件的最外层是一个`RelativeLayout`，我们将它的宽度设为`match_parent`，高度设为180 dp，这是一个`NavigationView`比较适合的高度，然后指定它的背景色为colorPrimary。

在`RelativeLayout`中我们放置了3个控件，`CircleImageView`是一个用于将图片圆形化的控件，它的用法非常简单，基本和ImageView是完全一样的，这里给它指定了一张图片作为头像，然后设置为居中显示。另外两个TextView分别用于显示用户名和邮箱地址，它们都用到了一些`RelativeLayout`的定位属性，相信肯定难不倒你吧？

现在menu和headerLayout都准备好了，我们终于可以使用`NavigationView`了。修改`activity_main.xml`中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="@color/colorPrimary"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
    </FrameLayout>

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/navView"
        android:layout_height="match_parent" 
        android:layout_width="match_parent"
        android:layout_gravity="start"
        app:menu="@menu/nav_menu"
        app:headerLayout="@layout/nav_header"/>


</androidx.drawerlayout.widget.DrawerLayout>
```

可以看到，我们将之前的TextView换成了`NavigationView`，这样滑动菜单中显示的内容也就变成`NavigationView`了。这里又通过`app:menu`和`app:headerLayout`属性将我们刚才准备好的menu和headerLayout设置了进去，这样`NavigationView`就定义完成了。`NavigationView`虽然定义完成了，但是我们还要处理菜单项的点击事件才行。修改MainActivity中的代码，如下所示：

```kotlin
package work.icu007.materialtes

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        setSupportActionBar(mainBinding.toolbar)
        supportActionBar?.let {
            it.setDisplayHomeAsUpEnabled(true)
            it.setHomeAsUpIndicator(R.drawable.ic_menu)
        }
        mainBinding.navView.setCheckedItem(R.id.navCall)
        mainBinding.navView.setNavigationItemSelectedListener {
            mainBinding.drawerLayout.closeDrawers()
            true
        }
    }

    ...
}
```

代码还是比较简单的，这里我们首先调用了`NavigationView`的`setCheckedItem()`方法将Call菜单项设置为默认选中。接着调用了`setNavigationItemSelectedListener()`方法来设置一个菜单项选中事件的监听器，当用户点击了任意菜单项时，就会回调到传入的Lambda表达式当中，我们可以在这里编写具体的逻辑处理。**这里调用了`DrawerLayout`的`closeDrawers()`方法将滑动菜单关闭，并返回true表示此事件已被处理。**

## 四、悬浮按钮和可交互提示

立面设计是`Material Design`中一条非常重要的设计思想，也就是说，按照`Material Design`的理念，应用程序的界面不仅仅是一个平面，而应该是有立体效果的。在官方给出的示例中，最简单且最具代表性的立面设计就是悬浮按钮了，这种按钮不属于主界面平面的一部分，而是位于另外一个维度的，因此就会给人一种悬浮的感觉。

本节中我们会对这个悬浮按钮的效果进行学习，另外还会学习一种可交互式的提示工具。关于提示工具，我们之前一直使用的是Toast，但是Toast只能用于告知用户某事已经发生了，用户却不能对此做出任何的响应，那么今天我们就将在这一方面进行扩展。

### 4.1 FloatingActionButton

`FloatingActionButton`是**Material**库中提供的一个控件，这个控件可以帮助我们比较轻松地实现悬浮按钮的效果。其实在之前的图12.2中，我们就已经预览过悬浮按钮的样子了，它默认会使用`colorAccent`作为按钮的颜色，我们还可以通过给按钮指定一个图标来表明这个按钮的作用是什么。

然后修改`activity_main.xml`中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">
        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="@color/colorPrimary"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
        <com.google.android.material.floatingactionbutton.FloatingActionButton
            android:id="@+id/fab"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="bottom|end"
            android:layout_margin="16dp"
            android:src="@drawable/ic_done"/>
    </FrameLayout>

    <com.google.android.material.navigation.NavigationView
        android:id="@+id/navView"
        android:layout_height="match_parent"
        android:layout_width="match_parent"
        android:layout_gravity="start"
        app:menu="@menu/nav_menu"
        app:headerLayout="@layout/nav_header"/>


</androidx.drawerlayout.widget.DrawerLayout>
```

可以看到，这里我们在主屏幕布局中加入了一个`FloatingActionButton`。这个控件的用法并没有什么特别的地方，layout_width和layout_height属性都指定成wrap_content，layout_gravity属性指定将这个控件放置于屏幕的右下角。其中end的工作原理和之前的start是一样的，即如果系统语言是从左往右的，那么end就表示在右边，如果系统语言是从右往左的，那么end就表示在左边。然后通过`layout_margin`属性给控件的四周留点边距，紧贴着屏幕边缘肯定是不好看的，最后通过`src`属性给`FloatingActionButton`设置了一个图标。

`FloatingActionButton`是悬浮在当前界面上的，既然是悬浮，那么理所应当会有投影，**Material**库连这种细节都帮我们考虑到了。说到悬浮，其实我们还可以指定`FloatingActionButton`的悬浮高度，如下所示：

```xml
<com.google.android.material.floatingactionbutton.FloatingActionButton
	android:id="@+id/fab"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:layout_gravity="bottom|end"
	android:layout_margin="16dp"
	android:src="@drawable/ic_done"
	app:elevation="8dp" />
```

这里使用`app:elevation`属性给`FloatingActionButton`指定一个高度值。高度值越大，投影范围也越大，但是投影效果越淡；高度值越小，投影范围也越小，但是投影效果越浓。当然这些效果的差异其实并不怎么明显。

接下来我们看一下`FloatingActionButton`是如何处理点击事件的，毕竟，一个按钮首先要能点击才有意义。修改`MainActivity`中的代码，如下所示：

```kotlin
package work.icu007.materialtes

class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        ...
        mainBinding.fab.setOnClickListener {
            Toast.makeText(this, "FAB clicked", Toast.LENGTH_SHORT).show()
        }
    }

    ...
}
```

`FloatingActionButton`并没有什么特殊用法，它和普通的**Button**其实没什么两样，都是调用`setOnClickListener()`方法来设置按钮的点击事件，这里我们只是弹出了一个Toast。

### 4.2 Snackbar

首先我们要明确，Snackbar并不是Toast的替代品，它们有着不同的应用场景。Toast的作用是告诉用户现在发生了什么事情，但用户只能被动接收这个事情，因为没有什么办法能让用户进行选择。而Snackbar则在这方面进行了扩展，它允许在提示中加入一个可交互按钮，当用户点击按钮的时候，可以执行一些额外的逻辑操作。打个比方，如果我们在执行删除操作的时候只弹出一个Toast提示，那么用户要是误删了某个重要数据的话，肯定会十分抓狂吧，但是如果我们增加一个Undo按钮，就相当于给用户提供了一种弥补措施，从而大大降低了事故发生的概率，提升了用户体验。

Snackbar的用法也非常简单，它和Toast是基本相似的，只不过可以额外增加一个按钮的点击事件。修改MainActivity中的代码，如下所示：

```kotlin
class MainActivity : AppCompatActivity() {
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        ...
        mainBinding.fab.setOnClickListener { view ->
            Snackbar.make(view, "Data deleted", Snackbar.LENGTH_SHORT)
                .setAction("Undo") {
                    Toast.makeText(this, "Data restored", Toast.LENGTH_SHORT).show()
                }
                .show()
        }
    }

    ...
}
```

可以看到，这里调用了Snackbar的`make()`方法来创建一个Snackbar对象。`make()`方法的第一个参数需要传入一个View，只要是当前界面布局的任意一个View都可以，Snackbar会使用这个View自动查找最外层的布局，用于展示提示信息；第二个参数就是Snackbar中显示的内容；第三个参数是Snackbar显示的时长，这些和Toast都是类似的。

接着这里又调用了一个`setAction()`方法来设置一个动作，从而让Snackbar不仅仅是一个提示，而是可以和用户进行交互的。简单起见，我们在动作按钮的点击事件里面弹出一个Toast提示。最后调用`show()`方法让Snackbar显示出来。

我们会发现这个Snackbar竟然将我们的悬浮按钮给遮挡住了。虽说也不是什么重大的问题，因为Snackbar过一会儿就会自动消失，但这种用户体验总归是不友好的。这时候只需要借助CoordinatorLayout就可以轻松解决。

### 4.3 CoordinatorLayout

`CoordinatorLayout`可以说是一个加强版的FrameLayout，由AndroidX库提供。它在普通情况下的作用和FrameLayout基本一致，但是它拥有一些额外的Material能力。

事实上，`CoordinatorLayout`可以监听其所有子控件的各种事件，并自动帮助我们做出最为合理的响应。举个简单的例子，刚才弹出的Snackbar提示将悬浮按钮遮挡住了，而如果我们能让`CoordinatorLayout`监听到Snackbar的弹出事件，那么它会自动将内部的`FloatingActionButton`向上偏移，从而确保不会被Snackbar遮挡。

至于`CoordinatorLayout`的使用也非常简单，我们只需要将原来的FrameLayout替换一下就可以了。修改`activity_main.xml`中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.coordinatorlayout.widget.CoordinatorLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <androidx.appcompat.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="@color/colorPrimary"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />

        <com.google.android.material.floatingactionbutton.FloatingActionButton
            android:id="@+id/fab"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="bottom|end"
            android:layout_margin="16dp"
            android:elevation="8dp"
            android:src="@drawable/ic_done" />
    </androidx.coordinatorlayout.widget.CoordinatorLayout>

    ...

</androidx.drawerlayout.widget.DrawerLayout>
```

由于`CoordinatorLayout`本身就是一个加强版的FrameLayout，因此这种替换不会有任何的副作用。

可以看到，悬浮按钮自动向上偏移了Snackbar的同等高度，从而确保不会被遮挡。当Snackbar消失的时候，悬浮按钮会自动向下偏移回到原来的位置。

另外，悬浮按钮的向上和向下偏移也是伴随着动画效果的，且和Snackbar完全同步，整体效果看上去特别赏心悦目。

不过我们回过头来再思考一下，刚才说的是`CoordinatorLayout`可以监听其所有子控件的各种事件，但是Snackbar好像并不是`CoordinatorLayout`的子控件吧，为什么它却可以被监听到呢？

其实道理很简单，**我们在Snackbar的make()方法中传入的第一个参数就是用来指定Snackbar是基于哪个View触发的，刚才我们传入的是FloatingActionButton本身，而FloatingActionButton是CoordinatorLayout中的子控件，因此这个事件就理所应当能被监听到了。**如果给Snackbar的make()方法传入一个DrawerLayout，那么Snackbar就会再次遮挡悬浮按钮，因为DrawerLayout不是`CoordinatorLayout`的子控件，`CoordinatorLayout`也就无法监听到Snackbar的弹出和隐藏事件了。

## 五、卡片式布局

虽然现在MaterialTest中已经应用了非常多的Material Design效果，不过你会发现，界面上最主要的一块区域还处于空白状态。这块区域通常用来放置应用的主体内容，接下来我们将使用一些精美的水果图片来填充这部分区域。

为了要让水果图片也能Material化，我们将会学习如何实现卡片式布局的效果。卡片式布局也是Materials Design中提出的一个新概念，它可以让页面中的元素看起来就像在卡片中一样，并且还能拥有圆角和投影，下面我们就开始具体学习一下。

### 5.1 MaterialCardView

`MaterialCardView`是用于实现卡片式布局效果的重要控件，由Material库提供。实际上，`MaterialCardView`也是一个FrameLayout，只是额外提供了圆角和阴影等效果，看上去会有立体的感觉。

先来看一下MaterialCardView的基本用法吧，其实非常简单，如下所示：

```xml
<com.google.android.material.card.MaterialCardView
	android:layout_height="wrap_content"
	android:layout_width="match_parent"
	app:cardCornerRadius="4dp"
	app:elevation="5dp">
	<TextView
    	android:id="@+id/infoText"
    	android:layout_height="wrap_content"
    	android:layout_width="match_parent" />
</com.google.android.material.card.MaterialCardView>
```

这里定义了一个`MaterialCardView`布局，我们可以通过`app:cardCornerRadius`属性指定卡片圆角的弧度，数值越大，圆角的弧度也越大。另外，还可以通过app:elevation属性指定卡片的高度：高度值越大，投影范围也越大，但是投影效果越淡；高度值越小，投影范围也越小，但是投影效果越浓。这一点和`FloatingActionButton`是一致的。

然后，我们在`MaterialCardView`布局中放置了一个TextView，那么这个TextView就会显示在一张卡片当中了，就是这么简单。

但是，我们显然不可能在如此宽阔的一块空白区域内只放置一张卡片。为了能够充分利用屏幕的空间，这里我准备综合运用一下第4章中学到的知识，使用`RecyclerView`填充**MaterialTest**项目的主界面部分。还记得之前实现过的水果列表效果吗？这次我们将升级一下，实现一个高配版的水果列表效果。

然后，由于我们还需要用到`RecyclerView`，因此必须在app/build.gradle文件中声明库的依赖：

```tex
dependencies {
    ...
    implementation("androidx.recyclerview:recyclerview:1.3.2")
    implementation("com.github.bumptech.glide:glide:4.16.0")
}
```

上述声明的第二行是添加了Glide库的依赖。Glide是一个超级强大的开源图片加载库，它不仅可以用于加载本地图片，还可以加载网络图片、GIF图片甚至是本地视频。最重要的是，Glide的用法非常简单，只需几行代码就能轻松实现复杂的图片加载功能，因此这里我们准备用它来加载水果图片。Glide的项目主页地址是：[GitHub - bumptech/glide: An image loading and caching library for Android focused on smooth scrolling](https://github.com/bumptech/glide)

接下来开始具体的代码实现，修改activity_main.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.coordinatorlayout.widget.CoordinatorLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <com.google.android.material.appbar.AppBarLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
            <androidx.appcompat.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:background="@color/colorPrimary"
                android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
                app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
        </com.google.android.material.appbar.AppBarLayout>


        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:layout_behavior="@string/appbar_scrolling_view_behavior"/>

        <com.google.android.material.floatingactionbutton.FloatingActionButton
            android:id="@+id/fab"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="bottom|end"
            android:layout_margin="16dp"
            android:elevation="8dp"
            android:src="@drawable/ic_done" />
    </androidx.coordinatorlayout.widget.CoordinatorLayout>

    ...


</androidx.drawerlayout.widget.DrawerLayout>
```

这段代码中的 `AppBarLayout` 是一个垂直的 `LinearLayout`，它实现了许多特性，使得 `Toolbar` 和其他视图（如 `TabLayout`）能够正确响应滚动事件。`app:layout_behavior="@string/appbar_scrolling_view_behavior"` 是一个指示，告诉 `RecyclerView` 它需要与 `AppBarLayout` 协同工作，以确保所有的滚动事件都能被正确处理。这样，当你向上滚动 `RecyclerView` 时，`Toolbar` 就会保持在顶部，而不会被覆盖。这里我们在`CoordinatorLayout`中添加了一个`RecyclerView`，给它指定一个id，然后将宽度和高度都设置为match_parent，这样RecyclerView就占满了整个布局的空间。

接着定义一个实体类Fruit，代码如下所示：

```kotlin
/*
 * Author: Charlie_Liao
 * Time: 2024/3/5-10:23
 * E-mail: rookie_l@icu007.work
 */

class Fruit(val name: String, val imageId: Int)
```

Fruit类中只有两个字段：name表示水果的名字，imageId表示水果对应图片的资源id。

然后需要为`RecyclerView`的子项指定一个我们自定义的布局，在layout目录下新建`fruit_item.xml`，代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<com.google.android.material.card.MaterialCardView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_margin="5dp"
    app:cardCornerRadius="4dp">

    <LinearLayout
        android:orientation="vertical"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <ImageView
            android:id="@+id/fruitImage"
            android:layout_width="match_parent"
            android:layout_height="100dp"
            android:scaleType="centerCrop"/>

        <TextView
            android:id="@+id/fruitName"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center_horizontal"
            android:layout_margin="5dp"
            android:textSize="16sp"/>
    </LinearLayout>

</com.google.android.material.card.MaterialCardView>
```

这里使用了`MaterialCardView`来作为子项的最外层布局，从而使得RecyclerView中的每个元素都是在卡片当中的。由于`MaterialCardView`是一个FrameLayout，因此它没有什么方便的定位方式，这里我们只好在`MaterialCardView`中再嵌套一个LinearLayout，然后在LinearLayout中放置具体的内容。

内容倒也没有什么特殊的地方，就是定义了一个ImageView用于显示水果的图片，又定义了一个TextView用于显示水果的名称，并让TextView在水平方向上居中显示。注意，在ImageView中我们使用了一个scaleType属性，这个属性可以指定图片的缩放模式。由于各张水果图片的长宽比例可能会不一致，为了让所有的图片都能填充满整个ImageView，这里使用了centerCrop模式，它可以让图片保持原有比例填充满ImageView，并将超出屏幕的部分裁剪掉。

接下来需要为`RecyclerView`准备一个适配器，新建`FruitAdapter`类，让这个适配器继承自`RecyclerView.Adapter`，并将泛型指定为`FruitAdapter.ViewHolder`，代码如下所示：

```kotlin
package work.icu007.materialtes

import android.content.Context
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ImageView
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView
import com.bumptech.glide.Glide


/*
 * Author: Charlie_Liao
 * Time: 2024/3/5-10:43
 * E-mail: rookie_l@icu007.work
 */

class FruitAdapter(val context: Context, val fruitList: List<Fruit>) :
    RecyclerView.Adapter<FruitAdapter.ViewHolder>() {
        inner class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
            val fruitImage: ImageView = view.findViewById(R.id.fruitImage)
            val fruitName: TextView = view.findViewById(R.id.fruitName)
        }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(context).inflate(R.layout.fruit_item, parent, false)
        return ViewHolder(view)
    }

    override fun getItemCount(): Int {
        return fruitList.size
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val fruit = fruitList[position]
        holder.fruitName.text = fruit.name
        Glide.with(context).load(fruit.imageId).into(holder.fruitImage)
    }
}
```

在`onBindViewHolder()`方法中我们使用了Glide来加载水果图片。

那么这里就顺便来看一下Glide的用法吧，其实并没有太多好讲的，因为Glide的用法实在是太简单了。首先调用`Glide.with()`方法并传入一个Context、Activity或Fragment参数，然后调用`load()`方法加载图片，可以是一个URL地址，也可以是一个本地路径，或者是一个资源id，最后调用`into()`方法将图片设置到具体某一个ImageView中就可以了。

那么我们为什么要使用Glide而不是传统的设置图片方式呢？因为这些水果图片像素非常高，如果不进行压缩就直接展示的话，很容易引起内存溢出。而使用Glide就完全不需要担心这回事，Glide在内部做了许多非常复杂的逻辑操作，其中就包括了图片压缩，我们只需要安心按照Glide的标准用法去加载图片就可以了。

这样我们将`RecyclerView`的适配器也准备好了，最后修改`MainActivity`中的代码，如下所示：

```kotlin
package work.icu007.materialtes

import android.os.Bundle
import android.view.Menu
import android.view.MenuItem
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.GravityCompat
import androidx.recyclerview.widget.GridLayoutManager
import com.google.android.material.snackbar.Snackbar
import work.icu007.materialtes.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private val fruits = mutableListOf(
        Fruit("Apple", R.drawable.apple),
        Fruit("Banana", R.drawable.banana),
        Fruit("Orange", R.drawable.orange),
        Fruit("Watermelon", R.drawable.watermelon),
        Fruit("Pear", R.drawable.pear),
        Fruit("Grape", R.drawable.grape),
        Fruit("Pineapple", R.drawable.pineapple),
        Fruit("Strawberry", R.drawable.strawberry),
        Fruit("Cherry", R.drawable.cherry),
        Fruit("Mango", R.drawable.mango),
    )
    private val fruitList = ArrayList<Fruit>()
    private lateinit var mainBinding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        ...

        initFruits()
        val layoutManager = GridLayoutManager(this, 2)
        mainBinding.recyclerView.layoutManager = layoutManager
        val adapter = FruitAdapter(this, fruitList)
        mainBinding.recyclerView.adapter = adapter
    }

    ...

    private fun initFruits() {
        fruitList.clear()
        repeat(520) {
            val index = (0 until fruits.size).random()
            fruitList.add(fruits[index])
        }
    }
}
```

在`MainActivity`中，我们首先定义了一个水果集合，集合里面存放了很多个Fruit的实例，每个实例都代表一种水果。然后在`initFruits()`方法中，先是清空了一下fruitList中的数据，接着使用一个随机函数，从刚才定义的Fruit数组中随机挑选一个水果放入fruitList当中，这样每次打开程序看到的水果数据都会是不同的。另外，为了让界面上的数据多一些，这里使用了`repeat()`函数，随机挑选50个水果。

之后的用法就是RecyclerView的标准用法了，不过这里使用了GridLayoutManager这种布局方式。GridLayoutManager的用法并没有什么特别之处，它的构造函数接收两个参数：第一个是Context，第二个是列数。这里我们希望每一行中会有两列数据。

### 5.2 AppBarLayout

我们的**Toolbar**怎么不见了！仔细观察一下原来是被`RecyclerView`给挡住了。这个问题又该怎么解决呢？这就需要借助另外一个工具了——`AppBarLayout`。

首先，我们来分析一下为什么`RecyclerView`会把**Toolbar**给遮挡住吧。其实并不难理解，由于`RecyclerView`和**Toolbar**都是放置在`CoordinatorLayout`中的，而前面已经说过，`CoordinatorLayout`就是一个加强版的`FrameLayout`，那么`FrameLayout`中的所有控件在不进行明确定位的情况下，默认都会摆放在布局的左上角，从而产生了遮挡的现象。

既然已经找到了问题的原因，那么该如何解决呢？在传统情况下，使用偏移是唯一的解决办法，即让`RecyclerView`向下偏移一个**Toolbar**的高度，从而保证不会遮挡到**Toolbar**。不过我们使用的并不是普通的`FrameLayout`，而是`CoordinatorLayout`，因此自然会有一些更加巧妙的解决办法。

这里我准备使用**Material**库中提供的另外一个工具——`AppBarLayout`。`AppBarLayout`实际上是一个垂直方向的`LinearLayout`，它在内部做了很多滚动事件的封装，并应用了一些`Material Design`的设计理念。

那么我们怎样使用`AppBarLayout`才能解决前面的遮挡问题呢？其实只需要两步就可以了，第一步将**Toolbar**嵌套到`AppBarLayout`中，第二步给`RecyclerView`指定一个布局行为。修改`activity_main.xml`中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.coordinatorlayout.widget.CoordinatorLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <com.google.android.material.appbar.AppBarLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
            <androidx.appcompat.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:background="@color/colorPrimary"
                android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
                app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
        </com.google.android.material.appbar.AppBarLayout>


        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:layout_behavior="@string/appbar_scrolling_view_behavior"/>

        <com.google.android.material.floatingactionbutton.FloatingActionButton
            android:id="@+id/fab"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="bottom|end"
            android:layout_margin="16dp"
            android:elevation="8dp"
            android:src="@drawable/ic_done" />
    </androidx.coordinatorlayout.widget.CoordinatorLayout>

    ...


</androidx.drawerlayout.widget.DrawerLayout>
```

我们首先定义了一个`AppBarLayout`，并将Toolbar放置在了`AppBarLayout`里面，然后在`RecyclerView`中使用`app:layout_behavior`属性指定了一个布局行为。其中`appbar_scrolling_view_behavior`这个字符串也是由
**Material**库提供的。

虽说使用`AppBarLayout`已经成功解决了`RecyclerView`遮挡Toolbar的问题，刚才提到过，`AppBarLayout`中应用了一些`Material Design`的设计理念，当`RecyclerView`滚动的时候就已经将滚动事件通知给`AppBarLayout`了，只是我们还没进行处理而已。那么下面就让我们来进一步优化，看看`AppBarLayout`到底能实现什么样的`Material Design`效果。

当`AppBarLayout`接收到滚动事件的时候，它内部的子控件其实是可以指定如何去响应这些事件的，通过`app:layout_scrollFlags`属性就能实现。修改`activity_main.xml`中的代码，如下：



```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.coordinatorlayout.widget.CoordinatorLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        <com.google.android.material.appbar.AppBarLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
            <androidx.appcompat.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:background="@color/colorPrimary"
                android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
                app:layout_scrollFlags="scroll|enterAlways|snap"
                app:popupTheme="@style/ThemeOverlay.AppCompat.Light" />
        </com.google.android.material.appbar.AppBarLayout>


        <androidx.recyclerview.widget.RecyclerView
            android:id="@+id/recyclerView"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:layout_behavior="@string/appbar_scrolling_view_behavior"/>

        <com.google.android.material.floatingactionbutton.FloatingActionButton
            android:id="@+id/fab"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="bottom|end"
            android:layout_margin="16dp"
            android:elevation="8dp"
            android:src="@drawable/ic_done" />
    </androidx.coordinatorlayout.widget.CoordinatorLayout>

    ...


</androidx.drawerlayout.widget.DrawerLayout>
```

这里在**Toolbar**中添加了一个`app:layout_scrollFlags`属性，并将这个属性的值指定成了`scroll|enterAlways|snap`。其中，**scroll**表示当`RecyclerView`向上滚动的时候，**Toolbar**会跟着一起向上滚动并实现隐藏；`enterAlways`表示当`RecyclerView`向下滚动的时候，**Toolbar**会跟着一起向下滚动并重新显示；**snap**表示当**Toolbar**还没有完全隐藏或显示的时候，会根据当前滚动的距离，自动选择是隐藏还是显示。

## 六、下拉刷新

下拉刷新这种功能早就不是什么新鲜的东西了，所有的应用里都会有这个功能。不过市面上现有的下拉刷新功能在风格上各不相同，并且和`Material Design`还有些格格不入的感觉。因此，**Google**为了让Android的下拉刷新风格能有一个统一的标准，在`Material Design`中制定了一个官方的设计规范。当然，我们并不需要深入了解这个规范到底是什么样的，因为**Google**早就提供好了现成的控件，我们在项目中直接使用就可以了。

`SwipeRefreshLayout`就是用于实现下拉刷新功能的核心类，我们把想要实现下拉刷新功能的控件放置到`SwipeRefreshLayout`中，就可以迅速让这个控件支持下拉刷新。那么在`MaterialTest`项目中，应该支持下拉刷新功能的控件自然就是`RecyclerView`了。

使用`SwipeRefreshLayout`之前首先需要在`app/build.gradle`文件中添加如下依赖：

```tex
dependencies {
    ...
    implementation("androidx.swiperefreshlayout:swiperefreshlayout:1.1.0")
}
```

由于`SwipeRefreshLayout`的用法也比较简单，下面我们就直接开始使用了。修改`activity_main.xml`中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.drawerlayout.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawerLayout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.coordinatorlayout.widget.CoordinatorLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">

        ...

        <androidx.swiperefreshlayout.widget.SwipeRefreshLayout
            android:id="@+id/swipeRefresh"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_behavior="@string/appbar_scrolling_view_behavior">
            
            <androidx.recyclerview.widget.RecyclerView
                android:id="@+id/recyclerView"
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                app:layout_behavior="@string/appbar_scrolling_view_behavior"/>
            
        </androidx.swiperefreshlayout.widget.SwipeRefreshLayout>
        ...
    </androidx.coordinatorlayout.widget.CoordinatorLayout>

    ...

</androidx.drawerlayout.widget.DrawerLayout>
```

可以看到，这里我们在`RecyclerView`的外面又嵌套了一层`SwipeRefreshLayout`，这样`RecyclerView`就自动拥有下拉刷新功能了。另外需要注意，由于`RecyclerView`现在变成了`SwipeRefreshLayout`的子控件，因此之前使用`app:layout_behavior`声明的布局行为现在也要移到`SwipeRefreshLayout`中才行。

不过这还没有结束，虽然`RecyclerView`已经支持下拉刷新功能了，但是我们还要在代码中处理具体的刷新逻辑才行。修改`MainActivity`中的代码，如下所示：

```kotlin
class MainActivity : AppCompatActivity() {
    ...
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        mainBinding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(mainBinding.root)
        ...
        mainBinding.swipeRefresh.setColorSchemeColors(R.color.colorPrimary)
        mainBinding.swipeRefresh.setOnRefreshListener {
            refreshFruits(adapter)
        }
    }

    ...

    private fun refreshFruits(adapter: FruitAdapter) {
        thread {
            Thread.sleep(1000)
            runOnUiThread {
                initFruits()
                adapter.notifyDataSetChanged()
                mainBinding.swipeRefresh.isRefreshing = false
            }
        }
    }
}
```

这段代码应该还是比较好理解的，首先调用`SwipeRefreshLayout`的`setColorSchemeResources()`方法来设置下拉刷新进度条的颜色，这里我们就使用主题中的`colorPrimary`作为进度条的颜色了。接着调用`setOnRefreshListener()`方法来设置一个下拉刷新的监听器，当用户进行了下拉刷新操作时，就会回调到Lambda表达式当中，然后我们在这里去处理具体的刷新逻辑就可以了。

通常情况下，当触发了下拉刷新事件，应该是去网络上请求最新的数据，然后再将这些数据展示出来。这里简单起见，我们就不和网络进行交互了，而是调用一个`refreshFruits()`方法进行本地刷新操作。`refreshFruits()`方法中先是开启了一个线程，然后将线程沉睡两秒钟。之所以这么做，是因为本地刷新操作速度非常快，如果不将线程沉睡的话，刷新立刻就结束了，从而看不到刷新的过程。沉睡结束之后，这里使用了`runOnUiThread()`方法将线程切换回主线程，然后调用`initFruits()`方法重新生成数据，接着再调用`FruitAdapter`的`notifyDataSetChanged()`方法通知数据发生了变化，最后调用`SwipeRefreshLayout`的`setRefreshing()`方法并传入false，表示刷新事件结束，并隐藏刷新进度条。

## 七、可折叠式标题栏

我们现在的标题栏是使用**Toolbar**来编写的，不过它看上去和传统的**ActionBar**没什么两样，只不过可以响应`RecyclerView`的滚动事件来进行隐藏和显示。而`Material Design`中并没有限定标题栏必须是长这个样子的，事实上，我们可以根据自己的喜好随意定制标题栏的样式。那么本节中我们就来实现一个可折叠式标题栏的效果，这需要借助`CollapsingToolbarLayout`这个工具。

### 7.1 CollapsingToolbarLayout

顾名思义，`CollapsingToolbarLayout`是一个作用于**Toolbar**基础之上的布局，它也是由**Material**库提供的。`CollapsingToolbarLayout`可以让**Toolbar**的效果变得更加丰富，不仅仅是展示一个标题栏，而且能够实现非常华丽的效果。

不过，`CollapsingToolbarLayout`是不能独立存在的，它在设计的时候就被限定只能作为**AppBarLayout**的直接子布局来使用。而**AppBarLayout**又必须是`CoordinatorLayout`的子布局，因此本节中我们要实现的功能其实需要综合运用前面所学的各种知识。那么话不多说，这就开始吧。

首先我们需要一个额外的**Activity**作为水果的详情展示界面，`右击work.icu007.materialtest包→New→Activity→Empty Activity`，创建一个`FruitActivity`，并将布局名指定成`activity_fruit.xml`，然后我们开始编写水果详情展示界面的布局。

由于整个布局文件比较复杂，这里准备采用分段编写的方式。`activity_fruit.xml`中的内容主要分为两部分，一个是水果标题栏，一个是水果内容详情，我们来一步步实现。

首先实现标题栏部分，这里使用`CoordinatorLayout`作为最外层布局，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FruitActivity">

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

接着我们在`CoordinatorLayout`中嵌套一个`AppBarLayout`，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FruitActivity">

    <com.google.android.material.appbar.AppBarLayout
        android:id="@+id/appBar"
        android:layout_width="match_parent"
        android:layout_height="250dp">
    </com.google.android.material.appbar.AppBarLayout>

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

目前为止也没有什么难理解的地方，我们给`AppBarLayout`定义了一个id，将它的宽度指定为**match_parent**，高度指定为250 dp.

接下来我们在`AppBarLayout`中再嵌套一个`CollapsingToolbarLayout`，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FruitActivity">

    <com.google.android.material.appbar.AppBarLayout
        android:id="@+id/appBar"
        android:layout_width="match_parent"
        android:layout_height="250dp">
        
        <com.google.android.material.appbar.CollapsingToolbarLayout
            android:id="@+id/collapsingToolbar"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            app:contentScrim="@color/colorPrimary"
            app:layout_scrollFlags="scroll|exitUntilCollapsed">
        </com.google.android.material.appbar.CollapsingToolbarLayout>
    </com.google.android.material.appbar.AppBarLayout>

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

这里我们使用了新的布局`CollapsingToolbarLayout`。其中，**id、layout_width和layout_height**这几个属性比较简单，我就不解释了。**android:theme属性指定了一个`ThemeOverlay.AppCompat.Dark.ActionBar`的主题**，其实对于这部分我们也并不陌生，因为之前在activity_main.xml中给Toolbar指定的也是这个主题，只不过**这里要实现更加高级的Toolbar效果，因此需要将这个主题的指定提到上一层来**。**`app:contentScrim`属性用于指定`CollapsingToolbarLayout`在趋于折叠状态以及折叠之后的背景色**，其实`CollapsingToolbarLayout`在折叠之后就是一个普通的**Toolbar**，那么背景色肯定应该是colorPrimary了，具体的效果我们待会儿就能看到。`app:layout_scrollFlags`属性我们也是见过的，只不过之前是给Toolbar指定的，现在也移到外面来了。其中，**`scroll`表示`CollapsingToolbarLayout`会随着水果内容详情的滚动一起滚动，`exitUntilCollapsed`表示当`CollapsingToolbarLayout`随着滚动完成折叠之后就保留在界面上，不再移出屏幕。**

接下来，我们在`CollapsingToolbarLayout`中定义标题栏的具体内容，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FruitActivity">

    <com.google.android.material.appbar.AppBarLayout
        android:id="@+id/appBar"
        android:layout_width="match_parent"
        android:layout_height="250dp">

        <com.google.android.material.appbar.CollapsingToolbarLayout
            android:id="@+id/collapsingToolbar"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            app:contentScrim="@color/colorPrimary"
            app:layout_scrollFlags="scroll|exitUntilCollapsed">
            
            <ImageView
                android:id="@+id/fruitImageView"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:scaleType="centerCrop"
                app:layout_collapseMode="parallax"/>
            <androidx.appcompat.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:layout_collapseMode="pin"/>
        </com.google.android.material.appbar.CollapsingToolbarLayout>
    </com.google.android.material.appbar.AppBarLayout>

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

可以看到，我们在`CollapsingToolbarLayout`中定义了一个**ImageView**和一个**Toolbar**，也就意味着，这个高级版的标题栏将是由普通的标题栏加上图片组合而成的。这里定义的大多数属性我们是已经见过的，就不再解释了，只有一个`app:layout_collapseMode`比较陌生。它用于指定当前控件在`CollapsingToolbarLayout`折叠过程中的折叠模式，**其中Toolbar指定成pin，表示在折叠的过程中位置始终保持不变**，**ImageView指定成parallax，表示会在折叠的过程中产生一定的错位偏移，这种模式的视觉效果会非常好。**

这样我们就将水果标题栏的界面编写完成了，下面开始编写水果内容详情部分。继续修改`activity_fruit.xml`中的代码，如下所示：

水果内容详情的最外层布局使用了一个`NestedScrollView`，注意它和`AppBarLayout`是平级的。`ScrollView`使用滚动的方式来查看屏幕以外的数据，而`NestedScrollView`在此基础之上还增加了嵌套响应滚动事件的功能。由于`CoordinatorLayout`本身已经可以响应滚动事件了，因此我们在它的内部就需要使用`NestedScrollView`或`RecyclerView`这样的布局。另外，这里还通过`app:layout_behavior`属性指定了一个布局行为，这和之前在`RecyclerView`中的用法是一模一样的。

不管是`ScrollView`还是`NestedScrollView`，它们的内部都只允许存在一个直接子布局。因此,如果我们想要在里面放入很多东西的话，通常会先嵌套一个`LinearLayout`，然后再在`LinearLayout`中放入具体的内容就可以了，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FruitActivity">

    ...

    <androidx.core.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">
        
        <LinearLayout
            android:orientation="vertical"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
            
        </LinearLayout>
    </androidx.core.widget.NestedScrollView>

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

这里我们嵌套了一个垂直方向的`LinearLayout`，并将**layout_width**设置为**match_parent**，将**layout_height**设置为**wrap_content**。

接下来在`LinearLayout`中放入具体的内容，这里准备使用一个TextView来显示水果的内容详情，并将TextView放在一个卡片式布局当中，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FruitActivity">

    ...

    <androidx.core.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <LinearLayout
            android:orientation="vertical"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">

            <com.google.android.material.card.MaterialCardView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:layout_marginBottom="15dp"
                android:layout_marginLeft="15dp"
                android:layout_marginRight="15dp"
                android:layout_marginTop="35dp"
                app:cardCornerRadius="4dp">
                
                <TextView
                    android:id="@+id/fruitContentText"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_margin="10dp"/>
            </com.google.android.material.card.MaterialCardView>
        </LinearLayout>
    </androidx.core.widget.NestedScrollView>

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

这段代码也没有什么难理解的地方，都是我们学过的知识。需要注意的是，这里为了让界面更加美观，我在`MaterialCardView`和`TextView`上都加了一些边距。其中，`MaterialCardView`的`marginTop`加了`35 dp`的边距，这是为下面要编写的东西留出空间。

好的，这样就把水果标题栏和水果内容详情的界面都编写完了，不过我们还可以在界面上再添加一个悬浮按钮。这个悬浮按钮并不是必需的，根据具体的需求添加就可以了，如果加入的话，我们将获得一些额外的动画效果。

为了做出示范，我就准备在`activity_fruit.xml`中加入一个悬浮按钮了。这个界面是一个水果详情展示界面，那么我就加入一个表示评论作用的悬浮按钮吧。首先需要提前准备好一个图标，这里我放置了一张`ic_comment.png`到`drawable-xxhdpi`目录下。然后修改`activity_fruit.xml`中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".FruitActivity">

    <com.google.android.material.appbar.AppBarLayout
        android:id="@+id/appBar"
        android:layout_width="match_parent"
        android:layout_height="250dp">

        ...
    </com.google.android.material.appbar.AppBarLayout>

    <androidx.core.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        ...
    </androidx.core.widget.NestedScrollView>
    
    <com.google.android.material.floatingactionbutton.FloatingActionButton
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:src="@drawable/ic_comment"
        app:layout_anchor="@id/appBar"
        app:layout_anchorGravity="bottom|end"/>

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

可以看到，这里加入了一个`FloatingActionButton`，它和`AppBarLayout`以及`NestedScrollView`是平级的。`FloatingActionButton`中使用`app:layout_anchor`属性指定了一个锚点，我们将锚点设置为AppBarLayout，这样悬浮按钮就会出现在水果标题栏的区域内，接着又使用`app:layout_anchorGravity`属性将悬浮按钮定位在标题栏区域的右下角。其他一些属性比较简单，就不再进行解释了。

好了，现在我们终于将整个`activity_fruit.xml`布局都编写完了，内容虽然比较长，但由于是分段编写的，并且每一步我都进行了详细的说明，相信你应该看得很明白吧。

界面完成了之后，接下来我们开始编写功能逻辑，修改`FruitActivity`中的代码，如下所示：

```kotlin
package work.icu007.materialtes

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.MenuItem
import com.bumptech.glide.Glide
import work.icu007.materialtes.databinding.ActivityFruitBinding

class FruitActivity : AppCompatActivity() {
    companion object {
        const val FRUIT_NAME = "fruit_name"
        const val FRUIT_IMAGE_ID = "fruit_image_id"
    }
    private lateinit var fruitBinding: ActivityFruitBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        fruitBinding = ActivityFruitBinding.inflate(layoutInflater)
        setContentView(fruitBinding.root)
        
        val fruitName = intent.getStringExtra(FRUIT_NAME) ?: ""
        val fruitImageId = intent.getIntExtra(FRUIT_IMAGE_ID, 0)
        
        setSupportActionBar(fruitBinding.toolbar)
        supportActionBar?.setDisplayHomeAsUpEnabled(true)
        fruitBinding.collapsingToolbar.title = fruitName
        Glide.with(this).load(fruitImageId).into(fruitBinding.fruitImageView)
        fruitBinding.fruitContentText.text == generateFruitContent(fruitName)
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        when (item.itemId) {
            android.R.id.home -> {
                finish()
                return true
            }
        }
        return super.onOptionsItemSelected(item)
    }

    private fun generateFruitContent(fruitName: String): String {
        return fruitName.repeat(500)
    }
}
```

`FruitActivity`中的代码并不是很复杂。首先，在`onCreate()`方法中，我们通过**Intent**获取了传入的水果名和水果图片的资源id。接着使用了**Toolbar**的标准用法，将它作为**ActionBar**显示，并启用**Home**按钮。由于**Home**按钮的默认图标就是一个返回箭头，这正是我们所期望的，因此就不用额外设置别的图标了。

接下来开始填充界面上的内容，调用`CollapsingToolbarLayout`的`setTitle()`方法，将水果名设置成当前界面的标题，然后使用**Glide**加载传入的水果图片，并设置到标题栏的**ImageView**上面。接着需要填充水果的内容详情，由于这只是一个示例程序，并不需要什么真实的数据，所以我使用了一个`generateFruitContent()`方法将水果名循环拼接500次，从而生成了一个比较长的字符串，将它设置到了`TextView`上面。

最后，我们在`onOptionsItemSelected()`方法中处理了Home按钮的点击事件，当点击这个按钮时，就调用`finish()`方法关闭当前的**Activity**，从而返回上一个**Activity**。

所有工作都完成了吗？其实还差最关键的一步，就是处理`RecyclerView`的点击事件，不然的话，我们根本就无法打开`FruitActivity`。修改`FruitAdapter`中的代码，如下所示：

```kotlin
class FruitAdapter(val context: Context, val fruitList: List<Fruit>) :
    RecyclerView.Adapter<FruitAdapter.ViewHolder>() {
    ...

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(context).inflate(R.layout.fruit_item, parent, false)
        val holder = ViewHolder(view)
        holder.itemView.setOnClickListener {
            val position = holder.bindingAdapterPosition
            val fruit = fruitList[position]
            val intent = Intent(context, FruitActivity::class.java).apply {
                putExtra(FruitActivity.FRUIT_NAME, fruit.name)
                putExtra(FruitActivity.FRUIT_IMAGE_ID, fruit.imageId)
            }
            context.startActivity(intent)
        }
        return holder
    }

    ...
}
```

最关键的一步其实也是最简单的，这里我们给fruit_item.xml的最外层布局注册了一个点击事件监听器，然后在点击事件中获取当前点击项的水果名和水果图片资源id，把它们传入Intent中，最后调用startActivity()方法启动FruitActivity。

### 7.2 充分利用系统状态栏空间

虽说现在水果详情展示界面的效果已经非常华丽了，但这并不代表我们不能再进一步地提升。观察一下图12.17，你会发现水果的背景图片和系统的状态栏总有一些不搭的感觉，如果我们能将背景图和状态栏融合到一起，那这个视觉体验绝对能提升好几个档次。

不过，在Android 5.0系统之前，我们是无法对状态栏的背景或颜色进行操作的，那个时候也还没有Material Design的概念，但是Android 5.0及之后的系统都是支持这个功能的。恰好我们整本书的所有代码最低兼容的就是Android 5.0系统，因此这里完全可以进一步地提升视觉体验。

想要让背景图能够和系统状态栏融合，需要借助`android:fitsSystemWindows`这个属性来实现。在`CoordinatorLayout`、`AppBarLayout`、`CollapsingToolbarLayout`这种嵌套结构的布局中，将控件的`android:fitsSystemWindows`属性指定成**true**，就表示该控件会出现在系统状态栏里。对应到我们的程序，那就是水果标题栏中的`ImageView`应该设置这个属性了。不过只给ImageView设置这个属性是没有用的，我们必须将ImageView布局结构中的所有父布局都设置上这个属性才可以，修改`activity_fruit.xml`中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context=".FruitActivity">

    <com.google.android.material.appbar.AppBarLayout
        android:id="@+id/appBar"
        android:fitsSystemWindows="true"
        android:layout_width="match_parent"
        android:layout_height="250dp">

        <com.google.android.material.appbar.CollapsingToolbarLayout
            android:id="@+id/collapsingToolbar"
            android:layout_width="match_parent"
            android:layout_height="235dp"
            android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
            android:fitsSystemWindows="true"
            app:contentScrim="@color/colorPrimary"
            app:layout_scrollFlags="scroll|exitUntilCollapsed">

            <ImageView
                android:id="@+id/fruitImageView"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:scaleType="centerCrop"
                android:fitsSystemWindows="true"
                app:layout_collapseMode="parallax"/>
            ...
        </com.google.android.material.appbar.CollapsingToolbarLayout>
    </com.google.android.material.appbar.AppBarLayout>

    ...

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

将`android:fitsSystemWindows`属性都设置好了之后还必须在程序的主题中将状态栏颜色指定成透明色才行。指定成透明色的方法很简单，在主题中将`android:statusBarColor`属性的值指定成`@android:color/transparent`就可以了。

打开 `res/values/theme.xml` 文件，对主题的内容进行修改，如下所示

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

这里我们定义了一个`FruitActivity`主题，它是专门给`FruitActivity`使用的。`FruitActivity`的父主题是`Base.Theme.MaterialTest`，也就是说，它继承了`Base.Theme.MaterialTest`中的所有特性。在此基础之上，我们将`FruitActivityTheme`中的状态栏的颜色指定成透明色。最后，还需要让`FruitActivity`使用这个主题才可以，修改`AndroidManifest.xml`中的代码，如下所示：

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
        android:theme="@style/Theme.MaterialTest"
        tools:targetApi="31">
        <activity
            android:name=".FruitActivity"
            android:theme="@style/Theme.FruitActivity"
            android:exported="false" />
        ...
    </application>

</manifest>
```

