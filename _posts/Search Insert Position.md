---
title: Search Insert Position.java
date: 
categories: LeetCode
tags: Medium
---
Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
You may assume no duplicates in the array.
Here are few examples.
[1,3,5,6], 5 → 2
[1,3,5,6], 2 → 1
[1,3,5,6], 7 → 4
[1,3,5,6], 0 → 0
<!-- more -->
**思路：**
左右比大小就好

``` java
public class Solution {
    public int searchInsert(int[] nums, int target) {
        int len = nums.length;
		int i = 0;
		while(i<len){
			if(nums[i] == target || nums[0]>target)
				return i;
			else if(i+1<len && nums[i]<target && target<nums[i+1])
				return i+1;
			else 
				i++;
		}
		return i;
    }
}
```