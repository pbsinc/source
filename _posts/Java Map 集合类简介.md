---
title: Java Map 集合类简介
date: 
categories: java总结
tags: Map
---
了解最常用的集合类型之一 Map 的基础知识以及如何针对您应用程序特有的数据优化 Map。
java.util 中的集合类包含 Java 中某些最常用的类。最常用的集合类是 List 和 Map。List 的具体实现包括 ArrayList 和 Vector，它们是可变大小的列表，比较适合构建、存储和操作任何类型对象元素列表。List 适用于按数值索引访问元素的情形。
Map 提供了一个更通用的元素存储方法。Map 集合类用于存储元素对（称作“键”和“值”），其中每个键映射到一个值。从概念上而言，您可以将 List 看作是具有数值键的 Map。而实际上，除了 List 和 Map 都在定义 java.util 中外，两者并没有直接的联系。本文将着重介绍核心 Java 发行套件中附带的 Map，同时还将介绍如何采用或实现更适用于您应用程序特定数据的专用 Map。
<!-- more -->
**1.Map 接口和方法**
Java 核心类中有很多预定义的 Map 类。在介绍具体实现之前，我们先介绍一下 Map 接口本身，以便了解所有实现的共同点。Map 接口定义了四种类型的方法，每个 Map 都包含这些方法。下面，我们从两个普通的方法（表 1 ）开始对这些方法加以介绍。
表 1：覆盖的方法。我们将这 Object 的这两个方法覆盖，以正确比较 Map 对象的等价性。
``` java
equals(Object o)	比较指定对象与此 Map 的等价性
hashCode()	返回此 Map 的哈希码
```
**2.Map 构建**
Map 定义了几个用于插入和删除元素的变换方法（表 2 ）。
表 2：Map 更新方法： 可以更改 Map 内容。
``` java
clear()	从 Map 中删除所有映射
remove(Object key)	从 Map 中删除键和关联的值
put(Object key, Object value)	将指定值与指定键相关联
clear()	从 Map 中删除所有映射
putAll(Map t)	将指定 Map 中的所有映射复制到此 map
```
尽管您可能注意到，纵然假设忽略构建一个需要传递给 putAll() 的 Map 的开销，使用 putAll() 通常也并不比使用大量的 put() 调用更有效率，但 putAll() 的存在一点也不稀奇。这是因为，putAll() 除了迭代 put() 所执行的将每个键值对添加到 Map 的算法以外，还需要迭代所传递的 Map 的元素。但应注意，putAll() 在添加所有元素之前可以正确调整 Map 的大小，因此如果您未亲自调整 Map 的大小（我们将对此进行简单介绍），则 putAll() 可能比预期的更有效。
**3.查看 Map**
表 3：返回视图的 Map 方法： 使用这些方法返回的对象，您可以遍历 Map 的元素，还可以删除 Map 中的元素。
``` java
entrySet()	返回 Map 中所包含映射的 Set 视图。Set 中的每个元素都是一个 Map.Entry 对象，可以使用 getKey() 和 getValue() 方法（还有一个 setValue() 方法）访问后者的键元素和值元素
keySet()	返回 Map 中所包含键的 Set 视图。删除 Set 中的元素还将删除 Map 中相应的映射（键和值）
values()	返回 map 中所包含值的 Collection 视图。删除 Collection 中的元素还将删除 Map 中相应的映射（键和值）
```
**4.访问元素**
表 4 中列出了 Map 访问方法。Map 通常适合按键（而非按值）进行访问。Map 定义中没有规定这肯定是真的，但通常您可以期望这是真的。例如，您可以期望 containsKey() 方法与 get() 方法一样快。另一方面，containsValue() 方法很可能需要扫描 Map 中的值，因此它的速度可能比较慢。
表 4：Map 访问和测试方法： 这些方法检索有关 Map 内容的信息但不更改 Map 内容。
``` java
get(Object key)	返回与指定键关联的值
containsKey(Object key)	如果 Map 包含指定键的映射，则返回 true
containsValue(Object value)	如果此 Map 将一个或多个键映射到指定值，则返回 true
isEmpty()	如果 Map 不包含键-值映射，则返回 true
size()	返回 Map 中的键-值映射的数目
```
对使用 containsKey() 和 containsValue() 遍历 HashMap 中所有元素所需时间的测试表明，containsValue() 所需的时间要长很多。实际上要长几个数量级！（参见图 1 和图 2 ，以及随附文件中的 。因此，如果 containsValue() 是应用程序中的性能问题，它将很快显现出来，并可以通过监测您的应用程序轻松地将其识别。这种情况下，我相信您能够想出一个有效的替换方法来实现 containsValue() 提供的等效功能。但如果想不出办法，则一个可行的解决方案是再创建一个 Map，并将第一个 Map 的所有值作为键。这样，第一个 Map 上的 containsValue() 将成为第二个 Map 上更有效的 containsKey()。

**5.核心 Map**
Java 自带了各种 Map 类。这些 Map 类可归为三种类型：
``` java
1.通用 Map，用于在应用程序中管理映射，通常在 java.util 程序包中实现
	HashMap
	Hashtable
	Properties
	LinkedHashMap
	IdentityHashMap
	TreeMap
	WeakHashMap
	ConcurrentHashMap
2.专用 Map，您通常不必亲自创建此类 Map，而是通过某些其他类对其进行访问
	java.util.jar.Attributes
	javax.print.attribute.standard.PrinterStateReasons
	java.security.Provider
	java.awt.RenderingHints
	javax.swing.UIDefaults
3.一个用于帮助实现您自己的 Map 类的抽象类
	AbstractMap
```
**6.选择适当的 Map**
应使用哪种 Map？ 它是否需要同步？ 要获得应用程序的最佳性能，这可能是所面临的两个最重要的问题。当使用通用 Map 时，调整 Map 大小和选择负载因子涵盖了 Map 调整选项。
以下是一个用于获得最佳 Map 性能的简单方法
``` java
1.将您的所有 Map 变量声明为 Map，而不是任何具体实现，即不要声明为 HashMap 或 Hashtable，或任何其他 Map 类实现。
	Map criticalMap = new HashMap(); //好
	HashMap criticalMap = new HashMap(); //差
 
2.这使您能够只更改一行代码即可非常轻松地替换任何特定的 Map 实例。

3.下载 Doug Lea 的 util.concurrent 程序包 (http://gee.cs.oswego.edu/dl/classes/EDU/oswego/cs/dl/util/concurrent/intro.html)。将 ConcurrentHashMap 用作默认 Map。当移植到 1.5 版时，将 java.util.concurrent.ConcurrentHashMap 用作您的默认 Map。不要将 ConcurrentHashMap 包装在同步的包装器中，即使它将用于多个线程。使用默认大小和负载因子。
监测您的应用程序。如果发现某个 Map 造成瓶颈，则分析造成瓶颈的原因，并部分或全部更改该 Map 的以下内容：Map 类；Map 大小；负载因子；关键对象 equals() 方法实现。专用的 Map 的基本上都需要特殊用途的定制 Map 实现，否则通用 Map 将实现您所需的性能目标。
```
不要为您的设计选择任何特定的 Map，除非实际的设计需要指定一个特殊类型的 Map。设计时通常不需要选择具体的 Map 实现。您可能知道自己需要一个 Map，但不知道使用哪种。而这恰恰就是使用 Map 接口的意义所在。直到需要时再选择 Map 实现 — 如果随处使用“Map”声明的变量，则更改应用程序中任何特殊 Map 的 Map 实现只需要更改一行，这是一种开销很少的调整选择。



**最后，更多内容，内部哈希： 哈希映射技术，优化 Hasmap，调整 Map 实现的大小，使用负载因子，同步 Map，本文转自：**
http://www.oracle.com/technetwork/cn/articles/maps1-100947-zhs.html