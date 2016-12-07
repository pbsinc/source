---
title: Best Time to Buy and Sell Stock II.java
date: 
categories: LeetCode
tags: Medium
---
Say you have an array for which the ith element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. You may complete as many transactions as you like (ie, buy one and sell one share of the stock multiple times). However, you may not engage in multiple transactions at the same time (ie, you must sell the stock before you buy again).
<!-- more -->
**思路:**
1. 与 I 一样：买入值设为最大，便利一次，遇到更小的值，设为买入值；将每次遍历遇到的值与买入值做差，与原利润比较，更大的设为利润。
2. II 的不同点在于：当后一个价格小于前一个时，卖出，重新买入再循环。
``` java
public class Solution {
    public int maxProfit(int[] prices) {
        int buy = Integer.MAX_VALUE,profit=0,out=0;
        for(int i=0;i<prices.length;i++){
            buy=Math.min(buy,prices[i]);
            profit=Math.max(profit,prices[i]-buy);
			if((i>0 && prices[i]<prices[i-1]) || i==prices.length-1){
				buy = prices[i];
				out+=profit;
				profit=0;
			}
        }
        return out;
    }
}
```

