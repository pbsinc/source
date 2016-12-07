---
title: Jump Game.java
date: 
categories: LeetCode
tags: Medium
---
Given an array of non-negative integers, you are initially positioned at the first index of the array.
Each element in the array represents your maximum jump length at that position.
Determine if you are able to reach the last index.
For example:
A = [2,3,1,1,4], return true.
A = [3,2,1,0,4], return false.
<!-- more -->
**思路：**
取到每次跳动可以最远的位置作为新的起点，变量变化比较多，注意。
``` java
public class Solution {
    public boolean canJump(int[] nums) {
		int end = nums.length-1;
		int start = 0;
		int jump = nums[0];
		if(end == 0) return true;
        while(start<=end){
			int temp = start;		
			int max=0;		
			start += jump;
			if(start >= end)
				return true;
			int temp2 = start;
			for(int i=temp+1;i<=temp2;i++){
				if(nums[i]+i >= max){
					max = nums[i]+i;
					jump = nums[i];
					start = i;
				}
			}
			if(jump == 0)
				return false;
		}
		return true;
    }
}
```