---
title: Count Primes.java
date: 
categories: LeetCode
tags: [Easy,重做]
---
Count the number of prime numbers less than a non-negative number, n.
![.](https://leetcode.com/static/images/solutions/Sieve_of_Eratosthenes_animation.gif)
<!-- more -->
**思路：**
如果一个数是另一个数的倍数，那这个数肯定不是素数。
利用这个性质，我们可以建立一个素数数组，从2开始将素数的倍数都标注为不是素数。
第一轮将4、6、8等表为非素数，然后遍历到3，发现3没有被标记为非素数，则将6、9、12等标记为非素数，一直到N为止，再数一遍素数数组中有多少素数。
``` java
public class Solution {
    public int countPrimes(int n) {
        boolean[] primes = new boolean[n];
        Arrays.fill(primes,true);
        for(int i=2;i*i<n;i++){
            if(primes[i]){
				// 将i的2倍、3倍、4倍...都标记为非素数
                for(int j=i*i;j<n;j+=i)
                    primes[j] = false;
            }
        }
        int count = 0;
        for(int i=2;i<n;i++){
            if(primes[i])
                count++;
        }
        return count;
    }
}
``` 
Hint：
https://leetcode.com/problems/count-primes/