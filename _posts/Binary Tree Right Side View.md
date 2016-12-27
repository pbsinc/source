---
title: Binary Tree Right Side View.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Given a binary tree, imagine yourself standing on the right side of it, return the values of the nodes you can see ordered from top to bottom.
For example:
Given the following binary tree,

	   1            <---
	 /   \
	2     3         <---
	 \     \
	  5     4       <---
You should return [1, 3, 4].
<!-- more -->
**思路：**
The core idea of this algorithm:

1.Each depth of the tree only select one node.

2.View depth is current size of result list.
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
    public List<Integer> rightSideView(TreeNode root) {
        List<Integer> res = new ArrayList<Integer>();
        rightView(res,root,0);
        return res;
    }
    public void rightView(List<Integer> res, TreeNode curNode, int curDepth){
        if(curNode == null){
            return;
        }
        
        if(res.size() == curDepth){
            res.add(curNode.val);
        }
        
        rightView(res,curNode.right,curDepth+1);
        rightView(res,curNode.left,curDepth+1);
    }
}
``` 