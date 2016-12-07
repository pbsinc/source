---
title: Single Number.java
date: 
categories: LeetCode
tags: [Easy, break跳出多层for循环]
---
Given an array of integers, every element appears twice except for one. Find that single one.
Note:
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?
<!-- more -->
**关键:**
1.获取int[] nums 的长度,nums.length；注意跟Sthring的区别，没有()；
2.break 加标签，跳出outer2层for循环
我的做法虽然A过，但太慢了
``` java
public class Solution {
    public int singleNumber(int[] nums) {
        int len = nums.length ;
        int ans = nums[0];
        outer2:for(int i=0;i<=len-1;i++){
            for(int j=0;j<=len-1;j++){
                if(i!=j & nums[i] == nums[j]){
                    break;                    //跳出一层for循环
                }
                if(j == len-1){
                    ans = nums[i];
                    break outer2;             //break 加标签，跳出outer2层for循环
                }
            }
        }
        return ans;
    }
}
```	

**高效答案:异或！**
使用异或：1^1^2^2^3^3^4 = 4;
按位来看：1001^1001 = 0000 ；1010^ 0011 = 1001;
**相同为0，相异为1**
``` java
 public class Solution {
    public int singleNumber（int[] A） {
		int result = A[0];
		for（int i = 1; i < A.length; i++）{
			result = result ^ A[i];
		}
		return result;
	}
}
```	