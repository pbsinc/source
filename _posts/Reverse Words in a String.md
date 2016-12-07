---
title: Reverse Words in a String.java
date: 
categories: LeetCode
tags: [Medium,重做,String的分割str.split(正则)方法,一个或者多个空格的正则]
---
Given an input string, reverse the string word by word.
For example,
Given s = "the sky is blue",
return "blue is sky the".
Update (2015-02-12):
For C programmers: Try to solve it in-place in O(1) space.
<!-- more -->
**一个或者多个空格的正则："\\s{1,}"  **
**思路：**
StringBuilder 的 append()方法；
逆序；
从后往前将每个单词安子夫顺序，append()到sb末尾；
这个题不能自定义测试，一直有问题，找不到在哪。
``` java
public class Solution {
    public String reverseWords(String s) {
     
        if(s == null) {
            return s;
        }
        
		int len = s.length();

        int left = 0,right = len-1;
		
		
		while(left<s.length()&&s.charAt(left)==' ') {  
            left++;  
        }  
        if(left==s.length()) {  
            return "";  
        }  
		
		left = 0;
		StringBuilder sb = new StringBuilder();
		for(int i=len-1;i>=0;i--){
			if(s.charAt(i) == ' '){
				left = i+1;
				//subReverse(s, left,right,sb);
				
				for(int j=left;j<=right;j++){
					sb.append(s.charAt(j));
				}
				while(i>=0 && s.charAt(i) == ' '){
					i--;
				}
				
				sb.append(' ');
				right = i;
			}
		}
		return sb.toString();
    }
}
``` 
**法二：**
StringBuilder 的 insert()方法；
顺序；
将每个单词作为一个 substring ；
然后插入sb的最开始。

看上去有点象以前两次reverse的那道题，但这道题有所不同的地方就是需要处理空格。
1，要求删除开头和结尾的多余空格。
2，如果两个单词之间有多余空格的话，也要删除。

这样的话，就不能用两次reverse的方法了。
解题思路：找到每一个单词，然后每次将单词加到要返回的字符串的前面。
``` java
public class Solution {
    public String reverseWords(String s) {  
        if(s==null) {  
            return s;  
        }  
          
        int begin = 0;  
        int end   = 0;  
        while(begin<s.length()&&s.charAt(begin)==' ') {  
            begin++;  
        }  
        if(begin==s.length()) {  
            return "";  
        }  
          
        if(s.length()<=1) {  
            return s;  
        }  
        StringBuilder result = new StringBuilder("");  
		
		for(int i=len-1;i>=0;i--){
			if(s.charAt(i) == ' '){
				left = i+1;
				//subReverse(s, left,right,sb);
				
				for(int j=left;j<=right;j++){
					sb.append(s.charAt(j));
				}
				
				sb.append(' ');
				right = i-1;
			}
		}
		
        while(begin<s.length()&&end<s.length()) {  
            while(begin<s.length()&&s.charAt(begin)==' ') {  
                begin++;  
            }  
            if(begin==s.length()) {  
                break;  
            }  
            end = begin + 1;  
            while(end<s.length()&&s.charAt(end)!=' ') {  
                end++;  
            }  
            if(result.length()!=0) {  
                result.insert(0," ");  
            }  
            if(end<s.length()) {  
                result.insert(0,s.substring(begin,end));  
            } else {  
                result.insert(0,s.substring(begin,s.length()));  
                break;  
            }  
            begin = end + 1;  
        }  
        return result.toString();  
    }  
}
``` 
**分割字符串法：**
StringBuilder 的 append()方法；
逆序；
从后往前将每个单词安子夫顺序，append()到sb末尾；
这个题不能自定义测试，一直有问题，找不到在哪。
``` java
public class Solution {
     public static String reverseWords(String s) {  
  
        // 若输入字符串直接是空串，则直接返回该字符串  
        if (s.equals("")) {  
            return "";  
        }  
  
        // 将字符串按空格"\\s{1,}"进行分割，一个或者多个空格的正则："\\s{1,}"  
        String[] strArr = s.split("\\s{1,}");  
        int len = strArr.length;  
  
        // 分割后字符串变成空串，直接返回  
        if (len == 0) {  
            return "";  
        }  
  
        // 用StringBuilder更加有效  
        StringBuilder sb = new StringBuilder("");  
  
        // 反向输出之前分割的字符串数组  
        for (int i = len - 1; i >= 0; i--) {  
            if (!strArr[i].equals("")) {  
                sb.append(strArr[i]);  
                sb.append(" ");  
            }  
        }  
  
        sb.deleteCharAt(sb.lastIndexOf(" "));  
          
        return sb.toString();  
    }  
}
``` 