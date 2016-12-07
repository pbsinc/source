---
title: Pascal's Triangle II.java
date: 
categories: LeetCode
tags: Easy
---
Given an index k, return the kth row of the Pascal's triangle.
For example, given k = 3,Return [1,3,3,1].
Note:
Could you optimize your algorithm to use only O(k) extra space?
<!-- more -->
**我的思路：**
杨辉三角，百度百科 ： http://baike.baidu.com/link?url=tDS0pG6dVFeF5Wu2bej3iTV7uT6zCM6j_qPBG-NwWkp5vfHz1u1DCyTd-_7P5h-4PmVuM2BuE8TttuUDRygTG_
第n行第m个值为C(n-1,m-1)，所以我的做法是实现这个算法，但不知为何n大于14开始，最后几位出错。
``` java
public class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> out = new ArrayList<Integer>();
        out.add(1);
        for(int i=1;i<=rowIndex;i++){
            int temp1 = rowIndex,ta=1,tb=1,temp2=1,time = i;
            while(time>0){
                ta = ta * (temp1--);
                time--;
                tb = tb * (temp2++);
            }
            out.add(ta/tb);
        }
        return out;
    }
}
```
**答案思路:**
这里只需要求出某一行的结果。Pascal's Triangle中因为是求出全部结果，所以我们需要上一行的数据就很自然的可以去取。而这里我们只需要一行数据，就得考虑一下是不是能只用一行的空间来存储结果而不需要额外的来存储上一行呢？这里确实是可以实现的。对于每一行我们知道如果从前往后扫，第i个元素的值等于上一行的res[i]+res[i+1]，可以看到数据是往前看的，如果我们只用一行空间，那么需要的数据就会被覆盖掉。所以这里采取的方法是从后往前扫，这样每次需要的数据就是res[i]+res[i-1]，我们需要的数据不会被覆盖，因为需要的res[i]只在当前步用，下一步就不需要了。这个技巧在动态规划省空间时也经常使用，主要就是看我们需要的数据是原来的数据还是新的数据来决定我们遍历的方向。时间复杂度还是O(n^2)，而空间这里是O(k)来存储结果，仍然不需要额外空间。代码如下：
``` java
public class Solution {
    public List<Integer> getRow(int rowIndex) {
        List<Integer> out = new ArrayList<Integer>();
        out.add(1);
        for(int i=1;i<=rowIndex;i++){      //这里一定要倒叙，因为是前项相加，顺序的话，上一行的值已经被覆盖了
            for(int j=out.size()-1;j>0;j--)
                out.set(j,out.get(j)+out.get(j-1));
            out.add(1);
        }
        return out;
    }
}
```