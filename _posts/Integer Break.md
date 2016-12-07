---
title: Integer Break.java
date: 
categories: LeetCode
tags: Medium
---
Given a positive integer n, break it into the sum of at least two positive integers and maximize the product of those integers. Return the maximum product you can get.
For example, given n = 2, return 1 (2 = 1 + 1); given n = 10, return 36 (10 = 3 + 3 + 4).
Note: You may assume that n is not less than 2 and not larger than 58.
Hint:
There is a simple O(n) solution to this problem.
You may check the breaking results of n ranging from 7 to 10 to discover the regularities.
<!-- more -->
**思路：**
规律就是尽量分割成3相加。
``` java
public class Solution {
    public int integerBreak(int n) {
        if(n==2) return 1;
        if(n==3) return 2;
        int re = n%3;
		int up = n/3;
		
		if(re == 1){
			up--;
			re = 4;
		}
		if(re == 0){
			re = 1;
		}
		
	    int ans = re * (int)Math.pow(3,up);
		return ans;
    }
}
``` 