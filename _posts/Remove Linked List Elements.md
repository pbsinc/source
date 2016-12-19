---
title: Remove Linked List Elements.java
date: 
categories: LeetCode
tags: Easy
---
Remove all elements from a linked list of integers that have value val.
Example，

	Given: 1 --> 2 --> 6 --> 3 --> 4 --> 5 --> 6, val = 6
	Return: 1 --> 2 --> 3 --> 4 --> 5
<!-- more -->
**思路：**
移除等于val的node即可；
把此node的left的next设为此node的right即可；
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
    public ListNode removeElements(ListNode head, int val) {
        if(head == null) return null;
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode left = dummy, right = dummy.next.next;

        while(left.next != null){
            if(left.next.val == val){
                left.next = right;
            }else{
                left = left.next;
            }
            if(right != null){
                right = right.next;
            }
        }
        return dummy.next;
    }
}
``` 
