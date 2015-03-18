title: Android和Windows 7下OpenVPN的设置
category: wordpress
tags:
- 互联网
date: 2012-04-22 21:37:33
---

[![](http://blog.igevin.info/wp-content/uploads/2012/04/最近更新.jpg "android and Windows7 OpenVPN设置")](http://blog.igevin.info/wp-content/uploads/2012/04/最近更新.jpg)

对于来阅读本文的朋友，[OpenVPN](http://en.wikipedia.org/wiki/OpenVPN)为何物想必不用解释了吧，Gevin写这么一篇文章，主要是对相应的设置做个简单总结，方便大家分享和交流。<span id="more-1696"></span>

# Android上OpenVPN的配置

## 1\. 配置环境

Android下使用OpenVPN需要以下app：[OpenVPN Settings](https://play.google.com/store/apps/details?id=de.schaeuffelhut.android.openvpn&amp;feature=search_result), [OpenVPN Installer](https://play.google.com/store/apps/details?id=de.schaeuffelhut.android.openvpn.installer&amp;feature=search_result)和[BusyBox](https://play.google.com/store/apps/details?id=stericson.busybox&amp;feature=search_result#?t=W251bGwsMSwxLDEsInN0ZXJpY3Nvbi5idXN5Ym94Il0.)，BusyBox和OpenVPN Installer需要root权限，手机没有root的用户就别折腾OpenVPN了，直接用系统自带的VPN设置得啦！

这三个app关系如下：

OpenVPN Settings 是android上的一个OpenVPN客户端，需要连接VPN的时候要打开这个应用。若要OpenVPN正常工作，仅有这个客户端并不够，我们还需要将OpenVPN安装到手机中，这就需要用到OpenVPN Installer这个app了，而OpenVPN Installer正常工作的先决条件是busybox，所以我们还需要装一个BusyBox 应用。

当三个app都安装好，并且OpenVPN Installer的视图和下面截图相同时，你的手机才具备使用OpenVPN的环境

<div id="attachment_1702" style="width: 394px" class="wp-caption aligncenter">[![OpenVPN Installer status](http://blog.igevin.info/wp-content/uploads/2012/04/Screenshot_2012-04-22-20-11-35.jpg "OpenVPN Installer status")](http://blog.igevin.info/wp-content/uploads/2012/04/Screenshot_2012-04-22-20-11-35.jpg)

OpenVPN Installer status
</div>

&nbsp;

## 2\. 参数设置

<span style="color: #008000;">**现在，我们再配置OpenVPN Settings，完成OpenVPN在android上的设置。**</span>

首先在SD卡上创建一个新的文件夹，重命名为<span style="color: #333333; font-style: normal; line-height: 24px;"><span style="color: #008000;">**openvpn**</span>，然后把你的VPN帐号中的相关配置文件（如密钥*.key, 证书*.crt和*.ovpn文件）放入该文件夹中，最后打开OpenVPN Settings 这个app，点击连接OpenVPN。当所有的设置都没问题时，应该看到下面的截图，可用的连接取决于你的VPN帐号。</span>

<div id="attachment_1703" style="width: 450px" class="wp-caption aligncenter">[![](http://blog.igevin.info/wp-content/uploads/2012/04/Screenshot_2012-04-21-16-09-21.png.jpg "OpenVPN Settings status")](http://blog.igevin.info/wp-content/uploads/2012/04/Screenshot_2012-04-21-16-09-21.png.jpg)

OpenVPN Settings status
</div>

当你做好这些设置，便可以通过VPN访问网络了。

[![](http://blog.igevin.info/wp-content/uploads/2012/04/stack1-001.jpg "no gfw")](http://blog.igevin.info/wp-content/uploads/2012/04/stack1-001.jpg)

_(上图借用了[这个地方的图片](http://www.flickr.com/photos/zola/4127996758/ "跨越长城 Across the Great Wall we can reach every corner in the world"))_

# Windows 7下设置OpenVPN

首先要下载OpenVPN的安装软件，这个自己去网上找

然后打开OpenVPN的安装目录，把上面提过的配置文件放入OpenVPN安装目录的config文件夹中

最后，以<span style="color: #008000;">管理员身份运行</span>OpenVPN的客户端—— OpenVPN GUI，连通并访问网络。

**注：**

*   Windows7 用户安装完OpenVPN后，第一次连接前，貌似要进行这个操作——开始菜单里选，Add a new TAP virtual ethernet adapter，然后再连
*   Windows7 用户使用OpenVPN程序，需要以<span style="color: #008000;">管理员身份运行程序</span>才可以
*   如果你希望节省流量，实现访问国内资源时不通过VPN，访问国外资源时通过VPN连接，可以参考[这个项目](http://code.google.com/p/chnroutes/)自己折腾，本文就不再介绍了
<div style="margin-top: 15px">

**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)

**本文链接地址:** [Android和Windows 7下OpenVPN的设置](http://blog.igevin.info/2012/04/configuration-of-openvpn-in-android-and-windows7/)

</div>
<div>
极客趣玩推荐：[![](http://blog.igevin.info/wp-content/uploads/2012/05/image.png)](http://s.click.taobao.com/t_8?e=7HZ5x%2BOzdZF6pq3H3z1mawcVRxM%3D&#038;p=mm_17737402_0_0)
</div>
<table class="wumii-related-items" cellspacing="0" cellpadding="3" border="0"  style="clear: both;">

<tr>
<td colspan="5">**<font size="-1"  style="display: block !important; padding: 20px 0 5px !important;">猜您也喜欢：</font>**</td>
</tr>

<tr>
<td width="102" valign="top" style="padding: 5px !important; margin: 0 !important;">
[
![](http://static.wumii.cn/site_images/ti/FdvNBwH8.jpg?i=Yigb6fRp)

<font size="-1" color="#333333" style="display: block !important; line-height: 15px !important; width: 102px !important; font: 12px/15px arial !important; height: 60px !important; margin: 3px 0 0 0 !important; padding: 0 !important; overflow: hidden !important;">Google技巧：优化搜索，优化结果</font>
](http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fblog.igevin.info%2F2011%2F01%2Fgoogle-search-skill%2F&from=http%3A%2F%2Fblog.igevin.info%2F2012%2F04%2Fconfiguration-of-openvpn-in-android-and-windows7%2F "Google技巧：优化搜索，优化结果")
</td>
<td width="102" valign="top" style="padding: 5px !important; margin: 0 !important; border-left: 1px solid #DDDDDD !important;">
[
![](http://static.wumii.cn/site_images/ti/3C65YGOh.jpg?i=ugASoxyy)

<font size="-1" color="#333333" style="display: block !important; line-height: 15px !important; width: 102px !important; font: 12px/15px arial !important; height: 60px !important; margin: 3px 0 0 0 !important; padding: 0 !important; overflow: hidden !important;">互联网上的免费</font>
](http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fblog.igevin.info%2F2012%2F11%2Ffree-mode-on-internet%2F&from=http%3A%2F%2Fblog.igevin.info%2F2012%2F04%2Fconfiguration-of-openvpn-in-android-and-windows7%2F "互联网上的免费")
</td>
<td width="102" valign="top" style="padding: 5px !important; margin: 0 !important; border-left: 1px solid #DDDDDD !important;">
[
![](http://static.wumii.cn/site_images/ti/kFwuNv2e.png?i=15LnjbCAP)

<font size="-1" color="#333333" style="display: block !important; line-height: 15px !important; width: 102px !important; font: 12px/15px arial !important; height: 60px !important; margin: 3px 0 0 0 !important; padding: 0 !important; overflow: hidden !important;">web2.0上网攻略——SNS篇</font>
](http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fblog.igevin.info%2F2011%2F02%2Fweb2-0-sns%2F&from=http%3A%2F%2Fblog.igevin.info%2F2012%2F04%2Fconfiguration-of-openvpn-in-android-and-windows7%2F "web2.0上网攻略——SNS篇")
</td>
<td width="102" valign="top" style="padding: 5px !important; margin: 0 !important; border-left: 1px solid #DDDDDD !important;">
[
![](http://static.wumii.cn/resources/images/related_item_default/84.jpg)

<font size="-1" color="#333333" style="display: block !important; line-height: 15px !important; width: 102px !important; font: 12px/15px arial !important; height: 60px !important; margin: 3px 0 0 0 !important; padding: 0 !important; overflow: hidden !important;">Google 搜索技巧</font>
](http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fblog.igevin.info%2F2010%2F06%2Fgoogle-skill-1%2F&from=http%3A%2F%2Fblog.igevin.info%2F2012%2F04%2Fconfiguration-of-openvpn-in-android-and-windows7%2F "Google 搜索技巧")
</td>
<td width="102" valign="top" style="padding: 5px !important; margin: 0 !important; border-left: 1px solid #DDDDDD !important;">
[
![](http://static.wumii.cn/site_images/ti/6gYhQNTQ.png?i=905ZwCUy)

<font size="-1" color="#333333" style="display: block !important; line-height: 15px !important; width: 102px !important; font: 12px/15px arial !important; height: 60px !important; margin: 3px 0 0 0 !important; padding: 0 !important; overflow: hidden !important;">15 个一定要会的 Windows7 快捷键</font>
](http://app.wumii.com/ext/redirect?url=http%3A%2F%2Fblog.igevin.info%2F2010%2F10%2F15-%25E4%25B8%25AA%25E4%25B8%2580%25E5%25AE%259A%25E8%25A6%2581%25E4%25BC%259A%25E7%259A%2584-windows7-%25E5%25BF%25AB%25E6%258D%25B7%25E9%2594%25AE%2F&from=http%3A%2F%2Fblog.igevin.info%2F2012%2F04%2Fconfiguration-of-openvpn-in-android-and-windows7%2F "15 个一定要会的 Windows7 快捷键")
</td>
</tr>

<tr>
<td colspan="5" align="right">
[
<font size="-1" color="#bbbbbb" style="display: block !important; font-family: arial !important; padding: 5px 0 !important; font-size: 12px !important; color: #bbb !important;">无觅</font>
](http://www.wumii.com/widget/relatedItems "无觅关联推荐")
</td>
</tr>
</table>
