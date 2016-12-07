---
title: 3Sum Closest.java
date: 
categories: LeetCode
tags: Medium
---
Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers. You may assume that each input would have exactly one solution.
For example, given array S = {-1 2 1 -4}, and target = 1.
The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
<!-- more -->

**思路：**
升序排列，使之变成非递减数组，调用java自带的Arrays.sort()方法即可。
然后求最接近的数。
``` java
public class Solution {
	int ans = Integer.MAX_VALUE;
    public int threeSumClosest(int[] nums, int target) {
        int len = nums.length;
		Arrays.sort(nums);
		for(int i =0;i<len-2;i++){
			int start = i+1;
			int end = len-1; 
			calculate(nums,nums[i],start,end,target);
			if(ans == 0)
			    break;
		}
		return ans+target;
    }
	public void calculate(int[] nums,int temp,int start,int end,int target){
		while (start < end){
			if(temp + nums[start] + nums[end] - target == 0){
				ans = 0;
				break;
			}
			else if(temp + nums[start] + nums[end] - target > 0){
				if(Math.abs(temp + nums[start] + nums[end] - target) < Math.abs(ans))
					ans = temp + nums[start] + nums[end] - target;
				end--;	
			}
			else {
				if(Math.abs(temp + nums[start] + nums[end] - target) < Math.abs(ans))
					ans = temp + nums[start] + nums[end] - target;
				start++;
			}
		}
	}
}
```