---
title: Swap Nodes in Pairs.java
date: 
categories: LeetCode
tags: [Easy,重做]
---
Given a linked list, swap every two adjacent nodes and return its head.
For example,
Given 1->2->3->4, you should return the list as 2->1->4->3.
Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.
<!-- more -->
**思路：**
保存一下head.next.next;
**head = temp这种肯定是错误的**；
**只能通过.next来连接node**;
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
    public ListNode swapPairs(ListNode head) {
      if(head == null || head.next == null)
        return head;
    
      ListNode res = new ListNode(-1);
      ListNode ans = res;
      
      while(head!=null && head.next!=null){
          ListNode temp = head.next.next;
          
          res.next = head.next;
          res = res.next;
          res.next = head;
          res = res.next;
          
          head.next = temp;
          head = head.next; 
      }
    return ans.next;
  }
}
``` 

