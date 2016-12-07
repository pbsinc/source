---
title:  Find Minimum in Rotated Sorted Array II.java
date: 
categories: LeetCode
tags: Hard
---
Follow up for "Find Minimum in Rotated Sorted Array":
What if duplicates are allowed?
Would this affect the run-time complexity? How and why?
Suppose a sorted array is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
Find the minimum element.
The array may contain duplicates.
<!-- more -->
**思路：**
对于数组里找某个值得问题显然首选二分法，对于此题有两种情况：
1. 中间值大于左值，此时左半边数组是有序的，所以左半边最小值是左半边第一个。
2. 中间值小于左值，此时右半边数组是有序的，所以右半边最小值是右半边第一个。
3. 如果数组中有重复元素，则还有可能是中间值等于左值，此时只需要左值右移一位即可。
``` java
public class Solution {
    public int findMin(int[] num) {
        int left = 0;
        int right = num.length-1;
        int min = Integer.MAX_VALUE;
        while(left < right-1){
            int mid = (left + right)/2;
            if(num[mid] > num[left]){
                min = Math.min(min, num[left]);
                left = mid + 1;
            }
            else if(num[mid] < num[left]){
                min = Math.min(min, num[mid]);
                right = mid - 1;
            }
            else{
                left++;
            }
        }
        min = Math.min(Math.min(min, num[left]), num[right]);
        return min;
    }
}
```

