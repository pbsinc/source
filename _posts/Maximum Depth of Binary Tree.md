---
title: Maximum Depth of Binary Tree.java
date: 
categories: LeetCode
tags: Easy
---
Given a binary tree, find its maximum depth.
The maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.
<!-- more -->
**遍历法**
非递归，用DFS 深度遍历的方法；
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
    private int depth;
    public int maxDepth(TreeNode root) {
        depth = 0;
        helper(root,1);
        return depth;
    }
    public void helper(TreeNode node, int curtdepth){
        if(node == null){
            return;
        }
        if(curtdepth > depth){
            depth = curtdepth;
        }
        
        helper(node.left,curtdepth + 1);
        helper(node.right,curtdepth + 1);
    }
}
```	
用BFS 广度遍历的方法；
``` java
public class Solution {
    private int depth;
    public int maxDepth(TreeNode root) {
        depth = 0;
        helper(root,1);
        return depth;
    }
    public void helper(TreeNode node, int curtdepth){
        if(node == null){
            return;
        }
        if(curtdepth > depth){
            depth = curtdepth;
        }
        
        helper(node.left,curtdepth + 1);
        helper(node.right,curtdepth + 1);
    }
}
```
**递归思路:**
返回左子树或者右子树中大的深度加1，作为自己的深度即可，代码如下： 
``` java
public int maxDepth(TreeNode root) {  
    if(root == null)  
        return 0;  
    return Math.max(maxDepth(root.left),maxDepth(root.right))+1;  
}  
```	
**此人对树总结很好:**
http://blog.csdn.net/linhuanmars/article/category/2336231 