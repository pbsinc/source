---
title: Majority Element.java
date: 
categories: LeetCode
tags: Easy
---
Given an array of size n, find the majority element. The majority element is the element that appears more than ⌊ n/2 ⌋ times.
You may assume that the array is non-empty and the majority element always exist in the array.
<!-- more -->
**正确思路:**
一个一个比较计数的话，运行时间过长通不过；
所以排序后去中间值肯定对：Arrays.sort(num);  
或者，利用不同就抵消的思想，最后剩下的就是答案：用count记录某个元素出现的次数，如果后面的元素和它相同就加一，不同就减一，当count等于0时换一个新的元素比较。 
``` java
public class Solution {
    public int majorityElement(int[] nums) {
        int ans =nums[0],count=0,len=nums.length;
        if(len == 1)
            return nums[0];
        for(int i=0;i<len-1;i++){
            if(nums[i] == ans) {
                count++;
            }
            else
                count--;
            if(count == 0)
                ans = nums[i+1];
        }
        return ans;
    }
}
```