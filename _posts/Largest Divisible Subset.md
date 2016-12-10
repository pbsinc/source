---
title: Largest Divisible Subset.java
date: 
categories: LeetCode
tags: [Medium,没做]
---
Given a set of distinct positive integers, find the largest subset such that every pair (Si, Sj) of elements in this subset satisfies: Si % Sj = 0 or Sj % Si = 0.
If there are multiple solutions, return any subset is fine.
Example 1:

	nums: [1,2,3]
	Result: [1,2] (of course, [1,3] will also be ok)
Example 2:

	nums: [1,2,4,8]
	Result: [1,2,4,8]
<!-- more -->
**思路：**
[1,3,9,18,54,90,108,180,360,540,720]
``` java
public class Solution {
    public List<Integer> largestDivisibleSubset(int[] nums) {
        List<Integer> result = new ArrayList();
        if (nums == null || nums.length == 0) return result;
        
        Arrays.sort(nums);
        int len = nums.length;
        int[] parent = new int[len];
        int[] count = new int[len];
        int max = 0, maxIndex = -1;
        //对于i从后往前看，找出每一个可以被它整除的数的数组，并更新它作为从这里开始，往后最大的subset，记录下最大数组开始的地方，并把下一个数记在parent里
        for (int i = len - 1; i >= 0; i--) {
            for (int j = i; j < len; j++) {
                if (nums[j] % nums[i] == 0 && count[i] < count[j] + 1) {
                    count[i] = count[j] + 1;
                    parent[i] = j;
                    if (count[i] > max) {
                        max = count[i];
                        maxIndex = i;
                    }
                }
            }
        }
        
        for (int i = 0; i < max; i++) {
            //找出最长的这个数组中的每一个数
            result.add(nums[maxIndex]);
            maxIndex = parent[maxIndex];
        }
        return result;
    }
}
``` 

https://segmentfault.com/a/1190000005922634
