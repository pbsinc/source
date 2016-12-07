---
title: Arithmetic Slices.java
date: 
categories: LeetCode
tags: Medium
---
A sequence of number is called arithmetic if it consists of at least three elements and if the difference between any two consecutive elements is the same.
For example, these are arithmetic sequence:
1, 3, 5, 7, 9
7, 7, 7, 7
3, -1, -5, -9
The following sequence is not arithmetic.
1, 1, 2, 5, 7
A zero-indexed array A consisting of N numbers is given. A slice of that array is any pair of integers (P, Q) such that 0 <= P < Q < N.
A slice (P, Q) of array A is called arithmetic if the sequence:
A[P], A[p + 1], ..., A[Q - 1], A[Q] is arithmetic. In particular, this means that P + 1 < Q.
The function should return the number of arithmetic slices in the array A.
Example:
A = [1, 2, 3, 4]
return: 3, for 3 arithmetic slices in A: [1, 2, 3], [2, 3, 4] and [1, 2, 3, 4] itself.
<!-- more -->
**思路：**
对连续相邻两数相等计数，即可。
``` java
public class Solution {
    public int numberOfArithmeticSlices(int[] a) {
		int res = 0;
        for(int i=0;i<a.length-2;i++){
			int temp = a[i+1] - a[i];
			for(int j=2;j<a.length-i;j++){
				if(a[i+j]-a[i+j-1] == temp){
					res++;
				}else
					break;
					
			}
		}
		return res;
    }
}
``` 