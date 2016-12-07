---
title: Combination Sum II.java
date: 
categories: LeetCode
tags: Medium
---
Given a collection of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.
Each number in C may only be used once in the combination.
Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
For example, given candidate set [10, 1, 2, 7, 6, 1, 5] and target 8, 
A solution set is: 
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]
<!-- more -->
**思路：**
与Combination Sum一样，只是数字不能重复使用，那么每次递归时之间到下一位数字；
注意去除重复的组合。

``` java
public class Solution {
    List<List<Integer>> ans = new ArrayList<List<Integer>>();
	List<Integer> temp = new ArrayList<Integer>();
    public List<List<Integer>> combinationSum2(int[] nums, int target) {
        Arrays.sort(nums);
		calculate(nums,target,0,0);
		return ans;
    }
	public void calculate(int[] nums,int target,int sum,int level){
		if(sum > target)
			return;
		else if(sum == target){
			List<Integer> t = new ArrayList<Integer>(temp); 
			ans.add(t);                                     
			//如直接ans.add(temp);实际上是重复将同一个对象temp往ans里面传，错误。
			return;
		}else{             // DFS
			for(int i=level;i<nums.length;i++){
				sum += nums[i];
				temp.add(nums[i]);
				calculate(nums,target,sum,i+1);   //数字不能重复使用
				temp.remove(temp.size()-1);
				sum -= nums[i];
				while(i<nums.length-1 && nums[i+1]==nums[i])  //去除重复的组合
					i++;  
			}
		}
	}
}
```
**借助Set：**
Set自动去除重复，最后将Set转换成List即可，但耗时较大
``` java
List<List<Integer>> anslist = new ArrayList<List<Integer>>(new HashSet<List<Integer>>(ans));
```
详见java总结**list,set,map,数组间的相互转换**