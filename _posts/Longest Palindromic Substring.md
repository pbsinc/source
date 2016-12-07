---
title: Longest Palindromic Substring.java
date: 
categories: LeetCode
tags: Medium
---
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.
Example:
Input: "babad";Output: "bab"
Note: "aba" is also a valid answer.
Example:
Input: "cbbd";Output: "bb"
<!-- more -->
**TLE思路：**
 第一种方法就是挨个检查，维护全局最长，时间复杂度为O（n3），会TLE。
``` java
public class Solution {
    public String longestPalindrome(String s) {
		if(s.length()==0) return "";
        int start = 0, end = s.length()-1;
		String res = String.valueOf(s.charAt(0));
		int tempend = 0;
		while(start<=end){
			if(end == tempend){
				end = start;
			}else if(s.length()-start <= res.length()){
				break;
			}else if(s.charAt(start) == s.charAt(end)){
				String substr = s.substring(start,end+1);
				
				if(substr.length() > res.length()){
					if(isPalindrome(substr)){	
						res = substr;
						tempend = end;
						end = start;
					}else
						end--;
				}else
					end = start;	
				
			}else{
				end--;
			}
			if(start == end){
				if(start == s.length()-1) break;
				start++;
				end = s.length()-1;
			}
			
		}
		return res;
		
    }
	 public boolean isPalindrome(String s) {
		if(s.length() == 0) return true;
		int start = 0, end = s.length()-1;
		boolean res = true;
		s=s.toLowerCase();
        while(start <= end){
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

**第二种方法：中心发散法**
对于每个子串的中心
（可以是一个字符，或者是两个字符的间隙，比如串abc,中心可以是a,b,c,或者是ab的间隙，bc的间隙，例如aba是回文，abba也是回文，这两种情况要分情况考虑）
往两边同时进行扫描，直到不是回文串为止。
假设字符串的长度为n,那么中心的个数为2*n-1(字符作为中心有n个，间隙有n-1个）。
对于每个中心往两边扫描的复杂 度为O(n),所以时间复杂度为O((2*n-1)*n)=O(n^2),空间复杂度为O(1)。
引自Code ganker（http://codeganker.blogspot.com/2014/02/longest-palindromic-substring-leetcode.html）
``` java
public class Solution {
    public String longestPalindrome(String s) {
        if (s.isEmpty()||s==null||s.length() == 1)
            return s;
     
        String longest = s.substring(0, 1);
        for (int i = 0; i < s.length(); i++) {
            // get longest palindrome with center of i
            String tmp = helper(s, i, i);
            
            if (tmp.length() > longest.length())
                longest = tmp;
     
            // get longest palindrome with center of i, i+1
            tmp = helper(s, i, i + 1);
            if (tmp.length() > longest.length())
                longest = tmp;
        }
     
        return longest;
    }
     
    // Given a center, either one letter or two letter, 
    // Find longest palindrome
    public String helper(String s, int begin, int end) {
        while (begin >= 0 && end <= s.length() - 1 && s.charAt(begin) == s.charAt(end)) {
            begin--;
            end++;
        }
        return s.substring(begin + 1, end);
    }
}
``` 
