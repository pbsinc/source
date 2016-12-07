---
title: Two Sum.java
date: 
categories: LeetCode
tags: Easy
---
Given an array of integers, return indices of the two numbers such that they add up to a specific target.
You may assume that each input would have exactly one solution.
Example:
Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,    return [0, 1].
<!-- more -->

**注意：**
return 一个int[]，首先定义一个int[]
``` java
public class Solution {
    public int[] twoSum(int[] nums, int target) {
            int[] ans = new int[2];
            int len = nums.length;
            for(int i =0;i<len;i++){
                 for(int j = 0;j<len;j++){
                     if(i!=j & nums[i]+nums[j]==target){
                         ans[0]=i;
                         ans[1]=j;
                         return ans;
                     }
                 }
            }
            return ans;
    }
}
```


**用HashMap解答：**
利用HashMap，把target-numbers[i]的值放入hashmap中，value存index。遍历数组时，检查hashmap中是否已经存能和自己加一起等于target的值存在，存在的话把index取出，连同自己的index也出去，存入结果数组中返回。如果不存在的话，把自己的值和index存入hashmap中继续遍历。由于只是一遍遍历数组，时间复杂度为O(n)。
``` java
public class Solution {
public int[] twoSum(int[] numbers, int target) {
        int [] res = new int[2];
        if(numbers==null||numbers.length<2)
            return res;
        HashMap<Integer,Integer> map = new HashMap<Integer,Integer>();
        for(int i = 0; i < numbers.length; i++){
            if(!map.containsKey(target-numbers[i])){
                map.put(numbers[i],i);
            }else{
                res[0]= map.get(target-numbers[i]);
                res[1]= i;
                break;
            }
        }
        return res;
    }
}
```

**HashMap详解:**
http://www.cnblogs.com/chenssy/p/3521565.html