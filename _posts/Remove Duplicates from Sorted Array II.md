---
title: Remove Duplicates from Sorted Array II.java
date: 
categories: LeetCode
tags: Medium
---
Follow up for "Remove Duplicates":
What if duplicates are allowed at most twice?
For example,
Given sorted array nums = [1,1,1,2,2,3],
Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3. It doesn't matter what you leave beyond the new length.
<!-- more -->
**注意：**
1.思路：给每次相等次数加入计数；
2.晚上11点后实在困得不行就不要刷算法了，自己都绕晕了。。。
``` java
public class Solution {
    public int removeDuplicates(int[] nums) {
        int len=nums.length;
        if(len==0)
            return 0;
        int count =1,s=0;
        for(int i=1;i<len;i++){
            if(nums[count-1]==nums[i]){
                s++;
                if(s<2){                 
	                if(nums[count]!=nums[i])
			            nums[count]=nums[i]; 
			        count++;       
                }
                continue;
            }
            else{
		        s=0;
                nums[count]=nums[i];
                count++;
                }
            }
        return count;
    }
}
```	