---
title: Validate Binary Search Tree.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Given a binary tree, determine if it is a valid binary search tree (BST).
Assume a BST is defined as follows:

	The left subtree of a node contains only nodes with keys less than the node's key.
	The right subtree of a node contains only nodes with keys greater than the node's key.
	Both the left and right subtrees must also be binary search trees.
Example 1:

		2
	   / \
	  1   3
Binary tree [2,1,3], return true.
Example 2:

		1
	   / \
	  2   3
Binary tree [1,2,3], return false.
<!-- more -->
**思路：**
``` java
public class Solution {
    public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }
    
    public boolean isValidBST(TreeNode root, long minVal, long maxVal) {
        if (root == null) return true;
        if (root.val >= maxVal || root.val <= minVal) return false;
        return isValidBST(root.left, minVal, root.val) && isValidBST(root.right, root.val, maxVal);
    }
}
``` 
http://blog.csdn.net/ljiabin/article/details/41699241