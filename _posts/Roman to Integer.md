---
title: Roman to Integer.java
date: 
categories: LeetCode
tags: [Easy, Roman Numeral]
---
Given a roman numeral, convert it to an integer.
Input is guaranteed to be within the range from 1 to 3999.
<!-- more -->
**思路1：**
罗马字母表：
http://wenku.baidu.com/link?url=ilLmXotR8yKePd80_gjicFaEIxqQWCy1Zmtdpl_JrnBHrTHt3STHiZys9pKj3raY_n4otk_na3lCxK44ehrEoAVUs36BlpT_j23oL_2ipPi
从前往后扫描，用一个临时变量记录分段数字。

如果当前处理的字符对应的值和上一个字符一样，那么临时变量加上这个字符。比如III = 3
如果当前比前一个大，说明这一段的值应该是当前这个值减去前面记录下的临时变量中的值。比如IIV = 5 – 2
如果当前比前一个小，那么就可以先将临时变量的值加到结果中，然后开始下一段记录。比如VI = 5 + 1
``` java
public class Solution {
    public int romanToInt(String s) {
        int res = 0;
		for(int i=0;i<s.length();i++){
			if(s.charAt(i) == 'M'){
				res += 1000;
				continue;
			}else if(s.charAt(i) == 'D'){
				res += 500;
			}else if(s.charAt(i) == 'C'){
				if(i<s.length()-1 && s.charAt(i+1) == 'M'){
					res += 900;
					i++;
				}else if(i<s.length()-1 && s.charAt(i+1) == 'D'){
					res += 400;
					i++;
				}else{
					res += 100;
				}
			}else if(s.charAt(i) == 'L'){
				res += 50;
			}else if(s.charAt(i) == 'X'){
				if(i<s.length()-1 && s.charAt(i+1) == 'C'){
					res += 90;
					i++;
				}else if(i<s.length()-1 && s.charAt(i+1) == 'L'){
					res += 40;
					i++;
				}else{
					res += 10;
				}
			}else if(s.charAt(i) == 'V'){
				res += 5;
			}else if(s.charAt(i) == 'I'){
				if(i<s.length()-1 && s.charAt(i+1) == 'X'){
					res += 9;
					i++;
				}else if(i<s.length()-1 && s.charAt(i+1) == 'V'){
					res += 4;
					i++;
				}else{
					res += 1;
				}
			}
		}
		return res;
    }
}
```
**写成函数形式：**
``` java
public class Solution {  
      
    public int romanToInt(String s) {  
        if(s.length() < 1) return 0;  
        int result = 0;  
        int sub = getRomanValue(s.charAt(0));  
        int lastv = sub;  
        for(int i = 1 ; i < s.length(); ++i) {  
            char curc = s.charAt(i);  
            int curv = getRomanValue(curc);  
            if(curv == lastv)   
                sub += curv;  
            else if( curv < lastv) {  
                result += sub;  
                sub = curv;  
            } else {  
                sub = curv - sub;  
            }  
            lastv = curv;  
        }  
        result += sub;  
        return result;  
    }  
    public int getRomanValue(char c) {  
        switch(c) {  
            case 'I': return 1;   
            case 'V': return 5;  
            case 'X': return 10;  
            case 'L': return 50;  
            case 'C': return 100;  
            case 'D': return 500;  
            case 'M': return 1000;  
            default: return 0;  
        }  
    }  
}  
```
