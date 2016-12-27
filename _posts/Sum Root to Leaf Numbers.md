---
title: Sum Root to Leaf Numbers.java
date: 
categories: LeetCode
tags: Medium
---
Given a binary tree containing digits from 0-9 only, each root-to-leaf path could represent a number.
An example is the root-to-leaf path 1->2->3 which represents the number 123.
Find the total sum of all root-to-leaf numbers.
For example,

		1
	   / \
	  2   3
The root-to-leaf path 1->2 represents the number 12.
The root-to-leaf path 1->3 represents the number 13.
Return the sum = 12 + 13 = 25.
<!-- more -->
**思路：**
递归，一层一层往下乘，加和；
``` java
public int sumNumbers(TreeNode root) {
	return sum(root, 0);
}

public int sum(TreeNode n, int s){
	if (n == null) return 0;
	if (n.right == null && n.left == null) return s*10 + n.val;
	return sum(n.left, s*10 + n.val) + sum(n.right, s*10 + n.val);
}
``` 
**我的思路：**
存入后再读取；
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
    public int sumNumbers(TreeNode root) {
        List<List<String>> ans = new LinkedList<List<String>>();
        List<String> sublist = new LinkedList<String>();
        calcu(ans,sublist,root);
        int sum = 0;
        for(List<String> sbl : ans){
            String temp = "";
            for(String str : sbl){
                temp += str;
            }
            sum += Integer.parseInt(temp);
        }
        return sum;
    }
    public void calcu(List<List<String>> ans, List<String> sublist, TreeNode root){
        if(root == null) return;
        sublist.add(String.valueOf(root.val));
        
        if(root.left == null && root.right == null){
            ans.add(new LinkedList<String>(sublist));//不能直接Add(sublist)；而是要add它的新的对象！
        }
        if(root.left != null){
            calcu(ans,sublist,root.left);
        }
        if(root.right != null){
            calcu(ans,sublist,root.right);
        }
        sublist.remove(sublist.size()-1);//注意要删除掉最后不符合的节点
    }
}
``` 