---
title: Reverse String.java
date: 
categories: LeetCode
tags: Easy
---


Write a function that takes a string as input and returns the string reversed.

Example:
Given s = "hello", return "olleh".
<!-- more -->
**关键：**
String 转 char[] : char a[] = str.toCharArray();
char 转 String ：String str = String.valueOf(char);
String 的长度 : str.length();
String 可直接相加 ： str = stra + strb;


``` java
public class Solution {
    public String reverseString(String s) {

        char a[] = s.toCharArray();
        int len = s.length();
        char b[] = new char[len];
        //String str = new String();
        for(int i=len-1;i>=0;i--){
            // String temp = String.valueOf(a[i]);              这种方法时间效率低，A不过
            // str += temp;
            b[len-1 - i] = a[i];
        }
        String str = new String(b);
        return str;
    }
}
```
