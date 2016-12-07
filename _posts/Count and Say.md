---
title: Count and Say.java
date: 
categories: LeetCode
tags: Easy
---
The count-and-say sequence is the sequence of integers beginning as follows:
1, 11, 21, 1211, 111221, ...

1 is read off as "one 1" or 11.
11 is read off as "two 1s" or 21.
21 is read off as "one 2, then one 1" or 1211.
Given an integer n, generate the nth sequence.
Note: The sequence of integers will be represented as a string.
<!-- more -->
**思路：**
递归的思想，直接读上一个（n-1）即可；
遍历元素，先输出元素重复出现的个数，再输出元素本身。
``` java
public class Solution {
    public String countAndSay(int n) {
		StringBuilder sb = new StringBuilder();
		if(n == 1)
			return "1";
		else{
			String last = countAndSay(n-1);
			for(int i=0;i<last.length();i++){
				int count = 1;
				while(i<last.length()-1 && last.charAt(i) == last.charAt(i+1)){
					count++;
					i++;
				}
				sb.append(count);
				sb.append(last.charAt(i));
			}
		}
		return sb.toString();
    }
}
```