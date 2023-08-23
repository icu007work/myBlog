---
title: 语雀同款链接卡片—butterfly主题标签外挂
abbrlink: 9765c01a
date: 2023-08-03 15:02:03
author: Rookie_l
avatar: https://icu007.work/wp-content/uploads/2022/08/head.jpeg
cover: https://pic.ziyuan.wang/2023/08/22/8e5180b414a23.jpg
ai: 
  - 这篇文章介绍了在Butterfly主题中添加链接卡片外挂的方法。首先，在themes/butterfly/scripts/tag文件夹下创建link.js文件，并将提供的代码粘贴进去。然后，在themes/butterfly/source/css/_tags文件夹下创建link.styl文件，并将提供的样式代码粘贴进去。使用时可以通过{% link %}标签指定相关参数来生成链接卡片，包括链接、标题、图标和介绍等。如果不填写参数，则会使用默认值。注意，在内容中不能存在英文逗号。最后，你可以根据自身需求进行修改和定制。
authorLink: https://hiheya.github.io/
categories: 技术
tags: 
keywords: 
description: 
password: 
message: 
abstract: 这是一篇加密文章，内容可能是个人情感宣泄或者收费技术。如果你非常好奇，请与我联系。
---
本文参考自：
{% link https://blog.leonus.cn/2022/link-card.html,Leonus,https://blog.leonus.cn/favicon.ico,进一寸有进一寸的欢喜。 %}

样式如下：

{% link https://icu007.work,Charlie_l,https://icu007.work/wp-content/uploads/2022/08/icno.png,与君共赴，万里征程 %}

### link.js

在 `\themes\butterfly\scripts\tag` 文件夹下面新建 `link.js` 并粘贴如下代码：

```js
JS

/**
 * link
 * {% link url,title,favicon,desc %}
 * {% link 链接,标题,图标,介绍 %}
 */

'use strict'
function link(args) {
    args = args.join(' ').split(',');
    // 获取参数
    let url = (args[0] || '').trim(),
        title = (args[1] || '点击直达链接').trim(),
        favicon = (args[2] ? `<img src="${args[2]}" class="no-lightbox">` : defaultIcon).trim(),
        desc = (args[3] || '').trim()

    return `<a href="${url}" ${url.includes('http')?'target="_blank"':''} title="${title}" referrerPolicy="no-referrer" class="link_card"><div class="link_icon">${favicon}</div><div class="link_content"><div class="link_title">${title}</div>${desc?`<div class="link_desc">${desc}</div>`:''}</div></a>`
}

hexo.extend.tag.register('link', link, { ends: false })
```

### link.styl

在 `\themes\butterfly\source\css\_tags` 文件夹下面新建 `link.styl` 并粘贴如下代码：

```css
.link_card
  display: flex
  margin: 10px 0
  color: var(--font-color) !important
  text-decoration: none !important
  background: var(--reward-pop)
  border-radius: 10px
  padding: 12px
  &:hover
    background: #4976f5
    color: white !important
  .link_icon,.link_content
    height: 4rem
  .link_icon
    img,svg
      height: 4rem
      width: 4rem
  .link_content
    margin-left: 1rem
    width: calc(100% - 6rem)
    overflow: hidden
    line-height: 1.5
    display: flex
    flex-direction: column
    justify-content: center
    .link_title
      font-weight: bold
      font-size: 1.2rem
    .link_title,.link_desc
      word-break: break-all
      overflow:hidden
      text-overflow: ellipsis
    &:not(:has(.link_desc)) .link_title
      display:-webkit-box
      -webkit-box-orient:vertical
      -webkit-line-clamp:2
    .link_desc
      opacity: .6
    .link_desc,&:has(.link_desc) .link_title
      white-space: nowrap
```

## 参数

注意：`内容不能有英文逗号`，不然会出bug

```HTML
<!-- 使用html是为了高亮代码，不必在意 -->
<!-- 参数如下： -->
{% link 链接,标题,图标,介绍 %}
<!-- 示例如下： -->
{% link https://blog.leonus.cn/,Leonus,https://blog.leonus.cn/favicon.ico,进一寸有进一寸的欢喜。 %}
<!-- 你也可以什么都不填，将会全部使用默认值，如下： -->
{% card %}
<!-- 你也可以省略部分内容，如下： -->
{% link https://blog.leonus.cn/ %}
<!-- 位置在后面的参数不填的话可以直接省略，但是如果中间的不想填必须留空，如下： -->
{% link https://blog.leonus.cn/,,,进一寸有进一寸的欢喜。 %}
```

| 参数 | 描述                                             | 默认值       |
| ---- | ------------------------------------------------ | ------------ |
| 链接 | 如果连接中包含http则新标签打开，否则本标签页打开 | 无           |
| 标题 | 网站的标题                                       | 点击直达链接 |
| 图标 | 网站favicon`链接`                                |              |
| 介绍 | 网站的description                                | 无           |

## 最后

有什么问题可以留言，也可以根据自身需求进行修改。
