---
title: Odd Even Linked List.java
date: 
categories: LeetCode
tags: Medium
---
Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.
You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.
Example:
Given 1->2->3->4->5->NULL,
return 1->3->5->2->4->NULL.
Note:
The relative order inside both the even and odd groups should remain as it was in the input. 
The first node is considered odd, the second node even and so on ...
<!-- more -->
**思路：**
把奇数和奇数连在一起；
偶数和偶数连在一起；
最后把偶数连在奇数的末尾；
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
    public ListNode oddEvenList(ListNode head) {
        if(head == null) return head;
		ListNode res = head;
		
		ListNode temp = head.next;
		ListNode even = temp;
		
		//这里要特别注意：这个条件可以保证到末尾（不论总共是奇数或偶数个node）就停止！
		while(temp!=null && temp.next!=null){
			head.next = temp.next;
			head = head.next;
			temp.next = head.next;
			temp = temp.next;
		}
		
		head.next = even;
		return res;
    }
}
``` 


