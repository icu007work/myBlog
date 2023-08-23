---
title: xray面板安装
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva4.sinaimg.cn/large/87c01ec7gy1fsnqqrzat4j21kw0w0wtr.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/ea0c43a2f8ddc.jpg
ai: 
  - 本文介绍了Xray面板的安装步骤
tags: 随笔
abbrlink: '25e76629'
date: 2022-08-26 11:37:14
authorAbout:
authorDesc:
keywords:
description:
---



## 一、升级yum，安装curl依赖包

```bash
yum update -y && yum install curl -y
```

其中：

`yum update -y`为更新yum

`yum install curl -y`为安装curl依赖包。

## 二、重启系统

```bash
reboot
```

## 三、安装Xray面板

```bash
bash <(curl -Ls https://raw.githubusercontent.com/vaxilu/x-ui/master/install.sh)
```

## 四、centos安装宝塔面板

```shell
yum install -y wget && wget -O install.sh http://download.bt.cn/install/install_6.0.sh && sh install.sh ed8484bec
```

