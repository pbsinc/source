---
title: Construct Binary Tree from Inorder and Postorder Traversal.java
date: 
categories: LeetCode
tags: [Medium,后序遍历,中序遍历]
---
Given inorder and postorder traversal of a tree, construct the binary tree.
Note:
You may assume that duplicates do not exist in the tree.
<!-- more -->
**思路：**
``` java
     1       
    / \   
   2   3   
  / \ / \   
 4  5 6  7
```
 
后序遍历与上一题思路基本一致！
 
对于上图的树来说，
          index: 0 1 2 3 4 5 6
     先序遍历为: 1 2 4 5 3 6 7 
     中序遍历为: 4 2 5 1 6 3 7
	 后序遍历为: 4 5 2 6 7 3 1

可以发现的规律是：
1. **先序遍历的从左数第一个为整棵树的根节点**。
2. **中序遍历中根节点是左子树右子树的分割点**。
3. **后序遍历中根节点是最后一位**。


再看这个树的左子树：
     先序遍历为: 2 4 5 
     中序遍历为: 4 2 5
	 后序遍历为: 4 5 2
依然可以套用上面发现的规律。

右子树：
     先序遍历为: 3 6 7 
     中序遍历为: 6 3 7
	 后序遍历为: 6 7 3
也是可以套用上面的规律的。

所以这道题可以用递归的方法解决。
具体解决方法是：
通过**后序遍历**找到最后一个点作为**根节点**，在**中序遍历**中**找到根节点**并记录index。
因为中序遍历中根节点左边为左子树，所以可以记录左子树的长度并在后序遍历中依据这个长度找到左子树的区间，用同样方法可以找到右子树的区间。
递归的建立好左子树和右子树就好。
``` java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
public class Solution {
    public TreeNode buildTree(int[] inorder, int[] postorder) {
       int prelength = postorder.length;
	   int inlength = inorder.length;
	   return buildtree(postorder,0,prelength-1,inorder,0,inlength-1);
    }
	
	public TreeNode buildtree(int[] postorder,int postStart,int postEnd,int[] inorder,int inStart,int inEnd){
		if(inStart > inEnd || postStart > postEnd)
             return null;
		
		int rootVal = postorder[postEnd];
		int inrootindex =0; 
		for(int i=inStart;i<=inEnd;i++){
			if(inorder[i] == rootVal){
				inrootindex = i;
				break;
			}
		}
		
		int len = inrootindex - inStart;
		TreeNode root = new TreeNode(rootVal);
		
		root.left = buildtree(postorder,postStart,postStart+len-1,inorder,inStart,inrootindex-1);
		root.right = buildtree(postorder,postStart+len,postEnd-1,inorder,inrootindex+1,inEnd);
		
		return root;
	}
}
```