---
title: Find All Duplicates in an Array.java
date: 
categories: LeetCode
tags: Medium
---
Given an array of integers, 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.
Find all the elements that appear twice in this array.
Could you do it without extra space and in O(n) runtime?
Example:
Input:[4,3,2,7,8,2,3,1]
Output:[2,3]
<!-- more -->
**思路：**
给sum排序,将重复的取出来
``` java
public class Solution {
    public List<Integer> findDuplicates(int[] nums) {
		List<Integer> res = new ArrayList<Integer>();
		Arrays.sort(nums);
        for(int i=0;i<nums.length;i++){
            if(i<nums.length-1 && nums[i] == nums[i+1]){
				res.add(nums[i]);
				while(i<nums.length-1 && nums[i] == nums[i+1])
				    i++;
            }
        }
        return res;
    }
}
```
