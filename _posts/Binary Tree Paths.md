---
title: Binary Tree Paths.java
date: 
categories: LeetCode
tags: Easy
---
Given a binary tree, return all root-to-leaf paths.
For example, given the following binary tree:

	   1
	 /   \
	2     3
	 \
	  5
All root-to-leaf paths are:
["1->2->5", "1->3"]
<!-- more -->
**思路：**
递归遍历读取；
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
    public List<String> binaryTreePaths(TreeNode root) {
        List<String> list = new LinkedList<String>();
        String str = "";
        calcu(list,str,root);
        
        return list;
    }
    public void calcu(List<String> list,String str, TreeNode root){
        if(root == null) return;
        if(str == "")
            str += root.val;
        else
            str += "->" + root.val;
        
        if(root.left == null && root.right == null){
            list.add(str);
        }
        if(root.left != null){
            calcu(list,str,root.left);
        }
        if(root.right != null){
            calcu(list,str,root.right);
        }
    }
}
``` 