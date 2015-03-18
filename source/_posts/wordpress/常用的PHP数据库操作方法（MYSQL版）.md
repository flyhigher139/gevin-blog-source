title: 常用的PHP数据库操作方法（MYSQL版）
category: wordpress
tags:
- mysql
- PHP
date: 2011-02-09 00:01:03
---

[![Gevin的空间](http://public.bay.livefilestore.com/y1pkUB2z4Oi2iUJIBpI4Wka8oM5K42o26_mBs9ciR9aRu8QvSIJs94Zzb6aKukSwoBwb-SPjnMVE6ZKdFt6KqY1uA/PHP-Programmers-Wanted.jpg?psid=1 "PHP help")](http://igevin.info)

Gevin最近一直在折腾自己的网站首页——[Gevin的空间](http://igevin.info/)，写的大部分PHP脚本都要用到和MYSQL数据库相关的操作，今天把这些方法和大家分享一下，希望大家能多多交流!

## 一、数据库操作

### 1\. 连接MYSQL数据

**mysql_connect()**

e.g.

> $db = mysql_connect(MYSQL_HOST, MYSQL_USER, MYSQL_PASSWORD) or die(&#8216;Unable to connect, please check connection paremeters&#8217;);

### <span id="more-990"></span>2\. 选择数据库

**mysql_select_db()**

连接上数据库后，PHP默认选择的数据库未必是我们后面操作中需要的数据库，为确保数据库选择正确，一般在数据库连接语句后面还要加上数据库选择语句。

e.g.

> mysql_select_db(MYSQL_DB, $db) or die(mysql_error($db));

### 3\. 执行SQL语句

mysql_query()

该函数将SQL语句发送到当前活动的数据库并执行语句，返回结果。

e.g.

> $query = &#8220;SELECT * FROM $table&#8221;
>
> $result = mysql_query($query, $db) or die(mysql_error($db));

### 4\. 关闭数据库

**mysql_close()**

该函数用于关闭不需要继续活跃的数据库，但该方法不是必须的，一般PHP会自动关闭不继续活跃的数据库。

e.g.

> mysql_close($db);

### 5\. 释放SQL结果

**mysql_free_result()**

该函数用于释放mysql_query()执行结果占用的内存，该函数很少被调用，除非result很大，占用太多内存；一般在PHP脚本执行结束之后很自动释放占用的内存。

## 二、SQL执行结果操作

### 1\. 返回执行结果中的一行

**mysql_fetch_row()**

返回执行结果的当前行的数值数组，执行这个函数后，结果指向下一行。

e.g.

> $row = mysql_fetch_row($result);

处理执行结果一般放在while循环中，遍历每一行

e.g.

> while($row = mysql_fetch_row($result))
>
> {……}

### 2\. mysql_fetch_row()的替代方法

**mysql_fetch_array()**

**mysql_fetch_assoc()**

mysql_fetch_array()返回键值对数组，键为查询的table的列名；

mysql_fetch_assoc()返回结果时可以先排序（如果为可选参数赋值），相当于mysql_fetch_array()+MYSQL_ASSOC

### 3\. 执行结果的字段（列）属性

**mysql_fetch_field()**

### 4\. 查询数据库中的表名

**mysql_list_tables()**

e.g.

> $db_name = MYSQL_DB;
>
> $result = mysql_list_tables($db_name);
>
> echo &#8220;数据库中包含如下表：&#8221;;
>
> while ($row = mysql_fetch_row($result))
>
> {
>
> echo $row[0];
>
> }

### 5\. 查询数据库的列名（字段名）

**mysql_list_fields()**

e.g.

> $fields = mysql_list_fields($db_name,$table);
>
> $columns = mysql_num_fields($fields);
>
> for ($i = 0; $i &lt; $columns; $i++)
>
> echo  mysql_field_name($fields, $i);

## 三、其他函数

**1\. mysql_num_rows()**

返回执行结果的行数。

e.g.

> $num = mysql_num_rows($result);

**2\. mysql_num_fields()**

返回执行结果的列数（字段数）。

e.g. $num = mysql_num_fields($result);

**3.mysql_set_charset()**

设置执行结果的编码，防止在网页中显示中文时乱码。

e.g.

> $query = &#8220;select * from $table_name&#8221;;
>
> mysql_query(&#8216;set names utf8&#8242;);
>
> $result = mysql_query($query, $db) or die(mysql_error($db));

## 注：

1\. 文中大写代码为预定义的内容，如define(MYSQL_HOST,  &#8216;localhost&#8217;);

2\. 本文仅总结了PHP操作数据库的主要函数，完整的内容请参考PHP手册的[相关内容](http://www.php.net/manual/en/book.mysql.php)。

如转载本文，请表明出处。
<div style="margin-top: 15px">
<p>**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)

**本文链接地址:** [常用的PHP数据库操作方法（MYSQL版）](http://blog.igevin.info/2011/02/php-mysql/)

</div>
<div>
极客趣玩推荐：[![](http://blog.igevin.info/wp-content/uploads/2012/05/image.png)](http://s.click.taobao.com/t_8?e=7HZ5x%2BOzdZF6pq3H3z1mawcVRxM%3D&#038;p=mm_17737402_0_0)
</div>
