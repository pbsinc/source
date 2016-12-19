---
title: Sort List.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Sort a linked list in O(n log n) time using constant space complexity.
<!-- more -->
**思路：**
考虑到要求用O(nlogn)的时间复杂度和constant space complexity来sort list，自然而然想到了merge sort方法。同时我们还已经做过了merge k sorted list和merge 2 sorted list。这样这个问题就比较容易了。

不过这道题要找linkedlist中点，那当然就要用最经典的faster和slower方法，faster速度是slower的两倍，当faster到链尾时，slower就是中点，slower的next是下一半的开始点。

解决了这些问题，题目就很容易解出了。
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
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null)
            return head;
		
        ListNode fast = head;
        ListNode slow = head;
		
        while (fast.next != null && fast.next.next != null) {
            slow = slow.next;
            fast =  fast.next.next;
        }
        
		ListNode newh = slow.next;
		slow.next = null;//断开，分割为两个list
		
		//继续递归分割，无限分割；
		ListNode l1 = sortList(head);
		ListNode l2 = sortList(newh);
		
		//合并两个list，并排序好；
		return merge(l1, l2);
    }
    public ListNode merge(ListNode h1, ListNode h2) {
		ListNode res = new ListNode(0), dummy = res;
		
		while(h1 != null && h2 != null){
			if(h1.val < h2.val){
				res.next = h1;
				h1 = h1.next;
			}else{
				res.next = h2;
				h2 = h2.next;
			}
			res = res.next;			
		}
        if(h1 != null)
			res.next = h1;
		if(h2 != null)
			res.next = h2;
		
		return dummy.next;
    }
}
``` 
