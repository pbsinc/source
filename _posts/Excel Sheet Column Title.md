---
title: Excel Sheet Column Title.java
date: 
categories: LeetCode
tags: Easy
---
Given a positive integer, return its corresponding column title as appear in an Excel sheet.
For example:

    1 -> A
    2 -> B
    3 -> C
    ...
    26 -> Z
    27 -> AA
    28 -> AB 
<!-- more -->
**思路：**
相当于10进制转26进制。
``` java
public class Solution {
    StringBuilder sb = new StringBuilder();
    
    public String convertToTitle(int n) {
        int up = (n-1)/26;
		int remainder = n%26;
		
		sb.insert(0,calculate(remainder));
		if(up != 0)
			convertToTitle(up);
			
		return sb.toString();	
    }
	public char calculate(int n){
		char[] res = new char[]{'Z','A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y'};
		return res[n];
	}
}
``` 