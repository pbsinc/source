---
title: Add Two Numbers.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
You are given two linked lists representing two non-negative numbers. 
The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
<!-- more -->
**思路：**
1.因为存储是反过来的，即数字342存成2->4->3，所以要注意进位是向后的；
2.链表l1或l2为空时，直接返回，这是边界条件，省掉多余的操作；
3.链表l1和l2长度可能不同，因此要注意处理某个链表剩余的高位；
4.2个数相加，可能会产生最高位的进位，因此要注意在完成以上1－3的操作后，判断进位是否为0，不为0则需要增加结点存储最高位的进位。
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
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
		ListNode c1 = l1;
        ListNode c2 = l2;
		ListNode res = new ListNode(0);
		ListNode restmp = res;// restmp 是res的引用(同一个地址)，后面对restmp的操作都直接改变着res!
		
		int temp = 0;		
		while(c1 != null || c2 != null){
			temp /= 10;
            if (c1 != null) {
                temp += c1.val;
                c1 = c1.next;
            }
            if (c2 != null) {
                temp += c2.val;
                c2 = c2.next;
            }
			restmp.next = new ListNode(temp%10);
			restmp = restmp.next;
		}
		if (temp / 10 == 1)
            restmp.next = new ListNode(1);
		
		return res.next;
    }
}
``` 
