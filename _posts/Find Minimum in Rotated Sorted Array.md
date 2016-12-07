---
title: Find Minimum in Rotated Sorted Array.java
date: 
categories: LeetCode
tags: Medium
---
Suppose a sorted array is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
Find the minimum element.
You may assume no duplicate exists in the array.
<!-- more -->
**思路：**
二分法：
分情况讨论，数组可能有以下三种情况：
![这里写图片描述](http://img.blog.csdn.net/20141025161730953?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGppYWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
然后，对每一种情况中，求二分法最小的；
与Search in Rotated Sorted Array思路一样
``` java
public class Solution {
    public int findMin(int[] nums) {
		if(nums.length == 0) return 0;
		if(nums.length == 1) return nums[0];
		int start = 0;
		int end = nums.length-1;
		int ans = 0;
		while(start <= end){
			int mid = (start+end) / 2;
			if(nums[start] <= nums[end]){
				ans = nums[start];
				break;
			}
			if(end-start == 1){
				ans = Math.min(nums[start],nums[end]);
				break;
			}
			if(mid>0 && mid<nums.length-1 && nums[mid]<nums[mid-1] && nums[mid]<nums[mid+1]){
				ans = nums[mid];
				break;
			}
			
			if(nums[mid] < nums[start])
				end = mid-1;
		
			if(nums[mid] > nums[start])
				start = mid+1;
			
		}
		return ans;
	}
}
```

