---
title: Maximum Subarray.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Find the contiguous subarray within an array (containing at least one number) which has the largest sum.
For example, given the array [-2,1,-3,4,-1,2,1,-5,4],
the contiguous subarray [4,-1,2,1] has the largest sum = 6.
More practice:
If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.
<!-- more -->
经典问题，1977布朗的一个教授提出来
http://en.wikipedia.org/wiki/Maximum_subarray_problem
**思路：**
这道题要求 求连续的数组值，加和最大。
 试想一下，如果我们从头遍历这个数组。对于数组中的其中一个元素，它只有两个选择：
 1. 要么加入之前的数组加和之中（跟别人一组）
 2. 要么自己单立一个数组（自己单开一组）
所以对于这个元素应该如何选择，就看他能对哪个组的贡献大。如果跟别人一组，能让总加和变大，还是跟别人一组好了；如果自己起个头一组，自己的值比之前加和的值还要大，那么还是自己单开一组好了。
所以利用一个sum数组，记录每一轮sum的最大值，sum[i]表示当前这个元素是跟之前数组加和一组还是自己单立一组好，然后维护一个全局最大值即位答案。
Kadane算法，算法复杂度O(n)
``` java
public class Solution {
    public int maxSubArray(int[] nums) {
        int max = nums[0];
		int[] sum = new int[nums.length];
		sum[0] = nums[0];
		for(int i=1;i<nums.length;i++){
		    sum[i] = Math.max(nums[i],sum[i-1]+nums[i]);
		    max = Math.max(max,sum[i]);
		}
		return max;
    }
}
```
**分治法：**
把序列分为两段，分别求最大连续子序列和，然后归并，算法复杂度为O(nlogn)。
``` java
public class Solution {
    public int maxSubArray(int[] A) {
         return divide(A, 0, A.length-1); 
    }
	public int divide(int A[], int low, int high){  
        if(low == high)
            return A[low];  
        if(low == high-1)  
            return Math.max(A[low]+A[high], Math.max(A[low], A[high]));
            
        int mid = (low+high)/2;  
        int lmax = divide(A, low, mid-1);  
        int rmax = divide(A, mid+1, high); 
        
        int mmax = A[mid];  
        int tmp = mmax;  
        for(int i = mid-1; i >=low; i--){  
            tmp += A[i];  
            if(tmp > mmax)
                mmax = tmp;  
        }  
        tmp = mmax;  
        for(int i = mid+1; i <= high; i++){  
            tmp += A[i];  
            if(tmp > mmax)
                mmax = tmp;  
        }  
        return Math.max(mmax, Math.max(lmax, rmax));  
          
    }
}
```