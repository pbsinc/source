---
title: Divide Two Integers.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Divide two integers without using multiplication, division and mod operator.
If it is overflow, return MAX_INT.
<!-- more -->
**思路：**
我们设想87 / 4，本来应该的得到21余3，那么如果我们把87忽略余数后分解一下，
87 = 4 * 21 = 4 * 16 + 4 * 4 + 4 * 1，也就是87 = 4 * (1 * 2^4 + 0 * 2^3 + 1 * 2^2 + 0 * 2^1 + 1 * 2^0)，
也就是把商分解为27 = 1 * 2^4 + 0 * 2^3 + 1 * 2^2 + 0 * 2^1 + 1 * 2^0，所以商的二进制是10101。
我们可以不断的将4乘2的一次方，二次方，等等，直到找到最大那个次方，在这里是2的四次方。
然后，我们就从四次方到零次方，按位看商的这一位该是0还是1。
``` java
public class Solution {
    public int divide(int dividend, int divisor) {
		//Reduce the problem to positive long integer to make it easier.
		//Use long to avoid integer overflow cases.
		int sign = 1;
		if ((dividend > 0 && divisor < 0) || (dividend < 0 && divisor > 0))
			sign = -1;
		long ldividend = Math.abs((long) dividend);
		long ldivisor = Math.abs((long) divisor);
		
		//Take care the edge cases.
		if (ldivisor == 0) return Integer.MAX_VALUE;
		if ((ldividend == 0) || (ldividend < ldivisor))	return 0;
		
		long lans = ldivide(ldividend, ldivisor);
		
		int ans;
		if (lans > Integer.MAX_VALUE){ //Handle overflow.
			ans = (sign == 1)? Integer.MAX_VALUE : Integer.MIN_VALUE;
		} else {
			ans = (int) (sign * lans);
		}
		return ans;
	}

	private long ldivide(long ldividend, long ldivisor) {
		// Recursion exit condition
		if (ldividend < ldivisor) return 0;
		
		//  Find the largest multiple so that (divisor * multiple <= dividend), 
		//  whereas we are moving with stride 1, 2, 4, 8, 16...2^n for performance reason.
		//  Think this as a binary search.
		long sum = ldivisor;
		long multiple = 1;
		while ((sum+sum) <= ldividend) { //类似于二分查找，比直接i++快很多！
			sum += sum;
			multiple += multiple;
		}
		//Look for additional value for the multiple from the reminder (dividend - sum) recursively.
		return multiple + ldivide(ldividend - sum, ldivisor);
	}
}
``` 

