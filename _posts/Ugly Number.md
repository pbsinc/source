---
title: Ugly Number.java
date: 
categories: LeetCode
tags: [Easy,判断是否为质数,重做]
---
Write a program to check whether a given number is an ugly number.
Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. For example, 6, 8 are ugly while 14 is not ugly since it includes another prime factor 7.
Note that 1 is typically treated as an ugly number.
<!-- more -->
**思路：**
要求只有 2 ，3 ，5，那么num = 2 * 3 * 5 *1 的任意次幂；
所以：对其不断出去这三个数，剩余为1 就是ugly numnber;
``` java
public class Solution {
    public boolean isUgly(int num) {
        if(num == 1)return true;
		
		while(num%2 == 0) num /= 2;
		while(num%3 == 0) num /= 3;
		while(num%5 == 0) num /= 5;
		
		return num==1;
    }
}
``` 

**算法总结：判断一个数是否为素数：**
http://blog.csdn.net/arvonzhang/article/details/8564836