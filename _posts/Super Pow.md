---
title: Super Pow.java
date: 
categories: LeetCode
tags: [Medium,快速求幂算法,重做]
---
Your task is to calculate ab mod 1337 where a is a positive integer and b is an extremely large positive integer given in the form of an array.
Example1:

	a = 2
	b = [3]

Result: 8
Example2:

	a = 2
	b = [1,0]

Result: 1024
<!-- more -->
题意：给出底数a,指数b（很大的数，用数组形式给出，每个元素为1位），求其对1337的模
**思路：**
在遍历数据过程中，用ans(n)表示上一次计算结果，则ans(n+1) = pow(ans(n), 10, 1337) * pow(a, b[n+1], 1337) % 1337
其中pow(a, b, mod)为快速求幂算法的实现
``` java
public class Solution  
{  
	public int superPow(int a, int[] b)  
    {  
        int ans = 1;  
        int mod = 1337;  
  
        for (int i = 0; i < b.length; i++) {  
            ans = pow(ans, 10, mod) * pow(a, b[i], mod) % mod;  
        }  
  
        return ans;  
    }  
	
    private int pow(int a, int b, int mod)  //快速求幂算法的实现
    {  
        int res = 1;  
        a = a % mod;  
        while (b > 0) {  
            if (b % 2 == 1) 
                res = res * a % mod;  
            
            a = a * a % mod;  
            b /= 2;  
        }  
        return res;  
    }  
}  
``` 
快速求幂算法:
http://www.doc88.com/p-5836182437827.html
