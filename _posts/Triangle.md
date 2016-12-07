---
title: Triangle.java
date: 
categories: LeetCode
tags: [Medium,动态规划]
---
Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.
For example, given the following triangle
``` java
[
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
```
The minimum path sum from top to bottom is 11 (i.e., 2 + 3 + 5 + 1 = 11).
Note:
Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.
<!-- more -->
**思路：**
迭代的思想，一直往下求，返回较小的，但超时。
``` java
public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
		if(triangle.size() > 0 && triangle.get(0).size() >0)
			return ans(triangle,0,0,0)+triangle.get(0).get(0);
		else
			return 0;
    }
	
	public int ans(List<List<Integer>> triangle,int row,int col,int sum){
		
		if(row < triangle.size()-1){
			int sumleft = sum + triangle.get(row+1).get(col);
			int sumright = sum + triangle.get(row+1).get(col+1);
			
			return Math.min(ans(triangle,row+1,col,sumleft),ans(triangle,row+1,col+1,sumright));
		}else{
			return sum;
		}
	}
}
```
**动态规划 DP方法：**
此题明显是一个求取约束条件下最小路径的题，用动态规划（Dynamic Programming）解再合适不过，既然是DP问题，那么我们需要抽象出状态转移方程：把三角形数阵可以抽象成一个二维矩阵，那么：
设：从位置（i,j）达到底部的最小路径和为MP(i,j)；根据约束条件，从位置（i,j）只能达到下一行的（i+1,j）和（i+1,j+1）两个位置；如果，根据题意我们知道，位置（i,j）处的权值为输入三角形数阵对应的数据：triangle[i][j];So,状态转移方程为：MP(i,j)=min{MP(i+1,j),MP(i+1,j+1)}+triangle[i][j];三角形顶部到底部最小路径和：MP(0,0)=min{MP(1,0),MP(1,1)}+triangle[0][0];而：MP(1,0)=min{MP(2,0),MP(2,1)}+triangle[1][0];MP(1,1)=min{MP(2,1),MP(2,2)}+triangle[1][1]....
很明显，这种自顶向下的求解方式会形成一个“树形结构”，并且自顶向下的求解过程，计算式中一直存在未知式，这显然不是一种好的方式，因此，我们采用自底向上的求解思路：以题目中给出的例子为例：

MP(3,0)=triangle[3][0]=4;
MP(3,1)=triangle[3][1]=1;
MP(3,2)=triangle[3][1]=8;
MP(3,3)=triangle[3][1]=3;
MP(2,0)=min{MP(3,0),MP(3,1)}+triangle[2][0]=1+6=7;
MP(2,1)=min{MP(3,1),MP(3,2)}+triangle[2][1]=1+5=6;
MP(2,2)=min{MP(3,2),MP(3,3)}+triangle[2][2]=3+7=10;
MP(1,0)=min{MP(2,0),MP(2,1)}+triangle[1][0]=6+3=9;
MP(1,1)=min{MP(2,1),MP(2,2)}+triangle[1][1]=6+6=12;
MP(0,0)=min{MP(1,0),MP(1,1)}+triangle[0][0]=9+2=11;

很明显，自底向上计算，最后MP(0,0)就是我们要的结果，采用两重循环即可完成求解；
经过上面的分析，我们进一步优化空间消耗的问题，事实上，我们可以用一个一维数组来存储自底向上的路径向量，路径向量初始化为三角形数阵底部向量，此后每计算一次，更新一次，空间复杂度为O(n),且不影响输入三角形数阵的原始数据，程序如下：
``` java
public class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
		int len = triangle.size();
		if(len == 0 )return 0;
		if(len == 1 )return triangle.get(0).get(0);
	
		List<Integer> temp = triangle.get(len-1);
		for(int i=len-2;i>=0;i--){
			for(int j=0;j<triangle.get(i).size();j++){
				int temp1 = triangle.get(i).get(j) + temp.get(j);
				int temp2 = triangle.get(i).get(j) + temp.get(j+1);
				temp.set(j,Math.min(temp1,temp2)); //当前位置j已经不会再被用到了，因此覆盖它
			}
		}
		return temp.get(0);
	}
}
```
