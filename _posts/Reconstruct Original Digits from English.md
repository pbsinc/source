---
title: Reconstruct Original Digits from English.java
date: 
categories: LeetCode
tags: Medium
---
Given a non-empty string containing an out-of-order English representation of digits 0-9, output the digits in ascending order.
Note:

	Input contains only lowercase English letters.
	Input is guaranteed to be valid and can be transformed to its original digits. That means invalid inputs such as "abc" or "zerone" are not permitted.
	Input length is less than 50,000.
Example 1:

	Input: "owoztneoer"
	Output: "012"
Example 2:

	Input: "fviefuro"
	Output: "45"
<!-- more -->
**思路：**
对字符串中的字符进行计数，根据这些字符与数字的关系可以直接得到结果。

例如：字符z只在zero中出现，字符w只在two中出现，字符x只在six中出现，字符g只在eigth中出现，字符u只在four中出现

那么我们根据z,w,x,g,u的个数就可以知道0,2,6,8,4的个数。

对于剩下的one,three,five,seven，one可以由字符o的个数减去在0,2,4,6,8中出现的o的个数。

three可以由字符h的个数减去字符t,r,e在0,2,4,6,8中出现的个数

同理可以得到剩下的数字的个数
``` java
public class Solution {
	public static String originalDigits(String s) {
	        if(s == null || s.length() == 0)
	        	return null;
	        int[] count = new int[10];//存放0-9出现的次数
	        for(char c : s.toCharArray())
	        {
	        	if(c == 'z') count[0]++;//'z'只有0才有
	        	if(c == 'w') count[2]++;//'w'只有2才有
	        	if(c == 'x') count[6]++;//'x'只有6才有
	        	if(c == 's') count[7]++;//6,7都有，最后只要count[7]-count[6]即可
	        	if(c == 'g') count[8]++;
	        	if(c == 'u') count[4]++;
	        	if(c == 'f') count[5]++;//5-4
	        	if(c == 'h') count[3]++;//3-8
	        	if(c == 'i') count[9]++;//9-5-6-8
	        	if(c == 'o') count[1]++;//1-2-0-4
	        }
	        	//可以得到各自唯一的字母
	        	count[7] -= count[6];
	        	count[5] -= count[4];
	        	count[3] -= count[8];
	        	count[9] = count[9] - count[5] - count[6] - count[8];
	        	count[1] = count[1] - count[2] - count[0] - count[4];
	        	//开始进行统计
	        	StringBuilder sb = new StringBuilder();
	        	for(int i = 0; i<10; ++i)
	        	{
	        		for(int j = 1; j<=count[i]; ++j)
	        		{
	        			sb.append(i);
	        		}
	        	}
	        	return sb.toString();
	    }
}
```

**我的思路：**
强行查询每个数字的，超时；
为保证唯一性，要按照以下顺序查询，导致最后还有排序问题顺序。
{6, 0, 2, 8, 7, 4, 5, 9, 1, 3};
``` java
public class Solution {
	int temp = 0;
	
    public String originalDigits(String s) {
        StringBuilder sb = new StringBuilder(s);
		StringBuilder res = new StringBuilder(); 
		
		return find(sb, res, 0).toString();
    }
	public StringBuilder find(StringBuilder sb, StringBuilder res,int n){
	    int dig[] = new int[]{6, 0, 2, 8, 7, 4, 5, 9, 1, 3};
		
	    if(n == 10) return res;
	    
		StringBuilder temp = new StringBuilder(sb);
			
		String[][] num = new String[][]{{"z","e","r","o"},{"o","n","e"},{"t","w","o"},{"t","h","r","e","e"},{"f","o","u","r"},{"f","i","v","e"},{"s","i","x"},{"s","e","v","e","n"},{"e","i","g","h","t"},{"n","i","n","e"}};
			
		int count = 0;
		
		for(String i : num[dig[n]]){
			if(temp.indexOf(i) >= 0){
				temp.deleteCharAt(temp.indexOf(i));
			}else{
			    return find(sb, res, n+1);
				
			}
			count++;
			
			if(count == num[dig[n]].length){
				// for(int j=0;j<res.elngth();j++){
					// if(res.charAr(j)-'0' > dig[n])
						// res.insert(i,dig[n]);
				// }
				res.append(String.valueOf(dig[n]));
				
				if(temp.length() == 0){
					return res;
				}else{
					return find(temp, res, n);
				}
				
			}
		}
		
		return res;
	}
}
```  
