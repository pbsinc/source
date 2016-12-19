---
title: Intersection of Two Linked Lists.java
date: 
categories: LeetCode
tags: Easy
---
Write a program to find the node at which the intersection of two singly linked lists begins.
For example, the following two linked lists:

	A:          a1 → a2
					   ↘
						 c1 → c2 → c3
					   ↗            
	B:     b1 → b2 → b3
begin to intersect at node c1.
Notes:

	If the two linked lists have no intersection at all, return null.
	The linked lists must retain their original structure after the function returns.
	You may assume there are no cycles anywhere in the entire linked structure.
	Your code should preferably run in O(n) time and use only O(1) memory.
<!-- more -->
**思路1：**
先求根据长度差；
1, Get the length of the two lists.

2, Align them to the same start point.

3, Move them together until finding the intersection point, or the end null
``` java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    int lenA = length(headA), lenB = length(headB);
    // move headA and headB to the same start point
    while (lenA > lenB) {
        headA = headA.next;
        lenA--;
    }
    while (lenA < lenB) {
        headB = headB.next;
        lenB--;
    }
    // find the intersection until end
    while (headA != headB) {
        headA = headA.next;
        headB = headB.next;
    }
    return headA;
}

private int length(ListNode node) {
    int length = 0;
    while (node != null) {
        node = node.next;
        length++;
    }
    return length;
}
``` 

**思路2：**
不用求长度；
设置两个指针：达末尾后分别更换链表继续循环；
如长度不同，第二次循环即可保持同步。
``` java
public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
    //boundary check
    if(headA == null || headB == null) return null;
    
    ListNode a = headA;
    ListNode b = headB;
    
    //if a & b have different len, then we will stop the loop after second iteration
    while( a != b){
    	//for the end of first iteration, we just reset the pointer to the head of another linkedlist
        a = a == null? headB : a.next;
        b = b == null? headA : b.next;    
    }
    
    return a;
}
``` 
