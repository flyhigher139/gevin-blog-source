title: 深度剖析WordPress主题结构
category: wordpress
tags:
- wordpress
date: 2011-05-26 00:40:16
---

本文也是Gevin在[帕兰映像](http://paranimage.com/wordpress-theme-depth-analysis-of-the-structure/)写的文章，在这里简单修改并备份一下

利用强大的技术，可以把基于wordpress的网站做成各种各样的形式，这除了要求wordpress主题开发人员精通html，PHP，JS，CSS等技术，还需要开发者掌握WordPress主题的框架。

Gevin今天结合**[The anatomy of a WordPress theme](http://yoast.com/wordpress-theme-anatomy/)**这篇文章，和大家一起剖析WordPress主题的结构。原文作者用图文形式，分别从**网站外观、页面组成和后台文件**三个方面，形象的向大家展示了WordPress的架构，下面Gevin和大家一起分析WordPress是如何架构的。

## <span id="more-1276"></span>网站外观

WordPress主题由一系列模板文件组成，每个模板文件控制主题的一部分。无论在博客的哪个个页面上，主题的框架总有一部分是不变的，这是主题的静态部分，它由header.php, sidebar.php 和 footer.php三个文件控制。我们可以修改这些文件，以便检测我们浏览的页面，并显示不同的内容，如在posts页面和page页面显示不同的导航。然而，通常，我们会让静态部分在整个网站上保持一致的风格。

网站外观由下面4个部分的代码控制：

*   **header.php**

显示博客头和导航，也包含html代码
*   **The Loop**

显示网站主题内容的模板文件称为The Loop（后面会详细介绍）。
*   **sidebar.php**

侧边栏由这个文件控制。多侧边栏的主题可以在functions.php中添加控制。
*   **footer.php**

网站的页尾和html的关闭标签。

## 页面组成

WordPress基本页面有Homepage_（index.php控制）_，Post页面（_单独显示一篇完整博客，由single.php控制）_，独立页面_（page.php控制）_，存档_（archive.php等控制）_，下面分别介绍这几个控制这几个页面的代码文件。

### index.php – home

index文件控制博客homepage的外观。默认情况下，index文件通过一个loop来显示最新博客，homepage底部还会由一个查看以前博客的链接。

### single.php – individual posts

该文件用于显示读者要查看的特定博客全文。

### page.php – individual pages

该文件控制博客中独立页面的外观。

WordPress允许我们为不同的独立页面（pages）设计不同的模板，方法如下：

1、复制page.php并重命名

2、在文件的最上方添加下面代码

<pre class="brush:php">     &lt;?php
/*
Template Name: YourPageNameHere
*/
?&gt;</pre>

### archive.php, category.php, tag.php – archives

我们同样可以自定义存档（archives）的外观。如果没有archive.php文件，存档和主页是一模一样的；然而，我们可以创建一个archive.php文件重构存档页面。如果创建category.php文件，存档页面会被覆盖为只显示目录；如果创建tag.php文件，存档页面会被覆盖为只显示标签。

## The Loop

Loop恐怕是WordPress最强大的部分。它是“循环的查询结果”。循环体中我们可以依次输出选中文章的标题，博客内容，元数据，评论等。我们还可以在single page中使用多个loop。例如，我们可以用一个loop显示博客全文，另一个loop显示相关文章的标题和缩略图。

The Loop结构如下：

*   Query post or page
*   Start Loop //循环开始
*   `the_title` (outputs the title of the post) //标题
*   `the_excerpt` (outputs the post excerpt) //摘要
*   `the_content` (outputs the full post content) //内容
*   `the_category` (outputs the post categories) //目录
*   `the_author` (outputs the post author) //作者
*   `the_date` (outputs the post date) //日期
*   other tags (there is a variety of other tags you can use in the loop) //标签
*   `endwhile; //结束循环`
*   Exit the loop //退出循环

## WordPress的后台文件

为了让主题工作，WordPress还需要一些必要的后台文件。这些文件可以根据个人需求进行修改，它们能够从极大程度上改变网站的外观或提供更强大的功能。

### comments.php

这个文件控制评论的输出，如果您希望在博客上提供评论功能，要把它放到loop中去。Comment.php文件可以被插件覆盖（如Disqus）

### functions.php

Functions.php让我们在WordPress上运行自定义代码，以便更自由的修改主题元素。

### style.css

这是控制主题样式的主要CSS文件。该文件顶部还包含主题的元信息，用于提供主题的名字，作者及相关链接

## 图文剖析

下面是原作者强大的原图

<div style="width: 580px" class="wp-caption aligncenter">[![深度剖析WordPress主题](http://cdn.yoast.com/wp-content/uploads/2011/01/anatomy-wordpress-yoast.png "深度剖析WordPress主题")](http://cdn.yoast.com/wp-content/uploads/2011/01/anatomy-wordpress-yoast.png)

深度剖析WordPress主题
</div>

&nbsp;
<div style="margin-top: 15px">
<p>**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)

**本文链接地址:** [深度剖析WordPress主题结构](http://blog.igevin.info/2011/05/anatomy-wordpress-theme-structure/)

</div>
<div>
极客趣玩推荐：[![](http://blog.igevin.info/wp-content/uploads/2012/05/image.png)](http://s.click.taobao.com/t_8?e=7HZ5x%2BOzdZF6pq3H3z1mawcVRxM%3D&#038;p=mm_17737402_0_0)
</div>
