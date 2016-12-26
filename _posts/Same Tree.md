---
title: Same Tree.java
date: 
categories: LeetCode
tags: Easy
---
Given two binary trees, write a function to check if they are equal or not.
Two binary trees are considered equal if they are structurally identical and the nodes have the same value.
<!-- more -->
**递归思路：**
直接每个节点是否相等即可；
若相等，返回递归的下一层是否相等。
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
    public boolean isSameTree(TreeNode p, TreeNode q) {
        if(p==null && q==null){
            return true;
        }
        
        if(p!=null && q!=null && p.val==q.val){
            return isSameTree(p.left,q.left) && isSameTree(p.right,q.right);
        }
        
        return false;
    }
}
``` 