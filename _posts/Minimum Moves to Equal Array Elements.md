---
title: Minimum Moves to Equal Array Elements.java
date: 
categories: LeetCode
tags: Easy
---
Given a non-empty integer array of size n, find the minimum number of moves required to make all array elements equal, where a move is incrementing n - 1 elements by 1.
Example:
Input:[1,2,3]
Output:3
Explanation:
Only three moves are needed (remember each move increments two elements):
[1,2,3]  =>  [2,3,3]  =>  [3,4,3]  =>  [4,4,4]
<!-- more -->
**思路：**
给n-1个数加1，也就是给一个数（最大数）减1；
总共次数因此便等于每个数要减1的次数之和；
知道所有数等于最小数，即每个数与最小数做差。
``` java
public class Solution {
    public int minMoves(int[] nums) {
        int count = 0;
		int min = nums[0];

		for(int i=0;i<nums.length;i++){
			min = Math.min(min,nums[i]);
		}
	
		for(int i=0;i<nums.length;i++){
			count += nums[i] - min;
		}
		return count;
    }
}
``` 