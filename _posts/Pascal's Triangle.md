---
title: Pascal's Triangle.java
date: 
categories: LeetCode
tags: Easy
---
Given numRows, generate the first numRows of Pascal's triangle.
For example, given numRows = 5, Return
``` java
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```
<!-- more -->
**思路：**
杨辉三角，当前值是上一层紧邻两个的和

``` java
public class Solution {
    public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> out = new ArrayList<List<Integer>>();
        for(int i=0;i<numRows;i++){
            List<Integer> inter = new ArrayList<Integer>();
            for(int j=0;j<=i;j++){
                int temp = 1;
                if(j>0 && j<i){
                    temp = out.get(i-1).get(j) + out.get(i-1).get(j-1);
                    inter.add(temp);
                }
                else
                    inter.add(temp);
            }
            out.add(inter);
        }
        return out;
    }
}
```
**List 接口 用法:**
1、掌握 List 接口与Collection 接口的关系
2、掌握 List 接口的常用子类：ArrayList 、Vector
3、掌握 ArrayList 与 Vector 类的区别
http://blog.csdn.net/hanshileiai/article/details/6726259