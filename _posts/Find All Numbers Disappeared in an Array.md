---
title: Find All Numbers Disappeared in an Array.java
date: 
categories: LeetCode
tags: [Easy,List要remove一个int对象]
---
Given an array of integers where 1 ≤ a[i] ≤ n (n = size of array), some elements appear twice and others appear once.
Find all the elements of [1, n] inclusive that do not appear in this array.
Could you do it without extra space and in O(n) runtime? You may assume the returned list does not count as extra space.
Example:
Input:[4,3,2,7,8,2,3,1]
Output:[5,6]
<!-- more -->
**思路：**
把非重复的元素都加入list，遍历1——n，如果list中已经contains，则remove，否则add.
``` java
public class Solution {
    public List<Integer> findDisappearedNumbers(int[] nums) {
        List<Integer> res = new ArrayList<Integer>();
        for(int i=0;i<nums.length;i++){
            if(!res.contains(nums[i]))
                res.add(nums[i]);
        }
        for(int i=1;i<=nums.length;i++){
            if(res.contains(i))
                res.remove(new Integer(i)); //remove(Object i)  |   res.remove(i);  //remove(index) 
            else
                res.add(i);
        }
        return res;
    }
}
```

