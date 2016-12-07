---
title: Happy Number.java
date: 
categories: LeetCode
tags: [Easy,Happy Number 快乐数]
---
Write an algorithm to determine if a number is "happy".
A happy number is a number defined by the following process: Starting with any positive integer, replace the number by the sum of the squares of its digits, 
and repeat the process until the number equals 1 (where it will stay), or it loops endlessly in a cycle which does not include 1. 
Those numbers for which this process ends in 1 are happy numbers.
Example: 19 is a happy number

	12 + 92 = 82
	82 + 22 = 68
	62 + 82 = 100
	12 + 02 + 02 = 1
<!-- more -->
**Happy Number 快乐数：**
以十进位为例：
2 8 → 2²+8²=68 → 6²+8²=100 → 1²+0²+0²=1
3 2 → 3²+2²=13 → 1²+3²=10 → 1²+0²=1
3 7 → 3²+7²=58 → 5²+8²=89 → 8²+9²=145 → 1²+4²+5²=42 → 4²+2²=20 → 2²+0²=4 → 4²=16 → 1²+6²=37……
因此28和32是快乐数，而在37的计算过程中，37重覆出现，继续计算的结果只会是上述数字的循环，不会出现1，因此37不是快乐数。
不是快乐数的数称为不快乐数（unhappy number）：
**所有不快乐数的数位平方和计算，最後都会进入 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4 的循环**中。
在十进位下，100以内的快乐数有（OEIS中的数列A00770） ：1, 7, 10, 13, 19, 23, 28, 31, 32, 44, 49, 68, 70, 79, 82, 86, 91, 94, 97, 100。

**思路：**
利用4会重复出现，检测；
``` java
public class Solution {
    public boolean isHappy(int n) {
        String num = String.valueOf(n);
		int newnum = 0;
		for(int i=0;i<num.length();i++){
			newnum += (num.charAt(i)-'0')*(num.charAt(i)-'0');
		}
		if(newnum == 1)
			return true;
		if(newnum == 4)
			return false;
		return isHappy(newnum);
    }
}
```

**HashSet思路：**
使用一个哈希集来记录每个数字的每个数字的平方和。
一旦当前总和无法添加到集合，返回false;（说明重复出现了，HashSet不允许出现重复元素；）
一旦当前总和等于1，返回true;

``` java
public boolean isHappy(int n) {
    Set<Integer> inLoop = new HashSet<Integer>();
    int squareSum,remain;
	while (inLoop.add(n)) { // 重复出现了， hashSet无法add，则此方法为false
		squareSum = 0;
		while (n > 0) {
		    remain = n%10;
			squareSum += remain*remain;
			n /= 10;
		}
		if (squareSum == 1)
			return true;
		else
			n = squareSum;

	}
	return false;

}
``` 