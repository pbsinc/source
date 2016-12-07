---
title: Game of Life.java
date: 
categories: LeetCode
tags: Medium
---
According to the Wikipedia's article: "The Game of Life, also known simply as Life, is a cellular automaton devised by the British mathematician John Horton Conway in 1970."
Given a board with m by n cells, each cell has an initial state live (1) or dead (0). Each cell interacts with its eight neighbors (horizontal, vertical, diagonal) using the following four rules (taken from the above Wikipedia article):
1.Any live cell with fewer than two live neighbors dies, as if caused by under-population.
2.Any live cell with two or three live neighbors lives on to the next generation.
3.Any live cell with more than three live neighbors dies, as if by over-population..
4.Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.
Write a function to compute the next state (after one update) of the board given its current state.
Follow up:
Could you solve it in-place? Remember that the board needs to be updated at the same time: You cannot update some cells first and then use their updated values to update other cells.
In this question, we represent the board using a 2D array. In principle, the board is infinite, which would cause problems when the active area encroaches the border of the array. How would you address these problems?
<!-- more -->
**思路：**
简单的想法就是copy 原来矩阵，然后根据copy 来修改原来的矩阵，如此做会使用O(m*n) space.
如何做到In-space呢，我们需要根据改动前的状态来数出live cell 的个数，已经更改的点如何知道原有的状态呢。就要多mark出几种conditions.
Dead->Dead: Condition 0; Live->Live : Condition 1; Live->Dead: Condition 2; Dead->Live:Condition 3
如此在数数的时候如果原道1 和 2他们原有的状态都是live，就都要算。
最后通过把所有的数%2来得到更改后的状态。
 
如何定义这四种状态？第一个原因是通过如此顺序是因为0本身就是dead cell, 1本身就是live cell, 如此在countLive完成后可以尽量少改动数组，如果还是live的1的状态就不用动了；
第二个原因最后对0,1,2,3改回0,1时 可以直接通过%2改动省事。
如果需要记住原有状态就可以通过增加condition 来搞定。
Time Complexity: O(m*n). Space: O(1).
``` java
public class Solution {
    public void gameOfLife(int[][] board) {
		if(board == null || board.length == 0 || board[0].length == 0){
            return;
        }
        int m = board.length;
		int n = board[0].length;
		//Mark four conditions
        // Dead->Dead: 0; Live->Live : 1; Live->Dead: 2; Dead->Live:3
		for(int i=0;i<m;i++){
			for(int j=0;j<n;j++){
			    int num = livenum(board,i,j);
				if(board[i][j] == 1 ){	
					if(num < 2)
						board[i][j] = 2;
					if(num == 2 || num == 3)
						board[i][j] = 1;
					if(num > 3)
						board[i][j] = 2;
				}
				else if(board[i][j] == 0 && num ==3){
						board[i][j] = 3;
				}
				
			}
		}
		
		for(int i=0;i<m;i++){
			for(int j=0;j<n;j++){
				board[i][j] = board[i][j]%2;
			}
		}		
    }
	
	public int livenum(int[][] board,int x,int y){
		int m = board.length;
		int n = board[0].length;
		int count = 0;
		for(int i=x-1;i<=x+1;i++){
			for(int j=y-1;j<=y+1;j++){
				if(i>=0 && j>=0 && i<m && j<n && !(i==x && j==y)) // 注意这里不能取当前点的情况
					if(board[i][j] == 1 || board[i][j] == 2)
						count++;
			}
		}
		return count;
	}
}
```
