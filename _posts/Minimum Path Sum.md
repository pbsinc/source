---
title: Minimum Path Sum.java
date: 
categories: LeetCode
tags: Medium
---
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.
Note: You can only move either down or right at any point in time.
<!-- more -->
**思路：**
与Unique Paths类似；
每个位置grid[i][j]的新值为原值与上一路径（左，上）的较小值相加。
``` java
public class Solution {
    public int minPathSum(int[][] grid) {
		
        int m = grid.length;
		if(m == 0) return 0;
		int n = grid[0].length;
		if(n == 0) return 0;
		
		if(m==1){
			int temp = 0;
			for(int i=0;i<n;i++)
				temp += grid[0][i]; 
			return temp;
		}
		if(n==1){
			int temp = 0;
			for(int i=0;i<m;i++)
				temp += grid[i][0]; 
			return temp;
		}
		
		for(int i=1;i<n;i++)
			grid[0][i] += grid[0][i-1] ;
			
		for(int i=1;i<m;i++)
			grid[i][0] += grid[i-1][0];
		
		for(int i=1;i<m;i++){
			for(int j=1;j<n;j++)
				grid[i][j] += Math.min(grid[i-1][j],grid[i][j-1]);
		}
		return grid[m-1][n-1];
    }
}
```