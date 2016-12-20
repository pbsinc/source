---
title: Rotate List.java
date: 
categories: LeetCode
tags: Medium
---
Given a list, rotate the list to the right by k places, where k is non-negative.
For example:
Given 1->2->3->4->5->NULL and k = 2,
return 4->5->1->2->3->NULL.
<!-- more -->
**思路：**
先算出链表的总长度，减去k为n，head走n步就是断点的位置。
``` java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }  123
 */
public class Solution {
    public ListNode rotateRight(ListNode head, int k) {
        if(head == null || k == 0) return head;
        
        ListNode copy = head;
        int count =1;
        while(copy.next != null){
            copy = copy.next;
            count++;
        }
        if(k % count == 0)return head;
        count -= k%count;
        
        ListNode tail = head;
        while(count>1){
            tail = tail.next;
            count--;
        }
        
        ListNode res = tail.next;
        tail.next = null;
        copy.next = head;
        
        return res;
    }
}
``` 
