---
title: Plus One.java
date: 
categories: LeetCode
tags: Easy
---
Given a non-negative number represented as an array of digits, plus one to the number.
The digits are stored such that the most significant digit is at the head of the list.
<!-- more -->
**思路：**
考虑进位，每一位数都与上一位的进位相加并得到新的进位。注意最高位的进位
``` java
public class Solution {
    public int[] plusOne(int[] digits) {
        int len=digits.length, j=1;
        for(int i=len-1;i>=0;i--){
            digits[i] += j;
            j = digits[i]/10;
            digits[i] = digits[i]%10;
        }
        if(j==1){
            int[] plusone = new int[len+1];
            plusone[0] = 1;
            for(int k=1;k<=len;k++)
                plusone[k]=digits[k-1];
            return plusone;    
        }
        else{
            return digits;    
        }
    }
}
```