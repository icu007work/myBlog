---
title: Kotlin安卓开发-UI
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
swiper_index: 1
top_group_index: 1
background: '#fff'
cover: 'https://pic2.ziyuan.wang/user/xiheya/2024/05/Android-UI_6d65774e7d43c.jpg'
ai:
  - >-
    这篇文章是关于Android
    Jetpack中的导航组件的介绍。导航组件是用于实现用户在应用中的导航交互，包括进入和退出不同的内容片段。无论是简单的按钮点击，还是应用栏、抽屉导航栏等复杂的模式，导航组件都能轻松应对。导航组件主要由三部分组成：1.
    导航图（navGraph）：这是包含所有导航相关信息的XML资源，包括应用内所有内容区域个体（称为目标，一般都是Fragment），以及用户可以通过应用跳转的可能路径。2.
    导航宿主（NavHost）：这是用来显示导航图中声明的目标的一个空白容器。导航组件包含一个默认的NavHost实现（NavHostFragment），可用于显示Fragment目标。3.
    导航控制器（NavController）：在NavHost中管理应用导航的目标，当用户在应用中进行操作时，导航控制器会控制目标的切换。使用导航组件有各种优势，包括自动处理Fragment事务。
abbrlink: f370ac25
date: 2023-10-26 15:00:55
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

## 〇、导航组件

### 0.1 导航简介

导航组件是 Android Jetpack 的一部分，主要用途是实现用户导航、进入和退出应用中不同内容片段的交互，不论是普通的按钮点击，还是应用栏、抽屉导航栏等复杂的模式，它都能轻松应对，当然，导航组件也有它既定的 导航原则 来确保一致且可预测的用户体验。

#### 0.1.1 导航组件的组成

 导航组件的组成**主要有三部分** 

- 导航图（<font color = "red">navGraph</font>）: 这是包含所有导航相关信息的 `XML` 资源，这些信息包括应用内所有内容区域个体（称为目标，一般都是 `Fragment`），以及用户可以通过应用跳转的可能路径。
- 导航宿主（ <font color = "red">NavHost</font> ）：这是用来显示导航图中声明的目标的一个空白容器。导航组件包含一个默认的 `NavHost` 实现 (`NavHostFragment`)，可用于显示 `Fragment` 目标。
- 导航控制器（<font color = "red"><strong>NavController  </strong></font> ）：在 `NavHost` 中管理应用导航的目标，当用户在应用中进行操作时，导航控制器会控制目标的切换。

**使用导航组件有各种优势，包括以下方面：**

- 自动处理 `Fragment` 事务；
- 在默认情况下，能够正确处理往返操作；
- 支持动画和转场动画；
- 支持导航界面模式（例如：抽屉式导航栏和底部导航栏）
- Safe Args 支持（一种可在导航和目标之间传递数据时提供类型安全的 Gradle 插件）
- `ViewModel` 支持
- 可以使用Android Studio的 Navigation Editor 来编辑和查看导航图（必须使用Android Studio 3.3及以上版本）

#### 0.1.2 导航的原则

 在使用导航组件时，应当遵循一些原则，以提高用户体验。

> 注意：即使您未在项目中使用 Navigation 组件，您的应用也应遵循这些设计原则。

- ##### 固定的起始目的地

顾名思义，您构建的应用必须有一个固定的起始目的地，这个起始目的地就是指当应用启动时蛋刀的第一个屏幕。起始目的地也是用户按返回按钮后，在回到启动器前看到的最后一个屏幕。

- ##### 导航状态表现为目的地堆栈

在用户启动应用时，系统会启动一个新任务，并且显示起始目的地，这个起始目的地是应用导航的而基础。当用户在应用中进行导航时，栈顶的目标就是显示在屏幕上的，而栈内的所有目标都是历史记录。

> 导航组件会为你管理所有返回栈的顺序，当然你也可以自行管理，已达到某些目的。

- ##### 在应用的任务中向上按钮和返回按钮行为相同

首先，说一下什么是向上按钮，向上按钮是指在应用中的返回上一级的按钮（一般是在用户导航栏中），返回按钮则是系统导航中的返回按钮。在应用的任务重，向上按钮和返回按钮的行为相同，都是将栈顶的目标移除，返回到上一个目标。

- ##### 向上按钮不会退出应用

在应用的任务重，向上按钮可以返回到上一个目标，但是绝不会退出应用。

- ##### 深度链接可以模拟手动导航

无论是通过深度链接至特定的目的地，还是手动导航到特定目的地，都可以使用向上按钮通过各个目的地导航回到起始目的地。当深度链接至特定的目的地时，会移除所有返回栈中的任务，并替换为深度链接的返回栈。值得注意的是，深度链接合成的返回栈是一个完整的返回栈，他跟手动导航至特定目的地具有相同的返回栈，这个是非常重要的，因为合成的返回栈必须是真实的。

### 0.2 使用入门

#### 0.2.1 添加依赖

在应用模块目录下的 `build.gradle`文件中添加 `dependencies` 依赖声明。

```tex
dependencies {
    implementation(platform("org.jetbrains.kotlin:kotlin-bom:1.8.0"))
    implementation 'androidx.core:core-ktx:1.7.0'
    implementation 'androidx.appcompat:appcompat:1.6.1'
    implementation 'com.google.android.material:material:1.9.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.4'
    implementation 'androidx.navigation:navigation-fragment-ktx:2.7.4'
    implementation 'androidx.navigation:navigation-ui-ktx:2.7.4'
    testImplementation 'junit:junit:4.13.2'
    androidTestImplementation 'androidx.test.ext:junit:1.1.5'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.5.1'
}
```

#### 0.2.2 创建导航图资源文件

导航是发生在各个目的地之间的，而这些目的地通过操作连接在一起。导航图是一种资源文件，它包含了所有的目的地和操作的声明。

创建导航图资源文件，可以按以下步骤进行：

右键res-> new -> android resource file -> resource type 选择Navigation，然后取个名字。

![1697508603575.png](https://pic.ziyuan.wang/2023/10/17/guest_a2c970b7a003e_IP210.22.23.7_UPTIME1697508605.png)

新建的导航图资源文件是一个 XML 资源文件，以 `navigation` 为根节点，大致内容如下。

```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/FirstFragment">

</navigation>
```

#### 0.2.3 向 Activity 添加导航宿主（NavHost）

导航宿主是导航组件的核心部分之一，导航宿主是一个空容器，用来存放和处理目的地。导航宿主必须派生于 NavHost、 NavHostFragment 是导航组件的默认导航宿主实现，负责处理 `Fragment` 目的地的交换。

> 注意：导航组件的设计理念是用于具有一个主 Activity 和多个 Fragment 目的地的应用，主 Activity 与导航图相关联，并且包含一个负责根据需要交换目的地的 NavHostFragment。如果您的应用需要在多个 Activity 上实现导航，就需要为每个 Activity 添加导航宿主，并在每个 Activity 关联其自己的导航图。

- ##### 通过 XML 添加 NavHostFragment

 在主 `Activity` 的布局文件中，添加 `<fragment>` 标签，并在内部指定导航图，如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:layout_behavior="@string/appbar_scrolling_view_behavior">

    <fragment
        android:id="@+id/nav_host_fragment_content_main"
        android:name="androidx.navigation.fragment.NavHostFragment"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:defaultNavHost="true"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:navGraph="@navigation/nav_graph" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

- `android:name` 属性包含 `NavHost` 实现类的名称（示例中使用的是默认实现 `NavHostFragment`，如果有需要，可以使用自定义的`Fragment`类，但是必须实现 `NavHost` 或者继承 `NavHostFragment`）
- `app:navGraph` 属性将导航宿主（`NavHostFragment`）与导航图关联，指向包含所有导航目的地的导航图资源文件
- `app:defaultNavHost="true"` 属性确保导航宿主会拦截系统返回按钮。请注意，只能有一个默认导航宿主，如果同一布局（例如，双窗格布局）中有多个导航宿主，请务必仅指定一个默认导航宿主。

> 说明：导航组件是 Android Jetpack 部分，不属于 Android 系统组件，所以需要在布局中添加属性引入，如：`xmlns:app="http://schemas.android.com/apk/res-auto"`

- ##### 向导航图中添加目的地

对于懒于编码的小伙伴可以使用 Navigation Editor 向导航中添加目的地，因为这些都是用户引导模式的，没什么可说，我这里主要讲一下手动添加目的地的步骤：

1. 新建 `Fragment` 类和布局文件，并实现相关逻辑代码；
2. 在导航图 XML 中新增 `<fragment>` 标签；
3. 配置 `<fragment>` 标签的相关属性，如：`android:id` 、`android:name` 、`android:lable` 等；

```xml
<?xml version="1.0" encoding="utf-8"?>
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/FirstFragment">

    <fragment
        android:id="@+id/FirstFragment"
        android:name="work.icu007.uiwidgettest.FirstFragment"
        android:label="@string/first_fragment_label"
        tools:layout="@layout/fragment_first">

        <action
            android:id="@+id/action_FirstFragment_to_SecondFragment"
            app:destination="@id/SecondFragment" />
    </fragment>
    <fragment
        android:id="@+id/SecondFragment"
        android:name="work.icu007.uiwidgettest.SecondFragment"
        android:label="@string/second_fragment_label"
        tools:layout="@layout/fragment_second">

        <action
            android:id="@+id/action_SecondFragment_to_FirstFragment"
            app:destination="@id/FirstFragment" />
    </fragment>
</navigation>
```

目的地属性详解

- Type：即标签名称，指示在源代码中，该目的地是作为 Fragment、Activity还是其他自定义类实现的

- anroid:label：这个属性指定目的地的名称

- android:id： 这个属性指定改目的地的ID，用于在代码中引用该目的地

- android:name：这个属性用来指定目的地所关联的类 

  除此之外，还可以通过tools:layout属性指定预览布局文件，这样就可以在导航图编辑器中看到对应的布局预览。

- ##### 指定起始目的地

导航的原则之一就是固定的起始目的地，指定起始目的地的方法有两种，一种是使用 Navigation Editor，在 “Design”窗口中，选中需要指定为起始目的地的目标，点击 “房子”图标（如下图）即可。另一种方法就是在 XML 源代码中，在 <navigation> 标签中添加 app:startDestination 属性进行指定，属性值为需要指定的目的地的ID（如下示例）。

![1697510921503.png](https://pic.ziyuan.wang/2023/10/17/guest_917c91f48d9c3_IP210.22.23.7_UPTIME1697510923.png)

或者在 xml 直接指定：`app:startDestination="@id/FirstFragment"`

- ##### 连接目的地

目的地之间的逻辑连接也叫做操作，操作一般是将一个目的地连接到另一个目的地，当然，你也可以定义 全局操作 ，这类操作可以在任意位置跳转到指定的目的地，这个我们在后面会详细讲到。
您可以使用 Navigation Editor 连接两个目的地，直接拖动箭头即可，在这里就不多介绍这种方式，直接介绍通过修改 XML 源码的方式（其实使用 Navigation Editor 也会自动修改 XML 源码），具体步骤如下：

1. 在 `<fragment>` 标签内部新增 `<action>` 标签;
2. 配置`anroid:id`和`app:destnation` 属性；
3. 如果需要，可配置`app:enterAnim`、`app:exitAnim`、`app:popEnterAnim`、`app:popExitAnim`属性定义动画。

详细解说：

- **`Type`**：即 `<action>` 标签；
- **`anroid:id`**：这个字段是操作ID，代码中通过这个ID执行操作；
- **`app:destnation`**：这个字段是操作的目的地，用来指定操作跳转的目的地。

例如：

```xml
<action
    android:id="@+id/action_FirstFragment_to_SecondFragment"
    app:destination="@id/SecondFragment" />
```

- ##### 导航到目的地

完成了导航图的各种配置，那么就需要在代码中实现导航到目的地了。导航到目的地是使用 NavController 完成的，这是在导航宿主中管理导航的对象的，每个导航宿主都有自己的相应导航控制器（`NavController`）。导航到目的地的步骤如下：

1. 检索导航控制器；
2. 导航到目的地。

- ###### 检索导航控制器

Kotlin：

- Fragment.findNavController()
- View.findNavController()
- Activity.findNavController(viewId: Int)

Java：

- NavHostFragment.findNavController(Fragment)
- Navigation.findNavController(Activity, @IdRes int viewId)
- Navigation.findNavController(View)

> 说明：Kotlin可以直接在`Fragment`、`View`以及`Activity`使用`findNavController`是因为使用了扩展方法，当然，也可以直接跟Java那样调用对应的接口。


- ###### 导航

检索到导航控制器之后，使用导航控制器类的 `NavController.navigate()`API 导航到指定的目的地，`NavController.navigate()`有多个变体，这里就以使用目的地ID进行导航为例：

```kotlin
class FirstFragment : Fragment() {

    private var _binding: FragmentFirstBinding? = null

    // This property is only valid between onCreateView and
    // onDestroyView.
    private val binding get() = _binding!!

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {

        _binding = FragmentFirstBinding.inflate(inflater, container, false)
        return binding.root

    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        binding.buttonFirst.setOnClickListener {
            // Kotlin 扩展方法检索当前导航宿主的导航控制器并导航到目的地
            findNavController().navigate(R.id.action_FirstFragment_to_SecondFragment)
        }
    }

    override fun onDestroyView() {
        super.onDestroyView()
        _binding = null
    }
}
```

- ###### 返回到指定目的地

返回到指定的目的地，是指返回到之前导航过的目的地，这些目的地必须是在任务栈内的，可以通过 NavController.popBackStack() 接口返回上一级，或者通过 NavController.popBackStack (int destinationId, boolean inclusive) 返回到指定的某个目的地。

---

## 一、常用的控件使用方法

Android给我们提供了大量的UI控件，合理地使用这些控件就可以非常轻松地编写出相当不错的界面，下面我们就挑选几种常用的控件，详细介绍一下它们的使用方法。

### 1.1 TextView

TextView可以说是Android中最简单的一个控件了，你在前面其实已经和它打过一些交道了。
它主要用于在界面上显示一段文本信息

```xml
<TextView
    android:id="@+id/textview_first"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/hello_first_fragment"
    app:layout_constraintBottom_toTopOf="@id/button_first"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="parent" />
```

在TextView中我们使用`android:id`给当前控件定义了一个唯一标识符，这个属性在上一章中已经讲解过了。然后使用`android:layout_width`和`android:layout_height`指定了控件的宽度和高度。Android中所有的控件都具有这两个属
性，可选值有3种：match_parent、wrap_content和固定值。**match_parent表示让当前控件的大小和父布局的大小一样**，也就是**由父布局来决定当前控件的大小**。**wrap_content表示让当前控件的大小能够刚好包含住里面的内容**，也就是**由控件内容决定当前控件的大小**。固定值表示表示给控件指定一个固定的尺寸，单位一般用dp，这是一种屏幕密度无关的尺寸单位，可以保证在不同分辨率的手机上显示效果尽可能地一致，如50 dp就是一个有效的固定值。

我们使用android:gravity来指定文字的对齐方式，可选值有top、bottom、start、end、center等，可以用“|”来同时指定多个值，这里我们指定的是"center"，效果等同于"center_vertical|center_horizontal"，表示文字在垂直和水平方向都居中对齐。

通过android:textColor属性可以指定文字的颜色，通过android:textSize属性可以指定文字的大小。文字大小要使用sp作为单位，这样当用户在系统中修改了文字显示尺寸时，应用程序中的文字大小也会跟着变化。

### 1.2 Button

```xml
<Button
    android:id="@+id/button_first"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="@string/next"
    app:layout_constraintBottom_toBottomOf="parent"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@id/textview_first" />
```

Android系统默认会将按钮上的英文字母全部转换成大写，可能是认为按钮上的内容都比较重要吧。如果这不是你想要的效果，可以在XML中添加android:textAllCaps="false"这个属性，这样系统就会保留你指定的原始文字内容了。

### 1.3 EditText

EditText是程序用于和用户进行交互的另一个重要控件，它允许用户在控件里输入和编辑内容，并可以在程序中对这些内容进行处理。

```xml
<EditText
    android:id="@+id/edittext_test"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:layout_constraintBottom_toTopOf="@id/button_first"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="@id/textview_first" />
```

### 1.4 ImageView

ImageView是用于在界面上展示图片的一个控件，它可以让我们的程序界面变得更加丰富多彩。图片通常是放在以drawable开头的目录下的，并且要带上具体的分辨率。现在最主流的手机屏幕分辨率大多是xxhdpi的，所以我们在res目录下再新建一个drawable-xxhdpi目录，然后将事先准备好的两张图片img_1.png和img_2.png复制到该目录当中。

```xml
<ImageView
    android:id="@+id/imageView"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:src="@drawable/img01"
    app:layout_constraintBottom_toTopOf="parent"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="@id/textview_first" />
```

还可以通过调用ImageView的setImageResource()方法来改变ImageView中的图片

### 1.5 ProgressBar

ProgressBar用于在界面上显示一个进度条，表示我们的程序正在加载一些数据。它的用法也
非常简单，修改布局文件中的代码，如下所示：

```xml
<ProgressBar
    android:id="@+id/progressBar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:layout_constraintBottom_toTopOf="@id/button_first"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toTopOf="@id/textview_first"
    />
```

Android控件的可见属性。所有的Android控件都具有这个属性，可以通过android:visibility进行指定，可选值有3种：visible、invisible和gone。visible表示控件是可见的，这个值是默认值，不指定android:visibility时，控件都是可见的。invisible表示控件不可见，但是它仍然占据着原来的位置和大小，可以理解成控件变成透明状态了。gone则表示控件不仅不可见，而且不再占用任何屏幕空间。我们可以通过代码来设置控件的可见性，使用的是setVisibility()方法，允许传入View.VISIBLE、View.INVISIBLE和View.GONE这3种值。

我们还可以把进度条样式设置成水平进度条：`style="?android:attr/progressBarStyleHorizontal"` 再加一个最大值 `android:max="100"`

还可以通过代码手动更新进度： `binding.progressBar.progress += 10`

### 1.6 AlertDialog

AlertDialog可以在当前界面弹出一个对话框，这个对话框是置顶于所有界面元素之上的，能够屏蔽其他控件的交互能力，因此AlertDialog一般用于提示一些非常重要的内容或者警告信息。比如为了防止用户误删重要内容，在删除前弹出一个确认对话框。下面我们来学习一下它的用法，修改代码，如下所示：

```kotlin
AlertDialog.Builder(this.context).apply {
    setTitle("this is Dialog")
    setMessage("something important")
    setCancelable(false)
    setPositiveButton("OK"){
        dialog,which ->
    }
    /*setNegativeButton("Cancel"){
        dialog,which ->
    }*/
    show()
}
```

首先通过AlertDialog.Builder构建一个对话框，这里我们使用了Kotlin标准函数中的apply函数。在apply函数中为这个对话框设置标题、内容、可否使用Back键关闭对话框等属性，接下来调用setPositiveButton()方法为对话框设置确定按钮的点击事件，调用setNegativeButton()方法设置取消按钮的点击事件，最后调用show()方法将对话框显示出来就可以了。

---

## 二、详解7种基本布局

一个丰富的界面是由很多个控件组成的，那么我们如何才能让各个控件都有条不紊地摆放在界面上，而不是乱糟糟的呢？这就需要借助布局来实现了。布局是一种可用于放置很多控件的容器，它可以按照一定的规律调整内部控件的位置，从而编写出精美的界面。当然，布局的内部除了放置控件外，也可以放置布局，通过多层布局的嵌套，我们就能够完成一些比较复杂的界面实现。

![1697601553846.png](https://pic.ziyuan.wang/2023/10/18/guest_974d4c96f8f85_IP210.22.23.7_UPTIME1697601554.png)

下面我们就来详细学习一下Android中3种最基本的布局。

### 2.1 LinearLayout线性布局

> android:gravity：内部控件对齐方式，常用属性值有center、center_vertical、center_horizontal、top、bottom、left、right等。
> 这个属性在布局组件RelativeLayout、TableLayout中也有使用，FrameLayout、AbsoluteLayout则没有这个属性。
> center：居中显示，这里并不是表示显示在LinearLayout的中心，当LinearLayout线性方向为垂直方向时，center表示水平居中，但是并不能垂直居中，此时等同于center_horizontal的作用；同样当线性方向为水平方向时，center表示垂直居中，等同于center_vertical。
> top、bottom、left、right顾名思义为内部控件居顶、低、左、右布局。
> 这里要与android:layout_gravity区分开，layout_gravity是用来设置自身相对于父元素的布局。
> android:layout_weight：权重，用来分配当前控件在剩余空间的大小。
> 使用权重一般要把分配该权重方向的长度设置为零，比如在水平方向分配权重，就把width设置为零

LinearLayout又称作线性布局，是一种非常常用的布局。正如它的名字所描述的一样，这个布局会将它所包含的控件在线性方向上依次排列。相信你之前也已经注意到了，我们在上一节中学习控件用法时，所有的控件就都是放在LinearLayout布局里的，因此上一节中的控件也确实是在垂直方向上线性排列的。

既然是线性排列，肯定就不只有一个方向，那为什么上一节中的控件都是在垂直方向排列的呢？这是由于我们通过android:orientation属性指定了排列方向是vertical，如果指定的是horizontal，控件就会在水平方向上排列了。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".UILayoutTest">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Button" />
</LinearLayout>
```

需要注意，如果LinearLayout的排列方向是horizontal，内部的控件就绝对不能将宽度指定为match_parent，否则，单独一个控件就会将整个水平方向占满，其他的控件就没有可放置的位置了。同样的道理，如果LinearLayout的排列方向是vertical，内部的控件就不能将高度指定为match_parent。

android:layout_gravity属性，它和我们上一节中学到的android:gravity属性看起来有些相似，这两个属性有什么区别呢？其实从名字就可以看出，android:gravity用于指定文字在控件中的对齐方式，而android:layout_gravity用于指定控件在布局中的对齐方式。android:layout_gravity的可选值和android:gravity差不多，但是需要注意，当LinearLayout的排列方向是horizontal时，只有垂直方向上的对齐方式才会生效。因为此时水平方向上的长度是不固定的，每添加一个控件，水平方向上的长度都会改变，因而无法指定该方向上的对齐方式。同样的道理，当LinearLayout的排列方向是vertical时，只有水平方向上的对齐方式才会生效.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".UILayoutTest">

    <Button
        android:id="@+id/button"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="top"
        android:text="Button" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_vertical"
        android:text="Button" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="bottom"
        android:text="Button" />
</LinearLayout>
```

android:layout_weight。这个属性允许我们使用比例的方式来指定控件的大小，它在手机屏幕的适配性方面可以起到非常重要的作用。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".UILayoutTest">

    <EditText
        android:id="@+id/editText"
        android:layout_width="0dp"
        android:layout_weight="2"
        android:layout_height="wrap_content"/>
    <Button
        android:id="@+id/button"
        android:layout_width="0dp"
        android:layout_weight="1"
        android:layout_height="wrap_content"
        android:layout_gravity="top"
        android:text="Button" />
</LinearLayout>
```

上面将EditText设置成 `android:layout_weight="2"` Button设置成 `android:layout_weight="1"`这样在水平方向EditText就占2/3，Button占1/3.

### 2.2 RelativeLayout相对布局

> 相对布局可以让子控件相对于兄弟控件或父控件进行布局，可以设置子控件相对于兄弟控件或父控件进行上下左右对齐。
> RelativeLayout能替换一些嵌套视图，当我们用LinearLayout来实现一个简单的布局但又使用了过多的嵌套时，就可以考虑使用RelativeLayout重新布局。
> 相对布局就是一定要加Id才能管理。
>
> RelativeLayout中子控件常用属性：
> 1、相对于父控件，例如：android:layout_alignParentTop=“true”
> android:layout_alignParentTop      控件的顶部与父控件的顶部对齐;
> android:layout_alignParentBottom  控件的底部与父控件的底部对齐;
> android:layout_alignParentLeft      控件的左部与父控件的左部对齐;
> android:layout_alignParentRight     控件的右部与父控件的右部对齐;
>
> 2、相对给定Id控件，例如：android:layout_above=“@id/**”
> android:layout_above 控件的底部置于给定ID的控件之上;
> android:layout_below     控件的底部置于给定ID的控件之下;
> android:layout_toLeftOf    控件的右边缘与给定ID的控件左边缘对齐;
> android:layout_toRightOf  控件的左边缘与给定ID的控件右边缘对齐;
> android:layout_alignBaseline  控件的baseline与给定ID的baseline对齐;
> android:layout_alignTop        控件的顶部边缘与给定ID的顶部边缘对齐;
> android:layout_alignBottom   控件的底部边缘与给定ID的底部边缘对齐;
> android:layout_alignLeft       控件的左边缘与给定ID的左边缘对齐;
> android:layout_alignRight      控件的右边缘与给定ID的右边缘对齐;
>
> 3、居中，例如：android:layout_centerInParent=“true”
> android:layout_centerHorizontal 水平居中;
> android:layout_centerVertical    垂直居中;
> android:layout_centerInParent  父控件的中央;

RelativeLayout又称作相对布局，也是一种非常常用的布局。和LinearLayout的排列规则不同，RelativeLayout显得更加随意，它可以通过相对定位的方式让控件出现在布局的任何位置。也正因为如此，RelativeLayout中的属性非常多，不过这些属性都是有规律可循的，其实并不难理解和记忆。我们还是通过实践来体会一下，修改activity_main.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".UILayoutTest">

    <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentTop="true"
        android:text="Button" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentRight="true"
        android:layout_alignParentTop="true"
        android:text="Button" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="Button" />
    
    <Button
        android:id="@+id/button4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentBottom="true"
        android:layout_alignParentLeft="true"
        android:text="Button" />
    
    <Button
        android:id="@+id/button5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentRight="true"
        android:layout_alignParentBottom="true"
        android:text="Button" />

</RelativeLayout>
```

我们让Button 1和父布局的左上角对齐，Button 2和父布局的右上角对齐，Button 3居中显示，Button 4和父布局的左下角对齐，Button 5和父布局的右下角对齐。虽然android:layout_alignParentLeft、android:layout_alignParentTop、android:layout_alignParentRight、android:layout_alignParentBottom、android:layout_centerInParent这几个属性我们之前都没接触过，可是它们的名字已经完全说明了它们的作用。

上面例子中的每个控件都是相对于父布局进行定位的，那控件可不可以相对于控件进行定位呢？当然是可以的。

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".UILayoutTest">

    <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_above="@id/button3"
        android:layout_toLeftOf="@id/button3"
        android:text="Button 1" />

    <Button
        android:id="@+id/button2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_above="@id/button3"
        android:layout_toRightOf="@id/button3"
        android:text="Button 2" />

    <Button
        android:id="@+id/button3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerInParent="true"
        android:text="Button 3" />

    <Button
        android:id="@+id/button4"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/button3"
        android:layout_toLeftOf="@id/button3"
        android:text="Button 4" />

    <Button
        android:id="@+id/button5"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/button3"
        android:layout_toRightOf="@id/button3"
        android:text="Button 5" />

</RelativeLayout>
```

这次的代码稍微复杂一点，不过仍然是有规律可循的。android:layout_above属性可以让一个控件位于另一个控件的上方，需要为这个属性指定相对控件id的引用，这里我们填入了@id/button3，表示让该控件位于Button 3的上方。其他的属性也是相似的，android:layout_below表示让一个控件位于另一个控件的下方，android:layout_toLeftOf表示让一个控件位于另一个控件的左侧，android:layout_toRightOf表示让一个控件位于另一个控件的右侧。注意，当一个控件去引用另一个控件的id时，该控件一定要定义在引用控件的后面，不然会出现找不到id的情况。

RelativeLayout中还有另外一组相对于控件进行定位的属性，android:layout_alignLeft表示让一个控件的左边缘和另一个控件的左边缘对齐，android:layout_alignRight表示让一个控件的右边缘和另一个控件的右边缘对齐。此外，还有android:layout_alignTop和android:layout_alignBottom，道理都是一样的。

### 2.3 FrameLayout帧布局

> 帧布局或叫层布局，从屏幕左上角按照层次堆叠方式布局，后面的控件覆盖前面的控件。
> 该布局在开发中设计地图经常用到，因为是按层次方式布局，我们需要实现层面显示的样式时就可以
> 采用这种布局方式，比如我们要实现一个类似百度地图的布局，我们移动的标志是在一个图层的上面。
> 在普通功能的软件设计中用得也不多。层布局主要应用就是地图方面。

FrameLayout又称作帧布局，它相比于前面两种布局就简单太多了，因此它的应用场景少了很多。这种布局没有丰富的定位方式，所有的控件都会默认摆放在布局的左上角。让我们通过例子来看一看吧.

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".UILayoutTest">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:gravity="left"
        android:text="This is TextView" />

    <Button
        android:id="@+id/button1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="right"
        android:text="Button 1" />



</FrameLayout>
```

如果没有添加gravity属性，文字和按钮都位于布局的左上角。由于Button是在TextView之后添加的，因此按钮压在了文字的上面。当然，除了这种默认效果之外，我们还可以使用layout_gravity属性来指定控件在布局中的对齐方式，这和LinearLayout中的用法是相似的。

### 2.4 AbsoluteLayout绝对布局

绝对布局是前端布局中最为简单的布局，但灵活性极差，不具有自动适应设备分辨率的能力，就好比在手机上设置的布局，在平板上布局就会全部混乱，所以在日常开发中很少使用绝对布局。

| 属性          | 作用            |
| ------------- | --------------- |
| layout_width  | 设置组件的宽度  |
| layout_height | 设置组件的高度  |
| layout_x      | 设置组件的X坐标 |
| layout_y      | 设置组件的Y坐标 |

代码示例：

```xml
<?xml version="1.0" encoding="utf-8"?>
<AbsoluteLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">


    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="账号: "
        android:textSize="30sp"
        android:layout_x="40dp"
        android:layout_y="350dp"
        />

    <EditText
        android:layout_width="250dp"
        android:layout_height="50dp"
        android:layout_x="110dp"
        android:layout_y="350dp"
        />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="密码: "
        android:textSize="30sp"
        android:layout_x="40dp"
        android:layout_y="420dp"
        />

    <EditText
        android:layout_width="250dp"
        android:layout_height="50dp"
        android:layout_x="110dp"
        android:layout_y="420dp"
        />

    <Button
        android:layout_width="150dp"
        android:layout_height="50dp"
        android:layout_x="40dp"
        android:layout_y="500dp"
        android:text="登录"
        />

    <Button
        android:layout_width="150dp"
        android:layout_height="50dp"
        android:layout_x="210dp"
        android:layout_y="500dp"
        android:text="注册"
        />

</AbsoluteLayout>
```

### 2.5 TableLayout表格布局

表格布局就是类似于excel中的表格，每个“单元格”的位置都是由行与列共同决定，最后组件元素放入某个“单元格”进行布局定位，以达到最终的布局效果。

| 属性            | 作用                                       |
| --------------- | ------------------------------------------ |
| collapseColumns | 设置需要被隐藏的列的列序号                 |
| shrinkColumns   | 设置允许被收缩的列的列序号                 |
| stretchColumns  | 设置允许被拉伸的列的列序号                 |
| layout_column   | 设置跳过指定的列，组件元素从下一列开始显示 |
| layout_span     | 合并指定列数的单元格                       |

代码示例：

```xml
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_height="match_parent"
    android:layout_width="match_parent"
    android:gravity="center"
    >

    //第一行
    <TableRow
        android:gravity="center"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="用户名："
            android:textSize="40px"
            />

        <EditText
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:minWidth="200px"
            android:textSize="40px"
            />
    </TableRow>

    //第二行
    <TableRow
        android:gravity="center"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content">

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="密  码："
            android:textSize="40px"
            />

        <EditText
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:minWidth="200px"
            android:textSize="40px"
            android:inputType="textPassword"
            />
    </TableRow>

    //第三行
    <TableRow
        android:gravity="center"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content">

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="登 录"
            android:textSize="40px"
            android:layout_marginRight="20dp"
            android:background="#00BCD4"
            />

        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="注 册"
            android:textSize="40px"
            android:background="#FFC107"
            />

    </TableRow>

</TableLayout>

```

### 2.6 GridLayout网格布局

网格布局容器属性

| 属性           | 作用                                               |
| -------------- | -------------------------------------------------- |
| orientation    | 设置布局中组件的排列方式，`vertical`与`horizontal` |
| layout_gravity | 设置布局容器的对齐方式，`center`、`bottom`等等     |
| rowCount       | 设置网格布局有多少行                               |
| columnCount    | 设置网格布局有多少列                               |

网格布局组件属性

| 属性              | 作用                       |
| ----------------- | -------------------------- |
| layout_row        | 设置组件位于布局中的某一行 |
| layout_column     | 设置组件位于布局中的某一列 |
| layout_rowSpan    | 设置组件横跨几行           |
| layout_columnSpan | 设置组件横跨几列           |

代码示例：

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/GridLayout1"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:columnCount="4"
    android:orientation="horizontal"
    android:layout_gravity="center|top"
    android:rowCount="6" >

    <TextView
        android:layout_row="0"
        android:layout_columnSpan="4"
        android:layout_gravity="fill"
        android:background="@drawable/setbar_bg"
        android:text="0"
        android:gravity="right"
        android:textSize="50sp" />

    <Button
        android:layout_columnWeight="1"
        android:text="1/X" />

    <Button
        android:layout_columnWeight="2"
        android:text="X^2" />

    <Button
        android:layout_columnWeight="1"
        android:text="根号x"
        />

    <Button
        android:text="/"
        />

    <Button
        android:layout_gravity="fill"
        android:text="%"
        />

    <Button
        android:layout_gravity="fill"
        android:text="CE" />

    <Button
        android:layout_gravity="fill"
        android:text="C"
        />

    <Button
        android:layout_columnSpan="1"
        android:layout_gravity="fill"
        android:text="X"
        />

    <Button
        android:layout_gravity="fill"
        android:text="1/X" />

    <Button
        android:layout_gravity="fill"
        android:text="X^2" />

    <Button
        android:layout_gravity="fill"
        android:text="根号x"
        />

    <Button
        android:layout_gravity="fill"
        android:text="/"
        />

    <Button
        android:layout_gravity="fill"
        android:text="7" />

    <Button
        android:layout_gravity="fill"
        android:text="8" />

    <Button
        android:layout_gravity="fill"
        android:text="9"
        />

    <Button
        android:layout_gravity="fill"
        android:text="X"
        />

    <Button
        android:layout_gravity="fill"
        android:text="4" />

    <Button
        android:layout_gravity="fill"
        android:text="5" />

    <Button
        android:layout_gravity="fill"
        android:text="6"
        />

    <Button
        android:layout_gravity="fill"
        android:text="-"
        />

    <Button
        android:layout_gravity="fill"
        android:text="1" />

    <Button
        android:layout_gravity="fill"
        android:text="2" />

    <Button
        android:layout_gravity="fill"
        android:text="3"
        />

    <Button
        android:layout_gravity="fill"
        android:text="+"
        />

    <Button
        android:layout_gravity="fill"
        android:text="+/-" />

    <Button
        android:layout_gravity="fill"
        android:text="0" />

    <Button
        android:layout_gravity="fill"
        android:text="."
        />

    <Button
        android:layout_gravity="fill"
        android:text="="
        android:background="#92c2e8"
        />


</GridLayout>

```

### 2.7 **ConstraintLayout**约束布局

> ConstraintLayout 是一个使用“相对定位”灵活地确定微件的位置和大小的一个布局，在 2016 年 Google I/O 中面世，它的出现是为了解决开发中过于复杂的页面层级嵌套过多的问题——层级过深会增加绘制界面需要的时间，影响用户体验，以灵活的方式定位和调整小部件。从 Android Studio 2.3起，创建layout文件就已经是默认ConstraintLayout了

#### 2.7.1 布局的使用

##### 2.7.1.1 位置约束

> `ConstraintLayout`采用方向约束的方式对控件进行定位，至少要保证水平和垂直方向都至少有一个约束才能确定控件的位置

###### 1 基本方向布局

比如我们想实现这个位置，顶部和界面顶部对齐，左部和界面左部对齐：

![1697697651050.png](https://pic.ziyuan.wang/2023/10/19/guest_1f9b53a9ca9f7_IP210.22.23.7_UPTIME1697697653.png)

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".UILayoutTest">

    <TextView
        android:id="@+id/textView"
        android:layout_width="220dp"
        android:layout_height="40dp"
        android:background="@drawable/img01"
        android:text="This is TextView"
        android:textColor="#ffffff"
        android:textSize="25sp"

        android:textStyle="bold"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:ignore="HardcodedText" />

    <Button
        android:id="@+id/button_finish"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="15dp"
        android:text="Finish"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

核心代码：

```xml
app:layout_constraintStart_toStartOf="parent"
app:layout_constraintTop_toTopOf="parent"
```

> 这两行代码的意思就是,控件的开始方向与父容器的开始方向对齐，控件的顶部方向与父容器的顶部方向对齐，其实layout_constraintStart_toStartOf也可以使用layout_constraintLeft_toLeftOf，但是使用start和end来表示左和右是为了考虑别的国家的习惯，有的国家开始方向是右，所以使用start和end可以兼容这种情况。到这里就可以看到该控件使用layout_constraintStart_toStartOf和layout_constraintTop_toTopOf两条约束确定了自己的位置，这里有一个使用技巧，就是，该控件的？？方向在哪个控件的？？方向，记住这一点就可以了。那么下面就介绍下全部的约束属性：

|               基本方向约束                |   我的什么位置在谁的什么位置   |
| :---------------------------------------: | :----------------------------: |
|    app:layout_constraintTop_toTopOf=""    |     我的顶部和谁的顶部对齐     |
| app:layout_constraintBottom_toBottomOf="" |     我的底部和谁的底部对齐     |
|   app:layout_constraintLeft_toLeftOf=""   |     我的左边和谁的左边对齐     |
|  app:layout_constraintRight_toRightOf=""  |     我的右边和谁的右边对齐     |
|  app:layout_constraintStart_toStartOf=""  | 我的开始位置和谁的开始位置对齐 |
|    app:layout_constraintEnd_toEndOf=""    | 我的结束位置和谁的结束位置对齐 |
|  app:layout_constraintTop_toBottomOf=""   |   我的顶部位置在谁的底部位置   |
|   app:layout_constraintStart_toEndOf=""   |   我的开始位置在谁的结束为止   |

那么`ConstraintLayout`就是使用这些属性来确定控件的位置，虽然比较多，但是有规律可循，没有任何记忆压力

###### 2 基线对齐

比如我们需要实现这样的需求

![1697698697496.png](https://pic.ziyuan.wang/2023/10/19/guest_f952016181c6f_IP210.22.23.7_UPTIME1697698699.png)

两个文本是基线对齐的，那就可以用到我们的一个属性`layout_constraintBaseline_toBaselineOf`来实现，它的意思就是这个控件的基线与谁的基线对齐，代码如下：

```xml
    <TextView
        android:id="@+id/textViewPrice"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="20"
        android:textColor="#673AB7"
        android:textSize="50sp"
        android:textStyle="bold"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/button_finish" />

    <TextView
        android:id="@+id/textViewCurrency"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBaseline_toBaselineOf="@+id/textViewPrice"
        app:layout_constraintStart_toEndOf="@+id/textViewPrice"
        android:text="￥"
        android:textColor="#673AB7"
        android:textSize="20sp"
        android:textStyle="bold"/>
```

###### 3 角度约束

有些时候我们需要一个控件在某个控件的某个角度的位置，那么通过其他的布局其实是不太好实现的，但是`ConstraintLayout`为我们提供了角度位置相关的属性

|                 属性                 |           作用            |
| :----------------------------------: | :-----------------------: |
|    app:layout_constraintCircle=""    |      设置目标控件id       |
| app:layout_constraintCircleAngle=""  | 设置对于目标的角度(0-360) |
| app:layout_constraintCircleRadius="" |   设置到目标中心的距离    |

![1697700880378.png](https://pic.ziyuan.wang/2023/10/19/guest_3cbe062341685_IP210.22.23.7_UPTIME1697700881.png)

比如我们要实现这个效果就可以这样写：

```xml
    <ImageView
        android:id="@+id/img01"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:src="@drawable/img01"
        app:layout_constraintStart_toStartOf="@+id/textViewCurrency"
        app:layout_constraintTop_toBottomOf="@+id/textViewCurrency"/>
    <ImageView
        android:id="@+id/img02"
        android:layout_width="100dp"
        android:layout_height="100dp"
        android:src="@drawable/img02"
        app:layout_constraintCircle="@+id/img01"
        app:layout_constraintCircleAngle="45"
        app:layout_constraintCircleRadius="70dp"/>
```

###### 4 百分比偏移

有的时候我们需要让控件在父布局的水平方向或垂直方向的百分之多少的位置，可以使用如下属性：

|                  属性                   |                作用                 |
| :-------------------------------------: | :---------------------------------: |
| app:layout_constraintHorizontal_bias="" | 设置水平偏移（取值范围是0-1的小数） |
|  app:layout_constraintVertical_bias=""  | 设置垂直偏移（取值范围是0-1的小数） |

如果想实现这个效果，可以这样写：

![1697702282502.png](https://pic.ziyuan.wang/2023/10/19/guest_f9c488e8bbfd8_IP210.22.23.7_UPTIME1697702283.png)

```xml
    <TextView
        android:id="@+id/textView"
        android:layout_width="220dp"
        android:layout_height="40dp"
        android:background="@drawable/img01"
        android:text="This is TextView"
        android:textColor="#ffffff"
        android:textSize="25sp"

        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintVertical_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:ignore="HardcodedText" />
```

##### 2.7.1.2 控件内边距、外边距、GONE Margin

> `ConstraintLayout`的内边距和外边距的使用方式其实是和其他布局一致的

|              外边距               |           内边距            |
| :-------------------------------: | :-------------------------: |
|    android:layout_margin="0dp"    |    android:padding="0dp"    |
| android:layout_marginStart="0dp"  |    android:padding="0dp"    |
|  android:layout_marginLeft="0dp"  |  android:paddingLeft="0dp"  |
|  android:layout_marginTop="0dp"   |  android:paddingTop="0dp"   |
|  android:layout_marginEnd="0dp"   |  android:paddingEnd="0dp"   |
| android:layout_marginRight="0dp"  | android:paddingRight="0dp"  |
| android:layout_marginBottom="0dp" | android:paddingBottom="0dp" |

> `ConstraintLayout`除此之外还有`GONE Margin`，当依赖的目标`view`隐藏时会生效的属性，例如B被A依赖约束，当B隐藏时B会缩成一个点，自身的`margin`效果失效，A设置的`GONE Margin`就会生效，属性如下：

```xml
<!--  GONE Margin  -->
app:layout_goneMarginBottom="0dp"
app:layout_goneMarginEnd="0dp"
app:layout_goneMarginLeft="0dp"
app:layout_goneMarginRight="0dp"
app:layout_goneMarginStart="0dp"
app:layout_goneMarginTop="0dp"
```

比如下述代码：textView不可见时，button_finish的Top间距会变为100dp。也就是说textView不可见时，button_finish的`app:layout_goneMarginTop`属性就会生效

```xml
    <TextView
        android:id="@+id/textView"
        android:layout_width="220dp"
        android:layout_height="40dp"
        android:background="@drawable/img01"
        android:text="This is TextView"
        android:textColor="#ffffff"
        android:textSize="25sp"

        android:visibility="gone"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintVertical_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:ignore="HardcodedText" />

    <Button
        android:id="@+id/button_finish"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="15dp"
        android:text="Finish"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/textView"
        app:layout_goneMarginTop="100dp"/>
```

##### 2.7.1.3 控件尺寸

###### 1 尺寸限制

> 在`ConstraintLayout`中提供了一些尺寸限制的属性，可以用来限制最大、最小宽高度，这些属性只有在给出的宽度或高度为`wrap_content`时才会生效，比如想给宽度设置最小或最大值，那宽度就必须设置为`wrap_content`，这个比较简单就不放示例代码了，具体的属性如下：

|         属性         |        作用        |
| :------------------: | :----------------: |
| android:minWidth=""  | 设置view的最小宽度 |
| android:minHeight="" | 设置view的最小高度 |
| android:maxWidth=""  | 设置view的最大宽度 |
| android:maxHeight="" | 设置view的最大高度 |

###### 2 0dp(MATCH_CONSTRAINT)

> 设置view的大小除了传统的wrap_content、指定尺寸、match_parent外，ConstraintLayout还可以设置为0dp（MATCH_CONSTRAINT），并且0dp的作用会根据设置的类型而产生不同的作用，进行设置类型的属性是layout_constraintWidth_default和layout_constraintHeight_default，取值可为spread、percent、wrap。具体的属性及示例如下：

`app:layout_constraintWidth_default="spread|percent|wrap"` `app:layout_constraintHeight_default="spread|percent|wrap"`

- **spread（默认）**：占用所有的符合约束的空间

```xml
    <TextView
        android:id="@+id/textViewTest"
        android:layout_width="0dp"
        android:layout_height="60dp"
        android:layout_marginStart="50dp"
        android:layout_marginTop="50dp"
        android:layout_marginEnd="50dp"
        android:gravity="center"
        android:text="AAA水果批发"
        android:textColor="#abfa00"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintWidth_default="spread" />
```

> 可以看到，view的宽度适应了所有有效的约束空间，左右留出了margin的设置值50dp，这种效果就就是：自身view的大小充满可以配置的剩余空间，因为左右约束的都是父布局，所以view可配置的空间是整个父布局的宽度，又因为设置了margin，所以会留出margin的大小，因为spread是默认值，所以可以不写 app:layout_constraintWidth_default="spread"。

![1697773200939.png](https://pic.ziyuan.wang/2023/10/20/guest_5ce2a91c00179_IP210.22.23.7_UPTIME1697773202.png)

- **percent**: 按照父布局的百分比设置

> percent模式的意思是自身view的尺寸是父布局尺寸的一定比例，上图所展示的是宽度是父布局宽度的0.5（50%，取值是0-1的小数），该模式需要配合layout_constraintWidth_percent使用，但是写了layout_constraintWidth_percent后，layout_constraintWidth_default="percent"其实就可以省略掉了。

```xml
    <TextView
        android:id="@+id/textViewTest"
        android:layout_width="0dp"
        android:layout_height="60dp"

        android:layout_marginTop="50dp"

        android:gravity="center"
        android:text="AAA水果批发"
        android:textColor="#abfa00"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintWidth_default="percent"
        app:layout_constraintWidth_percent="0.3" />
```

![1697773522381.png](https://pic.ziyuan.wang/2023/10/20/guest_4d3c5295d3427_IP210.22.23.7_UPTIME1697773523.png)

- **wrap**：匹配内容大小但不超过约束限制

> 这里写了两个控件作为对比，控件A宽度设置为wrap_content，宽度适应内容大小，并且设置了margin，但是显然宽度已经超过margin的设置值了，而控件B宽度设置为0dp wrap模式，宽度适应内容大小，并且不会超过margin的设置值，也就是不会超过约束限制，这就是这两者的区别。Google还提供了两个属性用于强制约束：

```xml
    <TextView
        android:id="@+id/textViewFruit"
        android:layout_width="wrap_content"
        android:layout_height="60dp"
        android:layout_marginTop="50dp"
        android:layout_marginStart="100dp"
        android:layout_marginEnd="100dp"
        android:gravity="center"
        android:text="AAAAAAA水果批发"
        android:textColor="#abfa00"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintWidth_default="spread" />

    <TextView
        android:id="@+id/textViewVegetable"
        android:layout_width="0dp"
        android:layout_height="60dp"
        android:layout_marginTop="150dp"
        android:layout_marginStart="100dp"
        android:layout_marginEnd="100dp"
        android:gravity="center"
        android:text="AAAAAAA蔬菜批发"
        android:textColor="#abfa00"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintWidth_default="wrap" />
```

![1697781482210.png](https://pic.ziyuan.wang/2023/10/20/guest_944871a114b2d_IP210.22.23.7_UPTIME1697781483.png)

> <!--  当一个view的宽或高,设置成wrap_content时  -->
> app:layout_constrainedWidth="true|false"
> app:layout_constrainedHeight="true|false"

如果上述代码将控件A设置了强制约束，展示出的效果和控件B是一样的了。

除此之外，`0dp`还有一些其他的独特属性用于设置尺寸的大小限制：

|                属性                |            作用             |
| :--------------------------------: | :-------------------------: |
| app:layout_constraintWidth_min=""  | 0dp下，**宽度**的最**小**值 |
| app:layout_constraintHeight_min="" |   0dp下，**高度**的最小值   |
| app:layout_constraintWidth_max=""  | 0dp下，**宽度**的最**大**值 |
| app:layout_constraintHeight_max="" | 0dp下，**高度**的最**大**值 |

###### 3 比例宽高

> `ConstraintLayout`中可以对宽高设置比例，前提是至少有一个约束维度设置为`0dp`，这样比例才会生效，该属性可使用两种设置：
> 1.浮点值，表示宽度和高度之间的比率
> 2.宽度:高度，表示宽度和高度之间形式的比率

```xml
    <TextView
        android:id="@+id/textViewVegetable"
        android:layout_width="0dp"
        android:layout_height="60dp"
        android:layout_marginTop="150dp"
        android:layout_marginStart="100dp"
        android:layout_marginEnd="100dp"
        android:gravity="center"
        android:text="AAAAAAA蔬菜批发"
        android:textColor="#abfa00"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintDimensionRatio="10:3"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintWidth_default="wrap" />
```

比如上述代码：`android:layout_height="60dp"`但是width确实0dp，这个时候设置 `app:layout_constraintDimensionRatio="10:3"`就表明宽高比为 10:3 那么width就会变成200dp

##### 2.7.1.4 Chains

> Chains(链)也是一个非常好用的特性，它是将许多个控件在水平或者垂直方向，形成一条链，用于平衡这些控件的位置，那么如何形成一条链呢？形成一条链要求链中的控件在水平或者垂直方向，首尾互相约束，这样就可以形成一条链，水平方向互相约束形成的就是一条水平链，反之则是垂直链，下面看示例：

![1697783697533.png](https://pic.ziyuan.wang/2023/10/20/guest_8e874243c695f_IP210.22.23.7_UPTIME1697783699.png)

```xml
    <TextView
        android:id="@+id/textViewFruit"

        android:layout_width="80dp"
        android:layout_height="80dp"
        android:layout_marginTop="50dp"
        android:gravity="center"
        android:text="A水果批发"
        android:textColor="#abfa00"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toStartOf="@+id/textViewVegetable"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintHorizontal_chainStyle="spread" />

    <TextView
        android:id="@+id/textViewVegetable"
        android:layout_width="80dp"
        android:layout_height="80dp"
        android:layout_marginTop="50dp"
        android:gravity="center"
        android:text="A蔬菜批发"
        android:textColor="#abfa00"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintStart_toEndOf="@+id/textViewFruit"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
```

textViewFruit和textViewVegetable控件在水平方向上首尾互相约束，这样就形成了一条水平链，他们默认的模式是spread，均分剩余空间，我们可以使用layout_constraintHorizontal_chainStyle和layout_constraintVertical_chainStyle分别对水平和垂直链设置模式，模式可选的值有：spread、packed、spread_inside。

- **spread（默认）**：均分剩余空间

![1697783697533.png](https://pic.ziyuan.wang/2023/10/20/guest_8e874243c695f_IP210.22.23.7_UPTIME1697783699.png)

- **spread_inside**：两侧的控件贴近两边，剩余的控件均分剩余空间

![1697783950544.png](https://pic.ziyuan.wang/2023/10/20/guest_0046bfa66c4d1_IP210.22.23.7_UPTIME1697783952.png)

- **packed**：所有控件贴紧居中

![image-20231020143955874](C:\Users\Charlie\AppData\Roaming\Typora\typora-user-images\image-20231020143955874.png)

`Chains(链)`还支持`weight（权重）`的配置，使用`layout_constraintHorizontal_weight`和`layout_constraintVertical_weight`进行设置链元素的权重

![1697784461286.png](https://pic.ziyuan.wang/2023/10/20/guest_f9a5843454f59_IP210.22.23.7_UPTIME1697784463.png)

```xml
    <TextView
        android:id="@+id/textViewFruit"

        android:layout_width="0dp"
        android:layout_height="80dp"
        android:layout_marginTop="50dp"
        android:gravity="center"
        android:text="A水果批发"
        android:textColor="#abfa00"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toStartOf="@+id/textViewVegetable"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintHorizontal_weight="2"
        app:layout_constraintHorizontal_chainStyle="spread" />

    <TextView
        android:id="@+id/textViewVegetable"
        android:layout_width="0dp"
        android:layout_height="80dp"
        android:layout_marginTop="50dp"
        android:gravity="center"
        android:text="A蔬菜批发"
        android:textColor="#abfa00"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintStart_toEndOf="@+id/textViewFruit"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintHorizontal_weight="1"/>
```

比如说这里权重比就是 2:1 那么Fruit就占2/3，Vegetable占 1/3。并且使用权重时对应的宽度或者高度要改为0dp

#### 2.7.2 辅助类

> `ConstraintLayout`为了解决嵌套问题还提供了一系列的辅助控件帮助开发者布局，这些工具十分的方便，我在日常开发工作中也是使用的非常频繁

##### 2.7.2.1 Guideline（参考线）

> Guideline是一条参考线，可以帮助开发者进行辅助定位，并且实际上它并不会真正显示在布局中，像是数学几何中的辅助线一样，使用起来十分方便，出场率很高，Guideline也可以用来做一些百分比分割之类的需求，有着很好的屏幕适配效果，Guideline有水平和垂直方向之分，位置可以使用针对父级的百分比或者针对父级位置的距离。

|                    属性                    |                 作用                 |
| :----------------------------------------: | :----------------------------------: |
| android:orientation="horizontal\|vertical" |           辅助线的对齐方式           |
|  app:layout_constraintGuide_percent="0-1"  | 距离父级宽度或高度的百分比(小数形式) |
|    app:layout_constraintGuide_begin=""     |  距离父级起始位置的距离(左侧或顶部)  |
|     app:layout_constraintGuide_end=""      |  距离父级结束位置的距离(右侧或底部)  |

![1697785850352.png](https://pic.ziyuan.wang/2023/10/20/guest_8c346b6c79397_IP210.22.23.7_UPTIME1697785851.png)

比如要实现这样的效果就可以这么写：

```xml
    <androidx.constraintlayout.widget.Guideline
        android:id="@+id/guideLine"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal"
        app:layout_constraintGuide_percent=".3"/>

    <TextView
        android:id="@+id/textViewFruit"

        android:layout_width="0dp"
        android:layout_height="80dp"
        android:layout_marginTop="50dp"
        android:gravity="center"
        android:text="A水果批发"
        android:textColor="#abfa00"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="@+id/guideLine" />
```

##### 2.7.2.2 Barrier（屏障）

这个`Barrier`和`Guideline`一样，也不会实际出现在布局中，它的作用如同其名，形成一个屏障、障碍，使用也非常多。

比如我们下面代码：

```xml
    <TextView
        android:id="@+id/textView1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="warehouse"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"/>

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="warehouse house"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/textView1"/>

    <TextView
        android:id="@+id/textView3"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="5dp"
        android:text="@string/text3"
        app:layout_constraintStart_toEndOf="@id/textView1"
        app:layout_constraintTop_toTopOf="parent" 
        app:layout_constraintEnd_toEndOf="parent"/>
```

效果是这样的

![1698027242366.png](https://pic.ziyuan.wang/2023/10/23/guest_5bb3d887d5194_IP210.22.23.7_UPTIME1698027243.png)

可以发现因为 textView2 字符过长，导致有一部分text被遮挡了。

其实这里我们是可以优化的，那要怎么优化呢?也很简单，只需要加一个 Barrier就可以了。让textView3 Start_toEndOf于Barrier。优化后效果如图

![1698027107734.png](https://pic.ziyuan.wang/2023/10/23/guest_3f61840a217e6_IP210.22.23.7_UPTIME1698027109.png)

代码如下：

```xml
    <TextView
        android:id="@+id/textView1"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="warehouse"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"/>

    <TextView
        android:id="@+id/textView2"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="warehouse house"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@id/textView1"/>

    <androidx.constraintlayout.widget.Barrier
        android:id="@+id/barrier"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:barrierDirection="end"
        app:constraint_referenced_ids="textView2,textView1"/>

    <TextView
        android:id="@+id/textView3"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_marginStart="5dp"
        android:text="@string/text3"
        app:layout_constraintStart_toEndOf="@id/barrier"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>
```

##### 2.7.2.3 Group（组）

> 工作当中常常会有很多个控件同时隐藏或者显示的场景，传统做法要么是进行嵌套，对父布局进行隐藏或显示，要么就是一个一个设置，这显然都不是很好的办法，ConstraintLayout中的Group就是来解决这个问题的。Group的作用就是可以对一组控件同时隐藏或显示，没有其他的作用，它的属性如下：

|                 属性                  |      作用      |
| :-----------------------------------: | :------------: |
| app:constraint_referenced_ids="id,id" | 加入组的控件id |

拿上面的代码举个栗子🌰

```xml
    <androidx.constraintlayout.widget.Group
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:visibility="visible"
        app:constraint_referenced_ids="textView1,textView2,textView3"/>
```

这里我们将 textView1-3 都加入到组中，当我们设置visibility的值时，这三个textView会一起改变。

##### 2.7.2.4 Placeholder（占位符）

> `Placeholder`的作用就是占位，它可以在布局中占好位置，通过`app:content=""`属性，或者动态调用`setContent()`设置内容，来让某个控件移动到此占位符中

```xml
    <androidx.constraintlayout.widget.Placeholder
        android:layout_width="100dp"
        android:layout_height="60dp"
        app:content="@id/textView"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintBottom_toBottomOf="parent" />
```

当我们设置`app:content="@+id/A"`或者调用`setContent()`时，控件A就会被移动到`Placeholder`中，当然在布局中使用`app:content=""`显然就失去了它的作用。

##### 2.7.2.5 Flow（流式虚拟布局）

> Flow是用于构建链的新虚拟布局，当链用完时可以缠绕到下一行甚至屏幕的另一部分。当您在一个链中布置多个项目时，这很有用，但是您不确定容器在运行时的大小。您可以使用它来根据应用程序中的动态尺寸（例如旋转时的屏幕宽度）构建布局。Flow是一种虚拟布局。在ConstraintLayout中，虚拟布局(Virtual layouts)作为virtual view group的角色参与约束和布局中，但是它们并不会作为视图添加到视图层级结构中，而是仅仅引用其它视图来辅助它们在布局系统中完成各自的布局功能。

```xml
<androidx.constraintlayout.helper.widget.Flow
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    app:constraint_referenced_ids="A,B,C,D,E"
    app:layout_constraintTop_toTopOf="parent" />
```

1. 链约束

   > Flow的constraint_referenced_ids关联的控件是没有设置约束的，这一点和普通的链是不一样的，这种排列方式是Flow的默认方式none，我们可以使用app:flow_wrapMode=""属性来设置排列方式，并且我们还可以使用flow_horizontalGap和flow_verticalGap分别设置两个view在水平和垂直方向的间隔，下面我们再添加几个控件来展示三种排列方式：

   - none（默认值）： 所有引用的`view`形成一条链，水平居中，超出屏幕两侧的`view`不可见
   - chain： 所引用的`view`形成一条链，超出部分会自动换行，同行的`view`会平分宽度。
   - aligned： 所引用的`view`形成一条链，但`view`会在同行同列。

   当`flow_wrapMode`的值是`chian`或`aligned`时，我们还可以针对不同的链进行配置，这里就不一一展示效果了，具体的属性如下：

   |                             属性                             |                作用                |
   | :----------------------------------------------------------: | :--------------------------------: |
   |   app:flow_horizontalStyle="packed｜spread｜spread_inside"   |          所有水平链的配置          |
   |    app:flow_verticalStyle="packed｜spread｜spread_inside"    |          所有垂直链的配置          |
   | app:flow_firstHorizontalStyle="packed｜spread｜spread_inside" |  第一条水平链的配置，其他条不生效  |
   | app:flow_firstVerticalStyle="packed｜spread｜spread_inside"  |  第一条垂直链的配置，其他条不生效  |
   | app:flow_lastHorizontalStyle="packed｜spread｜spread_inside" | 最后一条水平链的配置，其他条不生效 |
   |  app:flow_lastVerticalStyle="packed｜spread｜spread_inside"  | 最后一条垂直链的配置，其他条不生效 |

2. 对齐约束

   > 上面展示的都是相同大小的`view`，那么不同大小`view`的对齐方式，`Flow`也提供了相应的属性进行配置(`flow_wrapMode="aligned"`时，我试着没有效果)

   |                          属性                          |                             作用                             |
   | :----------------------------------------------------: | :----------------------------------------------------------: |
   | app:flow_verticalAlign="top｜bottom｜center｜baseline" | top:顶对齐、bottom:底对齐、center:中心对齐、baseline:基线对齐 |
   |          app:flow_horizontalAlign=start\|end"          |        start:开始对齐、end:结尾对齐、center:中心对齐         |

   > 使用`flow_verticalAlign`时，要求`orientation`的方向是`horizontal`，而使用`flow_horizontalAlign`时，要求`orientation`的方向是`vertical`

3. 数量约束

   > 当`flow_wrapMode`属性为`aligned`和`chian`时，通过`flow_maxElementsWrap`属性控制每行最大的子`View`数量，例如我们设置为`flow_maxElementsWrap=4`，那么每行最大子 `View`数量就是4.

##### 2.7.2.6 Layer（层布局）

> Layer继承自ConstraintHelper，是一个约束助手，相对于Flow来说，Layer的使用较为简单，常用来增加背景，或者共同动画，图层 (Layer) 在布局期间会调整大小，其大小会根据其引用的所有视图进行调整，代码的先后顺序也会决定着它的位置，如果代码在所有引用view的最后面，那么它就会在所有view的最上面，反之则是最下面，在最上面的时候如果添加背景，就会把引用的view覆盖掉，下面展示下添加背景的例子，做动画的例子这里不再展示了

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#DAF3FE"
    tools:context=".MainActivity"
    tools:ignore="HardcodedText">

    <androidx.constraintlayout.helper.widget.Layer
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@drawable/common_rect_white_100_10"
        android:padding="10dp"
        app:constraint_referenced_ids="AndroidImg,NameTv" />

    <ImageView
        android:id="@+id/AndroidImg"
        android:layout_width="200dp"
        android:layout_height="wrap_content"
        android:src="@drawable/android"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/NameTv"
        android:layout_width="100dp"
        android:layout_height="40dp"
        android:gravity="center"
        android:text="Android"
        android:textColor="@color/black"
        android:textSize="25sp"
        android:textStyle="bold"
        app:layout_constraintEnd_toEndOf="@id/AndroidImg"
        app:layout_constraintStart_toStartOf="@id/AndroidImg"
        app:layout_constraintTop_toBottomOf="@id/AndroidImg" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

当`Layer`的代码在所有引用`view`的上面时，效果是正常的，因为此时所有的`view`都在`Layer`的上面。但当`Layer`代码在最后面时,`Layer`就会把所有的`view`覆盖住，效果就不正常了。

##### 2.7.2.7 ImageFilterButton & ImageFilterView

> ImageFilterButton和ImageFilterView是两个控件，他们之间的关系就和ImageButton与ImageView是一样的，所以这里就只拿ImageFilterView来做讲解。从名字上来看，它们的定位是和过滤有关系的，它们的大致作用有两部分，一是可以用来做圆角图片，二是可以叠加图片资源进行混合过滤，下面一一展示：

1. 圆角图片

   > ImageFilterButton和ImageFilterView可以使用两个属性来设置图片资源的圆角，分别是roundPercent和round，roundPercent接受的值类型是0-1的小数，根据数值的大小会使图片在方形和圆形之间按比例过度，round=可以设置具体圆角的大小，我在使用的过程中发现我的AndroidStudio，没有这两个属性的代码提示，也没有预览效果，但是运行起来是有效果的，可能是没有做好优化吧。最近很热门的一个话题，小米花费200万设计的新logo，我们拿来做做例子：

   ```xml
   <androidx.constraintlayout.utils.widget.ImageFilterView
       android:layout_width="100dp"
       android:layout_height="100dp"
       android:src="@drawable/mi"
       app:layout_constraintBottom_toBottomOf="parent"
       app:layout_constraintEnd_toEndOf="parent"
       app:layout_constraintStart_toStartOf="parent"
       app:layout_constraintTop_toTopOf="parent"
       app:roundPercent="0.7"/>
   ```

   > 虽然和小米新logo的圆弧不太一样，不过这也不是我们考虑的地方，可以看到我们使用`roundPercent`设置了圆角为0.7（70%），实现一个圆角图片就是如此简单。

2. 图片过滤

   > ImageFilterButton和ImageFilterView不但可以使用src来设置图片资源，还可以使用altSrc来设置第二个图片资源，altSrc提供的资源将会和src提供的资源通过crossfade属性形成交叉淡化效果,默认情况下，crossfade=0，altSrc所引用的资源不可见，取值在0-1。下面看例子：

   ```xml
       <androidx.constraintlayout.utils.widget.ImageFilterView
           android:layout_width="100dp"
           android:layout_height="100dp"
           android:src="@drawable/mi"
           app:altSrc="@drawable/ic_launcher_background"
           app:crossfade=".5"
           app:layout_constraintBottom_toBottomOf="parent"
           app:layout_constraintEnd_toEndOf="parent"
           app:layout_constraintStart_toStartOf="parent"
           app:layout_constraintTop_toTopOf="parent"
           app:roundPercent="0.7"/>
   ```

   > 除此之外，`warmth`属性可以用来调节色温，`brightness`属性用来调节亮度，`saturation`属性用来调节饱和度，`contrast`属性用来调节对比度

##### 2.7.2.8 MockView

> 你家产品经理经常会给你画原型图，但这绝对不是他们的专属，我们也有自己的原型图，一个成熟的程序员要学会给自己的产品经理画大饼，我们可以使用`MockView`来充当原型图，下面看例子：

---

## 三、自定义控件

在前两节我们学习了Android中的一些常用控件和基本布局的用法，不过当时我们并没有关注这些控件和布局的继承结构，现在是时候来看一下了，如图所示。

![1698114246144.png](https://pic.ziyuan.wang/2023/10/24/xiheya_a377f4dd3ff37.png)

可以看到，我们所用的所有控件都是直接或间接继承自View的，所用的所有布局都是直接或间接继承自ViewGroup的。View是Android中最基本的一种UI组件，它可以在屏幕上绘制一块矩形区域，并能响应这块区域的各种事件，因此，我们使用的各种控件其实就是在View的基础上又添加了各自特有的功能。而ViewGroup则是一种特殊的View，它可以包含很多子View和子ViewGroup，是一个用于放置控件和布局的容器。

这个时候我们就可以思考一下，当系统自带的控件并不能满足我们的需求时，可不可以利用上面的继承结构来创建自定义控件呢？答案是肯定的.

### 3.1 引入布局

一般我们的程序中可能有很多个Activity需要这样的标题栏，如果在每个Activity的布局中都编写一遍同样的标题栏代码，明显就会导致代码的大量重复。这时我们就可以使用引入布局的方式来解决这个问题，在layout目录下新建一个title.xml布局，代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="@drawable/title_bg">

    <Button
        android:id="@+id/titleBack"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_margin="5dp"
        android:background="@drawable/back_bg"
        android:text="Back"
        android:textColor="#fff"/>

    <TextView
        android:id="@+id/titleText"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_weight="1"
        android:gravity="center"
        android:text="Title Text"
        android:textColor="#fff"
        android:textSize="25sp"/>

    <Button
        android:id="@+id/titleEdit"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:layout_margin="5dp"
        android:background="@drawable/edit_bg"
        android:text="Edit"
        android:textColor="#fff"/>

</LinearLayout>
```

可能会出现修改button背景失败的情况，这个时候只需要修改 `themes.xml`中的`<style name="Theme.UICustomViews" parent="Theme.MaterialComponents.DayNight.DarkActionBar">`，把他修改成： `<style name="Theme.UICustomViews" parent="Theme.MaterialComponents.DayNight.DarkActionBar.Bridge">`这样就可以修改背景颜色啦。

可以看到，我们在LinearLayout中分别加入了两个Button和一个TextView，左边的Button可用于返回，右边的Button可用于编辑，中间的TextView则可以显示一段标题文本。上面代码中的大多数属性是你已经见过的，下面我来说明一下几个之前没有讲过的属性。android:background用于为布局或控件指定一个背景，可以使用颜色或图片来进行填充。这里我提前准备好了3张图片——title_bg.png、back_bg.png和edit_bg.png（资源下载地址见前言），分别用于作为标题栏、返回按钮和编辑按钮的背景。另外，在两个Button中我们都使用了android:layout_margin这个属性，它可以指定控件在上下左右方向上的间距。当然也可以使用android:layout_marginLeft或android:layout_marginTop等属性来单独指定控件在某个方向上的间距。

现在标题栏布局已经编写完成了，剩下的就是如何在程序中使用这个标题栏了，修改activity_main.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <include layout="@layout/title"/>

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

没错！我们只需要通过一行include语句引入标题栏布局就可以了。 `<include layout="@layout/title"/>`

最后我们需要在MainActivity中将自带的标题栏隐藏掉，如下所示。

```kotlin
package work.icu007.uicustomviews

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import work.icu007.uicustomviews.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        supportActionBar?.hide()
    }
}
```

这里我们调用了getSupportActionBar()方法来获得ActionBar的实例，然后再调用它的hide()方法将标题栏隐藏起来。由于ActionBar有可能为空，所以这里还使用了?.操作符。

> 如果使用 viewbinding 出现Unresolved reference: databinding的报错，可能是对应module的build.gradle没有添加databinding配置

这个时候只需要在对应module的 bulid.gradle中加入：

```tex
    buildFeatures {
        viewBinding true
    }
    
// 如下
android {
    compileSdk 34

    defaultConfig {
        applicationId "work.icu007.uicustomviews"
        minSdk 21
        targetSdk 33
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
    buildFeatures {
        viewBinding true
    }
}
```

即可

### 3.2 创建自定义控件

引入布局的技巧确实解决了重复编写布局代码的问题，但是如果布局中有一些控件要求能够响应事件，我们还是需要在每个Activity中为这些控件单独编写一次事件注册的代码。比如标题栏中的返回按钮，其实不管是在哪一个Activity中，这个按钮的功能都是相同的，即销毁当前Activity。而如果在每一个Activity中都需要重新注册一遍返回按钮的点击事件，无疑会增加很多重复代码，这种情况最好是使用自定义控件的方式来解决。

新建TitleLayout继承自LinearLayout，让它成为我们自定义的标题栏控件，代码如下所示：

```kotlin
package work.icu007.uicustomviews

import android.content.Context
import android.util.AttributeSet
import android.view.LayoutInflater
import android.widget.LinearLayout


/*
 * Author: Charlie_Liao
 * Time: 2023/10/27-11:50
 * E-mail: rookie_l@icu007.work
 */

class TitleLayout(context: Context, attrs: AttributeSet) : LinearLayout(context, attrs) {
    init {
        LayoutInflater.from(context).inflate(R.layout.title, this)
    }
}
```

这里我们在TitleLayout的主构造函数中声明了Context和AttributeSet这两个参数，在布局中引入TitleLayout控件时就会调用这个构造函数。然后在init结构体中需要对标题栏布局进行动态加载，这就要借助LayoutInflater来实现了。通过LayoutInflater的from()方法可以构建出一个LayoutInflater对象，然后调用inflate()方法就可以动态加载一个布局文件。inflate()方法接收两个参数：第一个参数是要加载的布局文件的id，这里我们传入R.layout.title；第二个参数是给加载好的布局再添加一个父布局，这里我们想要指定为TitleLayout，于是直接传入this。

现在自定义控件已经创建好了，接下来我们需要在布局文件中添加这个自定义控件，修改activity_main.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <work.icu007.uicustomviews.TitleLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:ignore="MissingConstraints" />

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

添加自定义控件和添加普通控件的方式基本是一样的，只不过在添加自定义控件的时候，我们需要指明控件的完整类名，包名在这里是不可以省略的。重新运行程序，你会发现此时的效果和使用引入布局方式的效果是一样的。

下面我们尝试为标题栏中的按钮注册点击事件，修改TitleLayout中的代码，如下所示：

```kotlin
package work.icu007.uicustomviews

import android.app.Activity
import android.content.Context
import android.util.AttributeSet
import android.view.LayoutInflater
import android.widget.Button
import android.widget.LinearLayout
import android.widget.Toast
import work.icu007.uicustomviews.databinding.TitleBinding


/*
 * Author: Charlie_Liao
 * Time: 2023/10/27-11:50
 * E-mail: rookie_l@icu007.work
 */

class TitleLayout(context: Context, attrs: AttributeSet) : LinearLayout(context, attrs) {
    init {
        LayoutInflater.from(context).inflate(R.layout.title, this)

        val btnBack = findViewById<Button>(R.id.titleBack)
        btnBack.setOnClickListener {
            val activity = context as Activity
            activity.finish()
        }

        val btnEdit = findViewById<Button>(R.id.titleEdit)
        btnEdit.setOnClickListener {
            Toast.makeText(context, "u click btnEdit", Toast.LENGTH_SHORT).show()
        }
    }
}
```

这里我们分别给返回和编辑这两个按钮注册了点击事件，当点击返回按钮时销毁当前Activity，当点击编辑按钮时弹出一段文本。

注意，TitleLayout中接收的context参数实际上是一个Activity的实例，在返回按钮的点击事件里，我们要先将它转换成Activity类型，然后再调用finish()方法销毁当前的Activity。Kotlin中的类型强制转换使用的关键字是as，由于是第一次用到，所以这里单独讲解一下。

重新运行程序，点击一下编辑按钮，效果如图所示。点击返回按钮，当前界面就会立即关闭。由此说明，我们的自定义控件确实已经可以正常工作了。

![1698488183331.png](https://pic.ziyuan.wang/2023/10/28/xiheya_d682163f33870.png)

这样的话，每当我们在一个布局中引入TitleLayout时，返回按钮和编辑按钮的点击事件就已经自动实现好了，这就省去了很多编写重复代码的工作。

### 3.3 最常用和最难用的控件：ListView

ListView在过去绝对可以称得上是Android中最常用的控件之一，几乎所有的应用程序都会用到它。由于手机屏幕空间比较有限，能够一次性在屏幕上显示的内容并不多，当我们的程序中有大量的数据需要展示的时候，就可以借助ListView来实现。ListView允许用户通过手指上下滑动的方式将屏幕外的数据滚动到屏幕内，同时屏幕上原有的数据会滚动出屏幕。你其实每天都在使用这个控件，比如查看QQ聊天记录，翻阅微博最新消息，等等。

#### 3.3.1 ListView的简单用法

新建一个ListViewTest项目，修改 activity_main.xml文件如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <ListView
        android:id="@+id/listView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>


</androidx.constraintlayout.widget.ConstraintLayout>
```

在布局中加入ListView控件还算非常简单，先为ListView指定一个id，然后将宽度和高度都设置为match_parent，这样ListView就占满了整个布局的空间。

接下来修改MainActivity中的代码

```kotlin
package work.icu007.listviewtest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.LayoutInflater
import android.widget.ArrayAdapter
import work.icu007.listviewtest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    private val data = listOf<String>("Apple","Banana", "Orange", "Watermelon",
        "Pear", "Grape", "Pineapple", "Strawberry", "Cherry", "Mango",
        "Apple", "Banana", "Orange", "Watermelon", "Pear", "Grape",
        "Pineapple", "Strawberry", "Cherry", "Mango")
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        val adapter = ArrayAdapter<String>(this,android.R.layout.simple_list_item_1,data)
        binding.listView.adapter = adapter
    }
}
```

既然ListView是用于展示大量数据的，那我们就应该先将数据提供好。这些数据可以从网上下载，也可以从数据库中读取，应该视具体的应用程序场景而定。这里我们就简单使用一个data集合来进行测试，里面包含了很多水果的名称，初始化集合的方式使用的是之前在第2章学过的listOf()函数。

不过，集合中的数据是无法直接传递给ListView的，我们还需要借助适配器来完成。Android中提供了很多适配器的实现类，其中我认为最好用的就是ArrayAdapter。它可以通过泛型来指定要适配的数据类型，然后在构造函数中把要适配的数据传入。ArrayAdapter有多个构造函数的重载，你应该根据实际情况选择最合适的一种。由于我们这里提供的数据都是字符串，因此将ArrayAdapter的泛型指定为String，然后在ArrayAdapter的构造函数中依次传入Activity的实例、ListView子项布局的id，以及数据源。注意，我们使用了android.R.layout.simple_list_item_1作为ListView子项布局的id，这是一个Android内置的布局文件，里面只有一个TextView，可用于简单地显示一段文本。这样适配器对象就构建好了。

最后，还需要调用ListView的setAdapter()方法，将构建好的适配器对象传递进去，这样ListView和数据之间的关联就建立完成了。效果如下

![效果](https://pic.ziyuan.wang/2023/10/31/xiheya_d0ff1190c945b.png)

#### 3.3.2 定制ListView的界面

只能显示一段文本的ListView实在是太单调了，现在就来对ListView的界面进行定制，让它可以显示更加丰富的内容。

定义一个实体类，作为ListView适配器的适配类型。新建Fruit类。Fruit类中只有两个字段：name表示水果的名字，imageId表示水果对应图片的资源id。代码如下所示：

```kotlin
package work.icu007.listviewtest
/*
 * Author: Charlie_Liao
 * Time: 2023/10/31-14:28
 * E-mail: rookie_l@icu007.work
 */

class Fruit(val name:String, val imageID: Int)
```

然后需要为ListView的子项指定一个我们自定义的布局，在layout目录下新建fruit_item.xml，代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="60dp">

    <ImageView
        android:id="@+id/fruitImage"
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:layout_gravity="center_vertical"
        android:layout_marginLeft="10dp"/>

    <TextView
        android:id="@+id/fruitName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_vertical"
        android:layout_marginLeft="20dp"/>

</LinearLayout>
```

在这个布局中，我们定义了一个ImageView用于显示水果的图片，又定义了一个TextView用于显示水果的名称，并让ImageView和TextView都在垂直方向上居中显示。

接下来需要创建一个自定义的适配器，这个适配器继承自ArrayAdapter，并将泛型指定为Fruit类。新建类FruitAdapter，代码如下所示：

```kotlin
package work.icu007.listviewtest

import android.app.Activity
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ArrayAdapter
import android.widget.ImageView
import android.widget.TextView


/*
 * Author: Charlie_Liao
 * Time: 2023/10/31-14:36
 * E-mail: rookie_l@icu007.work
 */

class FruitAdapter(activity: Activity, private val resourceId: Int, data: List<Fruit>) :
    ArrayAdapter<Fruit>(activity, resourceId, data) {

    override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {
        // 将 resourceId 指向的视图 一一填充到父布局当中，并存为view变量
        val view = LayoutInflater.from(context).inflate(resourceId, parent, false)
        val fruitImage: ImageView = view.findViewById(R.id.fruitImage)
        val fruitName: TextView = view.findViewById(R.id.fruitName)
        // 根据position 获取当前的Fruit实例
        val fruitItem = getItem(position)
        if(fruitItem != null){
            // 若实例不为空，则为fruitImage 和fruitName 设置成该实例中的ImageID及name
            fruitImage.setImageResource(fruitItem.imageID)
            fruitName.text = fruitItem.name
        }
        return view
    }

}
```

FruitAdapter定义了一个主构造函数，用于将Activity的实例、ListView子项布局的id和数据源传递进来。另外又重写了getView()方法，这个方法在每个子项被滚动到屏幕内的时候会被调用。

在getView()方法中，首先使用LayoutInflater来为这个子项加载我们传入的布局。LayoutInflater的inflate()方法接收3个参数，前两个参数我们已经知道是什么意思了，第三个参数指定成false，表示只让我们在父布局中声明的layout属性生效，但不会为这个View添加父布局。因为一旦View有了父布局之后，它就不能再添加到ListView中了。如果你现在还不能理解这段话的含义，也没关系，只需要知道这是ListView中的标准写法就可以了，当你以后对View理解得更加深刻的时候，再来读这段话就没有问题了。

我们继续往下看，接下来调用View的findViewById()方法分别获取到ImageView和TextView的实例，然后通过getItem()方法得到当前项的Fruit实例，并分别调用它们的setImageResource()和setText()方法设置显示的图片和文字，最后将布局返回，这样我们自定义的适配器就完成了。

最后修改MainActivity中的代码，如下所示：

```kotlin
package work.icu007.listviewtest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.LayoutInflater
import android.widget.ArrayAdapter
import work.icu007.listviewtest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    /*private val data = listOf<String>("Apple","Banana", "Orange", "Watermelon",
        "Pear", "Grape", "Pineapple", "Strawberry", "Cherry", "Mango",
        "Apple", "Banana", "Orange", "Watermelon", "Pear", "Grape",
        "Pineapple", "Strawberry", "Cherry", "Mango")*/
    private val fruitList = ArrayList<Fruit>()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        /*val adapter = ArrayAdapter<String>(this,android.R.layout.simple_list_item_1,data)
        binding.listView.adapter = adapter*/
        // 初始化
        initFruits()
        // 创建一个adapter
        val adapter = FruitAdapter(this, R.layout.fruit_item, fruitList)
        // 将新建的adapter 设置为 listView的adapter
        binding.listView.adapter = adapter
    }
    private fun initFruits() {
        repeat(2){
            fruitList.add(Fruit("Apple", R.drawable.apple_pic))
            fruitList.add(Fruit("Banana", R.drawable.banana_pic))
            fruitList.add(Fruit("Orange", R.drawable.orange_pic))
            fruitList.add(Fruit("Watermelon", R.drawable.watermelon_pic))
            fruitList.add(Fruit("Pear", R.drawable.pear_pic))
            fruitList.add(Fruit("Grape", R.drawable.grape_pic))
            fruitList.add(Fruit("Pineapple", R.drawable.pineapple_pic))
            fruitList.add(Fruit("Strawberry", R.drawable.strawberry_pic))
            fruitList.add(Fruit("Cherry", R.drawable.cherry_pic))
            fruitList.add(Fruit("Mango", R.drawable.mango_pic))
        }
    }
}
```

效果如下：

![1698737713152.png](https://pic.ziyuan.wang/2023/10/31/xiheya_1a87af42ff442.png)

#### 3.3.3 提升ListView的运行效率

之所以说ListView这个控件很难用，是因为它有很多细节可以优化，其中运行效率就是很重要的一点。目前我们ListView的运行效率是很低的，因为在FruitAdapter的getView()方法中，每次都将布局重新加载了一遍，当ListView快速滚动的时候，这就会成为性能的瓶颈。

仔细观察你会发现，getView()方法中还有一个convertView参数，这个参数用于将之前加载好的布局进行缓存，以便之后进行重用，我们可以借助这个参数来进行性能优化。修改FruitAdapter中的代码，如下所示：

```kotlin
package work.icu007.listviewtest

import android.app.Activity
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ArrayAdapter
import android.widget.ImageView
import android.widget.TextView


/*
 * Author: Charlie_Liao
 * Time: 2023/10/31-14:36
 * E-mail: rookie_l@icu007.work
 */

class FruitAdapter(activity: Activity, private val resourceId: Int, data: List<Fruit>) :
    ArrayAdapter<Fruit>(activity, resourceId, data) {

    override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {
        // 将 resourceId 指向的视图 一一填充到父布局当中，并存为view变量
        var view = LayoutInflater.from(context).inflate(resourceId, parent, false)
        view = convertView ?: LayoutInflater.from(context).inflate(resourceId, parent, false)
        val fruitImage: ImageView = view.findViewById(R.id.fruitImage)
        val fruitName: TextView = view.findViewById(R.id.fruitName)
        // 根据position 获取当前的Fruit实例
        val fruitItem = getItem(position)
        if(fruitItem != null){
            // 若实例不为空，则为fruitImage 和fruitName 设置成该实例中的ImageID及name
            fruitImage.setImageResource(fruitItem.imageID)
            fruitName.text = fruitItem.name
        }
        return view
    }

}
```

可以看到，现在我们在getView()方法中进行了判断：如果convertView为null，则使用LayoutInflater去加载布局；如果不为null，则直接对convertView进行重用。这样就大大提高了ListView的运行效率，在快速滚动的时候可以表现出更好的性能。

不过，目前我们的这份代码还是可以继续优化的，虽然现在已经不会再重复去加载布局，但是每次在getView()方法中仍然会调用View的findViewById()方法来获取一次控件的实例。我们可以借助一个ViewHolder来对这部分性能进行优化，修改FruitAdapter中的代码，如下所示：

```kotlin
package work.icu007.listviewtest

import android.app.Activity
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ArrayAdapter
import android.widget.ImageView
import android.widget.TextView



/*
 * Author: Charlie_Liao
 * Time: 2023/10/31-14:36
 * E-mail: rookie_l@icu007.work
 */

class FruitAdapter(activity: Activity, private val resourceId: Int, data: List<Fruit>) :
    ArrayAdapter<Fruit>(activity, resourceId, data) {

    // 内部类用于缓存 ImageView和TextView的控件实例
    inner class ViewHolder(val fruitImage: ImageView, val fruitName: TextView)

    override fun getView(position: Int, convertView: View?, parent: ViewGroup): View {
        // 将 resourceId 指向的视图 一一填充到父布局当中，并存为view变量
        var view: View
        var viewHolder: ViewHolder
        if (convertView == null){
            view = LayoutInflater.from(context).inflate(resourceId, parent, false)
            val fruitImage: ImageView = view.findViewById(R.id.fruitImage)
            val fruitName: TextView = view.findViewById(R.id.fruitName)
            viewHolder = ViewHolder(fruitImage, fruitName)
            view.tag = viewHolder
        } else {
            view = convertView
            viewHolder = view.tag as ViewHolder
        }
        // 根据position 获取当前的Fruit实例
        val fruitItem = getItem(position)
        if(fruitItem != null){
            // 若实例不为空，则为fruitImage 和fruitName 设置成该实例中的ImageID及name
            viewHolder.fruitImage.setImageResource(fruitItem.imageID)
            viewHolder.fruitName.text = fruitItem.name
        }
        return view
    }

}
```

我们新增了一个内部类ViewHolder，用于对ImageView和TextView的控件实例进行缓存，Kotlin中使用inner class关键字来定义内部类。当convertView为null的时候，创建一个ViewHolder对象，并将控件的实例存放在ViewHolder里，然后调用View的setTag()方法，将ViewHolder对象存储在View中。当convertView不为null的时候，则调用View的getTag()方法，把ViewHolder重新取出。这样所有控件的实例都缓存在了ViewHolder里，就没有必要每次都通过findViewById()方法来获取控件实例了。

#### 3.3.4 ListView的点击事件

话说回来，ListView 的滚动毕竟只是满足了我们视觉上的效果，可是如果ListView中的子项不能点击的话，这个控件就没有什么实际的用途了。因此，接下来就来学习一下ListView如何才能响应用户的点击事件。

修改 MainActivity中的代码，如下所示：

```kotlin
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        /*val adapter = ArrayAdapter<String>(this,android.R.layout.simple_list_item_1,data)
        binding.listView.adapter = adapter*/
        initFruits()
        val adapter = FruitAdapter(this, R.layout.fruit_item, fruitList)
        binding.listView.adapter = adapter
        binding.listView.setOnItemClickListener { parent, view, position, id ->
            val fruit = fruitList[position]
            Toast.makeText(this, fruit.name, Toast.LENGTH_SHORT).show()
        }
    }
```

可以看到，我们使用setOnItemClickListener()方法为ListView注册了一个监听器，当用户点击了ListView中的任何一个子项时，就会回调到Lambda表达式中。这里我们可以通过position参数判断用户点击的是哪一个子项，然后获取到相应的水果，并通过Toast将水果的名字显示出来。

上述代码的Lambda表达式在参数列表中声明了4个参数，那么我们如何知道需要声明哪几个参数呢？，按住Ctrl键（Mac系统是command键）点击setOnItemClickListener()方法查看它的源码，你会发现setOnItemClickListener()方法接收一个OnItemClickListener参数，这当然就是一个Java单抽象方法接口了，要不然这里我们也无法使用函数式API的写法。OnItemClickListener接口的定义如图：

![1698740235331.png](https://pic.ziyuan.wang/2023/10/31/xiheya_217b830e91550.png)

可以看到，它的唯一待实现方法onItemClick()中接收4个参数，这些就是我们要在Lambda表达式的参数列表中声明的参数了。

另外你会发现，虽然这里我们必须在Lambda表达式中声明4个参数，但实际上却只用到了position这一个参数而已。针对这种情况，Kotlin允许我们将没有用到的参数使用下划线来替代，因此下面这种写法也是合法且更加推荐的：

![1698739922027.png](https://pic.ziyuan.wang/2023/10/31/xiheya_3926c95898e78.png)

注意，即使将没有用到的参数使用下划线来代替，它们之间的位置是不能改变的，position参数仍然得在第三个参数的位置。

### 3.4 更强大的滚动控件：RecyclerView

ListView由于强大的功能，在过去的Android开发当中可以说是贡献卓越，直到今天仍然还有不计其数的程序在使用ListView。不过ListView并不是完美无缺的，比如如果不使用一些技巧来提升它的运行效率，那么ListView的性能就会非常差。还有，ListView的扩展性也不够好，它只能实现数据纵向滚动的效果，如果我们想实现横向滚动的话，ListView是做不到的。

为此，Android提供了一个更强大的滚动控件——RecyclerView。它可以说是一个增强版的ListView，不仅可以轻松实现和ListView同样的效果，还优化了ListView存在的各种不足之处。目前Android官方更加推荐使用RecyclerView，未来也会有更多的程序逐渐从ListView转向RecyclerView，那么本节我们就来详细讲解一下RecyclerView的用法。

#### 3.4.1 RecyclerView的基本用法

和之前我们所学的所有控件不同，RecyclerView属于新增控件，那么怎样才能让新增的控件在所有Android系统版本上都能使用呢？为此，Google将RecyclerView控件定义在了AndroidX当中，我们只需要在项目的build.gradle中添加RecyclerView库的依赖，就能保证在所有Android系统版本上都可以使用RecyclerView控件了。

```ini
dependencies {
	implementation fileTree(dir: 'libs', include: ['*.jar'])
	implementation"org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
	implementation 'androidx.appcompat:appcompat:1.0.2'
	implementation 'androidx.core:core-ktx:1.0.2'
	implementation 'androidx.constraintlayout:constraintlayout:1.1.3'
	implementation 'androidx.recyclerview:recyclerview:1.0.0'
	testImplementation 'junit:junit:4.12'
	androidTestImplementation 'androidx.test:runner:1.1.1'
	androidTestImplementation 'androidx.test.espresso:espresso-core:3.1.1'
}
```

上述代码就表示将RecyclerView库引入我们的项目当中，其中除了版本号部分可能会变化，其他部分是固定不变的。那么可能你会好奇，我怎么知道每个库现在最新的版本号是多少呢？这里告诉你一个小窍门，当你不能确定最新的版本号是多少的时候，可以不输入完整库，然后按下tab，Android Studio会主动提醒你，并告诉你最新的版本号是多少然后填充。

![1698805151517.png](https://pic.ziyuan.wang/2023/11/01/xiheya_af1ec520afa3d.png)

接下来修改activity_main.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

在布局中加入RecyclerView控件也是非常简单的，先为RecyclerView指定一个id，然后将宽度和高度都设置为match_parent，这样RecyclerView就占满了整个布局的空间。需要注意的是，由于RecyclerView并不是内置在系统SDK当中的，所以需要把完整的包路径写出来。

这里我们想要使用RecyclerView来实现和ListView相同的效果，因此就需要准备一份同样的水果图片。简单起见，我们就直接从ListViewTest项目中把图片复制过来，另外顺便将Fruit类和fruit_item.xml也复制过来，省得将同样的代码再写一遍。

接下来需要为RecyclerView准备一个适配器，新建FruitAdapter类，让这个适配器继承自RecyclerView.Adapter，并将泛型指定为FruitAdapter.ViewHolder。其中，ViewHolder是我们在FruitAdapter中定义的一个内部类，代码如下所示:

```kotlin
package work.icu007.recyclerviewtest


import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ImageView
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView


/*
 * Author: Charlie_Liao
 * Time: 2023/11/1-15:58
 * E-mail: rookie_l@icu007.work
 */

class FruitAdapter(val fruitList: List<Fruit>) :
    RecyclerView.Adapter<FruitAdapter.ViewHolder>() {

    inner class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val fruitImage: ImageView = view.findViewById(R.id.fruitImage)
        val fruitName: TextView = view.findViewById(R.id.fruitName)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.fruit_item, parent, false)
        return ViewHolder(view)
    }

    override fun getItemCount(): Int {
        return  fruitList.size
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val fruit = fruitList[position]
        holder.fruitImage.setImageResource(fruit.imageId)
        holder.fruitName.text = fruit.name
    }

}
```

这是RecyclerView适配器标准的写法，虽然看上去好像多了好几个方法，但其实它比ListView的适配器要更容易理解。这里我们首先定义了一个内部类ViewHolder，它要继承自RecyclerView.ViewHolder。然后ViewHolder的主构造函数中要传入一个View参数，这个参数通常就是RecyclerView子项的最外层布局，那么我们就可以通过findViewById()方法来获取布局中ImageView和TextView的实例了。FruitAdapter中也有一个主构造函数，它用于把要展示的数据源传进来，我们后续的操作都将在这个数据源的基础上进行。

继续往下看，由于FruitAdapter是继承自RecyclerView.Adapter的，那么就必须重写onCreateViewHolder()、onBindViewHolder()和getItemCount()这3个方法。onCreateViewHolder()方法是用于创建ViewHolder实例的，我们在这个方法中将fruit_item布局加载进来，然后创建一个ViewHolder实例，并把加载出来的布局传入构造函数当中，最后将ViewHolder的实例返回。onBindViewHolder()方法用于对RecyclerView子项的数据进行赋值，会在每个子项被滚动到屏幕内的时候执行，这里我们通过position参数得到当前项的Fruit实例，然后再将数据设置到ViewHolder的ImageView和TextView当中即可。getItemCount()方法就非常简单了，它用于告诉RecyclerView一共有多少子项，直接返回数据源的长度就可以了。

适配器准备好了之后，我们就可以开始使用RecyclerView了，修改MainActivity中的代码，如下所示：

```kotlin
package work.icu007.recyclerviewtest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import androidx.constraintlayout.widget.ConstraintLayout
import androidx.recyclerview.widget.LinearLayoutManager
import work.icu007.recyclerviewtest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    private val fruitList = ArrayList<Fruit>()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        initFruits()

        val layoutManager = LinearLayoutManager(this)
        binding.recyclerView.layoutManager = layoutManager
        val adapter = FruitAdapter(fruitList)
        binding.recyclerView.adapter = adapter
    }

    private fun initFruits() {
        repeat(2){
            fruitList.add(Fruit("Apple", R.drawable.apple_pic))
            fruitList.add(Fruit("Banana", R.drawable.banana_pic))
            fruitList.add(Fruit("Orange", R.drawable.orange_pic))
            fruitList.add(Fruit("Watermelon", R.drawable.watermelon_pic))
            fruitList.add(Fruit("Pear", R.drawable.pear_pic))
            fruitList.add(Fruit("Grape", R.drawable.grape_pic))
            fruitList.add(Fruit("Pineapple", R.drawable.pineapple_pic))
            fruitList.add(Fruit("Strawberry", R.drawable.strawberry_pic))
            fruitList.add(Fruit("Cherry", R.drawable.cherry_pic))
            fruitList.add(Fruit("Mango", R.drawable.mango_pic))
        }
    }
}
```

可以看到，这里使用了一个同样的initFruits()方法，用于初始化所有的水果数据。接着在onCreate()方法中先创建了一个LinearLayoutManager对象，并将它设置到RecyclerView当中。LayoutManager用于指定RecyclerView的布局方式，这里使用的LinearLayoutManager是线性布局的意思，可以实现和ListView类似的效果。接下来我们创建了FruitAdapter的实例，并将水果数据传入FruitAdapter的构造函数中，最后调用RecyclerView的setAdapter()方法来完成适配器设置，这样RecyclerView和数据之间的关联就建立完成了。

使用RecyclerView可以实现和ListView几乎一模一样的效果，虽说在代码量方面并没有明显的减少，但是逻辑变得更加清晰了。当然这只是RecyclerView的基本用法而已，接下来再来学习如何使用RecyclerView实现那些ListView实现不了的效果。

#### 3.4.2 实现横向滚动和瀑布流布局

ListView的扩展性并不好，它只能实现纵向滚动的效果，如果想进行横向滚动的话，ListView就做不到了。但是RecyclerView却能很简单的做到横向滚动。

首先要对fruit_item布局进行修改，因为目前这个布局里面的元素是水平排列的，适用于纵向滚动的场景，而如果我们要实现横向滚动的话，应该把fruit_item里的元素改成垂直排列才比较合理。修改fruit_item.xml中的代码，如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="80dp"
    android:orientation="vertical"
    android:layout_height="wrap_content">

    <ImageView
        android:id="@+id/fruitImage"
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:layout_gravity="center_horizontal"
        android:layout_marginLeft="10dp"/>

    <TextView
        android:id="@+id/fruitName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:layout_marginLeft="10dp"/>

</LinearLayout>
```

先把LinearLayout改成垂直方向排列，并把宽度设为80 dp。再将ImageView和TextView都设置成了在布局中水平居中，并且使用layout_marginTop属性让文字和图片之间保持一定距离。

MainActivity中只需要加入一行代码，调用LinearLayoutManager的setOrientation()方法设置布局的排列方向。默认是纵向排列的，我们传入LinearLayoutManager.HORIZONTAL表示让布局横行排列，这样RecyclerView就可以横向滚动了。

```kotlin
package work.icu007.recyclerviewtest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import androidx.constraintlayout.widget.ConstraintLayout
import androidx.recyclerview.widget.LinearLayoutManager
import work.icu007.recyclerviewtest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    private val fruitList = ArrayList<Fruit>()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        // 初始化水果数据
        initFruits()
        // 将 recyclerView的布局方式设置成线性布局
        val layoutManager = LinearLayoutManager(this)
        layoutManager.orientation = LinearLayoutManager.HORIZONTAL
        binding.recyclerView.layoutManager = layoutManager
        // 创建FruitAdapter实例，将fruitList传入
        val adapter = FruitAdapter(fruitList)
        binding.recyclerView.adapter = adapter
    }

    private fun initFruits() {
        repeat(2){
            fruitList.add(Fruit("Apple", R.drawable.apple_pic))
            fruitList.add(Fruit("Banana", R.drawable.banana_pic))
            fruitList.add(Fruit("Orange", R.drawable.orange_pic))
            fruitList.add(Fruit("Watermelon", R.drawable.watermelon_pic))
            fruitList.add(Fruit("Pear", R.drawable.pear_pic))
            fruitList.add(Fruit("Grape", R.drawable.grape_pic))
            fruitList.add(Fruit("Pineapple", R.drawable.pineapple_pic))
            fruitList.add(Fruit("Strawberry", R.drawable.strawberry_pic))
            fruitList.add(Fruit("Cherry", R.drawable.cherry_pic))
            fruitList.add(Fruit("Mango", R.drawable.mango_pic))
        }
    }
}
```

效果如图：

![1698891531731.png](https://pic.ziyuan.wang/2023/11/02/xiheya_1c6a666edac00.png)

为什么ListView很难或者根本无法实现的效果在RecyclerView上这么轻松就实现了呢？这主要得益于RecyclerView出色的设计。ListView的布局排列是由自身去管理的，而RecyclerView则将这个工作交给了LayoutManager。LayoutManager制定了一套可扩展的布局排列接口，子类只要按照接口的规范来实现，就能定制出各种不同排列方式的布局了。

除了LinearLayoutManager之外，RecyclerView还给我们提供了GridLayoutManager和StaggeredGridLayoutManager这两种内置的布局排列方式。GridLayoutManager可以用于实现网格布局，StaggeredGridLayoutManager可以用于实现瀑布流布局。

瀑布流该怎么实现呢？现在我们尝试使用瀑布流布局来展示我们的数据

首先修改fruit_item.xml代码：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:orientation="vertical"
    android:layout_height="wrap_content"
    android:layout_margin="5dp">

    <ImageView
        android:id="@+id/fruitImage"
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:layout_gravity="center_horizontal"
        android:layout_marginLeft="10dp"/>

    <TextView
        android:id="@+id/fruitName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="left"
        android:layout_marginLeft="10dp"/>

</LinearLayout>
```

将LinearLayout的宽度由80 dp改成了match_parent，因为瀑布流布局的宽度应该是根据布局的列数来自动适配的，而不是一个固定值。其次我们使用了layout_margin属性来让子项之间互留一点间距，这样就不至于所有子项都紧贴在一些。最后还将TextView的对齐属性改成了居左对齐，因为待会我们会将文字的长度变长，如果还是居中显示就会感觉怪怪的。

接着修改MainActivity中的代码，如下所示：

```kotlin
package work.icu007.recyclerviewtest

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import androidx.constraintlayout.widget.ConstraintLayout
import androidx.recyclerview.widget.LinearLayoutManager
import androidx.recyclerview.widget.StaggeredGridLayoutManager
import work.icu007.recyclerviewtest.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding
    private val fruitList = ArrayList<Fruit>()
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        // 初始化水果数据
        initFruits()
        // 将 recyclerView的布局方式设置成想要的布局
        val layoutManager = StaggeredGridLayoutManager(3,StaggeredGridLayoutManager.VERTICAL)
        binding.recyclerView.layoutManager = layoutManager
        // 创建FruitAdapter实例，将fruitList传入
        val adapter = FruitAdapter(fruitList)
        binding.recyclerView.adapter = adapter
    }

    private fun initFruits() {
        repeat(2){
            fruitList.add(Fruit(getRandomLengthString("Apple"), R.drawable.apple_pic))
            fruitList.add(Fruit(getRandomLengthString("Banana"), R.drawable.banana_pic))
            fruitList.add(Fruit( getRandomLengthString("Orange"), R.drawable.orange_pic))
            fruitList.add(Fruit(getRandomLengthString("Watermelon"), R.drawable.watermelon_pic))
            fruitList.add(Fruit(getRandomLengthString("Pear"), R.drawable.pear_pic))
            fruitList.add(Fruit(getRandomLengthString("Grape"), R.drawable.grape_pic))
            fruitList.add(Fruit(getRandomLengthString("Pineapple"), R.drawable.pineapple_pic))
            fruitList.add(Fruit(getRandomLengthString("Strawberry"), R.drawable.strawberry_pic))
            fruitList.add(Fruit(getRandomLengthString("Cherry"), R.drawable.cherry_pic))
            fruitList.add(Fruit(getRandomLengthString("Mango"), R.drawable.mango_pic))
        }
    }

    private fun getRandomLengthString(string: String): String{
        val length = (1..20).random()
        val stringBuilder = StringBuilder()
        repeat(length){
            stringBuilder.append(string)
        }
        return stringBuilder.toString()
    }
}
```

在onCreate()方法中，我们创建了一个StaggeredGridLayoutManager的实例。StaggeredGridLayoutManager的构造函数接收两个参数：第一个参数用于指定布局的列数，传入3表示会把布局分为3列；第二个参数用于指定布局的排列方向，传入StaggeredGridLayoutManager.VERTICAL表示会让布局纵向排列。最后把创建好的实例设置到RecyclerView当中就可以了，就是这么简单！

#### 3.4.3 RecyclerView的点击事件

和ListView一样，RecyclerView也必须能响应点击事件才可以，不然的话就没什么实际用途了。不过不同于ListView的是，RecyclerView并没有提供类似于setOnItemClickListener()这样的注册监听器方法，而是需要我们自己给子项具体的View去注册点击事件。这相比于ListView来说，实现起来要复杂一些。

那么你可能就有疑问了，为什么RecyclerView在各方面的设计都要优于ListView，偏偏在点击事件上却没有处理得非常好呢？其实不是这样的，ListView在点击事件上的处理并不人性化，setOnItemClickListener()方法注册的是子项的点击事件，但如果我想点击的是子项里具体的某一个按钮呢？虽然ListView也能做到，但是实现起来就相对比较麻烦了。为此，RecyclerView干脆直接摒弃了子项点击事件的监听器，让所有的点击事件都由具体的View去注册，就再没有这个困扰了。

下面我们来具体学习一下如何在RecyclerView中注册点击事件，修改FruitAdapter中的代码，如下所示：

```kotlin
package work.icu007.recyclerviewtest


import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.ImageView
import android.widget.TextView
import android.widget.Toast
import androidx.recyclerview.widget.RecyclerView


/*
 * Author: Charlie_Liao
 * Time: 2023/11/1-15:58
 * E-mail: rookie_l@icu007.work
 */

class FruitAdapter(private val fruitList: List<Fruit>) :
    RecyclerView.Adapter<FruitAdapter.ViewHolder>() {

    inner class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val fruitImage: ImageView = view.findViewById(R.id.fruitImage)
        val fruitName: TextView = view.findViewById(R.id.fruitName)
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.fruit_item, parent, false)
        val viewHolder = ViewHolder(view)
        viewHolder.itemView.setOnClickListener { 
            val position = viewHolder.bindingAdapterPosition
            val fruit = fruitList[position]
            Toast.makeText(parent.context, "u clicked view ${fruit.name}", Toast.LENGTH_SHORT).show()
        }
        viewHolder.fruitImage.setOnClickListener{
            val position = viewHolder.bindingAdapterPosition
            val fruit = fruitList[position]
            Toast.makeText(parent.context, "u clicked image ${fruit.name}", Toast.LENGTH_SHORT).show()
        }
        return viewHolder
    }

    override fun getItemCount(): Int {
        return  fruitList.size
    }

    override fun onBindViewHolder(holder: ViewHolder, position: Int) {
        val fruit = fruitList[position]
        holder.fruitImage.setImageResource(fruit.imageId)
        holder.fruitName.text = fruit.name
    }

}
```

这里我们是在onCreateViewHolder()方法中注册点击事件。上述代码分别为最外层布局和ImageView都注册了点击事件，itemView表示的就是最外层布局。RecyclerView的强大之处也在于此，它可以轻松实现子项中任意控件或布局的点击事件。我们在两个点击事件中先获取了用户点击的position，然后通过position拿到相应的Fruit实例，再使用Toast分别弹出两种不同的内容以示区别。

### 3.5 编写界面的最佳实践

#### 3.5.1 制作9-Patch图片

在实战正式开始之前，我们需要先学习一下如何制作9-Patch图片。你之前可能没有听说过这个名词，它是一种被特殊处理过的png图片，能够指定哪些区域可以被拉伸、哪些区域不可以。

在Android Studio中，我们可以将任何png类型的图片制作成9-Patch图片。首先对着message_left.png图片右击→Create 9-Patch file.在这之后，我们可以适当拉伸 9-Patch 图片以达到我们想要的效果。

我们可以在图片的4个边框绘制一个个的小黑点，在上边框和左边框绘制的部分表示当图片需要拉伸时就拉伸黑点标记的区域，在下边框和右边框绘制的部分表示内容允许被放置的区域。使用鼠标在图片的边缘拖动就可以进行绘制了，按住Shift键拖动可以进行擦除。

![1698896587676.png](https://pic.ziyuan.wang/2023/11/02/xiheya_24db80f5c4b4a.png)

#### 3.5.2 编写精美的聊天界面

既然是要编写一个聊天界面，那肯定要有收到的消息和发出的消息。上一小节中我们制作的message_left.9.png可以作为收到消息的背景图，那么毫无疑问我们还需要再制作一张message_right.9.png作为发出消息的背景图。制作过程是完全一样的。

图片都准备好了之后，就可以开始编码了。由于待会我们会用到RecyclerView，因此首先需要在app/build.gradle当中添加依赖库，如下所示：

```ini
dependencies {
    implementation("androidx.core:core-ktx:1.9.0")
    implementation("androidx.appcompat:appcompat:1.6.1")
    implementation("com.google.android.material:material:1.10.0")
    implementation("androidx.constraintlayout:constraintlayout:2.1.4")
    implementation("androidx.recyclerview:recyclerview:1.3.2")
    testImplementation("junit:junit:4.13.2")
    androidTestImplementation("androidx.test.ext:junit:1.1.5")
    androidTestImplementation("androidx.test.espresso:espresso-core:3.5.1")
}
```

接下来编写主界面

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        android:layout_weight="1"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toTopOf="@+id/send"/>

    <EditText
        android:id="@+id/inputText"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        android:hint="Type something here"
        android:maxLines="2"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toStartOf="@id/send"/>
    <Button
        android:id="@+id/send"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Send"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

我们在主界面中放置了一个RecyclerView用于显示聊天的消息内容，又放置了一个EditText用于输入消息，还放置了一个Button用于发送消息。

接下来定义消息的实体类，新建Msg，代码如下

```kotlin
package work.icu007.uibestpractice


/*
 * Author: Charlie_Liao
 * Time: 2023/11/2-14:13
 * E-mail: rookie_l@icu007.work
 */

class Msg(val content: String, val type: Int) {
    companion object{
        const val TYPE_RECEIVED = 0
        const val TYPE_SENT = 1
    }
}
```

Msg类中只有两个字段：content表示消息的内容，type表示消息的类型。其中消息类型有两个值可选：TYPE_RECEIVED表示这是一条收到的消息，TYPE_SENT表示这是一条发出的消息。这里我们将TYPE_RECEIVED和TYPE_SENT定义成了常量，定义常量的关键字是const，注意只有在单例类、companion object或顶层方法中才可以使用const关键字。

接下来开始编写RecyclerView的子项布局，新建msg_left_item.xml，代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        android:background="@drawable/message_left_originall">

        <TextView
            android:id="@+id/leftMsg"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:layout_margin="10dp"
            android:textColor="@color/white"/>
    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

这是接收消息的子项布局。这里我们让收到的消息居左对齐，并使用message_left.9.png作为背景图。
类似地，我们还需要再编写一个发送消息的子项布局，新建msg_right_item.xml，我们让发出的消息居右对齐，并使message_right.9.png作为背景图，基本上和刚才的msg_left_item.xml是差不多的。代码如下所示：

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        android:background="@drawable/message_right_originall">

        <TextView
            android:id="@+id/rightMsg"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:layout_margin="10dp"
            android:textColor="@color/white"/>

    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

接下来需要创建RecyclerView的适配器类，新建类MsgAdapter，代码如下所示：

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

class MsgAdapter(private val msgList: List<Msg>) : RecyclerView.Adapter<RecyclerView.ViewHolder>() {
    inner class LeftViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val leftMsg: TextView = view.findViewById(R.id.leftMsg)
    }
    inner class RightViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val rightMsg: TextView = view.findViewById(R.id.rightMsg)
    }

    // 根据消息来返回对应的type
    override fun getItemViewType(position: Int): Int {
        val msg = msgList[position]
        return  msg.type
    }

    // 根据不同的viewType 来加载不同的布局并创建不同的ViewHolder；
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): RecyclerView.ViewHolder {
        return if (viewType == Msg.TYPE_RECEIVED){
            val view = LayoutInflater.from(parent.context).inflate(R.layout.msg_left_item,parent,false)
            LeftViewHolder(view)
        } else {
            val view = LayoutInflater.from(parent.context).inflate(R.layout.msg_right_item,parent,false)
            RightViewHolder(view)
        }
    }

    // 返回Item数量
    override fun getItemCount(): Int {
        return msgList.size
    }

    // 如果是LeftViewHolder，就将内容显示到左边的消息布局；如果是RightViewHolder，就将内容显示到右边的消息布局。
    override fun onBindViewHolder(holder: RecyclerView.ViewHolder, position: Int) {
        val msg = msgList[position]
        when(holder){
            is LeftViewHolder -> holder.leftMsg.text = msg.content
            is RightViewHolder -> holder.rightMsg.text = msg.content
            else -> throw IllegalArgumentException()
        }
    }
}
```

上述代码中根据不同的viewType创建不同的界面。首先定义了LeftViewHolder和RightViewHolder这两个ViewHolder，分别用于缓存msg_left_item.xml和msg_right_item.xml布局中的控件。然后要重写getItemViewType()方法，并在这个方法中返回当前position对应的消息类型。

最后修改MainActivity中的代码，为RecyclerView初始化一些数据，并给发送按钮加入事件响应，代码如下所示：

```kotlin
package work.icu007.uibestpractice

import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.view.View
import androidx.recyclerview.widget.LinearLayoutManager
import work.icu007.uibestpractice.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity(), View.OnClickListener {
    private val msgList = ArrayList<Msg>()
    private var adapter:MsgAdapter? = null
    lateinit var binding: ActivityMainBinding
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)
        // 初始化Msg
        initMsg()
        // 将recyclerView的布局设置成线性布局
        val layoutManager = LinearLayoutManager(this)
        binding.recyclerView.layoutManager = layoutManager
        //
        adapter = MsgAdapter(msgList)
        //
        binding.recyclerView.adapter = adapter
        binding.send.setOnClickListener(this)
    }

    private fun initMsg() {
        val msg1 = Msg("Hello guy.", Msg.TYPE_RECEIVED)
        msgList.add(msg1)
        val msg2 = Msg("Hello. Who is that?", Msg.TYPE_SENT)
        msgList.add(msg2)
        val msg3 = Msg("This is Tom. Nice talking to you. ", Msg.TYPE_RECEIVED)
        msgList.add(msg3)
    }

    // 监听点击事件
    override fun onClick(v: View?) {
        when (v) {
            binding.send -> {
                val content = binding.inputText.text.toString()

                if (content.isNotEmpty()){
                    val msg = Msg(content, Msg.TYPE_SENT)
                    // 将 msg 添加到msgList中
                    msgList.add(msg)
                    // 通知列表有新的数据插入
                    adapter?.notifyItemInserted(msgList.size - 1)
                    // 将显示的数据定位到最后一行
                    binding.recyclerView.scrollToPosition(msgList.size - 1)
                    // 清空输入框内容
                    binding.inputText.setText("")
                }
            }
        }
    }
}
```

我们先在initMsg()方法中初始化了几条数据用于在RecyclerView中显示，接下来按照标准的方式构建RecyclerView，给它指定一个LayoutManager和一个适配器。

然后在发送按钮的点击事件里获取了EditText中的内容，如果内容不为空字符串，则创建一个新的Msg对象并添加到msgList列表中去。之后又调用了适配器的notifyItemInserted()方法，用于通知列表有新的数据插入，这样新增的一条消息才能够在RecyclerView中显示出来。或者你也可以调用适配器的notifyDataSetChanged()方法，它会将RecyclerView中所有可见的元素全部刷新，这样不管是新增、删除、还是修改元素，界面上都会显示最新的数据，但缺点是效率会相对差一些。接着调用RecyclerView的scrollToPosition()方法将显示的数据定位到最后一行，以保证一定可以看得到最后发出的一条消息。最后调用EditText的setText()方法将输入的内容清空。

