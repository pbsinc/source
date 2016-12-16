---
title: Insertion Sort List.java
date: 
categories: LeetCode
tags: Medium
---
Sort a linked list using insertion sort.
<!-- more -->
**思路：**
制作两个dummy节点；
每次找出值最小的节点，作为答案的next;
保存值最小的节点的左右节点：left,right；
每次找出最小的以后，left.next = right;
使剩余的节点跳过已经找出的节点；
剩余节点为空时，说明都找完了，跳出循环。
不过89ms，有点慢...
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
    public ListNode insertionSortList(ListNode head) {
        if(head == null) return null;
        
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        ListNode res = dummy;
        ListNode dummy2 = new ListNode(0);
        dummy2.next = head;
        
        while(true){
            
            ListNode temp = dummy2;
            int flag = Integer.MAX_VALUE;
            
            ListNode left = null, right = null;
            
            while(temp.next != null){
                
                if(temp.next.val <= flag){
                    dummy.next = temp.next;
                    flag = temp.next.val;
                    left = temp;
                    right = temp.next.next;
                }
                temp = temp.next;
            }
            left.next = right;
            dummy = dummy.next;
            
            if(dummy2.next == null) break;
        }
        return res.next;
    }
}
``` 
