title: ArcGIS Engine中实现字段的添加
category: wordpress
tags:
- GIS
date: 2010-08-06 04:03:24
---

转载本文，请写明[出处](http://blog.igevin.info/archives/151)：[Gevin的博客](http://blog.igevin.info)

ArcGIS Engine中添加字段比较简单，用接口IFieldsEdit接口和IFieldEdit接口。

添加字段代码实现比较简单，如果上面这两个两个接口不熟悉，查一下AE的帮助文档即可。

下面介绍一个简单的例子：


```csharp
/// &lt;summary&gt;
/// 添加字段
/// &lt;/summary&gt;
/// &lt;param name="pFeatureClass"&gt;需要添加字段的FeatureClass&lt;/param&gt;
/// &lt;param name="fieldName"&gt;添加的字段的名称&lt;/param&gt;
public void AddField(IFeatureClass pFeatureClass, string fieldName)
{
IFields pFields = pFeatureClass.Fields;
IFieldsEdit pFieldsEdit = pFields as IFieldsEdit;
IFieldEdit pFieldEdit;

pFieldEdit = new FieldClass();
pFieldEdit.Name_2 = fieldName;
pFieldEdit.Type_2 = esriFieldType.esriFieldTypeString;
pFieldsEdit.AddField((IField)pFieldEdit);

}
```

**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)

