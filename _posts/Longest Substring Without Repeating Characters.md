---
title: Longest Substring Without Repeating Characters.java
date: 
categories: LeetCode
tags: Medium
---
Given a string, find the length of the longest substring without repeating characters.
Examples:
Given "abcabcbb", the answer is "abc", which the length is 3.
Given "bbbbb", the answer is "b", with the length of 1.
Given "pwwkew", the answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
<!-- more -->
**思路：**
用StringBuilder维护一个动态的没有重复字符串，遍历一遍。
``` java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int len = s.length();
        if(len == 0) return 0;
		int maxlen = 1;
		
		StringBuilder sb = new StringBuilder();
		sb.append(s.charAt(0));
		for(int j=1;j<len;j++){
			if(sb.indexOf(String.valueOf(s.charAt(j))) < 0){ //sb.indexOf(str) 返回str的指针，没有为-1
				sb.append(s.charAt(j));
				maxlen = Math.max(maxlen,sb.length());
			}else{ //如果重复，删除重复元素及之前的所有元素
				int index = sb.indexOf(String.valueOf(s.charAt(j)));
				sb.delete(0,index+1);
				sb.append(s.charAt(j));
			}
		}
		return maxlen;
    }
}

``` 
**hashmap思路：**
the basic idea is, keep a hashmap which stores the characters in string as keys and their positions as values, 
and keep two pointers which define the max substring. move the right pointer to scan through the string , 
and meanwhile update the hashmap. If the character is already in the hashmap, then move the left pointer to the right of the same character last found. 
Note that the two pointers can only move forward.
``` java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s.length()==0) return 0;
        HashMap<Character, Integer> map = new HashMap<Character, Integer>();
        int max=0;
        for (int i=0, j=0; i<s.length(); ++i){
            if (map.containsKey(s.charAt(i))){
                j = Math.max(j,map.get(s.charAt(i))+1);
            }
            map.put(s.charAt(i),i);
            max = Math.max(max,i-j+1);
        }
        return max;
    }
}


``` 