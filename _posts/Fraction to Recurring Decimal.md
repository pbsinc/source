---
title: Fraction to Recurring Decimal.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Given two integers representing the numerator and denominator of a fraction, return the fraction in string format.
If the fractional part is repeating, enclose the repeating part in parentheses.
For example,

	Given numerator = 1, denominator = 2, return "0.5".
	Given numerator = 2, denominator = 1, return "2".
	Given numerator = 2, denominator = 3, return "0.(6)".
Hint:

	No scary math, just apply elementary math knowledge. Still remember how to perform a long division?
	Try a long division on 4/9, the repeating part is obvious. Now try 4/333. Do you see a pattern?
	Be wary of edge cases! List out as many test cases as you can think of and test your code thoroughly.
<!-- more -->

**题意**
给定一个分子和一个分母，以字符串的形式返回该小数。如果小数无限循环的话，用括号扩住循环体。

**官方Solution**

		0.16  
	6 ) 1.00
		0 
		1 0       <-- Remainder=1, mark 1 as seen at position=0.
		- 6 
		  40      <-- Remainder=4, mark 4 as seen at position=1.
		- 36 
		   4      <-- Remainder=4 was seen before at position=1, so the fractional part which is 16 starts repeating at position=1 => 1(6).
The key insight here is to notice that once the remainder starts repeating, so does the divided result.
You will need a hash table that maps from the remainder to its position of the fractional part. Once you found a repeating remainder, you may enclose the reoccurring fractional part with parentheses by consulting the position from the table.
The remainder could be zero while doing the division. That means there is no repeating fractional part and you should stop right away.
Just like the question Divide Two Integers, be wary of edge case such as negative fractions and nasty extreme case such as -2147483648 / -1.

**中文解析**
难点：如何识别循环体？

解决方法：用一个HashMap记录每一个余数，当出现重复的余数时，那么将会进入循环，两个重复余数之间的部分就是循环体。

示例：1/13=0.076923076923076923...，当小数部分第二次出现0时，就意味着开始了循环，那么需要把076923用括号括起来，结果为0.(076923)。

涉及技巧：1）在不断相除的过程中，把余数乘以10再进行下一次相除，保证一直是整数相除；2）HashMap的key和value分别是<当前余数, 对应结果下标>，这样获取076923时就可根据value值来找。

注意点1：考虑正负数，先判断符号，然后都转化为正数；

注意点2：考虑溢出，如果输入为Integer.MIN_VALUE，取绝对值后会溢出。

代码中已添加注释，来看代码：
``` java
public class Solution {  
    public String fractionToDecimal(int numerator, int denominator) {  
        if (numerator == 0) return "0";  
        if (denominator == 0) return "";  
          
        String ans = "";  
          
        //如果结果为负数  
        if ((numerator < 0) ^ (denominator < 0)) {  
            ans += "-"; //String可以直接相加，注意不一定非要使用StringBuilder!
        }  
          
        //下面要把两个数都转为正数，为避免溢出，int转为long  
        long num = numerator, den = denominator;  
        num = Math.abs(num);  
        den = Math.abs(den);  
          
        //结果的整数部分  
        long res = num / den;  
        ans += String.valueOf(res);  //String可以直接相加
          
        //如果能够整除，返回结果  
        long rem = (num % den) * 10;  
        if (rem == 0) return ans;  
          
        //结果的小数部分  
        HashMap<Long, Integer> map = new HashMap<Long, Integer>();  
        ans += ".";  
        while (rem != 0) {  
            //如果前面已经出现过该余数，那么将会开始循环  
            if (map.containsKey(rem)) {  
                int beg = map.get(rem); //循环体开始的位置  
                String part1 = ans.substring(0, beg);  
                String part2 = ans.substring(beg, ans.length());  
                ans = part1 + "(" + part2 + ")";  
                return ans;  
            }  
              
            //继续往下除  
            map.put(rem, ans.length());  
            res = rem / den;  
            ans += String.valueOf(res);  
            rem = (rem % den) * 10;  
        }  
          
        return ans;  
    }  
}  
``` 
Java编程误区：一定要先把 int 转为 long，然后再取绝对值。如果写成 long num = Math.abs(numerator) 就会有问题，
因为这句代码在 numerator=Integer.MIN_VALUE 时相当于 long num = Math.abs(-2147483648)，这样得到的 num还是 -2147483648。
转自：http://blog.csdn.net/ljiabin/article/details/42025037
