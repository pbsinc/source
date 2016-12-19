---
title: Partition List.java
date: 
categories: LeetCode
tags: Medium
---
Given a linked list and a value x, partition it such that all nodes less than x come before nodes greater than or equal to x.
You should preserve the original relative order of the nodes in each of the two partitions.
For example,

	Given 1->4->3->2->5->2 and x = 3,
	return 1->2->2->4->3->5.
<!-- more -->
**思路：**
与Insertion Sort List思路类似，制作两个dummy节点；
每次找出值小于x的节点，作为答案的next，保存此节点的左右节点：left,right；
每次找出值小于x的节点以后，left.next = right，使剩余的节点跳过已经找出的节点；
最后将剩余节点连在答案最后。
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
    public ListNode partition(ListNode head, int x) {
        if(head == null) return null;

        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode res = dummy;
        
        ListNode dummy2 = new ListNode(0);
        dummy2.next = head;
        
        ListNode left = dummy2, right = dummy2.next.next;

        while(left.next != null){
            if(left.next.val < x){
                dummy.next = left.next;
                left.next = right;
                dummy = dummy.next;
            }else{
                left = left.next;
            }
            if(right != null){
                right = right.next;
            }
        }
        dummy.next = dummy2.next;
        return res.next;
    }
}

``` 
