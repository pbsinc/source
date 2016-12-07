---
title: Set Matrix Zeroes.java
date: 
categories: LeetCode
tags: Medium
---
Given a m x n matrix, if an element is 0, set its entire row and column to 0. Do it in place.
Follow up:
Did you use extra space?
A straight forward solution using O(mn) space is probably a bad idea.
A simple improvement uses O(m + n) space, but still not the best solution.
Could you devise a constant space solution?
<!-- more -->
**思路：**
用HashSet记录每个0的坐标，最后给该行该列全置为0即可；
``` java
public class Solution {
    public void setZeroes(int[][] matrix) {
        int m = matrix.length;
		int n = matrix[0].length;
		Set<Integer> row = new HashSet<Integer>();
		Set<Integer> col = new HashSet<Integer>();
		for(int i=0;i<m;i++){
			for(int j=0;j<n;j++){
				if(matrix[i][j] == 0){
					row.add(i);
					col.add(j);
				}
			}
		}
		
		for(int i : row){
			for(int k=0;k<n;k++)
				matrix[i][k] = 0;
		}
		for(int j : col){
			for(int k=0;k<m;k++)
				matrix[k][j] = 0;
		}
    }
}
```
**另一种方法是不需要额外空间的：**
find zeroes and store the info in first row and column；
``` java
public void setZeroes(int[][] matrix) {
    int rownum = matrix.length;
    if (rownum == 0)  return;
    int colnum = matrix[0].length;
    if (colnum == 0)  return;

    boolean hasZeroFirstRow = false, hasZeroFirstColumn = false;

    // Does first row have zero?
    for (int j = 0; j < colnum; ++j) {
        if (matrix[0][j] == 0) {
            hasZeroFirstRow = true;
            break;
        }
    }

    // Does first column have zero?
    for (int i = 0; i < rownum; ++i) {
        if (matrix[i][0] == 0) {
            hasZeroFirstColumn = true;
            break;
        }
    }

    // find zeroes and store the info in first row and column
    for (int i = 1; i < matrix.length; ++i) {
        for (int j = 1; j < matrix[0].length; ++j) {
            if (matrix[i][j] == 0) {
                matrix[i][0] = 0;
                matrix[0][j] = 0;
            }
        }
    }

    // set zeroes except the first row and column
    for (int i = 1; i < matrix.length; ++i) {
        for (int j = 1; j < matrix[0].length; ++j) {
            if (matrix[i][0] == 0 || matrix[0][j] == 0)  matrix[i][j] = 0;
        }
    }

    // set zeroes for first row and column if needed
    if (hasZeroFirstRow) {
        for (int j = 0; j < colnum; ++j) {
            matrix[0][j] = 0;
        }
    }
    if (hasZeroFirstColumn) {
        for (int i = 0; i < rownum; ++i) {
            matrix[i][0] = 0;
        }
    }
}
```