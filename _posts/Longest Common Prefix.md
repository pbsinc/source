---
title: Longest Common Prefix.java
date: 
categories: LeetCode
tags: Easy
---
Write a function to find the longest common prefix string amongst an array of strings.
<!-- more -->
**思路：**
对strs数组的每一位进行比较，这一位全相同，count++;
直到遇到某一位不同，跳出循环，此时count数即为相同元素的个数。
``` java
public class Solution {
    public String longestCommonPrefix(String[] strs) {
		if(strs.length == 0) return "";
		if(strs.length == 1) return strs[0];
        int count = 0;
		boolean stop = false;
		while(true){
			
			for(int i=1;i<strs.length;i++){
				if(count >= strs[i].length() || count >= strs[0].length()){
					stop = true;
					break;
				}
				if(strs[i].charAt(count) == strs[0].charAt(count)){
					if(i == strs.length-1)
						count++;
					continue;
				}else{
					stop = true;
					break;
				}
					
			}
			
			if(stop)
				break;
		}
		
		char[] temp = new char[count];
		for(int i=0;i<count;i++){
			temp[i] = strs[0].charAt(i);
		}
		
		return new String(temp);
    }
}
```