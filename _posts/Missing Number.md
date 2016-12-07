---
title: Missing Number.java
date: 
categories: LeetCode
tags: Medium
---
Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.
For example,
Given nums = [0, 1, 3] return 2.
Note:Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?
<!-- more -->
**思路：**
对原数组进行排序:
找出不连续的数；
``` java
public class Solution {
    public int missingNumber(int[] nums) {
        int len = nums.length;
		Arrays.sort(nums);
		int ans = 0;
		if(len == 1){
			if(nums[0] == 0)
				return 1;
			else
				return 0;
		}
		for(int i=0;i<len-1;i++){
			if((nums[i]+1) != nums[i+1]){
				ans = nums[i]+1;
				break;
			}
			if(i == len-2)
				if(nums[0] == 0)
					ans = len;
		}
		return ans;
    }
}
```
**1ms方法：**
1：0-n求和，再减去数组元素的总和，即为缺失元素。 
2：亦或操作。两个相同的数亦或结果为0，一个不为0的数与0亦或，其值为本身。
``` java
//1. Runtime: 1 ms
public class Solution {
    public int missingNumber(int[] nums) {
        int sum = (int)((nums.length + 1) / 2.0 * nums.length);
        for (int n : nums) {
            sum -= n;
        }

        return sum;
    }
}

//2. Runtime: 1 ms
public class Solution {
    public int missingNumber(int[] nums) {
        int miss = nums[0] ^ 0;
        for (int i = 1; i < nums.length; i++) {
            miss ^= nums[i];
            miss ^= i;
        }

        return miss ^= nums.length;
    }
}
```
