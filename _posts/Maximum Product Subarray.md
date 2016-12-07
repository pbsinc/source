---
title: Maximum Product Subarray.java
date: 
categories: LeetCode
tags: Medium
---
Find the contiguous subarray within an array (containing at least one number) which has the largest product.
For example, given the array [2,3,-2,4],
the contiguous subarray [2,3] has the largest product = 6.
<!-- more -->
**思路：**
找出0的位置，以0为分割，将数组分为很多小块；
对每小块计算负数的个数；
最终所求得范围里，只要保证负数的个数为偶数即可。
``` java
public class Solution {
    public int maxProduct(int[] nums) {

		if(nums.length == 0) return 0;
		if(nums.length == 1) return nums[0];
		int start = -1;
		int end = -1;
		List<Integer> ans = new ArrayList<Integer>();
		for(int i=0;i<nums.length;i++){
			if(nums[i] == 0){
				start = end;
				end = i;
				ans.add(calculate(nums,start,end));
			}
		}
		start = end;
		end = nums.length;
		if(start != end)
			ans.add(calculate(nums,start,end));
		
		return Collections.max(ans);
		
	}

	public int calculate(int[] nums,int start,int end){
		
		List<Integer> temp = new ArrayList<Integer>();
		int count = 0;   //负值的个数
        for(int i=start+1;i<end;i++){
			if(nums[i] < 0){
				count++;
				temp.add(i);
			}
		}
		
		int res = 0;
			if(start+1 < nums.length)
				res = nums[start+1];
		
		if(count % 2 == 0){
			for(int i=start+2;i<end;i++){
				res = res * nums[i];
			}
		}else{
			int indexFirst = temp.get(0);
			int indexLast = temp.get(temp.size()-1);
			int res1 = nums[start+1];
			int res2 = 0;
			if(indexFirst+1 < nums.length)
				res2 = nums[indexFirst+1];
			for(int i=start+2;i<indexLast;i++){
				res1 = res1 * nums[i];
			}
			for(int i=indexFirst+2;i<end;i++){
				res2 = res2 * nums[i];
			}
			res = Math.max(res1,res2);
		}
		
		return res;
    }
}
```
**动态规划 的思想：**
其实子数组乘积最大值的可能性为：累乘的最大值碰到了一个正数；或者，累乘的最小值（负数），碰到了一个负数。所以每次要保存累乘的最大（正数）和最小值（负数）。
同时还有一个选择起点的逻辑，如果之前的最大和最小值同当前元素相乘之后，没有当前元素大（或小）那么当前元素就可作为新的起点。
例如，前一个元素为0的情况，{1,0,9,2}，到9的时候9应该作为一个最大值，也就是新的起点，{1,0,-9,-2}也是同样道理，-9比当前最小值还小，所以更新为当前最小值。
这种方法只需要遍历一次数组即可，算法时间复杂度为O(n)。
``` java
public class Solution {
    public int maxProduct(int A[]) {
        if(A==null||A.length==0) {
	      return 0;
	    }
	    int maxProduct = A[0];
	    int max_temp   = A[0];
	    int min_temp   = A[0];
	    
	    for(int i=1;i<A.length;i++) {
	    	int a = A[i]*max_temp;
	    	int b = A[i]*min_temp;
	    	max_temp = Math.max(Math.max(a,b), A[i]);
	    	min_temp = Math.min(Math.min(a,b), A[i]);
	    	maxProduct = Math.max(maxProduct, max_temp);
	    }
	    return maxProduct;
    }
}
```
