---
title: Subsets.java
date: 
categories: LeetCode
tags: Medium
---
Given a set of distinct integers, nums, return all possible subsets.
Note: The solution set must not contain duplicate subsets.
For example,If nums = [1,2,3], a solution is:
[[3],[1],[2],[1,2,3],[1,3],[2,3],[1,2],[]]
<!-- more -->
**思路：**
取res中现有list，每个list都加新的元素nums[i]然后再放回res中，同时保留原有list. 从[]开始一次加一个新元素。
``` java
public class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> ans = new ArrayList<List<Integer>>();
		
		if(nums.length == 0) return ans;
		
		ans.add(new ArrayList<Integer>());//先把空集加进去
		
		for(int i=0;i<nums.length;i++){
			int size = ans.size();
			//这里要注意，必须提前把size提取出来，不能把提取过程直接嵌入到下面的for语句中
			//因为res的size会在下面语句中一直变化
			
			for(int j=0;j<size;j++){
				List<Integer> temp = new ArrayList<Integer>(ans.get(j));
				temp.add(nums[i]);
				ans.add(temp);
			}
		}
		return ans;
    }
}
```
**NP问题：**
http://baike.baidu.com/link?url=9yv4i91iIkqT3Z9umB55URm5o_oChpNJWLwFRDn6JSrbKldgwTI5LPKS_Xm4WLCbp0r4BuAb-vgxOE7qD-dkLK