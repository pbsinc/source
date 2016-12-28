---
title: Binary Tree Zigzag Level Order Traversal.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).
For example:
Given binary tree [3,9,20,null,null,15,7],

		3
	   / \
	  9  20
		/  \
	   15   7
return its zigzag level order traversal as:

	[
	  [3],
	  [20,9],
	  [15,7]
	]
<!-- more -->
**思路：**
给每一层构建一个sublist；
然后记录当前层数；
把本层的值add到对应的sublist中。
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
    public List<List<Integer>> zigzagLevelOrder(TreeNode root) 
    {
        List<List<Integer>> sol = new ArrayList<>();
        travel(root, sol, 0);
        return sol;
    }
    
    private void travel(TreeNode curr, List<List<Integer>> sol, int level)
    {
        if(curr == null) return;
        
        if(sol.size() <= level){
            List<Integer> sublist = new ArrayList<Integer>();
            sol.add(sublist);
        }
        
        List<Integer> collection = sol.get(level);
        if(level%2 == 0){
            collection.add(curr.val);
        }else{
            collection.add(0,curr.val);
        }
        travel(curr.left, sol, level+1);
        travel(curr.right, sol, level+1);
    }
}
``` 