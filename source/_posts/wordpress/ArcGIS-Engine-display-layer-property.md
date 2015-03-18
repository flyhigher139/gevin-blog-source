title: ArcGIS Engine中实现图层属性数据的显示
category: wordpress
tags:
- GIS
date: 2010-08-02 05:36:18
---

转载[本文](http://blog.igevin.info/archives/133)，请写明出处：[Gevin的博客](http://blog.igevin.info/archives/133)

本文使用下面三个方法实现图层属性数据的显示：

1.  public static DataTable ShowPropertyTable(ILayer pLayer){……}
2.  public static DataTable ShowIFeatureLayer(IFeatureLayer pLayer){……}
3.  public static DataTable ShowIRasterLayer(IRasterLayer pLayer){……}

方法1功能是将属性数据存入DataTable中，该方法将调用方法2和方法3

方法2功能是将矢量数据的属性信息存入DataTable中

方法3功能是将栅格数据的属性信息存入DataTable中

下面将分别介绍这三个方法。<span id="more-133"></span>

## 1、将属性数据存入DataTable中

代码如下：

```csharp
/// ‹summary&gt;
/// 显示属性表
/// ‹/summary&gt;
/// ‹param name="pLayer"&gt;选中的图层‹/param&gt;
/// ‹returns&gt;‹/returns&gt;
public static DataTable ShowPropertyTable(ILayer pLayer)
{
    DataTable propertyTable = null;
    if (pLayer is IFeatureLayer)
        propertyTable = ShowIFeatureLayer((IFeatureLayer)pLayer);
    else
    {
        if (pLayer is IRasterLayer)
            propertyTable = ShowIRasterLayer((IRasterLayer)pLayer);
    }
    return propertyTable;
}
```

## 2、将矢量数据的属性信息存入DataTable中

代码如下：

```csharp
/// ‹summary&gt;
/// 显示FeatureLayer属性表
/// ‹/summary&gt;
/// ‹param name="pLayer"&gt;选中的图层‹/param&gt;
/// ‹returns&gt;‹/returns&gt;
public static DataTable ShowIFeatureLayer(IFeatureLayer pLayer)
{
    DataTable propertyTable = new DataTable("Feature Layer Property");
    IFeatureClass pFeatureClass = pLayer.FeatureClass;

    string shape = string.Empty;
    switch (pFeatureClass.ShapeType)
    {
        case esriGeometryType.esriGeometryPoint: shape = "Point";
            break;
        case esriGeometryType.esriGeometryPolyline: shape = "Polyline";
            break;
        case esriGeometryType.esriGeometryPolygon: shape = "Polygon";
            break;
    }

    for (int i = 0; i &lt; pFeatureClass.Fields.FieldCount; i++)
    {
        DataColumn tempColumn = new DataColumn(pFeatureClass.Fields.get_Field(i).Name);
        propertyTable.Columns.Add(tempColumn);
    }

    IFeatureCursor pCursor = pFeatureClass.Search(null, false);
    IFeature pFeature = pCursor.NextFeature();
    int shapeIndex = pFeature.Fields.FindField("shape");
    while (pFeature != null)
    {
        DataRow tempRow = propertyTable.NewRow();
        for (int i = 0; i &lt; pFeature.Fields.FieldCount; i++)
        {
            if (i == shapeIndex)
                tempRow[i] = shape;
            else
            {
                tempRow[i] = pFeature.get_Value(i);
            }
        }
        propertyTable.Rows.Add(tempRow);
        pFeature = pCursor.NextFeature();
    }

    return propertyTable;
}
```

## 3、将栅格数据的属性信息存入DataTable中

代码如下：

```csharp
/// ‹summary&gt;
/// 显示栅格图层属性表
/// ‹/summary&gt;
/// ‹param name="pLayer"&gt;选中的图层‹/param&gt;
/// ‹returns&gt;‹/returns&gt;
public static DataTable ShowIRasterLayer(IRasterLayer pLayer)
{
    IRaster pRaster = pLayer.Raster;
    IRasterProps pRasterProps = pRaster as IRasterProps;
    pRasterProps.PixelType = rstPixelType.PT_LONG;
    if (pRasterProps.PixelType == rstPixelType.PT_LONG)
    {
        IRasterBandCollection pRBCollection = pRaster as IRasterBandCollection;
        IRasterBand pRasterBand = pRBCollection.Item(0);
        ITable pRTable = pRasterBand.AttributeTable;
        DataTable propertyTable = new DataTable("Raster Layer Property");
        for (int i = 0; i &lt; pRTable.Fields.FieldCount; i++)
        {
            DataColumn tempColumn = new DataColumn(pRTable.Fields.get_Field(i).Name);
            propertyTable.Columns.Add(tempColumn);
        }
        ICursor pCursor = pRTable.Search(null, false);
        IRow pRow = pCursor.NextRow();
        while (pRow != null)
        {
            DataRow tempRow = propertyTable.NewRow();
            for (int i = 0; i &lt; pRow.Fields.FieldCount; i++)
                tempRow[i] = pRow.get_Value(i).ToString();
            propertyTable.Rows.Add(tempRow);
            pRow = pCursor.NextRow();
        }

        return propertyTable;
    }
    return null;
}
```

## 4、显示属性数据

本不打算写这一步的，太简单了，但为了与标题呼应，还是简单说一下吧。

通过上面的方法，我们把属性数据存入DataTable中，假设这个DataTable为TempDataTable，要显示数据，需要加一个控件DataGridView，定义变量名为TempDataGridView，然后写这句话：empDataGridView.DataSource = TempDataTable;

这样就OK了！


**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)
