---
title: Remove Duplicates from Sorted Array.java
date: 
categories: LeetCode
tags: Easy
---
Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.
Do not allocate extra space for another array, you must do this in place with constant memory.
For example,
Given input array nums = [1,1,2],
Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.
<!-- more -->
**思路：**
我的思路完全跑偏了，这是正确思路，一次遍历，比较相邻，若不同按顺序重新填入数组！

（1）主要是因为数组**已经是排好顺序**的。如果不仔细看题目，把数组当作无序数组进行操作，OJ时会显示超时。

（2）题目要求是不能申请二额外空间，如果提交时有申请额外空间，也是不通过的。

（3）还需要注意的一个地方是，数组中元素的位置不能改变。比如对于数组[1,1,1,4,5]，移除重复元素后为[1,4,5]，起始数字为1，而不能是其它数字。

（4）我们只需对对组遍历一次，并设置一个计数器，每当遍历前后元素不相同，计数器加1，并将计数器对应在数组中位置定位到当前遍历的元素。
``` java
public class Solution {
    public int removeDuplicates(int[] nums) {
        int len = nums.length;
        if(len ==0)
            return 0;
        int count =1;
        for(int i=1;i<len;i++){
            if(nums[i]==nums[i-1])
                continue;
            else{
                nums[count]=nums[i];
                count++;
            }
        }
        return count;
    }
}
```