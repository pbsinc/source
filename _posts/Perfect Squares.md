---
title: Perfect Squares.java
date: 
categories: LeetCode
tags: [Medium,四平方和定理,重做]
---
Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.
For example, given n = 12, return 3 because 12 = 4 + 4 + 4; given n = 13, return 2 because 13 = 4 + 9.
<!-- more -->
**思路：**
转分析：这道题并没有想出来，看到网上提到动态规划算法时，还在想这题怎么动态规划，后来才知道确实是动态规划，
对于要求的当前节点而言都是从前面的节点转移过来的，只是这些转移节点并非一个，而是多个，比如1*1，2*2，3*3，，，那么相应的res[i-1]、res[i-4]、res[i-9]等等都是转移点。
从这些候选项中找到最小的那个，然后加1即可。
``` java
dp[0] = 0 
dp[1] = dp[0]+1 = 1
dp[2] = dp[1]+1 = 2
dp[3] = dp[2]+1 = 3
dp[4] = Min{ dp[4-1*1]+1, dp[4-2*2]+1 } 
      = Min{ dp[3]+1, dp[0]+1 } 
      = 1				
dp[5] = Min{ dp[5-1*1]+1, dp[5-2*2]+1 } 
      = Min{ dp[4]+1, dp[1]+1 } 
      = 2
						.
						.
						.
dp[13] = Min{ dp[13-1*1]+1, dp[13-2*2]+1, dp[13-3*3]+1 } 
       = Min{ dp[12]+1, dp[9]+1, dp[4]+1 } 
       = 2
						.
						.
						.
dp[n] = Min{ dp[n - i*i] + 1 },  n - i*i >=0 && i >= 1
and the sample code is like below:

public int numSquares(int n) {
	int[] dp = new int[n + 1];
	Arrays.fill(dp, Integer.MAX_VALUE);
	dp[0] = 0;
	for(int i = 1; i <= n; ++i) {
		int min = Integer.MAX_VALUE;
		int j = 1;
		while(i - j*j >= 0) {
			min = Math.min(min, dp[i - j*j] + 1);
			++j;
		}
		dp[i] = min;
	}		
	return dp[n];
}
``` 
四平方和定理:
https://zh.wikipedia.org/wiki/%E5%9B%9B%E5%B9%B3%E6%96%B9%E5%92%8C%E5%AE%9A%E7%90%86

http://www.cnblogs.com/grandyang/p/4800552.html
