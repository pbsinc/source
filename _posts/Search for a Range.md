---
title: Search for a Range.java
date: 
categories: LeetCode
tags: Medium
---
Given a sorted array of integers, find the starting and ending position of a given target value.
Your algorithm's runtime complexity must be in the order of O(log n).
If the target is not found in the array, return [-1, -1].
For example,
Given [5, 7, 7, 8, 8, 10] and target value 8,
return [3, 4].
<!-- more -->
**思路：**
类似二分查找，但等于目标值时需要向左向右对其相邻元素进行判断。
``` java
public class Solution {
    public int[] searchRange(int[] nums, int target) {
        int len = nums.length;
		int i = 0;
		int[] ans = new int[2];
		int start = -1, end = -1;
		while(len != i && i>=0){
			if(nums[i] == target){
				start = i;
				end = i;
				while(start-1>=0 && nums[start-1] == target)
					start--;
				while(end+1<len && nums[end+1] == target)
					end++;
				break;
			}else if(nums[i] > target){
				int temp = i;
				if(len-i == 1)
				    i--;
				else
				    i = i - (len-i)/2;
				len = temp;
			}else
				i = len - (len-i)/2;
		}
		ans[0] = start;
		ans[1] = end;
		return ans;
    }
}
```