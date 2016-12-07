---
title: Sort Colors.java
date: 
categories: LeetCode
tags: Medium
---
Given an array with n objects colored red, white or blue, sort them so that objects of the same color are adjacent, with the colors in the order red, white and blue.
Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.
Note:You are not suppose to use the library's sort function for this problem.
Follow up:
A rather straight forward solution is a two-pass algorithm using counting sort.
First, iterate the array counting number of 0's, 1's, and 2's, then overwrite array with total number of 0's, then 1's and followed by 2's.
Could you come up with an one-pass algorithm using only constant space?
<!-- more -->
**思路：**
迭代，右边大跟右边换，左边大跟左边换；
注意：也可用三指针的方法，类似于快速排序,
left = 0，指针right = n-1；一次遍历，如果遇到0，交换给left，遇到2，交换给right，遇到1别管。
``` java
public class Solution {
    public void sortColors(int[] nums) {
        int len = nums.length;
        for(int i=0;i<nums.length-1;i++){
            if(nums[i] > nums[i+1])
                calculate(nums,i);
        }
    }
	public void calculate(int[] nums,int i){
	  
			if(i < nums.length-1 && nums[i] > nums[i+1]){
				int temp = nums[i+1];
				nums[i+1] = nums[i];
				nums[i] = temp;
				calculate(nums,i+1);
				calculate(nums,i);
			}
			if(i>0 && nums[i] < nums[i-1]){
				int temp = nums[i-1];
				nums[i-1] = nums[i];
				nums[i] = temp;
				calculate(nums,i-1);
				calculate(nums,i);
			}
		
	}
}
```
**巧妙方法：**
分别统计出现0、1、2出现的次数，然后直接给数组赋值就ok了，复杂度是0(n)。
``` java
public class Solution {  
    public void sortColors(int[] A) {  
        int count0 = 0;  
        int count1 = 0;  
        int count2 = 0;  
        for(int i = 0; i < A.length; i++){  
            if(A[i] == 0){  
                count0++;  
            }  
            if(A[i] == 1){  
                count1++;  
            }  
            if(A[i] == 2){  
                count2++;  
            }  
        }  
        for(int i = 0; i < count0; i++){  
            A[i] = 0;  
        }  
        for(int i = count0; i < count0+count1; i++){  
            A[i] = 1;  
        }  
        for(int i = count0+count1; i < count0+count1+count2; i++){  
            A[i] = 2;  
        }  
    }  
}  
```