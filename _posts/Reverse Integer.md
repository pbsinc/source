---
title: Reverse Integer.java
date: 
categories: LeetCode
tags: Easy
---
Reverse digits of an integer.

	Example1: x = 123, return 321
	Example2: x = -123, return -321
Have you thought about this?
Here are some good questions to ask before coding. Bonus points for you if you have already thought through this!
If the integer's last digit is 0, what should the output be? ie, cases such as 10, 100.
Did you notice that the reversed integer might overflow? Assume the input is a 32-bit integer, then the reverse of 1000000003 overflows. How should you handle such cases?
For the purpose of this problem, assume that your function returns 0 when the reversed integer overflows.
<!-- more -->
**思路1：**
换成String用StringBuilder逆序来填；
``` java
public class Solution {
    public int reverse(int x) {
        if(x == 0 || x == -0) return 0;

        String str = String.valueOf(x);
        StringBuilder sb = new StringBuilder();
        for(int i=str.length()-1;i>=0;i--){
            if(i!=0)
                sb.append(str.charAt(i));
            else{
                if(str.charAt(i) == '-'){
                    while(sb.charAt(0) == '0'){
                        sb.deleteCharAt(0);
                    }
                    sb.insert(0,'-');
                }
                else{
                    sb.append(str.charAt(i));
                    while(sb.charAt(0) == '0'){
                        sb.deleteCharAt(0);
                    }
                }
            }
        }
        long tmp = Long.parseLong(sb.toString());
        if(tmp > Integer.MAX_VALUE || tmp<Integer.MIN_VALUE) return 0;
        return (int)tmp;
    }
}
``` 

**思路2：**
每次取余数，除以10，往下算；
``` java
class Solution {
public:
    int reverse(int x) {
        int sign = x < 0 ? -1 : 1;
        x = abs(x);
        int res = 0;
        while (x > 0) {
            if (INT_MAX / 10 < res || (INT_MAX - x % 10) < res * 10) {
                return 0;
            }
            res = res * 10 + x % 10;
            x /= 10;
        }
        
        return sign * res;
    }
};
``` 