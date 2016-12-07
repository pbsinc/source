---
title: List,Set,Map,数组间的相互转换
date: 
categories: java总结
tags: List,Set,Map,数组间的相互转换
---
转自：
http://teamojiao.iteye.com/blog/436139
<!-- more -->
**1.list转set**
``` java
Set set = new HashSet(new ArrayList());    
```
**2.set转list**
``` java
List list = new ArrayList(new HashSet());    
```
**3.数组转为list**
``` java
List stooges = Arrays.asList("Larry", "Moe", "Curly");    
此时stooges中有有三个元素。 
```
**4.数组转为set **
int[] a = { 1, 2, 3 };  
``` java
Set set = new HashSet(Arrays.asList(a));    
```
**5.map的相关操作。**
``` java
Map map = new HashMap();     
map.put("1", "a");     
map.put('2', 'b');     
map.put('3', 'c');     
System.out.println(map);     
// 输出所有的值     
System.out.println(map.keySet());     
// 输出所有的键     
System.out.println(map.values());     
// 将map的值转化为List     
List list = new ArrayList(map.values());     
System.out.println(list);     
// 将map的值转化为Set     
Set set = new HashSet(map.values());     
System.out.println(set);    
```
**6.list转数组**
``` java
List list = Arrays.asList("a","b");     
System.out.println(list);     
             
String[] arr = (String[])list.toArray(new String[list.size()]);     
System.out.println(Arrays.toString(arr));    
```
