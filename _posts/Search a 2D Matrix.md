---
title: Search a 2D Matrix.java
date: 
categories: LeetCode
tags: Medium
---
Write an efficient algorithm that searches for a value in an m x n matrix. This matrix has the following properties:
Integers in each row are sorted from left to right.
The first integer of each row is greater than the last integer of the previous row.
For example,
Consider the following matrix:
[
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
Given target = 3, return true.
<!-- more -->
**思路：**
找到大小范围所在的行，遍历一遍该行即可;
``` java
public class Solution {
    public boolean searchMatrix(int[][] matrix, int target) {
        int m = matrix.length;
		int n = matrix[0].length;
		int row = -1;
		if(target < matrix[0][0])
			return false;
		if(target > matrix[m-1][n-1])
			return false;
		for(int i=0;i<m;i++){
			if(target == matrix[i][0])
				return true;
			else if(target < matrix[i][0]){
				row = i-1;
				break;
			}
			if(i == m-1 && row == -1)
				row = i;
		}

		for(int i=0;i<n;i++){
			if(matrix[row][i] == target)
				return true;
		}
		return false;
    }
}
```