---
title: Excel Sheet Column Number.java
date: 
categories: LeetCode
tags: Easy
---
Related to question Excel Sheet Column Title
Given a column title as appear in an Excel sheet, return its corresponding column number.
For example:

    A -> 1
    B -> 2
    C -> 3
    ...
    Z -> 26
    AA -> 27
    AB -> 28 
<!-- more -->
**思路：**
相当于26进制转10进制。
``` java
public class Solution {
    public int titleToNumber(String s) {
        int num = 0;
		for(int i=0;i<s.length();i++){
			int charint = s.charAt(i) - 'A' + 1;
			num = num*26 + charint;
		}
		return num;
    }
}
``` 