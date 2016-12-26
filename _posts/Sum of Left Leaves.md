---
title: Sum of Left Leaves.java
date: 
categories: LeetCode
tags: Easy
---
Find the sum of all left leaves in a given binary tree.
Example:

		3
	   / \
	  9  20
		/  \
	   15   7
There are two left leaves in the binary tree, with values 9 and 15 respectively. Return 24.
<!-- more -->
**递归思路：**
直接递归找到最端点的左叶节点即可；
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
    
    public int sumOfLeftLeaves(TreeNode root) {
        int l = 0,r=0;
        if(root == null) return 0;
        
        if(root.left != null){
            if(root.left.left == null && root.left.right == null){
                l = root.left.val;
            }else{
                l = sumOfLeftLeaves(root.left);
            }
        }
        if(root.right != null){
            r = sumOfLeftLeaves(root.right);
        }
        
        return l+r;
    }
}
``` 