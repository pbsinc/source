---
title: Populating Next Right Pointers in Each Node.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Given a binary tree

    struct TreeLinkNode {
      TreeLinkNode *left;
      TreeLinkNode *right;
      TreeLinkNode *next;
    }
Populate each next pointer to point to its next right node. If there is no next right node, the next pointer should be set to NULL.
Initially, all next pointers are set to NULL.
Note:
You may only use constant extra space.
You may assume that it is a perfect binary tree (ie, all leaves are at the same level, and every parent has two children).
For example,
Given the following perfect binary tree,

			 1
		   /  \
		  2    3
		 / \  / \
		4  5  6  7
After calling your function, the tree should look like:

			 1 -> NULL
		   /  \
		  2 -> 3 -> NULL
		 / \  / \
		4->5->6->7 -> NULL
<!-- more -->
**思路：**
把层次遍历修改一下，就是下面的解法了，我们使用2个循环，一个指针P1专门记录每一层的最左边节点，另一个指针P2扫描本层，把下一层的链接上。
下层链接完成后，将P1移动到它的左孩子即可。
这个算法的空间复杂度是O(1). 没有额外的空间。
``` java
/**
 * Definition for binary tree with next pointer.
 * public class TreeLinkNode {
 *     int val;
 *     TreeLinkNode left, right, next;
 *     TreeLinkNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void connect(TreeLinkNode root) {
        if(root == null) return;
        TreeLinkNode pre = root;
        TreeLinkNode cur = null;
        while(pre.left != null){
            cur = pre;
            while(cur != null){
                cur.left.next = cur.right;
                if(cur.next != null){
                    cur.right.next = cur.next.left;
                }
                cur = cur.next;
            }
            pre = pre.left;
        }
    }
}
``` 
**递归思路：**
们可以用递归处理左右子树. 这个算法可能会消耗O(h)的栈空间。
``` java
/* 试一下 recursion */
    public void connect(TreeLinkNode root) {
        if (root == null) {
            return;
        }
        
        rec(root);
    }

    public void rec(TreeLinkNode root) {
        if (root == null) {
            return;
        }

        if (root.left != null) {
            root.left.next = root.right;            
        }

        if (root.right != null) {
            root.right.next = root.next.left;
        }
        
        rec(root.left);
        rec(root.right);
    }
``` 