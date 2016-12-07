---
title: Combination Sum III.java
date: 
categories: LeetCode
tags: Medium
---
Find all possible combinations of k numbers that add up to a number n, given that only numbers from 1 to 9 can be used and each combination should be a unique set of numbers.
Example 1:
Input: k = 3, n = 7
Output:[[1,2,4]]
Example 2:
Input: k = 3, n = 9
Output:[[1,2,6], [1,3,5], [2,3,4]]
<!-- more -->
**思路：**
与Combination SumII一样，只是判断一下组合中元素个数是否为k即可;

``` java
public class Solution {
    List<List<Integer>> ans = new ArrayList<List<Integer>>();
	List<Integer> temp = new ArrayList<Integer>();
    public List<List<Integer>> combinationSum3(int k, int target) {
		int[] nums = new int[]{1,2,3,4,5,6,7,8,9};
		calculate(nums,target,0,0,k);
		return ans;
    }
	public void calculate(int[] nums,int target,int sum,int level,int k){
		if(sum > target)
			return;
		else if(sum == target && temp.size() == k){        // 判断组合中元素个数是否为k
			List<Integer> t = new ArrayList<Integer>(temp); 
			ans.add(t);                                     
			//如直接ans.add(temp);实际上是重复将同一个对象temp往ans里面传，错误。
			return;
		}else{             // DFS
			for(int i=level;i<nums.length;i++){
				sum += nums[i];
				temp.add(nums[i]);
				calculate(nums,target,sum,i+1,k);   //数字不能重复使用
				temp.remove(temp.size()-1);
				sum -= nums[i];
				while(i<nums.length-1 && nums[i+1]==nums[i])  //去除重复的组合
					i++;  
			}
		}
	}
}
```