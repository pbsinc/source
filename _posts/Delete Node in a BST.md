---
title: Delete Node in a BST.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Given a root node reference of a BST and a key, delete the node with the given key in the BST. Return the root node reference (possibly updated) of the BST.
Basically, the deletion can be divided into two stages:
Search for a node to remove.
If the node is found, delete the node.
Note: Time complexity should be O(height of tree).
Example:
root = [5,3,6,2,4,null,7]
key = 3

		5
	   / \
	  3   6
	 / \   \
	2   4   7

Given key to delete is 3. So we find the node with value 3 and delete it.

One valid answer is [5,4,6,2,null,null,7], shown in the following BST.

		5
	   / \
	  4   6
	 /     \
	2       7

Another valid answer is [5,2,6,null,4,null,7].

		5
	   / \
	  2   6
	   \   \
		4   7
<!-- more -->
**思路：**
首先搜索到delete的node，然后分三种情况讨论，
1.左边空，return 右边

2.右边空，return 左边

3.左右两边都不空，找到最右边的最小值，然后把root改成最右边的最小值，然后delete那个最右边的最小node。
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
    public TreeNode deleteNode(TreeNode root, int key) {  
        if(root == null) return root;  
        if(root.val > key){  
            root.left = deleteNode(root.left, key);  
        } else if(root.val < key){  
            root.right = deleteNode(root.right, key);  
        } else { // root.val == key;  
            if(root.left == null){  
                return root.right;  
            } else if(root.right == null){  
                return root.left;  
            } else {  
                root.val = minLeftNodeValue(root.right);  
                root.right = deleteNode(root.right, root.val);  
            }  
        }  
        return root;  
    }  
    
    public int minLeftNodeValue(TreeNode root){  
        TreeNode node = root;  
        while(node.left != null){  
            node = node.left;  
        }  
        return node.val;  
    }  
}  
``` 