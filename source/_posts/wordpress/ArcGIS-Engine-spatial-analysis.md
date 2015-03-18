title: ArcGIS Engine空间分析简单实现
category: wordpress
tags:
- GIS
date: 2010-11-21 09:43:38
---



[![coding](http://1hebha.bay.livefilestore.com/y1phIyIGWO2u8nGOxNeaijgMfWRetQJ1WYysAslX-vegPgzIozElvdaqvwDZSex8HarEeknRmsKdeEcNjhCW-IikGgZHuC93Iax/coding.jpg?psid=1 "coding")](http://blog.igevin.info/archives/587)

[本文](http://blog.igevin.info/archives/587)简单介绍一下ArcGIS Engine中空间分析实现。空间分析需要的接口是ITopologicalOperator接口，本文介绍一下缓冲区的生成以及多边形的合并。利用ArcGIS Engine进行空间分析是比较简单的，希望本文有抛砖引玉之效

##### 一、缓冲区的生成

缓冲区的生成是利用ITopologicalOperator接口的Buffer方法实现，非常简单，所以这里直接贴代码啦！



```csharp
/// &lt;summary&gt;
/// 为图形建立缓冲区
/// &lt;/summary&gt;
/// &lt;param name="geometry"&gt;选中要素的Geometry&lt;/param&gt;
/// &lt;param name="distance"&gt;设置缓冲区大小&lt;/param&gt;
/// &lt;returns&gt;创建好的缓冲区&lt;/returns&gt;

public static IGeometry CreateBuffer(IGeometry geometry, double distance)
{
    ITopologicalOperator pTopologicalOperator = geometry as ITopologicalOperator;
    IGeometry buffer = pTopologicalOperator.Buffer(distance);
    return buffer;
}
```

**二、合并多边形**

合并多边形使用ITopologicalOperator的Union方法，也非常简单，代码如下：

```csharp
/// &lt;summary&gt;
/// 将两个Geometry对象合并为一个新的Geometry对象，其中GeometryA的值会被修改
/// &lt;/summary&gt;
/// &lt;param name="GeometryA"&gt;要合并的一个Geometry&lt;/param&gt;
/// &lt;param name="GeometryB"&gt;要合并的另一个Geometry&lt;/param&gt;
/// &lt;returns&gt;合并后的Geometry&lt;/returns&gt;
public static IGeometry UnionTwoGeometries(IGeometry GeometryA, IGeometry GeometryB)
{
    ITopologicalOperator pTopologicalOperator = GeometryA as ITopologicalOperator;
    IGeometry UnionGeometry = pTopologicalOperator.Union(GeometryB);
    return UnionGeometry;
}
```
<div>好，今天就介绍这么多了！</div>



**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)



