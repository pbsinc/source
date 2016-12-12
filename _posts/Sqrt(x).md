---
title: Sqrt(x).java
date: 
categories: LeetCode
tags: Medium
---
Implement int sqrt(int x).
Compute and return the square root of x.
<!-- more -->
**思路2ms：**
利用一个参数因子，每次加一个因子，判断与x的大小关系；
参数因子flag不断除以10，在变小；
``` java
public class Solution {
    public int mySqrt(int x) {
        int flag = 10000;
		long ans = 0;
		while(flag != 0){
			if(ans*ans > x){
				ans -= flag;
				
				flag /= 10;
			}else{
				ans += flag;
			}
		}
		return (int)ans;
    }
}
``` 
别人的方法：
https://discuss.leetcode.com/category/77/sqrt-x