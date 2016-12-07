---
title: Implement strStr().java
date: 
categories: LeetCode
tags: Easy
---
Implement strStr().
Returns the index of the first occurrence of needle in haystack, or -1 if needle is not part of haystack.
<!-- more -->
**思路：**
一遍过，直到 needle的每一个都配对上，输出index；
如果没配上，从index的下一位开始重新配对，以防以下情况：
"mississippi"
"issip"
``` java
public class Solution {
    public int strStr(String haystack, String needle) {
        int j = 0,index = 0;
        if(needle.length() == 0) return 0;
		boolean first = true,isPart = false;
		for(int i=0;i<haystack.length();i++){
			if(haystack.charAt(i) == needle.charAt(j)){
				if(first){
					index = i;
					first = false;
				}
				if(j == needle.length()-1){
					isPart = true;
					break;
				}
				j++;
			}else if(!first){
				i = index;
				j = 0;
				first = true;
			}
		}
		if(!isPart)
			index = -1;
		return index;
    }
}
```