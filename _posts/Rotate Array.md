---
title: Rotate Array.java
date: 
categories: LeetCode
tags: Easy
---
Rotate an array of n elements to the right by k steps.
For example, with n = 7 and k = 3, the array [1,2,3,4,5,6,7] is rotated to [5,6,7,1,2,3,4].
<!-- more -->
**思路:**
注意一下右移k>len的情况
``` java
public class Solution {
    public void rotate(int[] nums, int k) {
        int[] temp = new int[k];
        int len = nums.length;
        k = k%len;                  //右移k>len的情况
        for(int i=0;i<k;i++)
            temp[i] = nums[len-k+i];
        for(int j=len-1;j>=k;j--)
            nums[j] = nums[j-k];
        for(int i=0;i<k;i++)
            nums[i] = temp[i];
    }
}
```