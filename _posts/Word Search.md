---
title: Word Search.java
date: 
categories: LeetCode
tags: [Medium, String转char]
---
Given a 2D board and a word, find if the word exists in the grid.
The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. 
The same letter cell may not be used more than once.
For example,Given board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
word = "ABCCED", -> returns true,
word = "SEE", -> returns true,
word = "ABCB", -> returns false.
<!-- more -->
**思路：**
这道题分析看，就是一个词，在一行出现也是true，一列出现也是true，一行往下拐弯也是true，一行往上拐弯也是true，一列往左拐弯也是true，一列往右拐弯也是true。所以是要考虑到所有可能性，基本思路是使用DFS来对一个起点字母上下左右搜索，看是不是含有给定的Word。还要维护一个visited数组，表示从当前这个元素是否已经被访问过了，过了这一轮visited要回false，因为对于下一个元素，当前这个元素也应该是可以被访问的。
``` java
public class Solution {
    public boolean exist(char[][] board, String word) {
        char[] Cword = word.toCharArray(); //String 转 char[],此题也可使用使用 char chr = word.charAt(i);String的长度是word.length()，有括号！
		int m = board.length;
		int n = board[0].length;
        boolean[][] visited = new boolean[m][n];  
		for(int i=0;i<m;i++){
			for(int j=0;j<n;j++){
				if(find(board,i,j,Cword,0,visited)){
					return true;
				}
			}
		}
		return false;
    }
	public boolean find(char[][] board,int i,int j,char[] Cword,int count,boolean[][] visited){
		int m = board.length;
		int n = board[0].length;
		
		if(count == Cword.length) 
			return true;		
		if(i<0 || j<0 || i>=m || j>=n)
			return false;
		if (visited[i][j])  
            return false;  
		if(board[i][j] != Cword[count])
			return false;
        
		visited[i][j] = true;  
		// 注意这里的count+1; 如果改成count++ 则不对了；因为互相影响！
		boolean temp = find(board,i,j-1,Cword,count+1,visited) || find(board,i-1,j,Cword,count+1,visited) || find(board,i+1,j,Cword,count+1,visited) || find(board,i,j+1,Cword,count+1,visited);
		visited[i][j] = false;  //如果这个字母不能往下走了，说明此路不通，因此恢复未访问过。
		return temp;
	}
}
```