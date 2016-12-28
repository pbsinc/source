---
title: Count Complete Tree Nodes.java
date: 
categories: LeetCode
tags: [Medium,重做,完全二叉树,满二叉树]
---
Given a complete binary tree, count the number of nodes.
Definition of a complete binary tree from Wikipedia:
In a complete binary tree every level, except possibly the last, is completely filled, and all nodes in the last level are as far left as possible. It can have between 1 and 2h nodes inclusive at the last level h.
<!-- more -->
**我的思路：**
直接递归，不过超时；
``` java
public class Solution {
    int count = 0;
    public int countNodes(TreeNode root) {
        if(root == null) return 0;
        return 1+countNodes(root.left)+countNodes(root.right);
    }
}
``` 
**思路：**
优先排除满二叉树的情况，可以省很多时间；
``` java
class Solution {
    public int countNodes(TreeNode root) {
        if (root == null)
            return 0;
        TreeNode left = root, right = root;
        int height = 0;
        while (right != null){
            left = left.left;
            right = right.right;
            height++;
        }
        if (left == null)
            return (1 << height) - 1;//满二叉树的情况，实际是2^height-1
        return 1 + countNodes(root.left) + countNodes(root.right);
    }
}
``` 
**完全二叉树**
若设二叉树的深度为h，除第 h 层外，其它各层 (1～h-1) 的结点数都达到最大个数，第 h 层所有的结点都连续集中在最左边，这就是完全二叉树。

完全二叉树是由满二叉树而引出来的。对于深度为K的，有n个结点的二叉树，当且仅当其每一个结点都与深度为K的满二叉树中编号从1至n的结点一一对应时称之为完全二叉树。

一棵二叉树至多只有最下面的一层上的结点的度数可以小于2，并且最下层上的结点都集中在该层最左边的若干位置上，则此二叉树成为完全二叉树。
http://baike.baidu.com/link?url=t8J_HKq7DwEsgE9kR6PA1caenDakG-RCLgMxLftC875xQleErwNojXPXVys9ljhSkOiWHPOp_KrI9RzJ4C5vZatylvFwAH-hassWxVeje4xKw-hsLy0Fie98IfjZGZkX0FU9zoAQ1Px7k6f6F1J9oq


**满二叉树**
除最后一层无任何子节点外，每一层上的所有结点都有两个子结点二叉树。

共有**2^height-1**个节点
http://baike.baidu.com/link?url=1bZaXpb6aT1LVV_2iOWFMQKRxOmUNHw2A4fFfHQqmVjWqywyB-y1qqYNoYm5Een8Ym1tQHAnDjzEBYmJduR6ft9AvMgtfXGqovaanCCaKB_5uw3RyE5bUmAr85ucLjfa