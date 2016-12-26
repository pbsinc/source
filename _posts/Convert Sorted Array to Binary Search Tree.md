---
title: Convert Sorted Array to Binary Search Tree.java
date: 
categories: LeetCode
tags: Medium
---
Given an array where elements are sorted in ascending order, convert it to a height balanced BST.
<!-- more -->
**递归思路：**
找到中间的值作为node,左边左子树，右边右子树；
构建二叉查找树
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
    public TreeNode sortedArrayToBST(int[] nums) {
        if(nums.length == 0) return null;
        return build(nums,0,nums.length);
    }
    public TreeNode build(int[] nums, int start, int end) {
        if(start == end)
            return null;
        
        TreeNode root = new TreeNode(nums[start+(end-start)/2]);
        root.left = build(nums,start,start+(end-start)/2);
        root.right = build(nums,start+(end-start)/2+1,end);
        return root;
    }    
}
``` 