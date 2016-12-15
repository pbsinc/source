---
title: Linked List Cycle.java
date: 
categories: LeetCode
tags: [Easy,重做,快慢遍历的思路判断链表是否有环]
---
Given a linked list, swap every two adjacent nodes and return its head.
For example,
Given 1->2->3->4, you should return the list as 2->1->4->3.
Your algorithm should use only constant space. You may not modify the values in the list, only nodes itself can be changed.
<!-- more -->
**思路：**
题目很明确，给定一个链表，判断其中是否包含一个环路。有个额外的要求就是不使用额外的空间。

假如说没有这个额外的要求，有什么方法可以解决呢？创建一个哈希表，遍历链表中的每个元素，并将其假如哈希表中，
加入哈希表之前判断是否已经存在于哈希表中，如果存在，则说明存在环，否则就是不存在环。
但是，这个哈希表却占用了O(n)的空间，因此该思路不符合要求。

这里，我们可以使用**快慢遍历**的思路。两个引用都指向链表头，**从链表头开始遍历，慢引用每次前进一步，而快引用每次前进两步，**
**如果链表是有环路的，那么快引用终将追上慢引用；**
如果没有环路，那么遍历就会有结束的时候。代码如下：
``` java
/**
 * Definition for singly-linked list.
 * class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null) return false;
		ListNode fast = head, slow = head;
		
		//注意要三个才能成环，所以走到还剩两个node的时候，就肯定没有环了
		while(fast.next!= null && fast.next.next !=null){ 
			fast = fast.next.next;
			slow = slow.next;
			if(fast == slow)
				return true;
		}
		return false;
    }
}
``` 

