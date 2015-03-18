title: "C#生成随机颜色"
category: wordpress
tags:
- GIS
- "C#"
date: 2010-08-13 08:19:28
---

转载[本文](http://blog.igevin.info/archives/230 "C#生成随机颜色")，请表明出处：[Gevin的博客](http://blog.igevin.info/)

C#生成随机颜色，就是一个思路的问题，[本文](http://igevin.info/blog/archives/230 "C#生成随机颜色")介绍一种思路，抛砖引玉了。

生成[RGB](http://baike.baidu.com/view/17423.htm)颜色，其实就是生产3个0～255范围内的随机数，分别代表R,G,和B

生成随机数每种编程语言都会有的，[本文](http://blog.igevin.info/archives/230 "C#生成随机颜色")就不详细介绍了，下面提供一个生成随机深颜色的方法代码：<span id="more-230"></span>

```csharp

/// &lt;summary&gt;
/// 生成随机的尽量深的颜色
/// &lt;/summary&gt;
/// &lt;returns&gt;&lt;/returns&gt;
public static IRgbColor GetRandomDarkColor()
{
    Random RandomNum_First = new Random((int)DateTime.Now.Ticks);
    System.Threading.Thread.Sleep(RandomNum_First.Next(50));

    Random RandomNum_Sencond = new Random((int)DateTime.Now.Ticks);
    System.Threading.Thread.Sleep(RandomNum_Sencond.Next(50));

    Random RandomNum_Third = new Random((int)DateTime.Now.Ticks);
    System.Threading.Thread.Sleep(RandomNum_Third.Next(50));

    //  为了在白色背景上显示，尽量生成深色
    int int_Red = RandomNum_First.Next(256);
    int int_Green = RandomNum_Sencond.Next(256);
    int int_Blue = (int_Red + int_Green &gt; 400) ?
    512 - int_Red - int_Green : int_Red + int_Green;
    int_Blue = (int_Blue &gt; 255) ? 255 : int_Blue;

    IRgbColor pRgbColor = new RgbColorClass();
    pRgbColor.Red = int_Red;
    pRgbColor.Green = int_Green;
    pRgbColor.Blue = int_Blue;

    return pRgbColor;
}
```

[本文](http://blog.igevin.info/archives/230 "C#生成随机颜色")只提供一个思路，由于我对颜色这一方面不是很敏感，可能配的颜色不太美观，欢迎大家多提意见！


**转载请注明：** 转载自[Gevin的博客](http://blog.igevin.info/)
