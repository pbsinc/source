---
title: Minimum Size Subarray Sum.java
date: 
categories: LeetCode
tags: Medium
---
Given an array of n positive integers and a positive integer s, find the minimal length of a subarray of which the sum ≥ s. If there isn't one, return 0 instead.
For example, given the array [2,3,1,2,4,3] and s = 7,
the subarray [4,3] has the minimal length under the problem constraint.
More practice:
If you have figured out the O(n) solution, try coding another solution of which the time complexity is O(n log n).
<!-- more -->
**思路：**
双指针法，维护一个动态窗口，保证窗口内sum>=s即可，每次取窗口最小长度为最终结果。
窗口左边为start，右边为i。
``` java
public class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int len = nums.length;
		if(len == 0) return 0;
		int start = 0;
		int sum = 0;
		int min = Integer.MAX_VALUE;
		for(int i=0;i<len;i++){
			sum += nums[i];
			while(sum  >= s){  //尝试将窗口左边不断右移
			    min = Math.min(min,i-start+1);
				sum -= nums[start];
				start++;
			}
		}
		if(min == Integer.MAX_VALUE) return 0;
		else return min;
    }
}
```
或者：
``` java
public class Solution {
    public int minSubArrayLen(int s, int[] nums) {
        int len = nums.length;
		if(nums == null || len == 0) return 0;

		int start = 0 , end = 0;
		int sum = 0;
		int min = Integer.MAX_VALUE;
		while(start<=end && end<len && sum < s){  //一个意思，for换了种写法
			sum += nums[end];
			
			while(start<=end && sum >= s){
				min = Math.min(min,end-start+1);
				sum -= nums[start];
				start++;
			}
			end++;
		}
		if(min == Integer.MAX_VALUE) return 0;
		else return min;
    }
}
```
