---
title: Two Sum II - Input array is sorted.java
date: 
categories: LeetCode
tags: Medium
---
Given an array of integers that is already sorted in ascending order, find two numbers such that they add up to a specific target number.
The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.
You may assume that each input would have exactly one solution.
Input: numbers={2, 7, 11, 15}, target=9
Output: index1=1, index2=2
<!-- more -->
**思路：**
与前面的3sum思路一样，此类问题都是：
1.对数组排序；
2.起点终点，两个指针；
3.两个指针的值加起来与target比较：大了终点左移，小了起点右移。
``` java
public class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int start = 0;
		int end = numbers.length-1;
		int[] ans = new int[2];
		while(start<end){
			if(numbers[start] + numbers[end] == target){
				ans[0] = start+1;
				ans[1] = end+1;
				break;
			}else if(numbers[start] + numbers[end] > target)
				end--;
			else
				start++;
		}
		return ans;
    }
}
```

