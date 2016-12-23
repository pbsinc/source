---
title: Path Sum.java
date: 
categories: LeetCode
tags: Easy
---
Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.
For example:
Given the below binary tree and sum = 22,

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \      \
        7    2      1
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
<!-- more -->
**递归思路：**
计算所有路径的和；
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
    boolean res = false;
    public boolean hasPathSum(TreeNode root, int sum) {
        if(root == null) return false;
        calcu(root,sum,root.val);
        return res;
    }
    public void calcu(TreeNode root, int sum, int count){
        if(root.left == null && root.right == null){
            if(count == sum){
                res = true;
            }
        }
        if(root.left != null){
            count += root.left.val;
            calcu(root.left, sum, count);
        }
        if(root.right != null){
            if(root.left != null){
                count -= root.left.val;
            }
            count += root.right.val;
            calcu(root.right, sum, count);
        }
    }
}
``` 