---
title: Remove Duplicates from Sorted List.java
date: 
categories: LeetCode
tags: Easy
---
Given a sorted linked list, delete all duplicates such that each element appear only once.
For example:

	Given 1->1->2, return 1->2.
	Given 1->1->2->3->3, return 1->2->3.
<!-- more -->
**思路：**
遇到跟上一个相同的跳过就好；
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
    public ListNode deleteDuplicates(ListNode head) {
        if(head == null) return head;
        ListNode res = head;
		
		int temp = head.val;

		while(head.next != null){

			if(temp == head.next.val){
				head.next = head.next.next;
			}else{
				head = head.next;
				temp = head.val;
			}
		}
		return res;
    }
}
``` 

