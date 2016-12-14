---
title: Delete Node in a Linked List.java
date: 
categories: LeetCode
tags: Easy
---
Write a function to delete a node (except the tail) in a singly linked list, given only access to that node.
Supposed the linked list is 1 -> 2 -> 3 -> 4 and you are given the third node with value 3, the linked list should become 1 -> 2 -> 4 after calling your function.
<!-- more -->
**思路：**
乍一看没法获取上一个链表节点，似乎无法将当前结点去除。实际上只要将下一个节点的值覆盖当前节点，然后删除下一个节点就好了。注意这样不适用于尾节点。
``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
public class Solution {
    public void deleteNode(ListNode node) {
        node.val = node.next.val;
        node.next = node.next.next;
    }
}
``` 

