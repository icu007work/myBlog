---
title: Hexo博客部署踩坑
author: Rookie_l
categories: 技术
keywords: 博客部署过程中踩过的坑
cover: 'https://pic.ziyuan.wang/2023/08/23/71e567b1eb485.jpg'
ai:
  - 该文章解决了在使用Hexo过程中可能遇到的Node.js版本和永久链接问题，提供了详细的解决方案和操作指南。一、Node.js 版本问题;二、permalink问题.
abbrlink: ee2c1e7d
date: 2023-04-13 16:26:26
tags:
description:
---

## 一、Node.js 版本问题
- Node.js 版本这个问题我已经栽了两次，首先来看Hexo官方文档给出的。

```tex
安装前提
安装 Hexo 相当简单，只需要先安装下列应用程序即可：

Node.js (Node.js 版本需不低于 10.13，建议使用 Node.js 12.0 及以上版本)
Git
如果您的电脑中已经安装上述必备程序，那么恭喜您！你可以直接前往 安装 Hexo 步骤。

如果您的电脑中尚未安装所需要的程序，请根据以下安装指示完成安装。
```
[原文链接](https://hexo.io/zh-cn/docs/index.html)
- 原文写的是建议使用Node.js 12.0以上版本，可是每当我使用16.0+的版本时总会出现奇奇怪怪的问题，所以建议大家使用12.0版本。
- 这里推荐一个Node.js 管理工具 `nvm` 全称 `node.js version management` 可以自行网上搜索安装。这里给一个：[GitHub地址](https://github.com/coreybutler/nvm-windows/releases)。
- 安装完之后就可以开始使用了，这里附nvm常用使用命令表
> 常用 nvm 命令表：
> 
>   安装指定的 nodejs 版本：nvm install 12.22.8
>   
> 查看已经安装的 nodejs 列表：nvm ls
>   
> 指定当前使用的 nodejs 版本：nvm use 12.22.8
>   
> 查看当前使用的 nodejs 版本：node -v
- 在我使用 16.0+版本的Node.js 就会出现如下问题:
```tex
TypeError [ERR_INVALID_URL]: Invalid URL: https://ip访问：重定向到https://tdom.ml(跳转的时候浏览器可能提示不安全是正常的)

TypeError: isPlainObject is not a function
```
- 网上的解决办法都是卸载重装

```bash
# 卸载 Hexo
npm uninstall hexo

# 安装 Hexo
npm install hexo


```

- 按照网上解决办法操作了好久还是无法解决，想了好久才想到是Node.js问题。
- 使用 `nvm` 将Node.js 切换到12.22.8 版本，然后在博客根目录执行:

```bash
# 卸载 Hexo
npm uninstall hexo

# 安装 Hexo
npm install hexo

# 重新安装依赖
npm install hexo --save
```

- 问题解决！！

## 二、permalink 问题
- Hexo 默认的 `permalink` 为 `permalink: :year/:month/:day/:title/` 这样就会导致文章标题为中文时生成的永久链接也会有中文，

### 2.1 三种解决方案
#### 2.1.1 安装插件方法一（推荐）
- 在 Hexo 根目录下使用 git bash 执行命令：

```bash
npm install hexo-abbrlink --save
```

- 在 Hexo 根目录下的 _config.yml 文件，修改为如下配置：

```tex
# permalink: :year/:month/:day/:title/
# permalink: article/:title/
permalink: article/:abbrlink.html
abbrlink:
  alg: crc32  # 算法：crc16(default) and crc32
  rep: hex    # 进制：dec(default) and hex
permalink_defaults:
```
- 然后在 git bash 按顺序运行如下命令：

```hexo clean #清除缓存 网页正常情况下可以忽略此条命令
hexo g #生成静态网页
hexo d #开始部署
```

- 再打开网站的文件就可以看到效果。

  - 效果（其中60762就是随机生成的）

    https://[你的网站域名]/article/60762.html

#### 2.1.2 安装插件方法二
- 中文链接转拼音

- 在 Hexo 根目录下使用 git bash 执行命令：

```bash
npm i hexo-permalink-pinyin --save
```

- 在 Hexo 根目录下的 _config.yml 文件中，修改以下的配置项：

``` tex
permalink: article/:title.html
permalink_pinyin:
  enable: true
  separator: '-' # default: '-'
permalink_defaults:
```

- 然后在 git bash 按顺序运行如下命令：

``` bash
hexo clean #清除缓存 网页正常情况下可以忽略此条命令
hexo g #生成静态网页
hexo d #开始部署
```
- 再打开网站的文件就可以看到效果。

  - 效果（标题为“我的个人博客”）

    https://[你的网站域名]/article/wo-de-ge-ren-bo-ke.html

#### 2.1.3 采用urlname
- 在写每篇md文章的时候，在 Front-matter 里加上urlname：

```markdown
---
title: typora-vue-theme主题介绍
date: 2018-09-07 09:25:00
urlname: 2019102101

---
```
- 在 Hexo 根目录下的 _config.yml 文件中，修改以下的配置项：

``` tex
permalink: article/:urlname.html  # urlname值文章里必须填写，格式201905260105
permalink_defaults:
```
- 然后在 git bash 按顺序运行如下命令：

``` bash
hexo clean #清除缓存 网页正常情况下可以忽略此条命令
hexo g #生成静态网页
hexo d #开始部署
```
- 再打开网站的文件就可以看到效果。

- 效果

    https://[你的网站域名]/article/2019102101.html

### 2.2 小结
- 第一种方法是我试过中最好的；第三次之，因为每次都要手动加上urlname；而第二种，当文章的中文标题名字过长时，效果并不好。