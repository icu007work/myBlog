---
title: Java基础语法04
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 技术
comments: true
photos: 'https://tva3.sinaimg.cn/large/87c01ec7gy1fsnqqxqwj6j21kw0w01a0.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/00c70a2ab8b2b.jpg
ai: 
  - 包机制是Java语言用于组织类的一种机制，可以通过包名来区分类的命名空间。在Java中，包语句的语法格式为package pkg1[.pkg2[.pkg3...]];，一般以公司域名倒置的形式作为包名。为了使用某个包的成员，需要在Java程序中明确导入该包，可以使用import package1[.package2...].(classname|*);语句实现。Java提供的Javadoc工具可以根据注释生成文档，可以使用命令javadoc -encoding UTF-8 -charset UTF-8 ***.java生成文档。
tags:
  - Java
  - Java基础语法
abbrlink: ad476873
date: 2022-08-25 22:37:45
authorAbout:
authorDesc:
keywords:
description:
---

## 包机制

- 为了更好地组织类，Java提供了包机制，用于区别类名的命名空间
- 包语句的语法格式为

```java
package pkg1[.pkg2[.pkg3...]];
```

- *** 一般利用公司域名倒置作为包名***
- 为了能够使用某一个包的成员，我们需要在Java程序中明确导入该包。使用import语句可完成此项功能

```java
import package1[.package2...].(classname|*);
```

## JavaDoc生成文档

Win + r 后输入：

```shell
javadoc -encoding UTF-8 -charset UTF-8 ***.java
```



