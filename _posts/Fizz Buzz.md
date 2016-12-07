---
title: Fizz Buzz.java
date: 
categories: LeetCode
tags: Easy
---



Write a program that outputs the string representation of numbers from 1 to n.

But for multiples of three it should output “Fizz” instead of the number and for the multiples of five output “Buzz”. For numbers which are multiples of both three and five output “FizzBuzz”.
<!-- more -->
**关键：**
List<String> test = new ArrayList<String>(); 列表add方法：list.add(str);
int转String三种方法：
1》String.valueOf(i)
2》 Integer.toString(i)
3》 i+""


``` java
public class Solution {
    public List<String> fizzBuzz(int n) {
        
        List<String> test = new ArrayList<String>();
        String str = new String();
        
        for(int i=1; i <=n;i++){
            if(i%3 == 0 & i%5 != 0){
                test.add("Fizz");
            }
            else if(i%3 != 0 & i%5 == 0){
                test.add("Buzz");
            }
            else if(i%3 == 0 & i%5 == 0){
                test.add("FizzBuzz");
            }
            else{
                str = Integer.toString(i);
                test.add(str);
            }
        }
        return test;     
    }
}
```
