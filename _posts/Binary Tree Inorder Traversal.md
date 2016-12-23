---
title: Binary Tree Inorder Traversal.java
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
return [1,3,2].
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
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new LinkedList<Integer>();
        if(root != null) calculate(res,root);
        return res;
    }
    public void calculate(List<Integer> res, TreeNode root){
        if(root.left != null){
            calculate(res, root.left);
        }
        res.add(root.val);
        if(root.right != null){
            calculate(res, root.right);
        }
    }
}
``` 
**迭代思路：**
利用栈，自上而下保存左子树节点，当左子树全部遍历完成之后：
pop栈顶，加入list，进入右节点；
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
public List<Integer> inorderTraversal(TreeNode root) {
    List<Integer> list = new ArrayList<Integer>();

    Stack<TreeNode> stack = new Stack<TreeNode>();
    TreeNode cur = root;

    while(cur!=null || !stack.empty()){
        while(cur!=null){
            stack.add(cur);
            cur = cur.left;
        }
        cur = stack.pop();
        list.add(cur.val);
        cur = cur.right;
    }

    return list;
}
``` 
