---
title: Ransom Note.java
date: 
categories: LeetCode
tags: Easy
---
Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.
Each letter in the magazine string can only be used once in your ransom note.
Note:
You may assume that both strings contain only lowercase letters.
canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
<!-- more -->
**思路1：**
这种解法比较简单，就是将ransomNote的字符串挨个遍历，每个字符再从magazine里遍历匹配，只是再创建了个数组，数组每个元素的索引表示magazine字符串的位置，元素值表示是否被校验过，0表示还未被校验过，非0就表示该位置已经被校验过。
不过这种做法效率不高。
``` java
public class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int len = magazine.length();
        if(ransomNote==null|| ransomNote.length() == 0) return true;
		if(magazine == null || len == 0) return false;
		int[] flag = new int[len];
		for(int i=0;i<ransomNote.length();i++){
			for(int j=0;j<len;j++){
				if(flag[j] == 0 && ransomNote.charAt(i) == magazine.charAt(j)){
					flag[j] = 1;
					break;
				}
				if(j == len-1)
					return false;
			}
		}
		return true;
    }
}
```
**思路2：**
这是LeetCode Discuss中的最热代码，它的原理就是列出了magazine的字母表，然后算出了出现个数，然后遍历ransomNote，保证有足够的字母可用，代码非常清晰。
``` java
public class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] arr = new int[26];
        for (int i = 0; i < magazine.length(); i++) {
            arr[magazine.charAt(i) - 'a']++;
        }
        for (int i = 0; i < ransomNote.length(); i++) {
            if(--arr[ransomNote.charAt(i)-'a'] < 0) {
                return false;
            }
        }
        return true;
    }
}
```
**思路3：**
将ransomNote和magazine都从小到大排序，然后对ransomNote遍历，同时在magazine中匹配，如果匹配到了，则记住此时magazine中字符的索引，便于下次操作，因为都是排序的。如果直到magazine中字符已经大于ransomNote中字符了，就说明就再也匹配不到了，则表示匹配失败。
``` java
public class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        boolean ret = true;
        char[] ra = ransomNote.toCharArray();
        Arrays.sort(ra);
        char[] ma = magazine.toCharArray();
        Arrays.sort(ma);

        int index = 0;
        boolean found = true;
        for (int i = 0; i < ra.length && ret; i++) {
            char ri = ra[i];
            found = false;
            for (int j = index; j < ma.length; j++) {
                if (ma[j] > ri) {
                    ret = false;
                    break;
                } else if (ma[j] == ri) {
                    index++;
                    found = true;
                    break;
                } else {
                    index++;
                }
            }
            if (!found) {
                ret = false;
                break;
            }
        }

        return ret;
    }
}
```
