---
title: Length of Last Word.java
date: 
categories: LeetCode
tags: Easy
---
Given a string s consists of upper/lower-case alphabets and empty space characters ' ', return the length of last word in the string.
If the last word does not exist, return 0.
Note: A word is defined as a character sequence consists of non-space characters only.
For example, 
Given s = "Hello World",
return 5.
<!-- more -->
**思路：**
逆序开始，记录非空格符号的个数，即为所求。
``` java
public class Solution {
    public int lengthOfLastWord(String s) {
        char[] chars = s.toCharArray(); 
		int count = 0;
		boolean last = false;
		for(int i=chars.length-1;i>=0;i--){
			if(chars[i] == ' '){
				if(last)
					break;
				continue;
			}
			else{
				last = true;
				count++;
			}
		}
		return count;
    }
}
```