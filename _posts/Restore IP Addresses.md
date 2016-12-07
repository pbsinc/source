---
title: Restore IP Addresses.java
date: 
categories: LeetCode
tags: [Medium,重做,String.equals(str)方法,String.substring( )方法]
---
Given a string containing only digits, restore it by returning all possible valid IP address combinations.
For example:
Given "25525511135",
return ["255.255.11.135", "255.255.111.35"]. (Order does not matter)
<!-- more -->
**思路：**
首先我们要明确传统IP 地址的规律是分4个Part，每个Part是从0-255的数字

0-255的数字，转换成字符，即每个Part 可能由一个字符组成，二个字符组成，或者是三个字符组成。那这又成为组合问题了，dfs便呼之欲出

所以我们写一个For循环每一层从1个字符开始取一直到3个字符，再加一个isValid的函数来验证取的字符是否是合法数字，如果是合法的数字，我们再进行下一层递归，否则跳过。

``` java
    public ArrayList<String> restoreIpAddresses(String s) {  
        ArrayList<String> res = new ArrayList<String>();  
        String item = new String();
        if (s.length()<4||s.length()>12) 
        return res;  
        
        dfs(s, 0, item, res);  
        return res;  
    }  
      
    public void dfs(String s, int start, String item, ArrayList<String> res){  
        if (start == 3 && isValid(s)) {  
            res.add(item + s);  
            return;  
        }
        if (start == 3 && !isValid(s))  // 到了最后一块，不符合直接返回，不再继续分块往下dfs
            return;  
			
        for(int i=0; i<3 && i<s.length()-1; i++){  
            String substr = s.substring(0,i+1);  
            if (isValid(substr))
                dfs(s.substring(i+1, s.length()), start+1, item + substr + '.', res);  
        }  
    }  
      
    public boolean isValid(String s){  
        if (s.charAt(0)=='0')
            return s.equals("0");  
            int num = Integer.parseInt(s);
            
        if(num <= 255 && num > 0)
            return true;
        else
            return false;
    }
``` 
**几点注意的地方：**

	1. 在验证字符串是否是数字的时候，要注意0的情况，001，010，03都是非法的。所以，如果第一位取出来是0，那么我们就判断字符串是否是"0"，不是的情况都是非法的
	2. 取字符串的时候，注意位数不够的问题，不仅<4, 而且<s.length()
	3. 注意substring的范围
	4. 字符串转换成数字 Integer.parseInt(); 
	5. 别忘了IP 地址里面的 "."
	6. 到第4个Part的时候我们就可以整体验证剩下的所有字符串（因为第4个Part最后一定要取到结尾才是正确的字符串）

转自：http://blog.csdn.net/u011095253/article/details/9158449

**附注：**

**String.equals(str)方法：**
相等为true；否则为false。

**String.substring(int start, int stop)方法：**
返回值：
一个新的字符串，该字符串值包含 stringObject 的一个子字符串，其内容是从 start 处到 stop-1 处的所有字符，其长度为 stop 减 start。
说明：
substring() 方法返回的子串包括 start 处的字符，但不包括 stop 处的字符。
如果参数 start 与 stop 相等，那么该方法返回的就是一个空串（即长度为 0 的字符串）。如果 start 比 stop 大，那么该方法在提取子串之前会先交换这两个参数。

	var str="Hello world!"
	document.write(str.substring(3))
	输出：
	lo world!
	
	document.write(str.substring(3,7))
	输出：
	lo w