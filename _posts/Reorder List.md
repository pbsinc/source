---
title: Reorder List.java
date: 
categories: LeetCode
tags: Medium
---
Given a singly linked list L: L0→L1→…→Ln-1→Ln,
reorder it to: L0→Ln→L1→Ln-1→L2→Ln-2→…
You must do this in-place without altering the nodes' values.
For example,
Given {1,2,3,4}, reorder it to {1,4,2,3}.
<!-- more -->
**思路：**
找到链表的中点，将后半部分反转，然后将两部分一个一个连接起来；
注意端点处断开操作，和循环判断条件。
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
    public void reorderList(ListNode head) {
        if(head == null || head.next == null || head.next.next == null) return ;
        
        ListNode slow = head, fast = head;
        
        while(fast.next != null && fast.next.next != null){
            slow = slow.next;
            fast = fast.next.next;
        }
        
        ListNode newhead = slow.next;//后半部分起点
        slow.next = null;//前后半部分断开
        ListNode p1 = newhead, p2 = p1.next, temp = p2;

        while(temp != null){//反转后半部分的顺序
            temp = temp.next;
            p2.next = p1;
            p1 = p2;
            p2 = temp;
        }
        newhead.next = null;//要断开，否则就是死循环
        ListNode temphead = head.next;
        
        while(p1 != null){
            
            temphead = head.next;
            
            head.next = p1;
            
            p1 = p1.next;

            head = head.next;
            
            head.next = temphead;
            
            head = head.next;
        }
    }
}
``` 
