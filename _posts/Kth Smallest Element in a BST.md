---
title: Kth Smallest Element in a BST.java
date: 
categories: LeetCode
tags: Medium
---
Given a binary search tree, write a function kthSmallest to find the kth smallest element in it.
Note: 
You may assume k is always valid, 1 ≤ k ≤ BST's total elements.
Follow up:
What if the BST is modified (insert/delete operations) often and you need to find the kth smallest frequently? How would you optimize the kthSmallest routine?
Hint:

	Try to utilize the property of a BST.
	What if you could modify the BST node's structure?
	The optimal runtime complexity is O(height of BST).
<!-- more -->
**递归思路：**
顺序遍历二叉查找树，返回第k个元素即可；
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
    public int kthSmallest(TreeNode root, int k) {
        List<Integer> tree = new ArrayList<Integer>();
        travel(tree,root);
        return tree.get(k-1);
        
    }
    public void travel(List<Integer> tree, TreeNode root){
        if(root == null) return;
        
        if(root.left != null){
            travel(tree,root.left);
        }
        
        tree.add(root.val);
        
        if(root.right != null){
            travel(tree,root.right);
        }
    }
}
``` 