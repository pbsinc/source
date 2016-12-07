---
title: Subsets II.java
date: 
categories: LeetCode
tags: Medium
---
Given a collection of integers that might contain duplicates, nums, return all possible subsets.
Note: The solution set must not contain duplicate subsets.
For example,If nums = [1,2,2], a solution is:
[[2],[1],[1,2,2],[2,2],[1,2],[]]
<!-- more -->
**思路：**
先按从小到大排序，判断当前元素是否遇上一个元素相同，如相同，则只把该元素上次新加入的元素累加，加入ans；
``` java
public class Solution {
    public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> ans = new ArrayList<List<Integer>>();
		if(nums.length == 0) return ans;
		ans.add(new ArrayList<Integer>());//先把空集加进去
		Arrays.sort(nums);
		int count = 0;
		for(int i=0;i<nums.length;i++){
			int size = ans.size();
			
			//这里要注意，必须提前把size提取出来，不能把提取过程直接嵌入到下面的for语句中
			//因为res的size会在下面语句中一直变化
			
			if(i>0 && nums[i] == nums[i-1]){ //判断当前元素是否遇上一个元素相同
				int n = size-count; 		 //这是上次元素运行前ans的size
				count = 0; 
				for(int j=n;j<size;j++){ 	 //如相同，则只把该元素上次新加入的元素累加，加入ans
					List<Integer> temp = new ArrayList<Integer>(ans.get(j));
					temp.add(nums[i]);
					ans.add(temp);
					count++;
				}
			}else{
				count = 0; 
				for(int j=0;j<size;j++){
					List<Integer> temp = new ArrayList<Integer>(ans.get(j));
					temp.add(nums[i]);
					ans.add(temp);
					count++;
				}
			}
		}
		return ans;
    }
}
```