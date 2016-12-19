---
title: Palindrome Linked List.java
date: 
categories: LeetCode
tags: Easy
---
Given a singly linked list, determine if it is a palindrome.
Follow up:
Could you do it in O(n) time and O(1) space?
<!-- more -->
**思路：**
使用快速指针：fast = fast.next.next;找出一半的位置，将前一半反向；
前一半跟后一半比较即可，注意总共元素奇偶的影响；
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
    public boolean isPalindrome(ListNode head) {
		if(head == null) return true;
		
        ListNode p1 = head, p2 = p1.next, fast = head, temp = p2;
		
		while(fast.next != null && fast.next.next != null){
			fast = fast.next.next;
			temp = temp.next;
			p2.next = p1;
			p1 = p2;
			p2 = temp;
		}
		
		if(fast.next == null) {
            p1 = p1.next;
        }else {//even number of elements, do nothing}
		
		while(temp != null){
			
			if(p1.val != temp.val){
				return false;
			}
			
			p1 = p1.next;
			temp = temp.next;
		}
		return true;
    }
}
``` 
