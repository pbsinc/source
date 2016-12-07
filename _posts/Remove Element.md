---
title: Remove Element.java
date: 
categories: LeetCode
tags: Easy
---
Given an array and a value, remove all instances of that value in place and return the new length.
Do not allocate extra space for another array, you must do this in place with constant memory.
The order of elements can be changed. It doesn't matter what you leave beyond the new length.
Example:
Given input array nums = [3,2,2,3], val = 3
Your function should return length = 2, with the first two elements of nums being 2.
<!-- more -->
**思路：**
把正序检测到的重复的数与逆序检测到的非重复的数交换。

``` java
public class Solution {
    public int removeElement(int[] nums, int val) {
        int len = nums.length,count=len;
        for(int i=0;i<len;i++){
            if(nums[i]==val)
                count--;
        }
        for(int i=0;i<len-1;i++){
            if(nums[i]==val){
                for(int j=len-1;j>i;j--){
                    if(nums[j]!=val){
                        nums[i]=nums[j];
                        nums[j]=val;
                        break;
                    }
                }
            }
        }
        return count;
    }
}
```