---
title: Palindrome Number.java
date: 
categories: LeetCode
tags: Easy
---
Given a positive integer n, find the least number of perfect square numbers (for example, 1, 4, 9, 16, ...) which sum to n.
For example, given n = 12, return 3 because 12 = 4 + 4 + 4; given n = 13, return 2 because 13 = 4 + 9.
<!-- more -->
**思路：**
转成string,前后比较；
``` java
public class Solution {
    public boolean isPalindrome(int x) {
		String strx = String.valueOf(x);
		if(strx.length() == 0) return false;
		if(strx.length() == 1) return true;
		boolean res = true;
        int left = 0, right = strx.length()-1;
		while(left<=right){
			if(strx.charAt(left) == strx.charAt(right)){
				left++;
				right--;
			}else{
				res = false;
				break;
			}
			
		}
		return res;
    }
}
``` 
**思路2：**
每次取第一个数和最后一个数比较；
``` java
public class Solution {
    public boolean isPalindrome(int x) {
        //negative numbers are not palindrome
        if (x < 0)
            return false;
 
        // initialize how many zeros
        int div = 1;
        while (x / div >= 10) {
            div *= 10;
        }
 
        while (x != 0) {
            int left = x / div;
            int right = x % 10;
 
            if (left != right)
                return false;
 
            x = (x % div) / 10;
            div /= 100;
        }
 
        return true;
    }
}
``` 
