---
title: Merge Sorted Array.java
date: 
categories: LeetCode
tags: [Easy,重做]
---
Given two sorted integer arrays nums1 and nums2, merge nums2 into nums1 as one sorted array.
Note:
You may assume that nums1 has enough space (size that is greater or equal to m + n) to hold additional elements from nums2. The number of elements initialized in nums1 and nums2 are m and n respectively.
<!-- more -->
**思路：**
从后往前比较；
对每个数组维护一个指针，然后比较指针所指的值，将大的放入新数组的最后中，然后比较为大的数组和新数组指针都左移一位（index-1） ，另一个不动。这里从后往前扫是因为这个题目中结果仍然放在A中，如果从前扫会有覆盖掉未检查的元素的可能性。
**从小到大排序一般都是从后往前比较，结果从后往前放，后面的不管怎么比都比前面的大，不会影响到前面的数。反之，如果从前往后比较，得出的较大的那个值，还要判断它跟后面的数的大小！**

``` java
public class Solution {  
    public void merge(int[] nums1, int m, int[] nums2, int n) {  
        int a_index = m - 1;  
        int b_index = n - 1;  
        int a_cur = m + n - 1;  
        while(a_index >= 0 && b_index >= 0){  
            if(nums1[a_index] >= nums2[b_index]){  
                nums1[a_cur] = nums1[a_index];  
                a_cur--;  
                a_index--;  
            }else{  
                nums1[a_cur] = nums2[b_index];  
                a_cur--;  
                b_index--;  
            }  
        }  
        while(b_index >= 0){  
            nums1[a_cur] = nums2[b_index];  
            a_cur--;  
            b_index--;  
        }  
    }  
}  

```