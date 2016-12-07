---
title: Power of Three.java
date: 
categories: LeetCode
tags: [Easy,Integer内3的次方最大为3^19=1162261467,重做]
---
Given an integer, write a function to determine if it is a power of three.
Follow up:
Could you do it without using any loop / recursion?
<!-- more -->
**思路1：**
不让用循环或递归；
那么有一个投机取巧的方法，由于输入是int，正数范围是0-231，在此范围中允许的最大的3的次方数为3^19=1162261467，那么我们只要看这个数能否被n整除即可
``` java
public class Solution {
    public boolean isPowerOfThree(int n) {
		
		return (n>0 && 1162261467 % n == 0);
    }
}
``` 


**思路2：**
最后还有一种巧妙的方法，利用**对数的换底公式**来做，高中学过的换底公式为logab = logcb / logca，
那么如果n是3的倍数，则log3n一定是整数，我们利用换底公式可以写为log3n = ln(n) / ln(3)，
注意这里一定要用10为底数，不能用自然数或者2为底数，否则当n=243时会出错，原因请看这个帖子。
现在问题就变成了判断log10n / log103是否为整数;


**判断数字a是否为整数**，我们可以用 **a - int(a) == 0 **来判断；

Sun的J2SE提供了一个单一的Java对数方法——double java.lang.Math.log(double)，这很轻易使用。请看如下代码：

　　double x = Math.log(5);

　　等价于：x = ln 5 或 x = loge5，即以**e为底**的自然对数。

``` java
public class Solution {
    public boolean isPowerOfThree(int n) {
        int maxPow = (int)(Math.pow(3, (int)(Math.log(Integer.MAX_VALUE) / Math.log(3))));
        return (n > 0 && maxPow % n == 0);
    }
}
``` 