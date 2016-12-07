---
title: Pow(x, n).java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Implement pow(x, n).
<!-- more -->
**思路1：**
用递归；
``` java
public class Solution {
    public double myPow(double x, int n) {
        if (n < 0) {
            x = 1 / x;
            n = -n;
            if (n == Integer.MIN_VALUE) {
                n = Integer.MAX_VALUE;
                x *= x;
             }
        }
		if(n<0){
            n = -n;
            x = 1/x;
        }
		
		if(n==0) return 1;
		
		if(n==2) return x*x;
		
		if(n %2 == 0)
		    return myPow(myPow(x,n/2),2);
		else
			return x*myPow(myPow(x,n/2),2);
		
		
    }
}
```

**思路2：**
循环；
``` java
public class Solution {
    public double myPow(double x, int n) {
        if (n == 0) return 1;
        if (x == 1) return 1;
        if (x == -1) {
            if (n % 2 == 0) return 1;
            else return -1;
        }
        if (n == Integer.MIN_VALUE) return 0;
        if (n < 0) {
            n = -n;
            x = 1/x;
        }
        double ret = 1.0;
        while (n > 0) {
            if (n % 2 == 1) 
                ret *= x;
            x = x * x;
            n = n /2; // n = n >> 1;也可以这样，二进制按位操作，右移一位
        }
        return ret;
    }
}
```