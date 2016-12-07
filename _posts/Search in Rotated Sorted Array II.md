---
title: Search in Rotated Sorted Array II.java
date: 
categories: LeetCode
tags: [Medium,重做]
---
Follow up for "Search in Rotated Sorted Array":
What if duplicates are allowed?
Would this affect the run-time complexity? How and why?
Write a function to determine if a given target is in the array.
<!-- more -->
**思路：**
在上一题的基础上加入Arrays.sort(nums)即可；
或以下思路：
这道题与之前Search in Rotated Sorted Array类似，问题只在于存在dupilcate。那么和之前那道题的解法区别就是，不能通过比较A[mid]和边缘值来确定哪边是有序的，会出现A[mid]与边缘值相等的状态。所以，解决方法就是对于A[mid]==A[low]和A[mid]==A[high]单独处理。
当中间值与边缘值相等时，让指向边缘值的指针分别往前移动，忽略掉这个相同点，再用之前的方法判断即可。
这一改变增加了时间复杂度，试想一个数组有同一数字组成{1，1，1，1，1}，target=2, 那么这个算法就会将整个数组遍历，时间复杂度由O(logn)升到O(n)
``` java
public boolean search(int [] A,int target){
       if(A==null||A.length==0)
         return false;
        
       int low = 0;
       int high = A.length-1;
      
       while(low <= high){
           int mid = (low + high)/2;
           if(target < A[mid]){
               if(A[mid]<A[high])//right side is sorted
                 high = mid - 1;//target must in left side
               else if(A[mid]==A[high])//cannot tell right is sorted, move pointer high
                 high--;
               else//left side is sorted
                 if(target<A[low])
                    low = mid + 1;
                 else 
                    high = mid - 1;
           }else if(target > A[mid]){
               if(A[low]<A[mid])//left side is sorted
                 low = mid + 1;//target must in right side
               else if(A[low]==A[mid])//cannot tell left is sorted, move pointer low
                 low++;
               else//right side is sorted
                 if(target>A[high])
                    high = mid - 1;
                 else
                    low = mid + 1;
           }else
             return true;
       }
       
       return false;
}
```