---
title: Binary Tree Preorder Traversal.java
date: 
categories: LeetCode
tags: Medium
---
Given a binary tree, return the inorder traversal of its nodes' values.
For example:
Given binary tree [1,null,2,3],

	   1
		\
		 2
		/
	   3
return [1,2,3].
Note: Recursive solution is trivial, could you do it iteratively?
<!-- more -->
**递归思路：**
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
    public List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<Integer>();
        if(root != null) calculate(res,root);
        return res;
    }
    public void calculate(List<Integer> res, TreeNode root){
        res.add(root.val);
        if(root.left != null){
            calculate(res, root.left);
        }
        if(root.right != null){
            calculate(res, root.right);
        }
    }
}
``` 
**迭代思路：**
利用栈，自上而下保存右子树节点，当左子树全部遍历完成之后，栈顶特就是当前左子树恰好对应的右子树；
``` java
public List<Integer> preorderTraversal(TreeNode node) {
	List<Integer> list = new LinkedList<Integer>();
	Stack<TreeNode> rights = new Stack<TreeNode>();
	while(node != null) {
		list.add(node.val);
		if (node.right != null) {
			rights.push(node.right);
		}
		node = node.left;
		if (node == null && !rights.isEmpty()) {
			node = rights.pop();
		}
	}
    return list;
}
``` 
