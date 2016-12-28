---
title: Flatten Binary Tree to Linked List.java
date: 
categories: LeetCode
tags: Medium
---
Given a binary tree, flatten it to a linked list in-place.
For example,
Given

         1
        / \
       2   5
      / \   \
     3   4   6
The flattened tree should look like:

	   1
		\
		 2
		  \
		   3
			\
			 4
			  \
			   5
				\
				 6
Hints:
If you notice carefully in the flattened tree, each node's right child points to the next node of a pre-order traversal.
<!-- more -->
**思路：**
递归遍历读取后，再构建树；
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
    public void flatten(TreeNode root) {
        if(root == null) return;
        List<Integer> res = new ArrayList<Integer>();
        travel(res, root);
        root.left = null;
        int count = 0;
        for(Integer i : res){
            root.val = i;
            if(count == res.size()-1) break;
            root.right = new TreeNode(0);
            root = root.right;
            count++;
        }
    
    }
    public void travel(List<Integer> res, TreeNode root){
        res.add(root.val);
        if(root.left != null){
            travel(res,root.left);
        }
        if(root.right != null){
            travel(res,root.right);
        }
    }
}
``` 