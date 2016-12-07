---
title: Super Ugly Number.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Write a program to find the nth super ugly number.
Super ugly numbers are positive numbers whose all prime factors are in the given prime list primes of size k. 
For example, [1, 2, 4, 7, 8, 13, 14, 16, 19, 26, 28, 32] is the sequence of the first 12 super ugly numbers given primes = [2, 7, 13, 19] of size 4.
Note:

	(1) 1 is a super ugly number for any given primes.
	(2) The given numbers in primes are in ascending order.
	(3) 0 < k ≤ 100, 0 < n ≤ 106, 0 < primes[i] < 1000.
<!-- more -->


**思路1：**
仿照Ugly Number II 的用List的思路，超时；
``` java
public class Solution {
    public int nthSuperUglyNumber(int n, int[] primes) {
        int ans = Integer.MAX_VALUE, len = primes.length;
        List<List<Integer>> list = new ArrayList<List<Integer>> ();
        
        for(int i=0;i<len;i++){
            List<Integer> temp = new ArrayList<Integer>();
            list.add(temp);
        }

        for(int i=0;i<len;i++){
          
            list.get(i).add(1);
        }

        for(int i=0;i<n;i++){
            ans = list.get(0).get(0);
            for(int j=0;j<len-1;j++){
                ans = Math.min(ans,list.get(j+1).get(0));
            }
            
            for(int j=0;j<len;j++){
                 if(ans == list.get(j).get(0)) 
                    list.get(j).remove(0);
            }

            for(int j=0;j<len;j++){
                 list.get(j).add(ans*primes[j]);
            }

        }   
        return ans;
    }
}
``` 

**思路2：**
仿照Ugly Number II 的用数组，三个指针的思路
``` java
ugly number II：
public int nthUglyNumber(int n){
	int i2=0, i3=0, i5=0;
	int[] k = new int[n];
	k[0] = 1;
	for (int i=1; i<n; i++) {
		k[i] = Math.min(k[i2]*2, Math.min(k[i3]*3, k[i5]*5));
		if (k[i]%2 == 0) i2++;
		if (k[i]%3 == 0) i3++;
		if (k[i]%5 == 0) i5++;
	}
	
	return k[n-1];
}

Similarly, for this problem, just use loop to replace above i2, i3, i5.

public int nthSuperUglyNumber(int n, int[] primes) {
    int len = primes.length;
    int[] index = new int[len]; //index[0]==0, ... index[len-1]==0
    int[] res = new int[n];
    res[0] = 1;
    for(int i=1; i<n; i++) {
        int min = Integer.MAX_VALUE;
        for(int j=0; j<len; j++){
            min = Math.min(res[index[j]]*primes[j], min);
        }
        res[i] = min;
        for (int j=0; j<len; j++) {
            if(res[i]%primes[j]==0) index[j]++;
        }

    }
    
    return res[n-1];
}
``` 
