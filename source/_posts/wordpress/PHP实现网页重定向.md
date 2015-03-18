title: PHP实现网页重定向
category: wordpress
tags:
- PHP
date: 2010-09-12 15:27:09
---

PHP实现网页重定向，用Header()方法

例如：

&lt;?php Header(&#8220;Location: [http://igevin.info](http://igevin.info)&#8220;)?&gt;

<span id="more-400"></span>

如果想要过几秒再跳转到其他网页，可以参考下面代码：

<pre>
<div id="_mcePaste">&lt;?php</div>
<div id="_mcePaste">//PHP重定向</div>
<div id="_mcePaste">Header("refresh:3;url=http://<span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; line-height: 19px; white-space: normal; font-size: 13px;">http://igevin.info</span>",3000);</div>
<div id="_mcePaste">?&gt;</div>
<div id="_mcePaste">&lt;html&gt;</div>
<div id="_mcePaste">&lt;head&gt;</div>
<div id="_mcePaste">&lt;title&gt;</div>
<div id="_mcePaste">Gevin的空间</div>
<div id="_mcePaste">&lt;/title&gt;</div>
<div id="_mcePaste">&lt;/head&gt;</div>
<div id="_mcePaste">&lt;body&gt;</div>
<div id="_mcePaste">网页即将转到&lt;a href="http://<span style="font-family: Georgia, 'Times New Roman', 'Bitstream Charter', Times, serif; line-height: 19px; white-space: normal; font-size: 13px;">http://igevin.info</span>"&gt;Gevin的空间&lt;/a&gt;</div>
<div id="_mcePaste">&lt;/body&gt;</div>
<div id="_mcePaste">&lt;/html&gt;</div></pre>
<div style="margin-top: 15px">

**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)

**本文链接地址:** [PHP实现网页重定向](http://blog.igevin.info/2010/09/php-redirection/)

</div>
<div>
极客趣玩推荐：[![](http://blog.igevin.info/wp-content/uploads/2012/05/image.png)](http://s.click.taobao.com/t_8?e=7HZ5x%2BOzdZF6pq3H3z1mawcVRxM%3D&#038;p=mm_17737402_0_0)
</div>
