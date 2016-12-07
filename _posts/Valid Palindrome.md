---
title: Valid Palindrome.java
date: 
categories: LeetCode
tags: [Easy,java统一字符串大小写,str.toLowerCase()方法,str.toUpperCase()方法,Character.isLetter(chr)方法,Character.isDigit(chr)方法]
---
Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.
For example,
"A man, a plan, a canal: Panama" is a palindrome.
"race a car" is not a palindrome.
Note:
Have you consider that the string might be empty? This is a good question to ask during an interview.
For the purpose of this problem, we define empty string as valid palindrome.
<!-- more -->
**思路：**
从前后同时两个指针开始比较即可；
先统一大小写：

	**str.toLowerCase()**转化成小写;
	**str.toUpperCase()**转化成大写。
	注意，这是它们的返回值变成小写或大写！
	必须要写成：str = str.toLowerCase();
	这种时候，str才统一变成小写！
跳过空格，符号；
数字不能跳过；
``` java
public class Solution {
    public boolean isPalindrome(String s) {
		if(s.length() == 0) return true;
		int start = 0, end = s.length()-1;
		boolean res = true;
		s=s.toLowerCase();
        while(start <= end){
			while(start < end && !Character.isLetter(s.charAt(start)) && !Character.isDigit(s.charAt(start))){
				start++;
			}
			while(start < end && !Character.isLetter(s.charAt(end)) && !Character.isDigit(s.charAt(end))){
				end--;
			}
			if(s.charAt(start) != s.charAt(end)){
				res = false;
				break;
			}else{
				start++;
				end--;
			}
		}
		return res;
    }
}
``` 

**附注：**

Character.isLetter(chr)方法：判断字符chr是否是字母

Character.isDigit(chr)方法：判断字符chr是否是数字