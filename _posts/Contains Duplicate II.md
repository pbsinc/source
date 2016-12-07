---
title: Contains Duplicate II.java
date: 
categories: LeetCode
tags: Easy
---
Given an array of integers and an integer k, find out whether there are two distinct indices i and j in the array such that nums[i] = nums[j] and the difference between i and j is at most k.
<!-- more -->

**我的思路:**
常规做法是超时的。
``` java
public class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        int len = nums.length;
        for(int i=0;i<len-1;i++){
            for(int j=1;j<=k;j++){
                if(nums[i]==nums[i+j])
                    return true;
                if(i+j == len-1)
                    break;
            }
        }
        return false;
    }
}
```
**正确思路:**
思路分析：这题比较简单，可以直接定义一个长度最大为k的滑动窗口，用一个set维护窗口内的数字判断是否出现重复，使用两个指针start和end标记滑动窗口的两端，初始都是0，然后end不断进行扩展，扫描元素判断是否出现重复元素，直到发现end-start>k, 就开始移动start，并且在set中移除对应的元素。如果以为扫描到数组末尾还没有发现重复元素，那就可以返回false。时间复杂度和空间复杂度都是O（N）。
``` java
public class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        Set<Integer> temp = new HashSet<Integer>();     
        int start=0,end=0;
        for(int i=0;i<nums.length;i++){
            if(temp.contains(nums[i]))
                return true;
            else{
                temp.add(nums[i]);
                end++;
            }
            if(end-start>k){
                temp.remove(nums[start]);
                start++;
            }
        }
        return false;
    }
}
```