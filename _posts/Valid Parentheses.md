---
title: Valid Parentheses.java
date: 
categories: LeetCode
tags: [Easy, char和Character]
---
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.
<!-- more -->
**思路：**
遍历一遍，记录左括号的个数，并把所有左括号加入ArratList；
每次遇到右括号与ArrayList的最后一个比较：
相同：ArrayList remove 掉最后一个符号；
不同: return false;
``` java
public class Solution {
    public boolean isValid(String s) {
		boolean res = true;
		int left = 0;
		List<Character> templeft = new ArrayList<Character>(); //Character是char的对象类型；类似于int和Integer
		
        for(int i=0;i<s.length();i++){
			if(s.charAt(i) == ')' || s.charAt(i) == ']' || s.charAt(i) == '}'){
				if(i == 0 || left == 0) {
					res = false; 
					break;
				}
				char temp = ' ';
				if(s.charAt(i) == ')') temp = '(';
				if(s.charAt(i) == ']') temp = '[';
				if(s.charAt(i) == '}') temp = '{';
				if(templeft.get(left-1) != temp){
					res = false; 
					break;
				}
				templeft.remove(left-1);
				left--;
			}else{
				templeft.add(s.charAt(i));
				left++;
			}
			if(i == s.length()-1){
				if(left != 0){
					res = false; 
					break;
				}
			}
			
		}
		return res;
    }
}
```