---
title: Power of Two.java
date: 
categories: LeetCode
tags: Easy
---
Given an integer, write a function to determine if it is a power of two.
<!-- more -->
**思路：**
直接一直除二，看最后是否为一，且每次对2取余要为零。
``` java
public class Solution {
    public boolean isPowerOfTwo(int n) {
        if(n == 0) return false;
		if(n == 1) return true;
        if(n%2 == 1) return false;

		return isPowerOfTwo(n/2);
    }
}
``` 
