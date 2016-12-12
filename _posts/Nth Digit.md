---
title: Nth Digit.java
date: 
categories: LeetCode
tags: [Easy,重做,Character.getNumericValue(char)]
---
Find the nth digit of the infinite integer sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ...
Note:
n is positive and will fit within the range of a 32-bit signed integer (n < 231).
Example 1:

	Input:3
	Output:3
Example 2:

	Input:11
	Output:0
	Explanation:
	The 11th digit of the sequence 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, ... is a 0, which is part of the number 10.
<!-- more -->
**思路：**
1.find the length of the number where the nth digit is from
2.find the actual number where the nth digit is from
3.find the nth digit and return
``` java
public class Solution {
    public int findNthDigit(int n) {
        int start = 1, len = 1;
        long count = 9;//这里不是long的话，当n是int最大值时，越界！
        while(n>len*count){
            n -= len*count;
            len++;
            count *= 10;
            start *= 10;
        }
        start += (n-1)/len;
        String str = String.valueOf(start);
        //return str.charAt((n-1)%len)-'0';
        return Character.getNumericValue(str.charAt((n - 1) % len));
    }
}
``` 