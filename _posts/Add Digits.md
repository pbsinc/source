---
title: Add Digits.java
date: 
categories: LeetCode
tags: [Easy,数根问题]
---
Given a non-negative integer num, repeatedly add all its digits until the result has only one digit.
For example:
Given num = 38, the process is like: 3 + 8 = 11, 1 + 1 = 2. Since 2 has only one digit, return it.
Follow up:
Could you do it without any loop/recursion in O(1) runtime?
<!-- more -->
**数根：**
在数学中，数根(又称位数根或数字根Digital root)是自然数的一种性质，换句话说，每个自然数都有一个数根。

**公式：**
a的数根:**b = (a - 1) % 9 + 1**

**用途:**

	数根可以计算模运算的同余，对于非常大的数字的情况下可以节省很多时间。
	数字根可作为一种检验计算正确性的方法。例如，两数字的和的数根等于两数字分别的数根的和。
	另外，数根也可以用来判断数字的整除性，如果数根能被3或9整除，则原来的数也能被3或9整除。
``` java
public class Solution {
    public int addDigits(int num) {
        return (num - 1)%9 + 1;
    }
}
``` 