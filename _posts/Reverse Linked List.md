---
title: Reverse Linked List.java
date: 
categories: LeetCode
tags: Easy
---
Reverse a singly linked list.
Hint:
A linked list can be reversed either iteratively or recursively. Could you implement both?
<!-- more -->
**思路：**
直接反向再构建一个list就好；
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
    public ListNode reverseList(ListNode head) {
        if (head == null) return null;
		ListNode res = new ListNode(0);

		while(head != null){
			
			res.val = head.val;
			ListNode upper = new ListNode(0);
			upper.next = res;
			res = upper;
			head = head.next;
			
		}
		
		return res.val == 0 ?res.next : res;
    }
}
``` 

