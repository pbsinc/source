---
title: java判断字符串String是否相等
date: 
categories: java总结
tags: java判断字符串String是否相等
---
String 的.equals("xxx") 方法用于比较两个字符串是否相等。
由于字符串是对象类型，所以不能用简单的“==”判断。而使用equals比较两个对象的内容是否相等。
注意：
.equals("xxx")比较的是对象的内容（区分字母的大小写格式），但是如果使用“==”比较两个对象时，比较的是两个对象的内存地址，所以不相等。
即使它们内容相等，但是不同对象的内存地址也是不相同的。
<!-- more -->
