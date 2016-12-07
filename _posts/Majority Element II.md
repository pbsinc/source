---
title: Majority Element II.java
date: 
categories: LeetCode
tags: [Medium,多数投票算法,重做]
---
Given an integer array of size n, find all elements that appear more than ⌊ n/3 ⌋ times. The algorithm should run in linear time and in O(1) space.
<!-- more -->
**思路：**
排序后，判断元素重复出现的次数为（n/3 +1）时，输出该元素；
但是，Arrays.sort(nums); 排序复杂度最小也是o(nlogn)，所以按道理此方法不可。
``` java
public class Solution {
    public List<Integer> majorityElement(int[] nums) {
		Arrays.sort(nums);
		List<Integer> res = new ArrayList<Integer>();
		if(nums.length == 1){
			res.add(nums[0]);
			return res;
		}
		if(nums.length == 2){
			res.add(nums[0]);
			if(nums[1] != nums[0])
				res.add(nums[1]);
			return res;
		}
		for(int i=0;i<nums.length-1;i++){
			int count = 1;
			while(i<nums.length-1 && nums[i] == nums[i+1]){
				count++;
				i++;
				if(count == nums.length/3 + 1){
					res.add(nums[i]);
				}
			}
		}
		return res;
    }
}
```
**正确思路：多数投票算法**
看到这题首先想到的是出现次数大于⌊ n/3 ⌋的数不只有一个，思考了一下，这个数最多有两个，最少一个也没有。所以可以用一个大小为2的数组保存候选结果，最后进行筛选。
多数投票算法:
1.如果count==0，则将now的值设置为数组的当前元素，将count赋值为1；
2.反之，如果now和现在数组元素值相同，则count++，反之count–；
3.重复上述两步，直到扫描完数组。
``` java
public class Solution {
    public List<Integer> majorityElement(int[] nums) {
		List<Integer> res = new ArrayList<Integer>();
		if(nums == null || nums.length == 0)
            return res;
		
		int numbers[] = new int[2];
		int count[] = new int[2];
		
		for(int i=0;i<nums.length;i++){
			if(count[0] == 0 && nums[i] != numbers[1]){
				numbers[0] = nums[i];
			}else if(count[1] == 0 && nums[i] != numbers[0]){
				numbers[1] = nums[i];
			}
			
			if(nums[i] == numbers[0]){
				count[0]++;
			}else if(nums[i] == numbers[1]){
				count[1]++;
			}else{
				count[0]--;
				count[1]--;
			}
		}
		
		count[0] = 0;
		count[1] = 0;
		
		for(int i=0;i<nums.length;i++){
			if(nums[i] == numbers[0])
				count[0]++;
			else if(nums[i] == numbers[1])
				count[1]++;
		}
		if(count[0] > nums.length/3)
			res.add(numbers[0]);
		if(count[1] > nums.length/3 && numbers[1] != numbers[0])
			res.add(numbers[1]);
		
		return res;
    }
}
```
