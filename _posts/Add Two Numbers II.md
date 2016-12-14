---
title: Add Two Numbers II.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
You are given two linked lists representing two non-negative numbers. The most significant digit comes first and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
You may assume the two numbers do not contain any leading zero, except the number 0 itself.
Follow up:
What if you cannot modify the input lists? In other words, reversing the lists is not allowed.
Example:

	Input: (7 -> 2 -> 4 -> 3) + (5 -> 6 -> 4)
	Output: 7 -> 8 -> 0 -> 7
<!-- more -->
**using Stack思路：**
利用Stack先进后出的特点；
``` java
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        Stack<Integer> s1 = new Stack<Integer>();
        Stack<Integer> s2 = new Stack<Integer>();
        
        while(l1 != null) {
            s1.push(l1.val);
            l1 = l1.next;
        };
        while(l2 != null) {
            s2.push(l2.val);
            l2 = l2.next;
        }
        
        int sum = 0;
        ListNode list = new ListNode(0);
        while (!s1.empty() || !s2.empty()) {
            if (!s1.empty()) sum += s1.pop();
            if (!s2.empty()) sum += s2.pop();
            list.val = sum % 10;
            ListNode head = new ListNode(sum / 10);
            head.next = list;  //从后往前加！
            list = head;
            sum /= 10;
        }
        
        return list.val == 0 ? list.next : list;
    }
}
``` 

**我的思路：**
转成int后加和，再填入ListNode;
数字非常大时，越界，所以不对；
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
        int num1=0,num2=0;
        while(l1!= null){
            num1 = num1*10 + l1.val;
            l1 = l1.next;
        }
            
        while(l2!= null){
            num2 = num2*10 + l2.val;
            l2 = l2.next;
        }
            
        int sum = num1 + num2;
        
        ListNode res = new ListNode(0);
		ListNode ans = res;
        String str = String.valueOf(sum);
		int len = str.length();
		int flag = (int)Math.pow(10,len-1);
        while(sum != 0){
            res.next = new ListNode(sum/flag);
			sum = sum%flag;
			flag/=10;
			res = res.next;
        }
		return ans.next;
        
    }
}
``` 


