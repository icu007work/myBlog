---
title: WP备份
author: Rookie_l
avatar: 'https://icu007.work/wp-content/uploads/2022/08/head.jpeg'
authorLink: 'https://hiheya.github.io/'
categories: 随想
comments: true
photos: 'https://tva2.sinaimg.cn/large/87c01ec7gy1fsnqqw744pj21kw0w0dwt.jpg'
cover: https://pic.ziyuan.wang/2023/08/22/edd353d123f0a.jpg
ai: 本文主要是记录WP相关文件的备份。
tags: WP
abbrlink: b6be31da
date: 2022-08-25 22:43:48
authorAbout:
authorDesc:
keywords:
description:
---

## argon主题选项

### 文本附加内容

```markdown
您当前正在 - %link% .页面，阅读由“%author%” 撰写的《%title%》
非常感谢您对我们的网站感兴趣并访问。在您使用本网站之前，请您仔细阅读本声明的所有条款。

版权声明：
1、本博客属个人所有，不涉及商业目的。
2、本博客内容均为本人编写，图片版权属于原作者，图片仅供大家欣赏和分享，切勿做为商业目的使用。如果侵害了您的合法权益，请您及时与我们，我们会在第一时间删除相关内容！
3、本博客所有原创作品，包括文字、资料、图片、网页格式，转载时请标注作者与来源。非经允许，不得用于赢利目的。
4、本博客受中国知识产权、互联网法规和知识共享条例保护和保障，任何人不得进行旨在破坏或牟取私利的行为。
5、做新时代合格网民，弘扬互联网精神：开放、平等、 协作 、分享；共同构建文明、清朗的网络环境
6、本声明未涉及的问题参见国家有关法律法规，当本声明与国家法律法规冲突时，以国家法律法规为准。
7、当您阅读到这里的时候，即表明已阅读并接受了上述各项条款;

```

---

### 页脚内容

```markdown
<p><b>Copyright  2022 Xiheya</b>, All Rights Reserved.</p> 
<p><b>本站已<del>稳定运行</b></del>:<span id="blog_running_days" class="odometer"></span> days , <span id="blog_running_hours" class="odometer"></span> h , <span id="blog_running_mins" class="odometer"></span> m , <span id="blog_running_secs" class="odometer"></span> s</p> 
<script no-pjax>
var blog_running_days=document.getElementById("blog_running_days");
var blog_running_hours=document.getElementById("blog_running_hours");
var blog_running_mins=document.getElementById("blog_running_mins");
var blog_running_secs=document.getElementById("blog_running_secs");
function refresh_blog_running_time(){
    var time = new Date() - new Date(2021, 10, 16, 5, 20, 0);
    var d=parseInt(time/24/60/60/1000);
    var h=parseInt(time%(24*60*60*1000)/60/60/1000);
    var m=parseInt(time%(60*60*1000)/60/1000);
    var s=parseInt(time%(60*1000)/1000);
    blog_running_days.innerHTML=d;
    blog_running_hours.innerHTML=h;
    blog_running_mins.innerHTML=m;
    blog_running_secs.innerHTML=s;
}
refresh_blog_running_time();
if (typeof(bottomTimeIntervalHasSet) == "undefined"){
    var bottomTimeIntervalHasSet = true;
    setInterval(function(){refresh_blog_running_time();},500);
}
</script>
```

---

### 主题颜色

```markdown
#9a92b9

banner副标题：记录我的日常生活&学习笔记
页面背景：https://img30.360buyimg.com/pop/jfs/t1/128389/8/25215/1839159/622820daEea902288/cdc24c6bd0f9c593.jpg
```

---

### 左侧栏

```markdown
左侧栏标题	
与君共赴，万里征程。

左侧栏子标题（格言）	
--hitokoto--

左侧栏作者名称	
未可知.

左侧栏作者头像地址	
https://img30.360buyimg.com/pop/jfs/t1/131661/35/25896/114014/62246b27Ec3b050cb/373a661d7463f92a.png

```

---

## 留言板与友链

### 留言板内容：

```markdown
留言规则

留言者应遵守国家相关法律法规，不得发表违反中华人民共和国宪法、法律和有关政策的言论;
留言者承担因留言行为而直接或间接引起的法律责任;
本博客拥有发布、编辑、删除公众留言的权利，凡不符合本须知规定的留言将予以删除;
如果你有任何问题或是要求，可以在这里给我发送消息;
如在本博客目留言，即表明已阅读并接受了上述各项条款;
```

---

### 友链内容

```
[friendlinks style="1-square" sort="rand"/]  
//方形头像，随机排序
```

友链格式 :

> 博客名称：Rookie_L’s Blog
>
> 描述：一个小菜鸡自建的blog，主要用于记录自己的生活日常&学习笔记
>
> 站点：https://solstice23.top
>
> Avatar (头像)：https://solstice23.top/friendlink_image/avatar/

---

## 菜单栏

### 顶部导航标签

```markdown
<i class="fa fa-home" aria-hidden="true"></i> 首页
<i class="fa fa-comments" aria-hidden="true"></i> 留言板
<i class="fa fa-link" aria-hidden="true"></i> 友情链接
<i class="fa fa-clock-o" aria-hidden="true"></i> 归档

<i class="fa fa-star" aria-hidden="true"></i>分类 <i class="fa fa-caret-down" style="margin-left:3px;"></i>

<i class="fa fa-tags" aria-hidden="true"></i> 标签<i class="fa fa-caret-down" style="margin-left:3px;"></i>
```

---

### 左侧栏菜单导航

```markdown
url：https://docs.oracle.com/javase/8/docs/api/
标签：<i class="fa fa-question-circle" aria-hidden="true"></i> API帮助文档

url：https://leetcode-cn.com/
标签：<i class="fa fa-code" aria-hidden="true"></i> LeetCode

url：https://codetop.cc/home
标签：<i class="fa fa-codepen" aria-hidden="true"></i> CodeTop

url：https://github.com/
标签：<i class="fa fa-github-alt" aria-hidden="true"></i> GayHub

url：https://www.programmercarl.com/
标签：<i class="fa fa-eye" aria-hidden="true"></i> 代码随想录

url：https://www.runoob.com/
标签：<i class="fa fa-child" aria-hidden="true"></i> 笨鸟先飞
```

---

### 左侧个人链接：

```markdown
1. URL：https://cloud.icu007.work/ 标签： <i class="fa fa-cloud" aria-hidden="true"></i> 可道云
2. URL：mailto:rookie_l@icu007.work 标签：<i class="fa fa-envelope" aria-hidden="true"></i> 联系我
3. URL：https://alist.icu007.work/ 标签：<i class="fa fa-hdd-o" aria-hidden="true"></i> 分享盘
4. URL：https://drive.icu007.work/ 标签： <i class="fa fa-download" aria-hidden="true"></i> 下载盘
5. URL：https://baidu.icu007.work 标签： <i class="fa fa-question" aria-hidden="true"></i> 百度一下
6. URL： http://hiheya.github.io 标签： <i class="fa fa-user" aria-hidden="true"></i> 子站
```

---

  
