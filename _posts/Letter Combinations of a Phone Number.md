---
title: Letter Combinations of a Phone Number.java
date: 
categories: LeetCode
tags: [Medium, 重做]
---
Given a digit string, return all possible letter combinations that the number could represent.
A mapping of digit to letters (just like on the telephone buttons) is given below.
![这里写图片描述](http://upload.wikimedia.org/wikipedia/commons/thumb/7/73/Telephone-keypad2.svg/200px-Telephone-keypad2.svg.png)
Input:Digit string "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
Note:
Although the above answer is in lexicographical order, your answer could be in any order you want.
<!-- more -->
**思路：**
递归法解题之。从第一个字符串开始确认结果，然后递归地确认余下各项结果。
``` java
public class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> res = new ArrayList<String>();
		String[] map = new String[] { "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz" };
		char[] tmp = new char[digits.length()];
		
		if(digits.length() == 0)
			return res;
		
		rec(digits,0,tmp,map,res);
		return res;
    }
	
	public void rec(String digits,int index,char[] tmp,String[] map,List<String> res){
		if(index == digits.length()){
			res.add(new String(tmp));
			return;
		}
		for(int i=0;i<map[digits.charAt(index) - '0'].length();i++){
			tmp[index] = map[digits.charAt(index) - '0'].charAt(i);
			rec(digits, index + 1, tmp, map, res);
		}
	}
}

```