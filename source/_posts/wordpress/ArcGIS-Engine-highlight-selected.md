title: ArcGIS Engine中选中要素闪烁方法的实现
category: wordpress
tags:
- GIS
date: 2010-08-08 14:45:15
---

[

![coding](http://public.bay.livefilestore.com/y1p91LjrUtcMgLVYH8DcsC2eDROHyLWrLmDaazbdthAjpiemOys6e3HMVxFkz9M3R05X05VCAOfN_jjTM4HrAPJQw/coding.jpg?psid=1 "coding")](http://blog.igevin.info/archives/176)

AE中要实现选中要素的闪烁，有2个关键点：（1）确定选中的元素是哪一个；（2）闪烁该元素。

实现第一点要用接口IQueryFilter接口，还需要IFeatureCursor接口作为配合；第二点用MapControl控件的一个方法就可以实现，该方法为FlashShape（）方法。

<span id="more-176"></span>

下面介绍一个例子。这个例子将闪烁属性表中选中的要素。代码如下:

```
//rIndex为选中要素在属性表中的行号
private void queryGeoObject(IFeatureLayer curFeatureLayer, int rIndex)
{
    if (curMapControl == null||rIndex==-1||curFeatureLayer==null)
        return;
    IQueryFilter pQueryFilter = new QueryFilterClass();
    string queryString = dataGridView1.Columns[0].HeaderText + "=" + dataGridView1.Rows[rIndex].Cells[0].Value.ToString();
    pQueryFilter.WhereClause = queryString;
    IFeatureCursor pFeatureCursor = curFeatureLayer.Search(pQueryFilter, false);
    IFeature pFindFeature = pFeatureCursor.NextFeature();
    while (pFindFeature != null)
    {
        IEnvelope pEnvelop;
        pEnvelop = pFindFeature.Shape.Envelope;
        pEnvelop.Expand(3, 3, true);
        curMapControl.Extent = pEnvelop;
        curMapControl.ActiveView.ScreenDisplay.UpdateWindow();
        switch (pFindFeature.Shape.GeometryType)
        {
            case esriGeometryType.esriGeometryPoint:
                IMarkerSymbol pMarkerSymbol = new SimpleMarkerSymbolClass();
                pEnvelop.Width = 10;
                pEnvelop.Height = 10;
                pEnvelop.CenterAt(pFindFeature.Shape as IPoint);
                curMapControl.Extent = pEnvelop;
                curMapControl.ActiveView.ScreenDisplay.UpdateWindow();
                curMapControl.FlashShape(pFindFeature.Shape,2,300,pMarkerSymbol);
                break;
            case esriGeometryType.esriGeometryPolyline:
                ILineSymbol pLineSymbol = new SimpleLineSymbolClass();
                curMapControl.FlashShape(pFindFeature.Shape,2, 300, pLineSymbol);
                break;
            case esriGeometryType.esriGeometryPolygon:
                IFillSymbol pFillSymbol = new SimpleFillSymbolClass();
                curMapControl.FlashShape(pFindFeature.Shape, 2, 300, pFillSymbol);
                break;
        }
        pFindFeature = pFeatureCursor.NextFeature();
    }
}
```

如果想在双击属性表中某一行时闪烁该元素，只需在DataGridiew的CellDoubleClick事件中这样写：

<div id="_mcePaste">private void dataGridView1_CellDoubleClick(object sender, DataGridViewCellEventArgs e)</div>
<div id="_mcePaste">{</div>
<div id="_mcePaste">rowIndex = e.RowIndex;</div>
<div id="_mcePaste">queryGeoObject(curFeatureLayer, rowIndex);</div>
<div id="_mcePaste">}</div>


**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)

