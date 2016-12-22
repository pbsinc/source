---
title: Minimum Depth of Binary Tree.java
date: 
categories: LeetCode
tags: Easy
---
Given a binary tree, find its minimum depth.
The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
<!-- more -->
**思路：**
直接迭代，注意叶节点只有一个的情况；
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
    public int minDepth(TreeNode root) {
        return res(root,1);
    }
    public int res(TreeNode root, int count){
        if(root == null) return count-1;
        if(root.left == null && root.right == null) return count;
        if(root.left == null) return res(root.right,count+1);
        if(root.right == null) return res(root.left,count+1);
        return Math.min(res(root.left,count+1),res(root.right,count+1));
    }
}
``` 
