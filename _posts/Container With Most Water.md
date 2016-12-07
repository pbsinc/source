---
title: Container With Most Water.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate (i, ai). n vertical lines are drawn such that the two endpoints of line i is at (i, ai) and (i, 0). Find two lines, which together with x-axis forms a container, such that the container contains the most water.
Note: You may not slant the container.
<!-- more -->
**常规思路:**
LTE，以后时间复杂度是n方的就不要想了，肯定不对。。。
``` java
public class Solution {
    public int maxArea(int[] height) {
        int max=0,temp=0,n = height.length;
        for(int i=1;i<n;i++){
            for(int j=i+1;j<=n;j++){
                temp = (j-i) * Math.min(height[i-1],height[j-1]);
                max = Math.max(max,temp);
            }
        }
        return max;
    }
}
```
**正确思路:**
最大盛水量取决于两边中较短的那条边，而且如果将较短的边换为更短边的话，盛水量只会变少。所以我们可以用两个头尾指针，计算出当前最大的盛水量后，将较短的边向中间移，因为我们想看看能不能把较短的边换长一点。这样一直计算到左边大于右边为止就行了。
注意：如果将较短的边向中间移后，新的边还更短一些，其实可以跳过，减少一些计算量
``` java
public class Solution {
    public int maxArea(int[] height) {
        int max=0,left=0, right = height.length - 1;
        while(left < right){ v
            max = Math.max(max,(right - left)*Math.min(height[left],height[right])); //right - left 减号中间要加空格，否则有可能报错
            // 如果左边较低，则将左边向中间移一点
            if(height[left] < height[right]){
                left++;
            // 如果右边较低，则将右边向中间移一点
            } else {
                right--;
            }
        }
        return max;
    }
}
```
