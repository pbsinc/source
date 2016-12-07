---
title: java中length,length(),size()区别
date: 
categories: java总结
tags: java中length,length(),size()区别
---
1. Java中的**length**属性是针对数组说的,比如说你声明了一个数组,想知道这个数组的长度则用到了length这个属性.

2. java中的**length()**方法是针对字符串String说的,如果想看这个字符串的长度则用到length()这个方法.

3. java中的**size()**方法是针对泛型集合说的,如果想看这个泛型有多少个元素,就调用此方法来查看!
<!-- more -->
这个例子来演示这两个方法和一个属性的用法
``` java
 public static void main(String[] args) {
        String []list={"ma","cao","yuan"};
        String a="macaoyuan";
        System.out.println(list.length);
        System.out.println(a.length());

        List<Object> array=new ArrayList();
        array.add(a);
        System.out.println(array.size());
    }
```
输出的值为:
3
9
1