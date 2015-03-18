title: "C#连接数据库并显示数据库中所有的表"
category: wordpress
tags:
- "C#"
date: 2010-08-01 10:52:47
---

<div id="attachment_1259" style="width: 610px" class="wp-caption aligncenter">[![C#连接数据库并显示数据库中所有的表](http://blog.igevin.info/wp-content/uploads/2010/08/coding.jpg "coding")](http://blog.igevin.info/wp-content/uploads/2010/08/coding.jpg)

C#连接数据库并显示数据库中所有的表
</div>

&nbsp;

# 一、C#连接数据库

C#连接数据库关键是2点，一个是定义恰当的connection变量conn（我喜欢取这样一个变量名），如SqlConnection类型的conn变量或OledbConnection类型的conn变量，二是要写出正确的ConnectionString。一般情况下，如果要连接sql server数据库，就选用SqlConnection类型的conn，否则用OledbConnection类型的conn，odbc类型的就不要考虑了，这个已经很老了，除了一些特定的数据库，用OledbConnection类型的conn几乎可以连接所有的数据库。

<span id="more-117"></span>ConnectionString麻烦一点，所有这里单独一段来介绍。数据库能否连接成功，关键是ConnectionString（string类型的）是否写的正确。只要ConnectionString写对了，用conn.open()方法就可以连接数据库成功。这个ConnectionString，也称为连接字符串，可以去网上查，不同的数据库对应的ConnectionString是什么，[本文](http://blog.igevin.info/archives/117)介绍一种更简单的方法，使用SqlConnectionStringBuilder生成相应的ConnectionString。  废话不多说了，下面请看代码：

<pre>先定义几个全局都要用的变量
</pre>

```csharp
SqlConnection conn = new SqlConnection();
DataTable selectedTable;
List selectedFieldsNames;

private void ConnectDB()
{
    SqlConnectionStringBuilder connStringBuilder = new SqlConnectionStringBuilder();
    connStringBuilder["Data Source"] = "GIS";
    connStringBuilder["database"] = "VehicleDB";
    connStringBuilder["uid"] = "sa";
    connStringBuilder["pwd"] = "seu";

    string connectionString = connStringBuilder.ConnectionString;

    conn.ConnectionString = connectionString;
    try
    {
        conn.Open();
        MessageBox.Show("连接数据库成功");

        List tableNames = getTableNames(conn);
        //getTableNames()显示数据库中所有的表,下面会介绍
        if (tableNames == null)
        return;

        tableNamesComboBox.Items.Clear();
        for (int i = 0; i &lt; tableNames.Count; i++)
        tableNamesComboBox.Items.Add(tableNames[i]);

        tableNamesComboBox.Text = tableNamesComboBox.Items[0].ToString();
    }
    catch(SqlException ex)
    {
        MessageBox.Show(ex.Message);
    }
}
```

有代码，其他的就不解释了

# 二、显示数据库中所有的表

显示数据库中所有的表的方法有好几种，[本文](http://blog.igevin.info/archives/117)介绍比较简单的一种。此方法的核心是构建cmdText(string类型)，cmdText（&amp;&amp;&amp;&amp;&amp;）内容如下：

<span style="font-family: Consolas, Monaco, 'Courier New', Courier, monospace; line-height: 18px; white-space: pre;">SELECT OBJECT_NAME (id)</span>

<pre class="brush:sql">FROM sysobjects

WHERE xtype = 'U' AND OBJECTPROPERTY (id, 'IsMSShipped') = 0</pre>

&nbsp;

&nbsp;

请直接参考下面代码：

```csharp
private List getTableNames(SqlConnection conn)
{
    if (conn.State == ConnectionState.Closed)
        return null;
    List tableNames =new List();
    if(conn==null)
        return null;
    if (conn.State == ConnectionState.Closed)
        conn.Open();

    string cmdText = "&amp;&amp;&amp;&amp;&amp;&amp;&amp;";
    SqlCommand sqlcmd = new SqlCommand(cmdText,conn);
    SqlDataReader namesReader = sqlcmd.ExecuteReader();
    while (namesReader.Read())
    {
        string name = namesReader.GetString(0);
        tableNames.Add(name);
    }
    tableNames.Sort();
    namesReader.Close();
    return tableNames;
}
```


# 三、关闭数据库

数据库操作结束，不要忘记关闭数据库

代码如下：

```csharp
private void CloseDB()
{
    if (conn.State == ConnectionState.Open)
    {
        conn.Close();
        tableNamesComboBox.Items.Clear();
        tableNamesComboBox.Text = "";
        MessageBox.Show("数据库已经关闭！");
    }
    else
        MessageBox.Show("数据库尚未打开！");
}
```

**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)
