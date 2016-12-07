---
title: 3Sum.java
date: 
categories: LeetCode
tags: Medium
---
Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.
Note: The solution set must not contain duplicate triplets.
For example, given array S = [-1, 0, 1, 2, -1, -4],A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
<!-- more -->

**思路：**
对于这样的无序数组首先进行升序排列，使之变成非递减数组，调用java自带的Arrays.sort()方法即可。
然后每次固定最小的那个数，在后面的数组中找出另外两个数之和为该数的相反数即可。
``` java
public class Solution {
    List<List<Integer>> ans = new ArrayList<List<Integer>>();
    public List<List<Integer>> threeSum(int[] nums) {
        Arrays.sort(nums);  //见java总结
        if(nums.length < 3)
            return ans;
        for(int i=0;i<nums.length-2;i++){
            if(i>0 && nums[i]==nums[i-1])
                continue;
            int target = nums[i];
            int begin = i+1;
            int end = nums.length - 1;
            TwoSum(nums,begin,end,target);
        }
        return ans;
    }
    public void TwoSum(int[] nums,int begin,int end,int target){
        while(begin < end){
            if(nums[begin] + nums[end] + target == 0){
                List<Integer> temp = new ArrayList<Integer>();
                temp.add(nums[begin]);
                temp.add(nums[end]);
                temp.add(target);
                ans.add(temp);
                while(begin<end && nums[begin +1] == nums[begin])
                    begin++;
                begin++;
                while(begin<end && nums[end-1] == nums[end])
                    end--;
                end--;    
            }
            else if(nums[begin] + nums[end] + target > 0)
                end--;
            else
                begin++;
        }
    }
}
```