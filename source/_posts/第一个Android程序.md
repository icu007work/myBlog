---
title: 第一个Android程序
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva1.sinaimg.cn/large/87c01ec7gy1fsnqr4nyw5j21kw0w0e07.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/8b489fc89372f.jpg
tags:
  - Android
  - 随笔
abbrlink: c521edc1
date: 2022-08-26 11:40:36
authorAbout:
authorDesc:
keywords:
description:
---

## 创建第一个Android项目

### 创建HelloWorld项目

先在Android上面new一个Project，输入应用名称、公司域名然后选择项目代码存放位置。然后再选择活动界面，给创建的活动和布局命名。然后等待项目创建成功。

### 启动设备（模拟器或者实体设备）

模拟器安装步骤：

1. 下载SDK：目前官网上已经没有单独的SDK下载安装包了。谷歌官网推荐的是下载包含有Android SDK的Android Studio。[官网下载](https://developer.android.com/studio/)地址

2. <font color = "red"><strong> 安装路径需记住 </strong></font> ：本人安装路径为：E:\Program Files\Android\SDK_Tools

![目录](https://m.360buyimg.com/babel/jfs/t1/87812/3/29255/39841/62b9474fE6fd09a54/b52fbb814d641d1f.png)

3. 双击打开AVD Manager.exe（选择下列文件安装）选择完成之后点install

![下载](https://m.360buyimg.com/babel/jfs/t1/36673/34/16367/135571/62b947d0E35bf8fde/e82391d638b98122.png)

4. 勾选左下角  Accept License，开始安装

![Accept](https://m.360buyimg.com/babel/jfs/t1/216102/34/19510/57309/62b94867E901dfbef/32f93d3c8c72f9c3.png)

5. 耐心等待其下载完成

![下载](https://m.360buyimg.com/babel/jfs/t1/213735/7/20495/57747/62b948a8Ef5d95be8/458f6cb626d76972.png)

6. 配置SDK环境

“右键 我的电脑”—“属性”—“高级系统设置”—“环境变量”—“系统变量”—“新建”

ANDROID_SDK_HOME其值为E:\Program Files\Android\SDK_Tools

![环境变量](https://m.360buyimg.com/babel/jfs/t1/14597/4/16956/49620/62b97c9eE81609139/5ae3ef33e9c23834.png)

编辑Path变量，新增如下两项，<font style="background:green" font color = white><strong> %ANDROID_SDK_HOME%\platform-tools </strong></font> 、<font style="background:green" font color = white><strong> %ANDROID_SDK_HOME%\tools </strong></font> 

![Path](https://m.360buyimg.com/babel/jfs/t1/119511/30/23462/35517/62b97cf2E1900381f/d8be8bb741150705.png)

### 运行第一个项目

![运行](https://m.360buyimg.com/babel/jfs/t1/8552/30/18256/147697/62b954f9Eb9ea1f86/c853c46249cc52c1.png)

![第一个Android项目](https://m.360buyimg.com/babel/jfs/t1/177838/11/25850/13873/62b91a90Ead9d07eb/fe87b0ca60bff876.png)

### 分析第一个Android程序

#### Project目录

![Project目录](https://m.360buyimg.com/babel/jfs/t1/198246/23/25013/22163/62b95440Ea92a2960/08d623ec793aa4a3.png)


1. <font color = "red"><strong> .gradle和.idea </strong></font> ：Android Studio自动生成的目录，无须关心。

1. <font color = "red"><strong> app </strong></font> ：项目中的代码、资源等内容 几乎都是放置在这个目录当中的，后续开发工作也是在这个目录当中进行的。

1. <font color = "red"><strong> build </strong></font> ：包含了一些在编译时自动生成的文件。

1. <font color = "red"><strong> gradle </strong></font> ：这个目录下包含gradle wrapper的配置文件，使用gradle wrapper的方式无须提前下载gradle，Android Studio会根据本地缓存情况来决定是否需要下载gradle。Android Studio默认没有开启gradle wrapper，如需打开，可点击导航栏 ---> File ---> Settings ---> Build， Execution, Deployment ---> Gradle,进行配置更改.

1. <font color = "red"><strong> .gitgnore </strong></font> ：用来指定目录或文件排除在版本控制之外。

1. <font color = "red"><strong> build.gradle </strong></font> ：这是项目的全局gradle构建脚本，通常情况下这个文件内容无须更改。

1. <font color = "red"><strong> gradle.properties </strong></font> ：这是项目的全局gradle配置文件，在这里配置的属性会影响到项目中所有的gradle编译脚本。

1. <font color = "red"><strong> gradlew和gradlew.bat </strong></font> ：这两个文件是用来在命令行界面执行gradle命令的，其中gradlew是在Linux和Mac系统中使用的，gradlew.bat是在Windows系统中使用的。

1. <font color = "red"><strong> ProjectName.iml </strong></font> ：iml文件是所有Intellij IDEA项目都会自动生成的一个文件（Android Studio基于Intellij IDEA），用于标识这是一个Intellij IDEA项目，我们无需修改。

1. <font color = "red"><strong> local.properties </strong></font> ：用于指定本机中的Android SDK路径，一般情况下无需修改，除非本机的SDK位置发生变化。

1. <font color = "red"><strong> settings.gradle </strong></font> ：这个文件用于指定项目中所有引入的模块。通常情况下模块的引入是自动完成的，需要手动修改的情况较少。

<font style="background:blue" font color = white> 我们不难发现，除app目录外其他目录均为自动生成的目录，所以app目录才是我们以后工作的重点 </font> 

****

#### app目录

![app目录](https://m.360buyimg.com/babel/jfs/t1/126318/30/23870/10698/62b9574eE8636e973/ca6126a62c59951b.png)

1. <font color = "red"><strong> build </strong></font> ：与外层build目录相似，主要也是包含了一些在编译时自动生成的文件，不过它里面的内容更为繁杂
1. <font color = "red"><strong> libs </strong></font> ：如果项目中使用了第三方jar包，就需要把这些jar包都放在libs目录下。放在这个目录的jar包将会被自动添加到构建路径当中。
1. <font color = "red"><strong> androidTest </strong></font> ：此处用来编写AndroidTest测试用例，可以对项目进行一个自动化测试。
1. <font color = "red"><strong> java </strong></font> ：java目录是放置所有java代码的地方，展开该目录就可以看到已经创建的目录。
1. <font color = "red"><strong> res  </strong></font> ：项目中所使用到的图片、布局、字符串等资源都要存放在这个目录中。此目录还存在很多子目录：图片存放在drawable目录下，布局放在layout下，字符串放在values目录下。
1. <font color = "red"><strong> AndroidManifest.xml </strong></font> ：这是整个Android项目的配置文件，在程序中定义的所有四大组件都需要在这个文件里注册，另外还可以在这个文件中给应用程序添加权限说明。
1. <font color = "red"><strong> test </strong></font> ：此处用来编写Unit Test测试用例，是对项目项目进行自动化测试的另一种方式。
1. **.gitgnore** ：这个文件用于将app模块内的指定目录或文件排除在版本控制之外，作用和外层的.gitgnore文件类似。
1. **app.iml** ：Intellij IDEA项目自动生成的文件，无需关心。
1. **build.gradle** ：是app模块的gradle构建脚本，这个文件中会指定很多项目构建相关的配置。
1. **proguard-rules.pro** ：这个文件用于指定代码的混淆规则，当代码开发完成后打成安装包文件，如果不希望被别人破解，通常会将代码进行混淆，从而让破解者难以阅读。
