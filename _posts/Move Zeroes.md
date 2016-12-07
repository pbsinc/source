---
title: Move Zeroes.java
date: 
categories: LeetCode
tags: Easy
---
Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.
For example, given nums = [0, 1, 0, 3, 12], after calling your function, nums should be [1, 3, 12, 0, 0].
Note:
You must do this in-place without making a copy of the array.
Minimize the total number of operations.
<!-- more -->

**思路:**
循环一遍，遇0且后一位非0时，左移一位。若后一位仍为0，记录0的个数n，将来遇到非0值左移n位。
``` java
public class Solution {
    public void moveZeroes(int[] nums) {
        int n =1;
        for(int i=0;i<nums.length-1;i++){
            if(nums[i]==0){
                if(nums[i+1]!=0){
                    nums[i-n+1]=nums[i+1];
                    nums[i+1]=0;
                }
                else
                    n++;
            }
        }
    }
}
```
