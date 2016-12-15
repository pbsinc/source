---
title: Remove Duplicates from Sorted List II.java
date: 
categories: LeetCode
tags: Medium
---
Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only distinct numbers from the original list.
For example:

	Given 1->2->3->3->4->4->5, return 1->2->5.
	Given 1->1->1->2->3, return 2->3.
<!-- more -->
**思路：**
加入一个boolean判断当前node是否重复；
是的话直接跳过当前node；
到下一个node时，作为新的判断标准跟以后的node进行判断；
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
		ListNode dummy = new ListNode(0);
        ListNode res = dummy;
		
		dummy.next = head;
		
		int temp = head.val;
		
		boolean isdup = false;
		while(head.next != null){

			if(temp == head.next.val){
				head = head.next;
				isdup = true;
				if(head.next == null) res.next = head.next;
			}else{
				if(isdup){
					head = head.next;
					temp = head.val;
				}
				if(head.next == null || temp != head.next.val){
					res.next = head;
					res = res.next;
					isdup = false;
					if(head.next != null){
    					head = head.next;
    					temp = head.val;
					}
				}
			}
		}
		return dummy.next;
    }
}
``` 

