---
title: Arranging Coins.java
date: 
categories: LeetCode
tags: Easy
---
You have a total of n coins that you want to form in a staircase shape, where every k-th row must have exactly k coins.
Given n, find the total number of full staircase rows that can be formed.
n is a non-negative integer and fits within the range of a 32-bit signed integer.

Example 1:

	n = 5

	The coins can form the following rows:
	¤
	¤ ¤
	¤ ¤

	Because the 3rd row is incomplete, we return 2.
Example 2:

	n = 8

	The coins can form the following rows:
	¤
	¤ ¤
	¤ ¤ ¤
	¤ ¤

	Because the 4th row is incomplete, we return 3.
<!-- more -->
**思路：**
转化成求一元二次方程的解；
``` java
public class Solution {
    public int arrangeCoins(int n) {
        double del = Math.sqrt(1.0+8.0*(double)n); // 注意这里不转double的话，int会越界！
        
        int ans = (int)((del-1.0)/2.0);
        return ans; 
    }
}

``` 
