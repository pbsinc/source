---
title: Reverse Linked List II.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Reverse a linked list from position m to n. Do it in-place and in one-pass.
For example:
Given 1->2->3->4->5->NULL, m = 2 and n = 4,
return 1->4->3->2->5->NULL.
Note:
Given m, n satisfy the following condition:
1 ≤ m ≤ n ≤ length of list.
<!-- more -->
**思路：**
就我目前学习到的，有用指向指针的指针的，有插入，有逆转的。
但是所有方法的核心，都其实是一个算法，就是利用3个指针修改区间内的节点的next值，且要保证已修改的链表是可以被找到的，即可以连入原链表中。

假设有一个链表有1,2,3,4,5,6，6个节点。m=2，n=5。

我们添加一个dummy节点，方便操作第一个节点。
![.](http://images.cnitblog.com/i/546654/201404/072244468407048.jpg)
首先让pre指向第m个节点前面那个，cur指向第m个节点，p1指向m的下一个节点，因为p1有可能是NULL，所以当p1不是NULL的时候，p2指向p1的下一个节点。

上图画出了这几个指针指向情况的开始状态和我们希望的终止状态。

在最终状态下，再通过其中方框中的两步就可以我们需要的链表了。

而cur，p1，p2三个指针在区间内向前移动并且将p1的next指向cur的过程则包含在一个for循环内部。如下：
![.](http://images.cnitblog.com/i/546654/201404/072249466684683.jpg)
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
    public ListNode reverseBetween(ListNode head, int m, int n) {
        
		ListNode dummy = new ListNode(0);
		dummy.next = head;
		ListNode pre = dummy, cur = head;
		//保证起始时，cur在起点处,pre是它的上一个节点，因此需要虚构一个dummy节点；
		for(int i=1;i<m;i++){
			pre = cur;
			cur = cur.next;
		}
		//制作p1始终是cur的next，p2始终是p1的next；
		ListNode p1 = cur.next;
		ListNode p2 = null;
		if(p1 != null) p2 = p1.next;
		//至此，cur指到了m节点处，pre始终保持在m节点的上一位，以下是对m到n节点转向；
		for(int i=m;i<n;i++){
			p1.next = cur;
			cur = p1;
			p1 = p2;
			if(p2!= null) //这里不能是p2.next！
			    p2 = p2.next;
		}
		//将三部分连接起来
		pre.next.next = p1;
		pre.next = cur;
		
		return dummy.next;//直接返回pre是错误的，因为m=1时，pre在虚构的节点dummy处！
    }
}
``` 
转自：
http://www.cnblogs.com/4everlove/p/3651002.html

