---
title: Third Maximum Number.java
date: 
categories: LeetCode
tags: Easy
---
Given a non-empty array of integers, return the third maximum number in this array. If it does not exist, return the maximum number. The time complexity must be in O(n).
<!-- more -->
Example 1:
Input: [3, 2, 1]
Output: 1
Explanation: The third maximum is 1.
------------------------------------
Example 2:
Input: [1, 2]
Output: 2
Explanation: The third maximum does not exist, so the maximum (2) is returned instead.
------------------------------------
Example 3:
Input: [2, 2, 3, 1]
Output: 1
Explanation: Note that the third maximum here means the third maximum distinct number.
Both numbers with value 2 are both considered as second maximum.
**思路:**
循环一遍，a b c,从大到小放。Integer.MIN_VALUE 为int的最小值
``` java
public class Solution {
    public int thirdMax(int[] nums) {
        int a=Integer.MIN_VALUE,b=Integer.MIN_VALUE,c=Integer.MIN_VALUE;
        int flag = 0;
        for(int x: nums){
            if(x > a){
                c=b;b=a;
                a=x;
            }
            else if(x > b && x<a){
                c = b;
                b = x;
            }
            else if(x >= c && x<b){
                c = x;  flag = 0;
                if(c == Integer.MIN_VALUE)
                    flag = 1;
            }    
            else if(x == Integer.MIN_VALUE)
                flag = 1;
        }
        if((c == Integer.MIN_VALUE && flag == 0) || b==c)
            return a;
        else
            return c;
    }
}
```
