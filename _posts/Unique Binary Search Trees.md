---
title: Unique Binary Search Trees.java
date: 
categories: LeetCode
tags: [Medium,重做,卡特兰数]
---
Given n, how many structurally unique BST's (binary search trees) that store values 1...n?
For example,
Given n = 3, there are a total of 5 unique BST's.

   1         3     3      2      1
    \       /     /      / \      \
     3     2     1      1   3      2
    /     /       \                 \
   2     1         2                 3
<!-- more -->
**思路：**
注意：二分查找树的定义是，左子树节点均小于root，右子树节点均大于root！不要想当然地将某个点作为root时，认为其他所有节点都能全部放在left/right中，除非这个点是 min 或者 max 的。

分析：本题其实关键是递推过程的分析，n个点中每个点都可以作为root，当 i 作为root时，小于 i  的点都只能放在其左子树中，大于 i 的点只能放在右子树中，此时只需求出左、右子树各有多少种，二者相乘即为以 i 作为root时BST的总数。
开始时，我尝试用递归实现，但是超时了，可见系统对运行时间有要求。因为递归过程中存在大量的重复计算，从n一层层往下递归，故考虑类似于动态规划的思想，让底层的计算结果能够被重复利用，
故用一个数组存储中间计算结果（即 1~n-1 对应的BST数目），这样只需双层循环即可，代码如下：
``` java
public class Solution {
    public int numTrees(int n) {
        int [] G = new int[n+1];
        G[0] = G[1] = 1;
        
        for(int i=2; i<=n; i++) {
        	for(int j=0; j<i; j++) {
        		G[i] += G[j] * G[i-j-1];
        	}
        }
        return G[n];
    }
}
``` 
这种求数量的题目一般都容易想到用动态规划的解法，这道题的模型正好是卡特兰数的定义。
http://baike.baidu.com/link?url=HH_bvU3csgIxUdqBEikq26KxExvxekp6M9EbJXUMplqhnuwb2Rs5nQQ4Qmx2rVCKngoHgR_mc2l7HUdR53R2MCCbHnTAYEkIDw7a3QS5j84Ugn4IDXuYvqGzaFawAUec