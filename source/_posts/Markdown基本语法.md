---
title: Markdown基本语法
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva3.sinaimg.cn/large/87c01ec7gy1fsnqqmdhn9j21kw0w07iu.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/793798198c74d.jpg
tags: Markdown
abbrlink: 5d36ff15
date: 2022-08-25 22:33:19
authorAbout:
authorDesc:
keywords:
description:
---

## Markdown 基本语法

### 一.标 题:

```shell
 1.1 一级标题 #+空格+文本+回车 

 1.2 二级标题 ##+空格+文本+回车 

 1.3 三级标题 ###+空格+文本+回车 

 .....以此类推
```

---



### 二.字体

```shell
粗体：文本两边加**
eg: **Hello World**
```

**Hello World**

---

```shell
斜体：文本两边加*
eg: *Hello World*
```

   *Hello World*

---

```shell
粗体加斜体：文本两边加***
eg: ***Hello World***
```

***Hello World***

---

```shell
中间横线：文本两边加~~
eg: ~~Hello World~~
```

 ~~Hello World~~

---



### 三.引用

```shell
右箭头后面接上文本>
eg: >与君共赴，万里征程。
```

> 与君共赴，万里征程。

---

### 四.分割线

```shell
三个-（减号）表示分割线
eg: ---
```

---

```shell
三个*（减号）表示分割线
eg: ***
```

***

### 五.图片

```shell
插入图片：!+[图片名字]+(图片路径)
eg:![示例1](https://images.unsplash.com/photo-1646408271568-977e12b6425a?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80)
```

![示例1](https://images.unsplash.com/photo-1646408271568-977e12b6425a?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80)

---



### 六. 超链接

```shell
语法：[标题](链接地址)
eg:[Idea快捷键](http://110.42.139.30:8000/index.php/2022/03/06/13/)
```

[idea快捷键](http://110.42.139.30:8000/index.php/2022/03/06/13)

---

### 七. 列表

#### 有序列表

```shell
语法：1 + . + 空格
```

1. A
2. B
3. C

#### 无序列表

```shell
语法： - + 空格
```

- A
- B
- C

---

### 八.表格

```shell
语法：
| 名字 | 性别 | 生日     |

| ---- | ---- | -------- |

| 张三 | 男   | 2000.1.1 |
```

---

| 名字 | 性别 | 生日     |
| ---- | ---- | -------- |
| 张三 | 男   | 2000.1.1 |

### 九.代码

```shell
语法 ``` + 编程语言名称
eg ```shell
```

``` java
public
```

### 十.快捷键

#### 文本编辑快捷键

- 无序列表：输入-之后输入空格 / ctrl + shift + ] (对选中行可用)
- 有序列表：输入数字 + “.”之后输入空格 / ctrl + shift + [ (对选中行可用)
- 引用内容：> + 空格 / ctrl + shift + q (对选中内容可用)
- 任务列表：-[空格]空格 文字
- 标题：ctrl + 数字
- 表格：ctrl + t
- 目录：[TOC]
- 任务列表：- [ ] 文字（注意 “-” 后与 “[]“ 中间都有空格）
- 选中一整行：ctrl + l (字母L)
- 选中单词：ctrl + d
- 选中相同格式的文字：ctrl + e
- 跳转到文章开头：ctrl + home
- 跳转到文章结尾：ctrl + end
- 搜索：ctrl + f
- 替换：ctrl + h
- 引用：输入>之后输入空格
- 代码块： ctrl + shift + k
- 行内代码：ctrl + shift + ` (对选中行可用)
- 加粗：ctrl + b
- 倾斜：ctrl + i
- 下划线：ctrl + u
- 删除线：alt + shift + 5
- 插入链接：ctrl + k
- 插入公式：ctrl + shift + m
- 插入图片：ctrl + shift + i
- 保存：ctrl + s
- 另存为：ctrl + shift + s

#### 编辑模式快捷键

- 源码模式编辑切换：ctrl + /
- 打字机模式切换：F9
- 专注模式切换：F8
- 全屏模式切换：F11
- Typora内部窗口焦点切换：ctrl + tab
- 侧边栏显示/隐藏切换：ctrl + shift + L
