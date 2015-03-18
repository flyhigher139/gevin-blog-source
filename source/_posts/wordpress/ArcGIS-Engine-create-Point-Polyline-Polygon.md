title: ArcGIS Engine中Point、Polyline和Polygon的创建
category: wordpress
tags:
- GIS
date: 2011-03-06 21:36:29
---

<div style="width: 490px" class="wp-caption aligncenter">[![编程](http://public.bay.livefilestore.com/y1pZCELUZVP_MdJXeZ0ytVGxdXc7lnCkPI7fyEcxBM9Y66_0BWCiGf_O-vy1yG6AkE9LJVCso9EnIUXlgphI0-nBg/coding.jpg?psid=1 "编程")](http://blog.igevin.info/archives/1091)

GIS二次开发
</div>

最近Gevin事情比较多，每天都要花大部分时间写程序，所以Web2.0系列的文章只能暂停更新一段时间了。

今天Gevin和大家分享的是GIS二次开发中Point、Polyline和Polygon的创建方法。本文介绍的方法使用C#和ArcGIS Engine进行开发。

##一、Point的创建

Point：是一个０维的几何图形，具有X,Y坐标值，以及一些可选的属性：如高程值(Z值)，度量值(M值) 和ID号，点对象用于描述精确定位的对象。AE的IPoint接口提供<span style="color: #008000;">**PutCoords()**</span>方法实现Point的创建。参考代码如下：

```csharp
/// &lt;summary&gt;
/// 根据点的坐标创建点要素
/// &lt;/summary&gt;
/// &lt;param name="x"&gt;x坐标&lt;/param&gt;
/// &lt;param name="y"&gt;y坐标&lt;/param&gt;
/// &lt;returns&gt;创建的点&lt;/returns&gt;
public IPoint CreatePoint(double x, double y)
{
    IPoint pPoint = new PointClass();
    pPoint.PutCoords(x, y);
    return pPoint;
}
```

##二、Polyline的创建

<p>Polyline对象是由一个或多个相连或者不相连的path对象的有序集合，它可以是单个Path对象组成，也可以是多个相连的Path对象组成，或者是多个分离的Path组成，如下图所示:

<div style="width: 464px" class="wp-caption aligncenter">[![Polyline](http://1hebha.bay.livefilestore.com/y1pTtRmrzYe0RoVU4z73WD6t6aNxM29WE5awt3ZfEQAbQ8exAvL5C2CyIkmK6Gvw38wFEpaWgwXaS30cxqFgw91BGah4GYyMQ2h/polygline.JPG?psid=1 "点击查看原图连接")](http://1hebha.bay.livefilestore.com/y1pTtRmrzYe0RoVU4z73WD6t6aNxM29WE5awt3ZfEQAbQ8exAvL5C2CyIkmK6Gvw38wFEpaWgwXaS30cxqFgw91BGah4GYyMQ2h/polygline.JPG?psid=1)

看图理解Polyline
</div>

&nbsp;

&nbsp;

<span style="color: #008000;">**所以，在介绍Polyline的创建之前，先要理解Path。**</span>

Path是连续的Segment的集合，除了路径的第一个Segment和最后一个Segment外其余的Segment的起始点都是前一个Segment的终止点，即Path对象的中的Segment不能出现分离，Path可以是任意数的Line,CircularArc,EllipticArc和BezierCurve的组合。

<span style="color: #008000;">**那么，Segment又是什么呢？**</span>直白的说，Segment是连接起点和终点的一段直线或曲线，它有四个子类，参考下图：

&nbsp;

<div style="width: 352px" class="wp-caption aligncenter">[![segment](http://1hebha.bay.livefilestore.com/y1pTtRmrzYe0RqmRFXnKXA0Cy0LxlHzKP_QzMK8s6WSBR_80DA9BaiNjca8khxLq8Y_T7YDA80W8AVvlGhU2HQ2tP1iaOXjGBe9/seg1.jpg?psid=1 "点击查看原图")](http://1hebha.bay.livefilestore.com/y1pTtRmrzYe0RqmRFXnKXA0Cy0LxlHzKP_QzMK8s6WSBR_80DA9BaiNjca8khxLq8Y_T7YDA80W8AVvlGhU2HQ2tP1iaOXjGBe9/seg1.jpg?psid=1)

理解segment
</div>
<div style="width: 486px" class="wp-caption aligncenter">[![segment的子类](http://1hebha.bay.livefilestore.com/y1prWMZf9WpeufcaWxABKHI4yXbV2pfBISxjYaxoZXF_8yhZJ71jj696rCfROGGuN5h9GqmgjxL636EnkXIiPwFG8fBnulUAFMD/segment.jpg?psid=1 "点击查看原图")](http://1hebha.bay.livefilestore.com/y1prWMZf9WpeufcaWxABKHI4yXbV2pfBISxjYaxoZXF_8yhZJ71jj696rCfROGGuN5h9GqmgjxL636EnkXIiPwFG8fBnulUAFMD/segment.jpg?psid=1)

segment的子类
</div>

&nbsp;

&nbsp;

<span style="color: #008000;">**OK，上述内容理解了，我们才能够正确创建Polyline。Gevin提供一个只有一个Path的Polyline的创建方法。**</span>


```csharp
/// &lt;summary&gt;
/// 创建直线line
/// &lt;/summary&gt;
/// &lt;param name="from"&gt;&lt;/param&gt;
/// &lt;param name="to"&gt;&lt;/param&gt;
/// &lt;returns&gt;&lt;/returns&gt;
public static ILine CreateLine(IPoint from, IPoint to)
{
    ILine pLine = new LineClass();
    pLine.PutCoords(from, to);
    return pLine;
}

/// &lt;summary&gt;
/// 创建只有一个Path的Polyline
/// &lt;/summary&gt;
/// &lt;param name="PolylineList"&gt;&lt;/param&gt;
/// &lt;returns&gt;&lt;/returns&gt;
public static IPolyline CreatePolyline(List&lt;IPoint&gt; PolylineList)
{
    ISegment pSegment;
    ILine pLine;
    object o = Type.Missing;
    ISegmentCollection pPath = new PathClass();
    for (int i = 0; i &lt; PolylineList.Count-1; i++)
    {
        pLine = CreateLine(PolylineList[i], PolylineList[i + 1]);
        pSegment = pLine as ISegment;
        pPath.AddSegment(pSegment, ref o, ref o);

    }
    IGeometryCollection pPolyline = new PolylineClass();
    pPolyline.AddGeometry(pPath as IGeometry, ref o, ref o);
    return pPolyline as IPolyline;
}
```
## 三、Polygon的创建

Polygon的创建也要基于对Polygon的理解，这点可以类比Polyline。所以，Gevin不再介绍Polygon，也不再提供源码，感兴趣的童鞋自己写吧，应该没有问题了，或者即使有问题，也一定能够凭借自己的能力解决问题。

最后再罗嗦几句：

1\. 写程序要保持思维清晰，写程序前要理解编程对象

2\. 本文的理论参考见这个连接：[ArcGIS Engine基础开发教程(2)——学习几何对象与空间参考](http://bbs.esrichina-bj.cn/ESRI/thread-46367-1-1.html)

3\. 编程心得，欢迎大家与Gevin交流

如转载本文，请表明出处！

**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)


