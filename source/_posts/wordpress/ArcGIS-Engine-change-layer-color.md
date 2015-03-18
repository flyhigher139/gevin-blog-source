title: AE中修改图层颜色
category: wordpress
tags:
- GIS
date: 2010-08-02 07:29:36
---

转载请标明[本文](http://blog.igevin.info/archives/142)出处：[Gevin的博客](http://blog.igevin.info/)

AE中修改图层颜色，可以简单按照下面思路实现：在TOCControl中的某一个图层上点击右键，在快捷菜单中选择更改颜色，然后弹出ColorDialog对话框，选中喜欢的颜色，点击确定，修改图层颜色成功。

右键弹出菜单，弹出ColorDialog对话框都是C#的基本操作，下面做简单介绍：

<span id="more-142"></span>

### 1. TOCControl中右键弹出菜单

```CSharp
private void axTOCControl1_OnMouseDown(object sender, ITOCControlEvents_OnMouseDownEvent e)
{
    esriTOCControlItem item = esriTOCControlItem.esriTOCControlItemNone;
    IBasicMap map = null;
    ILayer layer = null;
    object other = null;
    object index = null;

    axTOCControl1.HitTest(e.x, e.y, ref item, ref map, ref layer, ref other, ref index);

    for (int i = 0; i &lt; axMapControl1.LayerCount; i++)
    {
        if (axMapControl1.get_Layer(i) == layer)
        {
            curLayerIndex = i;
            curLayer = layer;
            break;
        }
    }
    if (e.button == 2)
    {
        if(item == esriTOCControlItem.esriTOCControlItemLayer)
            TOCContextMenuStrip.Show(axTOCControl1, e.x, e.y);
        else if (item == esriTOCControlItem.esriTOCControlItemLegendClass)
        {
            TOCContextMenuStrip2.Show(axTOCControl1, e.x, e.y);
        }
    }

}
```

### 2.选择修改颜色，弹出ColorDialog对话框

```CSharp
private void ChangeColorToolStripMenuItem_Click(object sender, EventArgs e)
{
    if (curLayer == null)
        return;
    if (colorDialog1.ShowDialog() == DialogResult.OK)
    {
        Color SelectColor = colorDialog1.Color;
        ChangeLayerColor(curLayer, SelectColor);//本文核心方法，下面介绍
        axMapControl1.ActiveView.Refresh();
        axTOCControl1.Update();
    }
}
```

### 3.修改颜色

这是[本文](http://blog.igevin.info/archives/142)的核心方法，修改颜色的本质是更改图层的Renderer。

```csharp
/// &lt;summary&gt;
/// 更改图层颜色
/// &lt;/summary&gt;
/// &lt;param name="pLayer"&gt;要更改颜色的图层&lt;/param&gt;
/// &lt;param name="colorDialog1"&gt;颜色对话框&lt;/param&gt;
public static void ChangeLayerColor(ILayer pLayer, Color selectColor)
{
    if (pLayer == null)
        return;

    IRgbColor pRgbColor = new RgbColorClass();
    pRgbColor.Red = selectColor.R;
    pRgbColor.Green = selectColor.G;
    pRgbColor.Blue = selectColor.B;

    IFeatureLayer pFeatureLayer = pLayer as IFeatureLayer;
    IFeatureClass pFeatureClass = pFeatureLayer.FeatureClass;
    ISimpleRenderer pSimpleRenderer = new SimpleRendererClass();

    switch (pFeatureClass.ShapeType)
    {
        case esriGeometryType.esriGeometryPoint:
            ISimpleMarkerSymbol pSimpleMarkerSymbol = new SimpleMarkerSymbolClass();
            pSimpleMarkerSymbol.Color = pRgbColor as IColor;
            pSimpleMarkerSymbol.Size = 4;
            pSimpleRenderer.Symbol = pSimpleMarkerSymbol as ISymbol;
            break;
        case esriGeometryType.esriGeometryPolyline:
            ISimpleLineSymbol pSimpleLineSymbol = new SimpleLineSymbolClass();
            pSimpleLineSymbol.Color = pRgbColor as IColor;
            pSimpleRenderer.Symbol = pSimpleLineSymbol as ISymbol;
            break;
        case esriGeometryType.esriGeometryPolygon:
            ISimpleFillSymbol pSimpleFillSymbol = new SimpleFillSymbolClass();
            pSimpleFillSymbol.Color = pRgbColor as IColor;
            pSimpleRenderer.Symbol = pSimpleFillSymbol as ISymbol;
            break;
        default:
            return;
    }
    IGeoFeatureLayer pGeoFeaturelayer = pLayer as IGeoFeatureLayer;
    pGeoFeaturelayer.Renderer = pSimpleRenderer as IFeatureRenderer;

}
```

如转载本文，请表明出处
