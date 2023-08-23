---
title: 记一次office无法联网解决方法
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 生活
comments: true
photos: 'https://tva1.sinaimg.cn/large/87c01ec7gy1fsnqq4k0okj21kw0w0dra.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/d2b6cf5f2f950.jpg
ai: 
   - 本文主要记录了博主office无法联网时的处理过程，并附上了解决方法。
tags: 随笔
abbrlink: eacc0609
date: 2022-08-25 22:43:38
authorAbout:
authorDesc:
keywords:
description:
---

## 登录onedrive显示无法连接服务器解决方法

今天更新了一下office三件套，但是更新完之后打开word显示无法连接服务器于是上网搜索后得之可以重置网络。具体操作如下：

1. Cmd + r ：依次输入

```shell
netsh int ip reset c:\resetlog.txt
netsh winsock reset
shutdown -r -t 0
```

2. 命令行解析：
   1. netsh int ip reset c:\resetlog.txt 和 netsh winsock reset为重置网络
   2. shutdown -r -t 0 为0s后重启电脑。
3. 重启电脑后再打开word就可以连上网啦！

