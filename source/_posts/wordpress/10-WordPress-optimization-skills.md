title: 10+ WordPress 优化技巧
category: wordpress
tags:
- wordpress
date: 2011-06-06 23:14:15
---

<div id="attachment_1343" style="width: 533px" class="wp-caption aligncenter">[

![](http://blog.igevin.info/wp-content/uploads/2011/06/seo2.jpg "seo")](http://blog.igevin.info/wp-content/uploads/2011/06/seo2.jpg)

seo
</div>

做独立博客，很重要的一点是搜索引擎优化（SEO）。今天，Gevin结合[《名博是怎样炼成的》](http://book.douban.com/subject/3266445/)一书中的相关内容，以及自己SEO的经验，与大家分享一些基本的SEO技巧。本文结尾，Gevin向大家介绍All in One SEO插件的使用方法、robots.txt文件的写法以及链接结构的写法，进一步加强SEO。

<span id="more-1335"></span>

&nbsp;

<div id="attachment_1339" style="width: 576px" class="wp-caption aligncenter">[![WordPress SEO优化技巧](http://blog.igevin.info/wp-content/uploads/2011/06/WordPress-SEO优化技巧.png.jpg "WordPress SEO优化技巧")](http://blog.igevin.info/wp-content/uploads/2011/06/WordPress-SEO优化技巧.png.jpg)

WordPress SEO优化技巧 点击查看大图
</div>

&nbsp;

&nbsp;

<span style="font-size: 20px; font-weight: bold;">WordPress SEO优化技巧</span>

### 文章URL链接结构优化

*   利用索引的结构

### 文章Post Slug优化

*   包含关键字
*   避免无意义的标题

### 文章Title优化

*   利于SEO的Title

*   <span style="color: #008000;">好：文章名-博客名</span>
*   <span style="color: #ff0000;">差：博客名-文章名</span>

*   All in One SEO

&nbsp;

### robots.txt优化

*   收录指定内容
*   避免收录后台程序、feed地址等

### 阻止垃圾留言/评论

*   Akismet插件

### 保持更新

*   SEO的关键
*   高质量的文章

### 避免变动博客

*   前期做好规划
*   避免变动：

*   <span style="color: #ff0000;">域名</span>
*   <span style="color: #ff0000;">博客名</span>
*   <span style="color: #ff0000;">链接结构</span>
*   <span style="color: #ff0000;">永久链接</span>

### 搜索引擎来源优化

*   Landing sites插件

### Sitemap优化

*   Sitemap Generator
*   baidu-sitemap

### 相关文章

*   博客中使用tag
*   simple tag，无觅等插件

&nbsp;

## All in One SEO设置

All in One SEO插件是最强大的WordPress插件，它的很多设置参数都遵循上面介绍的规则，能够最大程度上进行SEO。

Google是世界上最好的搜索引擎，也是用户最多的搜索引擎。All in One SEO对Google非常友好，如果只针对Google，使用All in One SEO的默认配置就足够了。但对于大部分中文独立博客而言，博客的读者主要是国内互联网用户，而国内大部分用户的主要搜索引擎是百度搜索。百度搜索技术很一般，All in One SEO的有些设置百度不支持，这使得All in One SEO的默认设置对百度不太友好。所以，为迁就百度，需要对All in One SEO的默认设置做如下修改：

*   所有关于noindex的选项，一律不勾选
*   Autogenerate Descriptions选项不勾选

另外，安装All in One SEO后，文章页面（Post）下，会有针对当前文章的SEO设置，这里按上面介绍的规则填写相应内容，才能加强SEO效果。

&nbsp;

## robots.txt文件的写法

robots.txt文件用于控制搜索引擎对网页内容的收录。基本用法如下：

<div>

### User-agent[](http://www.ezloo.com/2011/04/robots_txt.html#user-agent "Link to this section")

User-agent是用来匹配爬虫的，每个爬虫都会有一个名字，比如百度的爬虫叫BaiDuSpider，Google的爬虫叫Googlebot，*表示所有爬虫。

</div>
<div id="more">

### Disallow[](http://www.ezloo.com/2011/04/robots_txt.html#disallow "Link to this section")

Disallow表示禁止爬虫访问的目录。Disallow: / 表示拦截整站。

### Allow[](http://www.ezloo.com/2011/04/robots_txt.html#allow "Link to this section")

Allow表示允许爬虫访问的目录。Allow: / 表示允许整站。

### Sitemap[](http://www.ezloo.com/2011/04/robots_txt.html#sitemap "Link to this section")

Sitemap用来指定sitemap的位置。

### Crawl-delay[](http://www.ezloo.com/2011/04/robots_txt.html#crawl-delay "Link to this section")

Crawl-delay用来告诉爬虫两次访问的间隔，单位是秒。爬虫如果爬得很勤，对动态网站来说，压力有点大，可能会导致服务器负载增高，用户访问变慢。

### 通配符|wildcard match[](http://www.ezloo.com/2011/04/robots_txt.html#wildcard-match "Link to this section")

*：匹配任意多个字符

$：表示URL的结尾

### **注意|notice**[](http://www.ezloo.com/2011/04/robots_txt.html#notice "Link to this section")

*   URL区分大小写，所以 /abc/ 和 /Abc/ 表示不同的目录。
*   后面有没有斜杠也是不一样的，/private 和 /private/也表示两个不同的地址。
</div>

《名博是怎样炼成的》提供了针对WordPress网站的robots.txt的一种写法：

&nbsp;

<pre class="brush:csharp">User-agent: *
Disallow: /wp-
Disallow: /feed/
Disallow: /comments/feed
Disallow: /trackback</pre>

&nbsp;

&nbsp;

## 链接结构的写法

链接结构要有利于索引，一般来说，比较好的链接结构有默认设置中的Numeric形式，和下面这种据说很多高手都使用的形式：/%year%/%monthnum%/%postname%. html 。

尽量不要修改链接结构。有时，修改了链接结构之后，点击博客上的任何一篇文章，可能会出现404错误，解决方法可以参考Gevin以前的的文章：[解决wordpress修改permalink后的404错误](http://blog.igevin.info/archives/700 "Permanent Link to 解决wordpress修改permalink后的404错误")

&nbsp;

如果大家有更好的SEO技巧，欢迎大家留言，交流心得。

&nbsp;
<div style="margin-top: 15px">
<p>**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)

**本文链接地址:** [10+ WordPress 优化技巧](http://blog.igevin.info/2011/06/10-wordpress-skill/)

</div>
<div>
极客趣玩推荐：[![](http://blog.igevin.info/wp-content/uploads/2012/05/image.png)](http://s.click.taobao.com/t_8?e=7HZ5x%2BOzdZF6pq3H3z1mawcVRxM%3D&#038;p=mm_17737402_0_0)
</div>
