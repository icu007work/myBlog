---
title: 'Win FR:一款数据恢复工具'
author: Rookie_l
categories: 技术
abbrlink: 2940757f
cover: 'https://pic.ziyuan.wang/2023/08/23/859265305f8ee.jpg'
ai:
  - 这篇文章介绍了微软的数据恢复工具 Windows File Recovery 以及基于它的图形化工具WinFR，对于需要恢复已删除文件的用户来说，这些工具都提供了实用的功能。
tags: 软件分享
date: 2023-05-23 15:31:47
keywords:
description:
---

## 一、官方工具

> 商店页面：https://www.microsoft.com/store/productId/9N26S50LN705

- 通常文件被删除后，数据还会残留在硬盘中一段时间，用数据恢复工具就有概率能够恢复文件。而微软就出品了一款数据恢复相关的工具 Windows File Recovery，相当不错！

![微软数据恢复](http://p1.meituan.net/csc/2118a5e67b680fee26cee58963cf7f3a127433.png)

- 微软的这款 Windows File Recovery 已经上架了 Window 商店了，它支持多种文件系统（比如NTFS、exFAT、FAT、ReFS），可对多种文件格式（比如照片、视频、文档等等）进行恢复。无论想要恢复 SSD、HDD还是U盘、SD卡的各种文件，Windows File Recovery 都可以发挥作用。

- Windows File Recovery 提供三种恢复模式，分别是 Windows File Recovery 普通、分段（segment mode）、签名（signature mode），各模式使用场景如下：

  - 1） 普通模式：用于最近删除过的文件恢复，支持文件格式为NTFS；

  - 2） 分段模式（segment mode）：用于恢复已删除一段时间的文件，或者对已经格式化过的磁盘执行恢复操作；

  - 3） 签名模式（signature mode）：针对FAT、exFAT、ReFS等文件系统恢复，此外如果其他恢复模式不顺利，也可用这个模式试一试。

- 需要注意的是，Windows File Recovery 没有图形界面，需要通过命令行使用，但并不难。它的命令语法是：

> winfr [被删文件所在盘符] [恢复文件对应盘符] [/开关] 文件详细路径

- 例如，我们要将E:\test\下一个名为“XX.txt”的文件找回来，具体命令就是：`winfr e: d: /n \test\XX.txt`。

- 而通过各种命令，Windows File Recovery 还可以实现其他功能，例如支持通配符、恢复整个文件夹等等。关于各种命令，微软专门提供了一个命令说明网页，以便新手熟悉。网址如下：

> https://support.microsoft.com/zh-cn/help/4538642/windows-10-restore-lost-files

- 感兴趣的可以看看，非常全面，可以翻译为中文。

## 二、图形化工具

- 简单来说，Windows File Recovery 的功能和效果都相当不错。如果你实在不想用命令行，那么也可以使用基于 Windows File Recovery 打造的软件，例如傲梅出品的 WinFR 界面版，恢复数据更加简单！

![WinFR界面版](http://p0.meituan.net/csc/e3d7aef8a3dc47ce6fbd039bf339f281161908.png)

> 官网：[WinFR官网 – 免费的数据恢复软件](https://www.winfr.com.cn/)
