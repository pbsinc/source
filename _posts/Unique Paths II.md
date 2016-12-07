---
title: Unique Paths II.java
date: 
categories: LeetCode
tags: Medium
---
Follow up for "Unique Paths":
Now consider if some obstacles are added to the grids. How many unique paths would there be?
An obstacle and empty space is marked as 1 and 0 respectively in the grid.
For example,There is one obstacle in the middle of a 3x3 grid as illustrated below.
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
The total number of unique paths is 2. Note: m and n will be at most 100.
<!-- more -->
**思路：**
与Unique Paths类似，遇到阻碍（元素为1时），将当前ans[i][j]堵死（置为0）即可；
注意第一行第一列遇到阻碍时，将后面都堵死。
``` java
public class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
		if(m == 0) return 0;
		int n = obstacleGrid[0].length;
		if(n == 0) return 0;
        int[][] ans = new int[m][n];
		if(obstacleGrid[0][0] == 1) 
			return 0;
		else
			ans[0][0] = 1;
			
		for(int i=0;i<n;i++){
			if(obstacleGrid[0][i] == 1){
				for(int j=i;j<n;j++)
					ans[0][j] = 0;
				break;
			}
			else
				ans[0][i] = 1;
		}	
					
		for(int i=0;i<m;i++){
			if(obstacleGrid[i][0] == 1){
				for(int j=i;j<m;j++)
					ans[j][0] = 0;
				break;
			}
			else
				ans[i][0] = 1;
		}	
		
		
		for(int i=1;i<m;i++){
			for(int j=1;j<n;j++)
				if(obstacleGrid[i][j] == 1)
					ans[i][j] = 0;
				else
					ans[i][j] = ans[i-1][j] + ans[i][j-1];
		}
		return ans[m-1][n-1];
    }
}
```