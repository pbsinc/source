---
title: Minimum Moves to Equal Array Elements II.java
date: 
categories: LeetCode
tags: Medium
---
Given a non-empty integer array, find the minimum number of moves required to make all array elements equal, where a move is incrementing a selected element by 1 or decrementing a selected element by 1.
You may assume the array's length is at most 10,000.
Example:
Input:[1,2,3]
Output:2
Explanation:
Only two moves are needed (remember each move increments or decrements one element):
[1,2,3]  =>  [2,2,3]  =>  [2,2,2]
<!-- more -->
**思路：**
按顺序排序后；
找到数组中间位置的元素作为target；
计算每个元素到此的路程总和即为答案。
``` java
public class Solution {
    public int minMoves2(int[] nums) {
        Arrays.sort(nums);
        int target=0 ,count=0;
		int targetid = (nums.length-1)/2;
		
		target = nums[targetid];
		
		for(int i=0;i<nums.length;i++){
			count += Math.abs(nums[i]-target);
		}
		
		return count;
    }
}
``` 