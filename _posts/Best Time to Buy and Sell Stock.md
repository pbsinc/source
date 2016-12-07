---
title: Best Time to Buy and Sell Stock.java
date: 
categories: LeetCode
tags: [Easy,Collections]
---
Say you have an array for which the ith element is the price of a given stock on day i.
If you were only permitted to complete at most one transaction (ie, buy one and sell one share of the stock), design an algorithm to find the maximum profit.
<!-- more -->
Example 1:
Input: [7, 1, 5, 3, 6, 4]
Output: 5

max. difference = 6-1 = 5 (not 7-1 = 6, as selling price needs to be larger than buying price)
Example 2:
Input: [7, 6, 4, 3, 1]
Output: 0

In this case, no transaction is done, i.e. max profit = 0.


**思路:**
买入值设为最大，便利一次，遇到更小的值，设为买入值；
将每次遍历遇到的值与买入值做差，与原利润比较，更大的设为利润。
``` java
public class Solution {
    public int maxProfit(int[] prices) {
        int buy = Integer.MAX_VALUE,profit=0;
        for(int i=0;i<prices.length;i++){
            buy=Math.min(buy,prices[i]);
            profit=Math.max(profit,prices[i]-buy);
        }
        return profit;
    }
}
```

**Collections:**
http://www.yiibai.com/java/util/java_util_collections.html