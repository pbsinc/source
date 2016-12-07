---
title: Basic Calculator II.java
date: 
categories: LeetCode
tags: [Medium,重做,switch,Character.isDigit(chr)方法,String.trim()方法]
---
Implement a basic calculator to evaluate a simple expression string.
The expression string contains only non-negative integers, +, -, *, / operators and empty spaces . The integer division should truncate toward zero.
You may assume that the given expression is always valid.
Some examples:
"3+2*2" = 7
" 3/2 " = 1
" 3+5 / 2 " = 5
Note: Do not use the eval built-in library function.
<!-- more -->
**思路：**
注意：测试中会出现“4*3/5”这样的情况，因此，可以用一个TempRes保存上一次运算的结果，TempOp表示距离本次运算符最近的+-运算符，具体看代码即可：
``` java
public class Solution {
    public int calculate(String s) {
    int res = 0, tempRes = 0, Option = 1, tempOp = 1;
    for (int i = 0; i < s.length(); i++) {
        switch (s.charAt(i)) {
        case ' ':
            break;
        case '+':
            Option = 1;
            break;
        case '-':
            Option = -1;
            break;
        case '*':
            Option = 2;
            break;
        case '/':
            Option = 3;
            break;
        default:
            int num = 0;
            while (i < s.length() && Character.isDigit(s.charAt(i)))
                num = num * 10 + s.charAt(i++) - '0';
            i--;
            switch (Option) {
            case 1:
                res += tempOp * tempRes;
                tempRes = num;
                tempOp = 1;
                break;
            case -1:
                res += tempOp * tempRes;
                tempRes = num;
                tempOp = -1;
                break;
            case 2:
                tempRes *= num;  //乘除时，临时结果保存下来，不加到最终结果res，等到下一次遇到加减时再加到res上
                break;
            case 3:
                tempRes /= num;
            }
 
        }
    }
    res += tempOp * tempRes;
    return res;
	}
}
```
**Character.isDigit(char cha)方法:**
cha 是数字，返回true;否则 false

**String.trim()方法:**
去掉字符串首位的空格
String str = "   world    ";
str.trim();
str变成"world"
