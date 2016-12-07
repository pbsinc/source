---
title: Repeated Substring Pattern.java
date: 
categories: LeetCode
tags: Easy
---
Given a non-empty string check if it can be constructed by taking a substring of it and appending multiple copies of the substring together. You may assume the given string consists of lowercase English letters only and its length will not exceed 10000.

Example 1:
Input: "abab"
Output: True
Explanation: It's the substring "ab" twice.

Example 2:
Input: "aba"
Output: False

Example 3:
Input: "abcabcabcabc"
Output: True
Explanation: It's the substring "abc" four times. (And the substring "abcabc" twice.)
<!-- more -->
**思路：**
一遍遍历，如跟第一个元素相等，则开始比较，记此时遍历之前的为subString,如此后所有元素均与subString相等且比较到终点，则true；
如比较不完，重新记录subString的长度，直到下一个元素相等时再比较。

``` java
public class Solution {
    public boolean repeatedSubstringPattern(String str) {

		boolean res = false;
		for(int i = 1;i<str.length();i++){
			if(str.charAt(i) == str.charAt(0)){
				int j = 0,len = i;
				
				while(i<str.length() && str.charAt(i) == str.charAt(j)){
					if(i == str.length()-1 && j==len-1){
						res = true;
						break;
					}
					i++;
					j++;
					j=j%len;
					
				}
				if(!res) i--; //注意比较失败时i多加了一，所以要减去
			}
		}
		return res;
    }
}
```