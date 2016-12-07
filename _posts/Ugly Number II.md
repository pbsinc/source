---
title: Ugly Number II.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Write a program to find the n-th ugly number.
Ugly numbers are positive numbers whose prime factors only include 2, 3, 5. For example, 1, 2, 3, 4, 5, 6, 8, 9, 10, 12 is the sequence of the first 10 ugly numbers.
Note that 1 is typically treated as an ugly number.
Hint:

	The naive approach is to call isUgly for every number until you reach the nth one. Most numbers are not ugly. Try to focus your effort on generating only the ugly ones.
	An ugly number must be multiplied by either 2, 3, or 5 from a smaller ugly number.
	The key is how to maintain the order of the ugly numbers. Try a similar approach of merging from three sorted lists: L1, L2, and L3.
	Assume you have Uk, the kth ugly number. Then Uk+1 must be Min(L1 * 2, L2 * 3, L3 * 5).
<!-- more -->


**思路1：**
按照本题的提示;
将每次得到的丑数从list中删除；
与思路二一致，只是方法不同；
``` java
public class Solution {
    public int nthUglyNumber(int n) {
        int ans = 0;
        List<Integer> list1 = new ArrayList<Integer>();
        List<Integer> list2 = new ArrayList<Integer>();
        List<Integer> list3 = new ArrayList<Integer>();
        
        list1.add(1);
        list2.add(1);
        list3.add(1);
        
        for(int i=0;i<n;i++){
            ans = Math.min(Math.min(list1.get(0),list2.get(0)),list3.get(0));
            
            if(ans == list1.get(0)) list1.remove(0);
            if(ans == list2.get(0)) list2.remove(0);
            if(ans == list3.get(0)) list3.remove(0);
            
            list1.add(ans*2);
            list2.add(ans*3);
            list3.add(ans*5);
        }   
        return ans;
        
    }
}
``` 

**思路2：**
The ugly-number sequence is 1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, …
because every number can only be divided by 2, 3, 5, one way to look at the sequence is to split the sequence to three groups as below:

	(1) 1×2, 2×2, 3×2, 4×2, 5×2, …
	(2) 1×3, 2×3, 3×3, 4×3, 5×3, …
	(3) 1×5, 2×5, 3×5, 4×5, 5×5, …
We can find that every subsequence is the ugly-sequence itself (1, 2, 3, 4, 5, …) multiply 2, 3, 5.

Then we use similar merge method as merge sort, to get every ugly number from the three subsequence.

Every step we choose the smallest one, and move one step after,including nums with same value.

Thanks for this author about this brilliant idea. Here is my java solution
``` java
public class Solution {
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
}
``` 


**思路3：**
维护一个ordered data structure to store ugly number, every time pop first one.
但是太慢了；暴力做法；
``` java
public class Solution {
    public int nthUglyNumber(int n) {  
        SortedSet<Long> s1 = new TreeSet<>();  
        s1.add((long)1);  
        long result = s1.first();  
        for(int i=0;i<n;i++){  
            result = s1.first();  
            s1.add(result * 2);  
            s1.add(result * 3);  
            s1.add(result * 5);  
            s1.remove(result);  
        }  
        return (int)result;  
    }  
}
``` 
