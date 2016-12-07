---
title: Combination Sum.java
date: 
categories: LeetCode
tags: [Medium,值传递&引用传递] 
---
Given a set of candidate numbers (C) and a target number (T), find all unique combinations in C where the candidate numbers sums to T.
The same repeated number may be chosen from C unlimited number of times.
Note:
All numbers (including target) will be positive integers.
The solution set must not contain duplicate combinations.
For example, given candidate set [2, 3, 6, 7] and target 7, 
A solution set is: 
[
  [7],
  [2, 2, 3]
]
<!-- more -->
**思路：**
典型使用DFS深度遍历，因为可以重复使用数字，所以注意重复后再++；

``` java
public class Solution {
	List<List<Integer>> ans = new ArrayList<List<Integer>>();
	List<Integer> temp = new ArrayList<Integer>();
    public List<List<Integer>> combinationSum(int[] nums, int target) {
		calculate(nums,target,0,0);
		return ans;
    }
	public void calculate(int[] nums,int target,int sum,int level){
		if(sum > target)
			return;
		else if(sum == target){
			List<Integer> t = new ArrayList<Integer>(temp); 
			//注意每次传给ans的temp必须是新的对象，这是把temp的值赋给新的对象t，此后temp的改变不影响t，temp与t完全无关，这是**值传递**；
			//t = temp;                                    
			//这是t引用了temp，传递的是引用的地址值(即temp的内存空间的地址)，他们指向同一个内存空间，此后temp的改变直接影响t，temp与t完全关联，这是**引用传递**；
			ans.add(t);                                     
			//如直接ans.add(temp);实际上是重复将同一个对象temp往ans里面传，错误。
			return;
		}else{             // DFS
			for(int i=level;i<nums.length;i++){
				sum += nums[i];
				temp.add(nums[i]);
				calculate(nums,target,sum,i);
				temp.remove(temp.size()-1);
				sum -= nums[i];
			}
		}
	}
}
```
**理解按引用传递的过程——内存分配示意图**
http://blog.csdn.net/zzp_403184692/article/details/8184751
**说明**
1：“在Java里面参数传递都是按值传递”这句话的意思是：按值传递是传递的值的拷贝，按引用传递其实传递的是引用的地址值，所以统称按值传递。
2：在Java里面只有基本类型和按照下面这种定义方式的String是按值传递，其它的都是按引用传递。就是直接使用双引号定义字符串方式：String str = “Java私塾”;