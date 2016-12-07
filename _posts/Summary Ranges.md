---
title: Summary Ranges.java
date: 
categories: LeetCode
tags: Medium
---
Given a sorted integer array without duplicates, return the summary of its ranges.
For example, given [0,1,2,4,5,7], return ["0->2","4->5","7"].
<!-- more -->
**思路：**
记录连续的起点终点就可以了。
``` java
public class Solution {
    public List<String> summaryRanges(int[] nums) {
		List<String> res = new ArrayList<String>();
        for(int i=0;i<nums.length;i++){
			int start =i;
			while(i< nums.length-1 && nums[i]+1 == nums[i+1]){
				i++;
			}
			if(i != start){
				String temp = new String(nums[start]+"->"+nums[i]);
				res.add(temp);
			}else{
				String temp = new String(Integer.toString(nums[i])); // int 转 Sting
				res.add(temp);
			}
		}
		return res;
    }
}
```

