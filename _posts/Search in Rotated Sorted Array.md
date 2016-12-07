---
title: Search in Rotated Sorted Array.java
date: 
categories: LeetCode
tags: Hard
---
Suppose a sorted array is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
You are given a target value to search. If found in the array return its index, otherwise return -1.
You may assume no duplicate exists in the array.
<!-- more -->
**思路：**
二分法：
分情况讨论，数组可能有以下三种情况：
![这里写图片描述](http://img.blog.csdn.net/20141025161730953?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbGppYWJpbg==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)
然后，再看每一种情况中，target在左边还是在右边，其中第一种情况还可以直接判断target有可能不在数组范围内。
``` java
public class Solution {
    public int search(int[] nums, int target) {
		int left = 0,right = nums.length-1;
		
		while(left <= right){
		    int mid = (left+right)/2;
		    
			if(nums[mid] == target)
				return mid;
		    if(nums[left] < nums[right]){
			    if(target < nums[mid])
			        right = mid-1;
			    else
			        left = mid+1;
			}else if(nums[mid] >= nums[left]){       //注意等号 ?
				if(nums[mid] > target && target > nums[right]) //注意要大于 left 而不是right，因为可以取到right
					right = mid-1;
				else
					left = mid+1;
			}else{
				if(nums[left] > target && target > nums[mid])
					left = mid+1;
				else
					right = mid-1;
			}
		}
        
        return -1;
    }
}
```