---
title: Linked List Cycle.java
date: 
categories: LeetCode
tags: Easy
---
Given a linked list, remove the nth node from the end of list and return its head.
For example,

	Given linked list: 1->2->3->4->5, and n = 2.
	After removing the second node from the end, the linked list becomes 1->2->3->5.
Note:

	Given n will always be valid.
	Try to do this in one pass.
<!-- more -->
**思路：**
一遍往后走，对当前node往后走n次；
如果到头了；说明此node就是需要被删除的node；
加入dummy节点，因为表头可能成为被删除的节点；
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
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        ListNode res = dummy;
        dummy.next = head;
        
        while(true){
            ListNode temp = head;
            
            for(int i=0;i<n;i++){
                temp = temp.next;//对当前node往后走n次，判断是否到头
            }
            
            if(temp == null){//如果到头了；说明此node就是需要被删除的node；
                dummy.next = dummy.next.next;
                break;
            }
            head = head.next;
            dummy = dummy.next;
        }
        return res.next;
    }
}
``` 

