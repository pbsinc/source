---
title: Permutation Sequence.java
date: 
categories: LeetCode
tags: Medium
---
The set [1,2,3,…,n] contains a total of n! unique permutations.
By listing and labeling all of the permutations in order,
We get the following sequence (ie, for n = 3):

	1."123"
	2."132"
	3."213"
	4."231"
	5."312"
	6."321"
Given n and k, return the kth permutation sequence.
Note: Given n will be between 1 and 9 inclusive.
<!-- more -->
**思路：**
利用StringBuilder，读一个，删一个；
n,k动态变化，递归；
``` java
public class Solution {
    public String getPermutation(int n, int k) {
        StringBuilder sb = new StringBuilder();
		for(int i=1;i <=n;i++){
			sb.append(i);
		}
		StringBuilder res = new StringBuilder();
		return calculate(n,k-1,sb,res);
    }
	public String calculate(int n,int k,StringBuilder sb,StringBuilder res){
		if(n==0)return res.toString();
		int temp1 = 1, newn = n;
		
		while(newn!=1){
			temp1 *= newn-1;
			newn--;
		}
		int temp2 = k/temp1;
		res.append(sb.charAt(temp2));
		sb.deleteCharAt(temp2);
		k -= temp2*temp1;
		n--;
		return calculate(n,k,sb,res);
	}
}
``` 

