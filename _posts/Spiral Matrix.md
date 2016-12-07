---
title: Spiral Matrix.java
date: 
categories: LeetCode
tags: Medium
---
Given a matrix of m x n elements (m rows, n columns), return all elements of the matrix in spiral order.
For example,Given the following matrix:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
You should return [1,2,3,6,9,8,7,4,5].
<!-- more -->
**思路：**
首先判断矩阵是空时，直接输出空；
有元素需时转圈：从左到右，从上到下，从右到左，从下到上；
从大圈依次循环到小圈即可。
注意：第一行第一次循环时，必须全部添加进去，而不可选择将最后一位留给从上到下时再添加，否则：当遇到[[1]]时，读不到！
``` java
public class Solution {
    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> ans = new ArrayList<Integer>();
		int m = matrix.length-1;
		if(m<0) return ans; 
		int a = 0,b = 0;
		int n = matrix[0].length-1;
		while(a<=m && b<=n){
			for(int i=b;i<=n;i++){     //当遇到[[1]]时，如为i<n，则读不到！
				ans.add(matrix[a][i]);	
			}
			for(int j=a+1;j<=m;j++){
				ans.add(matrix[j][n]);
			}
			if(a !=m){
			    for(int k=n-1;k>=b;k--){
		    		ans.add(matrix[m][k]);
		    	}
			}
			if(b!=n){
			    for(int s=m-1;s>a;s--){
			    	ans.add(matrix[s][b]);
		    	}
		    }
			a++;
			b++;
			m--;
			n--;
		}
		return ans;
    }
}
```