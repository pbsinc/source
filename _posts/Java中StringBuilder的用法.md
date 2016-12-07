---
title: Java中StringBuilder的用法
date: 
categories: java总结
tags: Java中StringBuilder的用法
---
String对象是不可改变的。每次使用 System.String类中的方法之一时，都要在内存中创建一个新的字符串对象，这就需要为该新对象分配新的空间。
在需要对字符串执行重复修改的情况下，与创建新的 String对象相关的系统开销可能会非常昂贵。
如果要修改字符串而不创建新的对象，则可以使用System.Text.StringBuilder类。
例如，当在一个循环中将许多字符串连接在一起时，使用 StringBuilder类可以提升性能。
通过用一个重载的构造函数方法初始化变量，可以创建 StringBuilder类的新实例，正如以下示例中所阐释的那样。
<!-- more -->

``` java
StringBuilder MyStringBuilder = new StringBuilder("Hello World!");
```

**1.设置容量和长度**
虽然 StringBuilder对象是动态对象，允许扩充它所封装的字符串中字符的数量，但是您可以为它可容纳的最大字符数指定一个值。
此值称为该对象的容量，不应将它与当前 StringBuilder对象容纳的字符串长度混淆在一起。
例如，可以创建 StringBuilder类的带有字符串“Hello”（长度为 5）的一个新实例，同时可以指定该对象的最大容量为 25。
当修改 StringBuilder时，在达到容量之前，它不会为其自己重新分配空间。
当达到容量时，将自动分配新的空间且容量翻倍。可以使用重载的构造函数之一来指定 StringBuilder类的容量。
以下代码示例指定可以将 MyStringBuilder对象扩充到最大 25个空白。

``` java
StringBuilderMyStringBuilder = new StringBuilder("Hello World!", 25);
```

另外，可以使用读/写 Capacity属性来设置对象的最大长度。
以下代码示例使用 Capacity属性来定义对象的最大长度。
MyStringBuilder.Capacity= 25;

**2.下面列出了此类的几个常用方法：**
**(1)append 方法**可用来将文本或对象的字符串表示形式添加到由当前 StringBuilder对象表示的字符串的结尾处。
以下示例将一个 StringBuilder对象初始化为“Hello World”，然后将一些文本追加到该对象的结尾处。将根据需要自动分配空间。
``` java
StringBuilderMyStringBuilder = new StringBuilder("Hello World!");
MyStringBuilder.append(" What a beautiful day.");
Console.WriteLine(MyStringBuilder);
```
此示例将 Hello World! What abeautiful day.显示到控制台。

**(2)appendFormat 方法**将文本添加到 StringBuilder的结尾处，而且实现了 IFormattable接口，因此可接受格式化部分中描述的标准格式字符串。
可以使用此方法来自定义变量的格式并将这些值追加到 StringBuilder的后面。
以下示例使用 AppendFormat方法将一个设置为货币值格式的整数值放置到 StringBuilder的结尾。
``` java
int MyInt= 25;
StringBuilder MyStringBuilder = new StringBuilder("Your total is ");
MyStringBuilder.appendFormat("{0:C} ", MyInt);
Console.WriteLine(MyStringBuilder);
```
此示例将 Your total is $25.00显示到控制台。

**(3)insert 方法**将字符串或对象添加到当前 StringBuilder中的指定位置。
以下示例使用此方法将一个单词插入到 StringBuilder的第六个位置。
``` java
StringBuilderMyStringBuilder = new StringBuilder("Hello World!");
MyStringBuilder.insert(6,"Beautiful ");
Console.WriteLine(MyStringBuilder);
```
此示例将 Hello BeautifulWorld!显示到控制台。

**(4)remove 方法**从当前 StringBuilder中移除指定数量的字符，移除过程从指定的从零开始的索引处开始。
以下示例使用 Remove方法缩短 StringBuilder。
``` java
StringBuilderMyStringBuilder = new StringBuilder("Hello World!");
MyStringBuilder.remove(5,7);
Console.WriteLine(MyStringBuilder);
```
此示例将 Hello显示到控制台。

**(5)replace 方法**可以用另一个指定的字符来替换 StringBuilder对象内的字符。
以下示例使用 Replace方法来搜索 StringBuilder对象，查找所有的感叹号字符 (!)，并用问号字符 (?)来替换它们。
``` java
StringBuilderMyStringBuilder = new StringBuilder("Hello World!");
MyStringBuilder.replace('!', '?');
Console.WriteLine(MyStringBuilder);
```
此示例将 Hello World?显示到控制台

**(6)toString() 方法** 将其转换为String类型。

**(7)delete(int start, int end) 方法** 将其转换为String类型。在这个序列的一个子字符串，删除start到end-1的字符.
``` java
	StringBuilder str = new StringBuilder("Java lang package");
    System.out.println("string = " + str);
 
    // deleting characters from index 4 to index 9
    str.delete(4, 9);
    System.out.println("After deletion = " + str);
```
这将产生以下结果：
string = Java lang package
After deletion = Java package

**(8)capacity() ，length() 方法** 
StringBuilder.capacity()：获取或设置可包含在当前实例所分配的内存中的最大字符数。
StringBuilder.length()：获取或设置当前 StringBuilder 对象的长度。

``` java
class Program
    {
        static void Main(string[] args)
        {
            StringBuilder sb = new StringBuilder("0123456789");//长度为10
            Console.WriteLine("sb.length():" + sb.length() + " sb.capacity():" + sb.capacity());
            Console.ReadKey();
        }
    }
```

输出结果为：10 16
这里可以说明StringBuilder的Capacity最小分配的长度是16。
当初始化一个长度为17的字符串时，如StringBuilder sb = new StringBuilder("01234567891234567")，显示的是17。
附：
1.超出了这里已经指定的Capacity的值15，Capacity成倍增长到30。
2.当Capacity小于Length时，会抛出异常System.ArgumentOutOfRangeException。
http://www.cnblogs.com/mszhangxuefei/archive/2013/01/09/csharp-6.html

**(9)reverse()方法** 
将序列顺序反向，原来是1---9，现在是9---1。

**(9)deleteCharAt(int index)方法** 
删除此序列中指定位置的字符。

**StringBuilder (Java Platform SE 8 )**
http://docs.oracle.com/javase/8/docs/api/java/lang/StringBuilder.html

**附注：**

     如果程序对附加字符串的需求很频繁，不建议使用+来进行字符串的串联。
	 可以考虑使用java.lang.StringBuilder 类，使用这个类所产生的对象默认会有16个字符的长度，您也可以自行指定初始长度。
	 如果附加的字符超出可容纳的长度，则StringBuilder 对象会自动增加长度以容纳被附加的字符。
	 如果有频繁作字符串附加的需求，使用StringBuilder 类能使效率大大提高。

StringBuilder 是j2se1.5.0才新增的类，在此之前的版本若有相同的需求，则使用java.util.StringBuffer。
事实上StringBuilder 被设计为与StringBuffer具有相同的操作接口。
在单机非线程(MultiThread)的情况下使用StringBuilder 会有较好的效率，因为StringBuilder 没有处理同步的问题。
**StringBuffer则会处理同步问题，如果StringBuilder 会有多线程下被操作，则要改用StringBuffer，让对象自行管理同步问题。**


