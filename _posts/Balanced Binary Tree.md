---
title: Balanced Binary Tree.java
date: 
categories: LeetCode
tags: [Medium,重做,平衡二叉树]
---
Given a binary tree, determine if it is height-balanced.
For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.
<!-- more -->
**平衡二叉树：**
它是一 棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树。
**递归思路：**
分别判断左右两棵子树是不是平衡二叉树，如果都是并且左右两颗子树的高度相差不超过1，那么这棵树就是平衡二叉树。
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
    public boolean isBalanced(TreeNode root) {
        if(root == null) return true;
        
        if(Math.abs(depth(root.left)-depth(root.right))>1){
            return false;
        }
        return isBalanced(root.left)&&isBalanced(root.right);
    }
    public int depth(TreeNode root){//求二叉树的深度；
        if(root == null) {
            return 0;
        }
        return 1+Math.max(depth(root.left),depth(root.right));
    }
}
``` 