---
title: Unique Paths.java
date: 
categories: LeetCode
tags: [Medium,动态规划,重做]
---
A robot is located at the top-left corner of a m x n grid (marked 'Start' in the diagram below).
The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
How many possible unique paths are there?
Above is a 3 x 7 grid. How many possible unique paths are there?
![这里写图片描述](http://leetcode.com/wp-content/uploads/2014/12/robot_maze.png)
Note: m and n will be at most 100.
<!-- more -->
**思路：**
跟爬梯子挺类似(只能往上爬)，这个就是方向换了。同样想法**动态规划**。 
分析方法也一样的，想想要到最右下角。到达右下角的方法只有两个，从上面往下，和从右面往左。
 利用到达终点的唯一性，就可以写出递推公式（ans[i][j]表示到坐标（i,j）的走法数量）：
 ans[i][j] = ans[i-1][j] + ans[i][j-1]
初始条件的话，当整个格子只有一行，那么到每个格子走法只有1种；只有一列的情况同理。
``` java
public class Solution {
    public int uniquePaths(int m, int n) {
        int[][] ans = new int[m][n];
		if(m==0 || n==0) return 0;
		if(m==1 || n==1) return 1;
		//第一行，只有一种走法到达这里  
		for(int i=0;i<n;i++)
			ans[0][i] = 1;
		//第一列，只有一种走法到达这里  
		for(int i=0;i<m;i++)
			ans[i][0] = 1;
		//此后，每点到达这里的方法等于左边和上边两点方法之和
		for(int i=1;i<m;i++){
			for(int j=1;j<n;j++)
				ans[i][j] = ans[i-1][j] + ans[i][j-1];
		}
		return ans[m-1][n-1];
    }
}
```