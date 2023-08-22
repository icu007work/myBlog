---
title: 预装APP安装过程
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva1.sinaimg.cn/large/87c01ec7gy1fsnqqxw2orj21kw0w01a9.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/65dc3e87aafec.jpg
tags:
  - 随笔
password: urovo
abstract: 这是机密文件！
abbrlink: 3e55a450
date: 2022-08-26 11:35:26
authorAbout:
authorDesc:
keywords:
description:
---

[TOC]

## 一、将app上传到项目目录

以更新SQ47的ScannerTool为例

进入目录，若有安装目录则直接进入，若没有则新建目录。

```bash
cd SQ47/vendor/urovo/prebuilt/apps/ScannerTool/
```

## 二、上传app

可使用Xftp，ftp文件传输工具上传，也可以使用磁盘映射直接拷贝。

## 三、修改配置文件

```bash
cd SQ47/vendor/urovo/prebuilt/XX
vim PREBUILT_SQ47_CN_XX_XX.csv
```

然后找到ScannerTool的包名那一行，更新版本号即可。如图所示：

![ScannerTool](https://m.360buyimg.com/babel/jfs/t1/68572/19/21316/127652/62f9af0fE1ace48d7/9be96eb761f1f84f.png)

==其他软件更新或预装步骤同上==
