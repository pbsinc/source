---
title: Factorial Trailing Zeroes.java
date: 
categories: LeetCode
tags: [Easy,N!末尾零的个数,重做]
---
Given an integer n, return the number of trailing zeroes in n!.
Note: Your solution should be in logarithmic time complexity.
<!-- more -->
**思路：**
求N!后面有多少个零，这题考的主要是数学，举一个简单例子102！=1*2*3*4*5  *6*7*8*9*10 .....*101*102  
我们把每五个数当成一组，可以发现每一组中都有一个5的倍数的数，而且连续的5个数中肯定有偶数，有5的倍数的数又有偶数，这就至少可以产生一个0，
所以102/5=20组，至少有20个零，每组中包含5的倍数的数分别是5 10 15 20 25 ...100我们把这几个数提出一个5，
得到1 2 3 4 5 6 ..20这里又可以分成4组 又可以得到4个0，因为最后只剩余4组所以不能再提取0了，所以只有24个零。
``` java
public class Solution {
    public int trailingZeroes(int n) {
        return calcu(0,n);
    }
    public int calcu(int count,int n){
        
        if(n/5 == 0) return count;
        count += n/5;
        return calcu(count,n/5);
    } 
}
``` 
