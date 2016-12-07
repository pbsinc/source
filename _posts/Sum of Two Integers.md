---
title: Sum of Two Integers.java
date: 
categories: LeetCode
tags: Easy
---
Calculate the sum of two integers a and b, but you are not allowed to use the operator + and -.
Example:
Given a = 1 and b = 2, return 3.
<!-- more -->
**关键:**
1.用BigInteger 的 biga.add(bigb)方法；
2.BigInteger 转 int 用 bigc.intValue()方法;
这是我的做法
``` java
import java.math.BigInteger;
public class Solution {
    public int getSum(int a, int b) {
        BigInteger biga = BigInteger.valueOf(a);
        BigInteger bigb = BigInteger.valueOf(b);
        BigInteger bigc = biga.add(bigb);
        int ans = bigc.intValue();
        return ans;
    }
}
```	
**标准答案:**
首先，先对数据a，b进行&（与）运算，原因是下面用^（异或）的方法来进行加法会漏掉进位，所以，先对数据进行&运算得到carry，carry中为1的位是会进行进位的位，接下来对数据进行^运算，结果记为add1，实现伪加法，之所以是伪加法，是因为它漏掉了进位。那漏掉的进位怎么办呢？对carry进行左移得到C，C+add1就是两个数据的真正的和。那怎么实现C+add1呢？将add1赋予a，C赋予b,重复以上的操作，直到b等于0.
``` java
public class Solution {  
    public int getSum(int a, int b) {  
        while (b != 0) {  
            int carry = (a & b) << 1;  
            a = a ^ b;  
            b = carry;  
        }  
        return a;  
    }  
} 
```	