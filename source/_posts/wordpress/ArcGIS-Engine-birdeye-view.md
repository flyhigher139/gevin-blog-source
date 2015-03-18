title: ArcGIS Engine中鹰眼图的实现
category: wordpress
tags:
- GIS
date: 2010-08-07 08:24:22
---

<div>如转载[本文](http://blog.igevin.info/archives/163)，请表明[出处](http://blog.igevin.info/archives/163):[Gevin的博客](http://blog.igevin.info)</div>
<div>AE中实现鹰眼地图，需要2个MapControl控件：axMapControl1作为主要的地图控件，axMapControl2作为鹰眼图。

鹰眼图的实现需要2个方法：（1）创建鹰眼图（2）在鹰眼图中标出axMapControl1所显示的范围创建鹰眼图用本文BirdEyeViewRefresh()方法实现，代码如下：</div>
<div><span id="more-163"></span></div>
<pre class="brush:csharp">        //创建鹰眼
private void BirdEyeViewRefresh()
{
axMapControl2.ClearLayers();
IMap pMap = axMapControl1.Map;
for (int i = 0; i &lt; pMap.LayerCount; i++)
{
axMapControl2.AddLayer(pMap.get_Layer(i));
}
axMapControl2.Extent = axMapControl2.FullExtent;
}</pre>

在鹰眼图中标出axMapControl1所显示的范围的方法要写在axMapControl1的onExtentUpdate事件中

<pre class="brush:csharp"> private void axMapControl1_OnExtentUpdated(object sender, IMapControlEvents2_OnExtentUpdatedEvent e)
{
IEnvelope pEnv = e.newEnvelope as IEnvelope;
IGraphicsContainer pGraphicsContainer = axMapControl2.Map as IGraphicsContainer;
IActiveView pActiveView = pGraphicsContainer as IActiveView;

//绘制矩形框前，清除Map中的所有图形元素
pGraphicsContainer.DeleteAllElements();

IRectangleElement pRectangleEle = new RectangleElementClass();
IElement pEle = pRectangleEle as IElement;
pEle.Geometry = pEnv;

IFillSymbol pFillSymbol =CreatFillSymbol();//该方法在下面
IFillShapeElement pFillShapeEle = pEle as IFillShapeElement;
pFillShapeEle.Symbol = pFillSymbol;
pEle = pFillShapeEle as IElement;
pGraphicsContainer.AddElement(pEle, 0);
pActiveView.PartialRefresh(esriViewDrawPhase.esriViewGraphics, null, null);
}</pre>

上面代码中用来这个方法：

<pre>        public static ISimpleFillSymbol CreatFillSymbol()
{
ISimpleFillSymbol createdSymbol = new SimpleFillSymbolClass();

IRgbColor color = new RgbColorClass();
color.RGB = 255;
ILineSymbol outLine = new SimpleLineSymbolClass();
outLine.Width = 1.5;
outLine.Color = color;

createdSymbol.Outline = outLine;
createdSymbol.Style = esriSimpleFillStyle.esriSFSHollow;
return createdSymbol;
}</pre>

转载请表明出处：[ArcGIS Engine中鹰眼图的实现](http://blog.igevin.info/archives/163) http://blog.igevin.info/archives/163
<div style="margin-top: 15px">
<p>**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)

**本文链接地址:** [ArcGIS Engine中鹰眼图的实现](http://blog.igevin.info/2010/08/arcgis-engine-hawk-eye-map/)

</div>
<div>
极客趣玩推荐：[![](http://blog.igevin.info/wp-content/uploads/2012/05/image.png)](http://s.click.taobao.com/t_8?e=7HZ5x%2BOzdZF6pq3H3z1mawcVRxM%3D&#038;p=mm_17737402_0_0)
</div>
