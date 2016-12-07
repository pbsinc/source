---
title: java值传递&引用传递
date: 
categories: java总结
tags: 值传递&引用传递
---
1.对象就是传引用
2.原始类型就是传值
3.String等immutable类型因为没有提供自身修改的函数，每次操作都是新生成一个对象，所以要特殊对待。可以认为是传值。
<!-- more -->
**扩展说就是：**
1、对象是按引用传递的
2、Java 应用程序有且仅有的一种参数传递机制，即按值传递
3、按值传递意味着当将一个参数传递给一个函数时，函数接收的是原始值的一个副本
4、按引用传递意味着当将一个参数传递给一个函数时，函数接收的是原始值的内存地址，而不是值的副本

**以题Combination Sum为例：**
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