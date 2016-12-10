---
title: Rectangle Area.java
date: 
categories: LeetCode
tags: Easy
---
Find the total area covered by two rectilinear rectangles in a 2D plane.
Each rectangle is defined by its bottom left corner and top right corner as shown in the figure.
![这里写图片描述](https://leetcode.com/static/images/problemset/rectangle_area.png)
Assume that the total area is never beyond the maximum possible value of int.
<!-- more -->
**思路：**
求两个矩形共同覆盖的面积；
两个的面积减去重叠部分即可；
``` java
public class Solution {
    public int computeArea(int A, int B, int C, int D, int E, int F, int G, int H) {
		int row=0,col=0;
		if(!(E>=C || A>=G || F>=D || B>=H)) {
			row = Math.min(C,G)  - Math.max(A,E);
			col = Math.min(D,H) - Math.max(B,F);
		}
		
		return (C-A)*(D-B)+(G-E)*(H-F)-row*col;
    }
}
``` 