---
title: Symmetric Tree.java
date: 
categories: LeetCode
tags: [Easy,重做]
---
Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).
For example, this binary tree [1,2,2,3,4,4,3] is symmetric:

		1
	   / \
	  2   2
	 / \ / \
	3  4 4  3
But the following [1,2,2,null,3,null,3] is not:

		1
	   / \
	  2   2
	   \   \
	   3    3
<!-- more -->
**思路：**
递归；
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
    public boolean isSymmetric(TreeNode root) {
        return root==null || isSymmetricHelp(root.left, root.right);
    }
    
    private boolean isSymmetricHelp(TreeNode left, TreeNode right){
        if(left == null || right == null){
            return left == right;
        }
        if(left.val != right.val){
            return false;
        }
        return isSymmetricHelp(left.left,right.right)&&isSymmetricHelp(left.right,right.left);
    }
}
``` 
**我的思路：**
如果是满数，是对的；存在极端情况出错！
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
    public boolean isSymmetric(TreeNode root) {
        if(root == null) return true;
        
        List<Integer> list = new ArrayList<Integer>();
        
        inTravel(list, root);
        
        int len = list.size();
        
        if(len%2 == 0) return false;
        
        int mid = len/2, l = 0, r = len-1;
        
        while(l<mid && r >mid){
            if(list.get(l) != list.get(r)){
                return false;
            }
            l++;
            r--;
        }
        return true;
    }
    public void inTravel(List<Integer> list, TreeNode root){
        if(root.left != null){
            inTravel(list, root.left);
        }
        if(root.left == null && root.right != null){
            list.add(-1);
        }
        list.add(root.val);
        if(root.right != null){
            inTravel(list, root.right);
        }
        if(root.left != null && root.right == null){
            list.add(-1);
        }
    }
}
``` 