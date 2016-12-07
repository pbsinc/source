---
title: Rotate Image.java
date: 
categories: LeetCode
tags: Medium
---
You are given an n x n 2D matrix representing an image.
Rotate the image by 90 degrees (clockwise).
Follow up:
Could you do this in-place?
<!-- more -->
**思路：**
正方形，还要求in place；
由外向内一圈一圈转，转的过程中，完成对应四个元素的替换；
每圈的起始位置是矩阵对角线开始。

``` java
public class Solution {
    public void rotate(int[][] matrix) {
        int row,col;
        int length = matrix.length;
        int edge = length-1;
        int nEdge;

        for(row = 0;row<edge;row++){
            nEdge = edge;
            for(col = row;col<edge;col++,nEdge--){
                int val=matrix[row][col];
                matrix[row][col] = matrix[nEdge][row];
                matrix[nEdge][row] = matrix[edge][nEdge];
                matrix[edge][nEdge] = matrix[col][edge];
                matrix[col][edge] = val;
            }
            edge--;//往里一圈
        }
    }
}
```