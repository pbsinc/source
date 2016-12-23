---
title: Binary Tree Level Order Traversal II.java
date: 
categories: LeetCode
tags: Easy
---
Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).
For example:
Given binary tree [3,9,20,null,null,15,7],

		3
	   / \
	  9  20
		/  \
	   15   7
return its bottom-up level order traversal as:

	[
	  [15,7],
	  [9,20],
	  [3]
	]
<!-- more -->
**BFS思路：**
使用队列Queue，广度遍历二叉树，最后再逆序输出；或直接加在表头，就不需要反向了！
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
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        Queue<TreeNode> queue = new LinkedList<TreeNode>();
        List<List<Integer>> res = new LinkedList<List<Integer>>();
        
        if(root == null) return res;
        
        queue.offer(root);
        while(!queue.isEmpty()){
            int levelnum = queue.size();
            List<Integer> sublist = new LinkedList<Integer>();
            for(int i=0;i<levelnum;i++){
                if(queue.peek().left != null) queue.offer(queue.peek().left);
                if(queue.peek().right != null) queue.offer(queue.peek().right);
                sublist.add(queue.poll().val);
            }
            //res.add(sublist);
			res.add(0, subList);//直接加在表头，不用以下反向！
        }
        
        //List<List<Integer>> ans = new LinkedList<List<Integer>>();
        //for(int i=res.size()-1;i>=0;i--){
        //    ans.add(res.get(i));
        //}
		
        return res;
    }
}
``` 
**DFS思路：**
使用迭代，深度遍历二叉树；
``` java
public class Solution {
        public List<List<Integer>> levelOrderBottom(TreeNode root) {
            List<List<Integer>> wrapList = new LinkedList<List<Integer>>();
            levelMaker(wrapList, root, 0);
            return wrapList;
        }
        
        public void levelMaker(List<List<Integer>> list, TreeNode root, int level) {
            if(root == null) return;
            if(level >= list.size()) {
                list.add(0, new LinkedList<Integer>());
            }
            levelMaker(list, root.left, level+1);
            levelMaker(list, root.right, level+1);
            list.get(list.size()-level-1).add(root.val);
        }
    }
``` 
