---
title: Valid Perfect Square.java
date: 
categories: LeetCode
tags: [Medium,牛顿迭代法求平方根,平方数=1+3+5+7+...,重做]
---
Given a positive integer num, write a function which returns True if num is a perfect square else False.
Note: Do not use any built-in library function such as sqrt.
Example 1:

	Input: 16
	Returns: True
Example 2:

	Input: 14
	Returns: False
<!-- more -->


**思路1：**
一个数一个数比较，超时，肯定错误；
``` java
public class Solution {
    public boolean isPerfectSquare(int num) {
        if(num == 1) return true;
        int flag = 1;
		
		while(num > flag*flag){
			flag += 10;
		}
		
		flag -= 10;
		
		boolean ans = false;
		for(int i=flag;i<flag+10;i++){
			if(Math.log(num) / Math.log(i) == 2){
				ans = true;
				break;
			}
			if(Math.log(num) / Math.log(i) < 2){
				break;
			}
		}
		return ans;
    }
}
``` 

**思路2：**
A square number is **1+3+5+7+...**,
The time complexity is O(sqrt(n))；
``` java
public boolean isPerfectSquare(int num) {
     int i = 1;
     while (num > 0) {
         num -= i; // 这里如果用加法，判断是否等于num的话，有可能到达Integer.MAX_VALUE，错误！
         i += 2;
     }
     return num == 0;
 }
``` 

**二分查找：**
a more efficient one using binary search whose time complexity is O(log(n)):
``` java
public boolean isPerfectSquare(int num) {
        int low = 1, high = num;
        while (low <= high) {
            long mid = (low + high) >>> 1;
            if (mid * mid == num) {
                return true;
            } else if (mid * mid < num) {
                low = (int) mid + 1;
            } else {
                high = (int) mid - 1;
            }
        }
        return false;
    }
``` 


**牛顿迭代法：**
公式：**X(n+1)=[X(n)+p/X(n)]/2**

你任说一个整数x，我任猜它的平方根为y，如果不对或精度不够准确，那我令y = (y+x/y)/2。如此循环反复下去，y就会无限逼近x的平方根。

例如，2的平方根：

	x(0) = 1
	x(1) = (1/2)(1+2/1) = 3/2 = 1.5
	x(2) = (1/2)[3/2+2/(3/2)] = 17/12 = 1.41666667
	x(3) = (1/2)[17/12 + 2/(17/12)] = 577/408 = 1.41421568…
就这样，反复代入上式计算，得到的值越来越精确。

或者这么解释：

	1.对x的平方根的值一个猜想y。
	2.通过执行一个简单的操作去得到一个更好的猜测：只需要求出y和x/y的平均值（它更接近实际的平方根值）。
``` java
public boolean isPerfectSquare(int num) {
        long x = num;
        while (x * x > num) {
            x = (x + num / x) >> 1;
        }
        return x * x == num;
    }
``` 
http://www.nowamagic.net/librarys/veda/detail/2268