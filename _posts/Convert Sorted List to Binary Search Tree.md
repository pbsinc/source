---
title: Convert Sorted List to Binary Search Tree.java
date: 
categories: LeetCode
tags: [Medium,重做,二叉查找树]
---
Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.
<!-- more -->
**二叉查找树：**
二叉查找树（Binary Search Tree），亦称二叉排序树（Binary Sort Tree），又称二叉搜索树。
二叉查找树或者是一棵空树，或者是具有下列性质的二叉树：

	（1）若左子树不空，则左子树上所有结点的值均小于它的根结点的值；
	（2）若右子树不空，则右子树上所有结点的值均大于或等于它的根结点的值；
	（3）左、右子树也分别为二叉排序树；
**思路：**
首先将链表从中间分开；
最中间的node就是根节点；
左边的一半就是左子树，右边的一半就是右子树；
然后将左边的一半链表当作新的链表，递归；
右边的一半同理。
``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
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
    public TreeNode sortedListToBST(ListNode head) {
        if(head==null) return null;
        return toBST(head,null);//最开始的起点和终点；
    }
    public TreeNode toBST(ListNode head, ListNode tail){
        ListNode slow = head;
        ListNode fast = head;
        if(head==tail) return null;
        
        while(fast!=tail&&fast.next!=tail){
            fast = fast.next.next;
            slow = slow.next;
        }
		//使得slow正好到达中点；
		
        TreeNode thead = new TreeNode(slow.val);
        thead.left = toBST(head,slow);//左边一半新的递归；
        thead.right = toBST(slow.next,tail);//右边一半新的递归；
        return thead;
    }
}
``` 
