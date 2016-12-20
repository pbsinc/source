---
title: Invert Binary Tree.java
date: 
categories: LeetCode
tags: Easy
---
Invert a binary tree.

		 4
	   /   \
	  2     7
	 / \   / \
	1   3 6   9
	to
		 4
	   /   \
	  7     2
	 / \   / \
	9   6 3   1
<!-- more -->
**思路：**
直接迭代，每次左右互换就好；
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
    public TreeNode invertTree(TreeNode root) {
        if(root == null || root.left == null && root.right == null){
            return root;
        }
        TreeNode temp = root.left;

        root.left = invertTree(root.right);
        root.right = invertTree(temp);   
        
        return root;
    }
}
``` 
