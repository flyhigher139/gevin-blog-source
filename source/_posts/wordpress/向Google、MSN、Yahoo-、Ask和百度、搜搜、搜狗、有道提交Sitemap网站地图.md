title: "向Google、MSN、Yahoo!、Ask和百度、搜搜、搜狗、有道提交Sitemap网站地图"
category: wordpress
tags:
- SEO
- wordpress
date: 2010-10-02 00:07:42
---

[网站优化](http://blog.igevin.info/archives/432 "查看 网站优化 的全部文章")很重要的方法就是[sitemap提交](http://blog.igevin.info/archives/432 "sitemap提交")。

向Google、MSN、Yahoo!、Ask和百度、搜搜、搜狗、有道提交Sitemap网站地图都可以通过robots.txt来定义提交。但下面的提交方式更简单快捷。

[sitemap 提交地址](http://blog.igevin.info/archives/432 "sitemap提交")：<span id="more-432"></span>

向Google提交网站地图Sitemap: 通过网址http://www.google.com/webmasters管理提交;

向Yahoo!提交网站地图Sitemap: 通过网址http://siteexplorer.search.yahoo.com管理提交;

向MSN提交网站地图Sitemap: 用URL直接提交：http://api.moreover.com/ping?u=http%3A//your.domainname/sitemap.xml。这是向MSN直接提交网站地图的后门URL。注意”:”被%3A替换掉。

向ASK提交网站地图Sitemap: 直接提交。http://submissions.ask.com/ping?sitemap=http%3A//your.domainname/sitemap.xml。注意”:”被%3A替换掉。

向百度Baidu提交网站地图Sitemap: 没办法，现在百度不支持Sitemap。但可通过http://www.baidu.com/search/url_submit.html来提交你的网址。百度会自行搜索，更新速度很快。

向搜搜soso提交网站地图Sitemap，搜搜不支持Sitemap。但可通过http://www.soso.com/help/usb/urlsubmit.shtml来提交你的网址。

向搜狗sogou提交网站地图Sitemap，搜狗不支持Sitemap。但可通过http://www.sogou.com/feedback/urlfeedback.php来提交你的网址。

向有道youdao提交网站地图Sitemap，有道不支持Sitemap。但可通过http://tellbot.youdao.com/report来提交你的网址。

什么是sitemap，从wiki上可以找到的解释是：(通俗的讲就是“网站地图”)

The Sitemaps protocol allows a webmaster to inform search engines about URLs on a website that are available for crawling. A Sitemap is an XML file that lists the URLs for a site. It allows webmasters to include additional information about each URL: when it was last updated, how often it changes, and how important it is in relation to other URLs in the site. This allows search engines to crawl the site more intelligently. Sitemaps are a URL inclusion protocol and complement robots.txt, a URL exclusion protocol.

即sitmaps是站点管理员向搜索引擎爬虫公布站点可被抓取页面的协议，sitemap文件内容必须遵循XML格式的定义。每个URL可以包含更新的周期和时间、URL在整个站点中的优先级。这样可以让搜索引擎更佳有效的抓取网站内容。

sitemap分为2种形式：

1、sitemap.html ： 这种主要是针对用户而言，让用户能够快速的寻找到自己所需的东西，也是方便搜索引擎来有效的爬取网页内容，提高网站质量。

2、sitemap.xml ： 这种格式主要是谷歌自己推出的一种网站地图写法，你可以通过相关规范写出网站地图 然后通过“谷歌管理员工具”提交，这样谷歌的蜘蛛就能有目的的高效的快速的来访问网站，但是 提交的内容 谷歌蜘蛛没有保证一定都会收录!这个误区 请大家要区分开来。

Sitemaps 的XML格式样例：

xmlns:xsi=”http://www.w3.org/2001/XMLSchema-instance”

xsi:schemaLocation=”http://www.sitemaps.org/schemas/sitemap/0.9

http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd”&gt;

http://w3c-at.de

2006-11-18

daily

0.8

目前Google Yahoo和Ask.com支持的最新sitemaps标准是0.9版本。sitemaps文件必须为utf-8的编码格式，每个sitemaps文件只能有一个的顶级标签。

每个标签是对一个URL的描述：

是URL的绝对地址，必须用http或https开头

是该URL的最后一次修改时间

表示该URL的更新频率，可以设置为daily weekly always是该URL在整个站点的权重，是1.0~0.1之间的数值

sitemaps文件的限制：

必须是utf-8的编码格式

每个sitemap.xml文件包含的URL建议不超过5w个URL

单个sitemap.xml文件不能超过10M大小
<div style="margin-top: 15px">
<p>**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)

**本文链接地址:** [向Google、MSN、Yahoo!、Ask和百度、搜搜、搜狗、有道提交Sitemap网站地图](http://blog.igevin.info/2010/10/%e5%90%91google%e3%80%81msn%e3%80%81yahoo%e3%80%81ask%e5%92%8c%e7%99%be%e5%ba%a6%e3%80%81%e6%90%9c%e6%90%9c%e3%80%81%e6%90%9c%e7%8b%97%e3%80%81%e6%9c%89%e9%81%93%e6%8f%90%e4%ba%a4sitemap%e7%bd%91/)

</div>
<div>
极客趣玩推荐：[![](http://blog.igevin.info/wp-content/uploads/2012/05/image.png)](http://s.click.taobao.com/t_8?e=7HZ5x%2BOzdZF6pq3H3z1mawcVRxM%3D&#038;p=mm_17737402_0_0)
</div>
