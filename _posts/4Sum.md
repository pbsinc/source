---
title: 4Sum.java
date: 
categories: LeetCode
tags: Medium
---
Given an array S of n integers, are there elements a, b, c, and d in S such that a + b + c + d = target? Find all unique quadruplets in the array which gives the sum of target.
Note: The solution set must not contain duplicate quadruplets.
<!-- more -->
For example, given array S = [1, 0, -1, 0, -2, 2], and target = 0.
A solution set is:
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]
**思路：**
升序排列，使之变成非递减数组，调用java自带的Arrays.sort()方法即可。
与3sum一脉相承，不过要注意前两个数中重复的数，跳过。
``` java
public class Solution {
	List<List<Integer>> ans = new ArrayList<List<Integer>>();
	int a1,b1;
    public List<List<Integer>> fourSum(int[] nums, int target) {
		Arrays.sort(nums);
		for(int i=0;i<nums.length-3;i++){
			for(int j=i+1;j<nums.length-2;j++){
				calculate(nums,i,j,j+1,nums.length-1,target);
				i=a1;
				j=b1;
			}
		}
		return ans;
    }
	public void calculate(int[] nums,int a,int b, int start,int end,int target){
		while(start < end){
			if(nums[a]+nums[b]+nums[start]+nums[end] == target){
				List<Integer> temp = new ArrayList<Integer>();
				temp.add(nums[a]);
				temp.add(nums[b]);
				temp.add(nums[start]);
				temp.add(nums[end]);
				ans.add(temp);
				while(start < end && nums[end] == nums[end - 1])
					end--;
				end--;
				while(start < end && nums[start] == nums[start + 1])
					start++;
				start++;
				while(a<nums.length-3 && nums[a]==nums[a+1])    //注意重复的数，跳过
					a++;
				while(b<nums.length-2 && nums[b]==nums[b+1])
					b++;
				
			}else if(nums[a]+nums[b]+nums[start]+nums[end] > target){
				while(start < end && nums[end] == nums[end - 1])
					end--;
				end--;
			}else{
				while(start < end && nums[start] == nums[start + 1])
					start++;
				start++;				
			}
			a1=a;
			b1=b;
		}
	}
}
```