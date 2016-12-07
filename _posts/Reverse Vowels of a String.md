---
title: Reverse Vowels of a String.java
date: 
categories: LeetCode
tags: Easy
---
Write a function that takes a string as input and reverse only the vowels of a string.
Example 1:
Given s = "hello", return "holle".
Example 2:
Given s = "leetcode", return "leotcede".
Note:
The vowels does not include the letter "y".
<!-- more -->
**思路：**
左右分别靠拢，遇到元音交换即可；
比较坑爹的是这个题的意思是左右换，而不是相邻换：aao答案是oaa，而不是aoa。

``` java
public class Solution {
    public String reverseVowels(String s) {
		ArrayList<Character> vowList = new ArrayList<Character>();
		vowList.add('a');
		vowList.add('e');
		vowList.add('i');
		vowList.add('o');
		vowList.add('u');
		vowList.add('A');
		vowList.add('E');
		vowList.add('I');
		vowList.add('O');
		vowList.add('U');
 
		char[] arr = s.toCharArray();
	 
		int i=0; 
		int j=s.length()-1;
	 
		while(i<j){
			if(!vowList.contains(arr[i])){
				i++;
				continue;
			}
	 
			if(!vowList.contains(arr[j])){
				j--;
				continue;
			}
	 
			char t = arr[i];
			arr[i]=arr[j];
			arr[j]=t;
	 
			i++;
			j--; 
		}
	 
		return new String(arr); // char[] 转 String: 1.String res = new String(arr);2.String res = String.valueOf(arr);
	}
}
```