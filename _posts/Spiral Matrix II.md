---
title: Spiral Matrix II.java
date: 
categories: LeetCode
tags: Medium
---
Given an integer n, generate a square matrix filled with elements from 1 to n2 in spiral order.
For example,Given n = 3,
You should return the following matrix:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]
<!-- more -->
**思路：**
与Spiral Matrix 思路一样，反过来就好了。
``` java
public class Solution {
    public int[][] generateMatrix(int n) {     
		int[][] matrix = new int[n][n];
		if(n<1) return matrix; 
		int a = 0,count = 1;	
		n=n-1;
		while(a<=n){
			for(int i=a;i<=n;i++){     //当遇到[[1]]时，如为i<n，则读不到！
				matrix[a][i] = count;
				count++;
			}
			for(int j=a+1;j<=n;j++){
				matrix[j][n] = count;
				count++;
			}
			if(a != n){
			    for(int k=n-1;k>=a;k--){
		    		matrix[n][k] = count;
					count++;
		    	}
			}
			if(a != n){
			    for(int s=n-1;s>a;s--){
			    	matrix[s][a] = count;
					count++;
		    	}
		    }
			a++;
			n--;
		}
		return matrix;		
    }
}
```