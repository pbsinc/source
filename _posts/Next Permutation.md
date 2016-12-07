---
title: Next Permutation.java
date: 
categories: LeetCode
tags: Medium
---
Implement next permutation, which rearranges numbers into the lexicographically next greater permutation of numbers.
If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).
The replacement must be in-place, do not allocate extra memory.
Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
1,2,3 → 1,3,2
3,2,1 → 1,2,3
1,1,5 → 1,5,1
<!-- more -->
**思路：**
数学中的排列组合，比如“1，2，3”的全排列，依次是：
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1

网上看来一个示例，觉得挺好的，也没必要另外找一个了。
6 5 4 8 7 5 1
一开始没看对方的后面介绍，就自己在想这个排列的下一个排列是怎样的。
首先肯定从后面开始看，1和5调换了没有用。
7、5和1调换了也没有效果，因此而发现了8、7、5、1是递减的。
如果想要找到下一个排列，**找到递增的位置**是关键。
因为在这里才可以使其增长得更大。
于是找到了4，显而易见4过了是5而不是8或者7更不是1。
因此就需要找出比4大但在这些大数里面最小的值，并将其两者调换。
那么整个排列就成了：6 5 5 8 7 4 1
然而最后一步将后面的8 7 4 1做一个递增。

``` java
public class Solution {
    public void nextPermutation(int[] nums) {
        int i = nums.length - 1;
		int temp = 0;
		while(i>0){
			if(nums[i-1]>=nums[i]){
				i--;
			}
			else{
			    temp = nums[i-1];
				int j = i;
				while(j<nums.length){
					if(temp < nums[j]){
						j++;
					}
					else{
						temp = nums[j-1];
						nums[j-1] = nums[i-1];
						nums[i-1] = temp;
						break;
					}
					if(j == nums.length){
						temp = nums[j-1];
						nums[j-1] = nums[i-1];
						nums[i-1] = temp;
						break;
					}
				}
				break;
			}
		}
		int k = nums.length - 1;
		while(i<k){
			temp = nums[i];
			nums[i] = nums[k];
			nums[k] = temp;       //Collections.swap(nums, i, k);   Collections工具类见java总结
			i++;
			k--;
		}
    }
}
```