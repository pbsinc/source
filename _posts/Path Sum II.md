---
title: Path Sum II.java
date: 
categories: LeetCode
tags: Medium
---
Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.
For example:
Given the below binary tree and sum = 22,

				  5
				 / \
				4   8
			   /   / \
			  11  13  4
			 /  \    / \
			7    2  5   1
return
[[5,4,11,2],[5,8,4,5]]
<!-- more -->
**递归思路：**
计算所有可能路径,顺便把路径记录下来即可；
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
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> ans = new LinkedList<List<Integer>>();
        List<Integer> sublist = new LinkedList<Integer>();
        
        calcu(ans,sublist,root,sum,0);
        return ans;
    }
    public void calcu(List<List<Integer>> ans, List<Integer> sublist, TreeNode root, int sum, int count){
        if(root == null) return;
        sublist.add(root.val);
        count += root.val;
        
        if(root.left == null && root.right == null){
            if(count == sum){
                ans.add(new LinkedList<Integer>(sublist));//不能直接Add(sublist)；而是要add它的新的对象！
            }
        }
        if(root.left != null){
            calcu(ans,sublist,root.left, sum, count);
        }
        if(root.right != null){
            calcu(ans,sublist,root.right, sum, count);
        }
        sublist.remove(sublist.size()-1);//注意要删除掉最后不符合的节点
    }
}
``` 