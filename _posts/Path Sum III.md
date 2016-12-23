---
title: Path Sum III.java
date: 
categories: LeetCode
tags: Easy
---
You are given a binary tree in which each node contains an integer value.

Find the number of paths that sum to a given value.

The path does not need to start or end at the root or a leaf, but it must go downwards (traveling only from parent nodes to child nodes).

The tree has no more than 1,000 nodes and the values are in the range -1,000,000 to 1,000,000.

Example:

root = [10,5,-3,3,2,null,11,3,-2,null,1], sum = 8

		  10
		 /  \
		5   -3
	   / \    \
	  3   2   11
	 / \   \
	3  -2   1

	Return 3. The paths that sum to 8 are:

	1.  5 -> 3
	2.  5 -> 2 -> 1
	3. -3 -> 11
<!-- more -->
**递归思路：**
计算所有可能路径即可；
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
    int res = 0;
    public int pathSum(TreeNode root, int sum) {
        if(root == null) return 0;
        alltravel(root,sum);
        return res;
    }
    public void calcu(TreeNode root, int sum, int count){
        
        if(count == sum){
            res ++;
        }
        if(root.left != null){
            count += root.left.val;
            calcu(root.left, sum, count);
        }
        if(root.right != null){
            if(root.left != null){
                count -= root.left.val;
            }
            count += root.right.val;
            calcu(root.right, sum, count);
        }
    }
    public void alltravel(TreeNode root,int sum){
        calcu(root, sum, root.val);
        if(root.left != null){
            alltravel(root.left,sum);
        }
        if(root.right != null){
            alltravel(root.right,sum);
        }
    }
}
``` 