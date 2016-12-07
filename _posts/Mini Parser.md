---
title: Mini Parser.java
date: 
categories: LeetCode
tags: [Medium,没做]
---
Given a nested list of integers represented as a string, implement a parser to deserialize it.
Each element is either an integer, or a list -- whose elements may also be integers or other lists.
Note: You may assume that the string is well-formed:

· String is non-empty.
· String does not contain white spaces.
· String contains only digits 0-9, [, - ,, ].

Example 1:
Given s = "324",
You should return a NestedInteger object which contains a single integer 324.

Example 2:
Given s = "[123,[456,[789]]]",
Return a NestedInteger object containing a nested list with 2 elements:

1. An integer containing value 123.
2. A nested list containing two elements:
    i.  An integer containing value 456.
    ii. A nested list with one element:
         a. An integer containing value 789.
<!-- more -->
**思路：**
用栈维护一个包含关系，类似于用栈维护带 '(' 的表达式。

思路不难想到，有个坑爹的细节要注意：添加一个数到list type的nestedInteger时 要将该数封装为integer type的NestedInteger，
然后用add方法添加该nestedInteger，不能直接用setInteger，否则会把list type的nestedInteger定义为一个integer type，会出错。
``` java
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *     // Constructor initializes an empty nested list.
 *     public NestedInteger();
 *
 *     // Constructor initializes a single integer.
 *     public NestedInteger(int value);
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // Set this NestedInteger to hold a single integer.
 *     public void setInteger(int value);
 *
 *     // Set this NestedInteger to hold a nested list and adds a nested integer to it.
 *     public void add(NestedInteger ni);
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return null if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class Solution {
    public NestedInteger deserialize(String s) {  
    Stack<NestedInteger> stack = new Stack<NestedInteger>();  
    String tokenNum = "";  
  
    for (char c : s.toCharArray()) {  
        switch (c) {  
        case '['://[代表一个list  
            stack.push(new NestedInteger());  
            break;  
        case ']'://list结尾  
            if (!tokenNum.equals(""))//前面token为数字   
                stack.peek().add(new NestedInteger(Integer.parseInt(tokenNum)));//将数字加入到本层list中  
            NestedInteger ni = stack.pop();//本层list结束  
            tokenNum = "";  
            if (!stack.isEmpty()) {//栈内有更高层次的list  
                stack.peek().add(ni);  
            } else {//栈为空，遍历到字符串结尾  
                return ni;  
            }  
            break;  
        case ',':  
            if (!tokenNum.equals("")) //将数字加入到本层list中  
                stack.peek().add(new NestedInteger(Integer.parseInt(tokenNum)));  
            tokenNum = "";  
            break;  
        default:  
            tokenNum += c;  
        }  
    }  
    if (!tokenNum.equals(""))//特殊case: 如果字符串只包含数字的情况  
        return new NestedInteger(Integer.parseInt(tokenNum));  
    return null;  
	}  
}
```
堆和栈的区别（转过无数次的文章）
http://blog.csdn.net/hairetz/article/details/4141043