---
title: Generate Parentheses.java
date: 
categories: LeetCode
tags: [Medium, 卡特兰数, 重做]
---
Given n pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
For example, given n = 3, a solution set is:
``` java
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
```
<!-- more -->
**思路：**
这道题按照DFS那种递归想法解决。
给定的n为括号对，所以就是有n个左括号和n个右括号的组合。

	1. 如果左括号数还没有用完，那么我们能继续放置左括号
	2. 如果已经放置的左括号数大于已经放置的右括号数，那么我们可以放置右括号 （如果放置的右括号数大于放置的左括号数，会出现不合法组合，也就是剩余的左括号大于有括号的时候，类似“)(” 这种解是不合法的。）

所以，运用dfs在每一层递归中，如果满足条件先放置左括号，如果满足条件再放置右括号

直接把str ＋（添加的字符）当参数传入下一层调用函数，这样返回后在上一层是之前传入的参数，不用删字符
``` java
public class Solution {
    public List<String> generateParenthesis(int n) {
        ArrayList<String> res = new ArrayList<String>();
		int left = n, right = n;
		String str = new String();
		dfs(res,str,left,right);
		return res;
		
    }
	public void dfs(ArrayList<String> res,String str,int left,int right){
		if(left > right) return;             //left:可用左括号数；right：可用右括号数
		if(left==0 && right==0) {
			res.add(str);
		}
		if(left > 0){
			dfs(res,str+"(",left-1,right);
		}
		if(right > 0){
			dfs(res,str+")",left,right-1);
		}
	}
}
```

**卡特兰数：**
这道题其实是关于卡特兰数的，如果只是要输出结果有多少组，那么直接用卡特兰数的公式就可以。

"从《编程之美》买票找零问题说起，娓娓道来卡特兰数——兼爬坑指南"
http://www.cnblogs.com/wuyuegb2312/p/3016878.html