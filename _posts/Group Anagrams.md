---
title: Group Anagrams.java
date: 
categories: LeetCode
tags: [Medium, 重做, 对String排序]
---
Given an array of strings, group anagrams together.
For example, given: ["eat", "tea", "tan", "ate", "nat", "bat"], 
Return:
[
  ["ate", "eat","tea"],
  ["nat","tan"],
  ["bat"]
]
Note: All inputs will be in lower-case.
<!-- more -->
**思路：**
利用hashcode 的思想。
统计每个字母的个数与 int[26] nums
然后Arrays.hashCode(nums) 拿到哈希值作为hashtable的key。
这道题坑爹之处在于，看题意希望输出的是按字母排序的，其实不是！
如需排序：Collections.sort(list);
``` java
public class Solution {
    public List<List<String>> groupAnagrams(String[] strs) {
		
        List<List<String>> ret = new ArrayList<List<String>>();
		
        if (strs == null || strs.length == 0) {
            return ret;
        }
		
        HashMap<Integer, List<String>> map = new HashMap<Integer, List<String>>();
		
        for (int i = 0; i < strs.length; i++) {
            int id = getID(strs[i]);
            if (!map.containsKey(id)) {
                map.put(id, new ArrayList<String>());
            }
            map.get(id).add(strs[i]);
        }

        for (Integer i : map.keySet()) {
            ret.add(map.get(i));
        }
        return ret;
    }

    private int getID(String s) {
        int[] nums = new int[26];
        for (int i = 0; i < s.length(); i++) {
            nums[s.charAt(i) - 'a']++;
        }
        return Arrays.hashCode(nums);
    }
}
```
**对String排序：**
先转换成char[]，Arrays.sort(chars)后，再转换成String。

for (int i = 0; i < strs.length; i++) {
        char[] chars = strs[i].toCharArray();
        Arrays.sort(chars);
        newStrs[i] = new String(chars);
    }