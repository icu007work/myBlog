---
title: 常见dos命令
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
cover: https://pic.ziyuan.wang/2023/08/22/33153505730e6.jpg
photos: 'https://tva4.sinaimg.cn/large/87c01ec7gy1fsnqqsevz4j21kw0w07ka.jpg'
tags:
  - 随笔
  - Dos
abbrlink: 794bccc2
date: 2022-08-25 22:29:04
authorAbout:
authorDesc:
keywords:
description:
---

## 基本Dos命令及计算机发展史

### 一、打开cmd窗口方式

1. 开始 + 系统+命令提示符
2. **Win键+R 输入cmd打开控制台**
3. 在任意的文件夹下面，按住Shift键 + 鼠标右击，在此窗口打开命令行窗口
4. 资源管理器的地址栏前面加上cmd路径
5. **管理员身份运行，选择以管理员身份运行**

### 二、常用的Dos命令

```shell
#盘符切换
D:
C:
E:

#进入任意目录 cd change directory
#跨盘：
cd /d E:\data\usr\root
#不跨盘
cd data
#返回上一级目录
cd ..

#查看当前目录下所有文件
dir 
ll
ls

#清理屏幕 cls clear screen
cls

#退出终端 exit
eixt

#查看IP ipconfig
ipconfig

#打开应用 calc 计算器；mspaint 画图软件； notepad 记事本
calc 
mspaint
notepad

#文件操作
#创建目录 md make directory
md test
#创建文件
cd>test.txt
#删除文件
del test.txt
#移除目录 rd remove directory
rd test

```

## 计算机语言发展史

### 一、第一代语言

- 机器语言
  - 计算机的基本计算方式都是基于*二进制*的方式
  - 二进制：01010110100100101
  - 这种代码是直接输入给计算机使用的，不经过任何转换

### 二、第二代语言

- 汇编语言
  - 解决人类无法读懂机器语言的问题
  - 指令代替二进制
- 目前应用
  - 逆向工程
  - 机器人
  - 病毒
  - ...

### 三、第三代语言

- 摩尔定律
  - 当价格不变时，集成电路上可容纳的集体管数目，约每隔18个月便会增加一倍，性能也会提升一倍。换言之，每一美元所能买到的电脑性能，将每隔18个月翻两倍以上
- 高级语言
  - 大体上分为：**面向过程**和**面向对象**两大类
    - ***c语言***是经典的面向过程的语言，**c++和Java**是典型的面向对象的语言
  - 
