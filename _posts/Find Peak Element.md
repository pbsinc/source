---
title: Find Peak Element.java
date: 
categories: LeetCode
tags: Medium
---
A peak element is an element that is greater than its neighbors.
Given an input array where num[i] ≠ num[i+1], find a peak element and return its index.
The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.
You may imagine that num[-1] = num[n] = -∞.
For example, in array [1, 2, 3, 1], 3 is a peak element and your function should return the index number 2.
<!-- more -->
**思路：**
二分法：
题目要求时间复杂度为O(logN)，logN时间的题目一般都是Heap，二叉树，分治法，二分搜索这些很“二”解法。这题是找特定元素，基本锁定二分搜索法。我们先取中点，由题意可知因为两端都是负无穷，有上坡就必定有一个峰，我们看中点的左右两边大小，如果向左是上坡，就抛弃右边，如果向右是上坡，就抛弃左边。直到两边都小于中间，就是峰了。
``` java
public class Solution {
    public int findPeakElement(int[] nums) {
        int start = 0;
		int end = nums.length-1;
		if(end==0) return 0;
		if(end>0 && nums[0] > nums[1]) return 0;
		if(end>0 && nums[end] > nums[end-1]) return end;
		int ans = 0;
		while(start <= end){
			int mid = (start+end)/2;
			if(start>0 && nums[start] > nums[start-1] && nums[start] > nums[start+1] ) {
				ans = start;
				break;
			}
			if(end<nums.length-1 && nums[end] > nums[end-1] && nums[end] > nums[end+1]) {
				ans = end;
				break;
			}
			if(mid>0 && mid<nums.length-1 && nums[mid-1]<nums[mid] && nums[mid+1]<nums[mid] ) {
				ans = mid;
				break;
			}
			
			if(mid>0 && nums[mid-1]>nums[mid]){
				end = mid-1;
			}else if(mid<nums.length-1 && nums[mid]<nums[mid+1]){
				start = mid+1;
			}
		}
		return ans;
    }
}
```

