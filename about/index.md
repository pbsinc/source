---
title: about
date: 2016-09-27 21:04:22
---
test
Given two binary strings, return their sum (also a binary string).：

- **For example,**
- **a = "11"**
- **b = "1"**
- **Return "100".**

-------------------
**关键思路：**
char ta= '1';
int test = ta - '0';
则：test 结果为 1
char 型的数字'1'的int值为49，'0'为48


``` java
public class Solution {
    public String addBinary(String a, String b) {
     int na = a.length();
     int nb = b.length();
     int max = Math.max(na,nb);
     int carry = 0;
     String out = "";
     for(int i=1;i<=max;i++)
     {
         int p=0,q=0;
         if(na-i >= 0)
         {
             p = a.charAt(na-i) - '0';
         }
         else
             p = 0;
             
         if(nb-i >=0)
         {
             q = b.charAt(nb-i) - '0';
         }
         else
             q = 0;
         
         int temp = p + q + carry;
         carry = temp / 2;
         out = temp % 2 + out;
     }
     if (carry == 1)
     {
         out = 1 + out;
     }
     return out;
    }
}
```