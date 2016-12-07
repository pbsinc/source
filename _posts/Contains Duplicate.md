---
title: Contains Duplicate.java
date: 
categories: LeetCode
tags: Easy
---
Given an array of integers, find if the array contains any duplicates. Your function should return true if any value appears at least twice in the array, and it should return false if every element is distinct.
<!-- more -->

**思路:**
使用set接口的方法,常规做法是超时的。
``` java
public class Solution {
    public boolean containsDuplicate(int[] nums) {
        Set<Integer> temp = new HashSet<Integer>();
        for(int x : nums){                            //------------ for循环，遍历nums所有项的简便方法，注意：x为nums[]的元素！
            if(temp.contains(x))
                return true;
            else
                temp.add(x);
        }
        return false;
    }
}
```

**set:**
http://blog.csdn.net/vegetable_bird_001/article/details/50992925