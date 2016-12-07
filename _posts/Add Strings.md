---
title: Add Strings.java
date: 
categories: LeetCode
tags: Easy
---
Given two non-negative numbers num1 and num2 represented as string, return the sum of num1 and num2.
Note:

	The length of both num1 and num2 is < 5100.
	Both num1 and num2 contains only digits 0-9.
	Both num1 and num2 does not contain any leading zero.
	You must not use any built-in BigInteger library or convert the inputs to integer directly.
<!-- more -->
**思路：**
直接模拟计算机加法及进位原理，用StringBuilder的insert（）方法把每次结果加在最前面。
``` java
public class Solution {
    public String addStrings(String num1, String num2) {
        int up=0;
		StringBuilder sb = new StringBuilder();
		
		int len1 = num1.length();
		int len2 = num2.length();
		
		int len = Math.max(len1,len2);
		
		for(int i=1;i<=len;i++){
			int a =0,b=0;
			
			if(len1 - i >= 0)
				a = num1.charAt(len1-i) - '0';
			if(len2 - i >= 0)
				b = num2.charAt(len2-i) - '0';
			
			int cur = up + a + b;
			
			up = cur / 10;
			cur = cur%10;
			sb.insert(0,String.valueOf(cur));
		}
		if(up != 0)
			sb.insert(0,String.valueOf(up));
		return sb.toString();
    }
}
``` 
