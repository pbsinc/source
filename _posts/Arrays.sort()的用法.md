---
title: Arrays.sort()的用法
date: 
categories: java总结
tags: Arrays.sort()的用法
---
Arrays.sort为java中的数组排序。
Arrays.sort(*Array) 需加包import java.util.*;或import java.util.Arrays;
Arrays.sort(数组名)为数组排序的操作,但这个方法在 java.util 这个包里面,所以在用到的时候需要先将它导入
<!-- more -->
**1.sort(byte[] a)**
``` java
	对指定的 byte 型数组按数字升序进行排序。
　　sort(byte[] a, int fromIndex, int toIndex)
　　对指定 byte 型数组的指定范围按数字升序进行排序。
　　sort(char[] a)
　　对指定的 char 型数组按数字升序进行排序。
　　sort(char[] a, int fromIndex, int toIndex)
　　对指定 char 型数组的指定范围按数字升序进行排序。
　　sort(double[] a)
　　对指定的 double 型数组按数字升序进行排序。
　　sort(double[] a, int fromIndex, int toIndex)
　　对指定 double 型数组的指定范围按数字升序进行排序。
　　sort(float[] a)
　　对指定的 float 型数组按数字升序进行排序。
　　sort(float[] a, int fromIndex, int toIndex)
　　对指定 float 型数组的指定范围按数字升序进行排序。
　　sort(int[] a)
　　对指定的 int 型数组按数字升序进行排序。
　　sort(int[] a, int fromIndex, int toIndex)
```
**2.sort(long[] a)**
``` java
	对指定的 long 型数组按数字升序进行排序。
　　sort(long[] a, int fromIndex, int toIndex)
　　对指定 long 型数组的指定范围按数字升序进行排序。
　　sort(Object[] a)
　　根据元素的自然顺序，对指定对象数组按升序进行排序。
　　sort(Object[] a, int fromIndex, int toIndex)
　　根据元素的自然顺序，对指定对象数组的指定范围按升序进行排序。
　　sort(short[] a)
　　对指定的 short 型数组按数字升序进行排序。
　　sort(short[] a, int fromIndex, int toIndex)
　　对指定 short 型数组的指定范围按数字升序进行排序。
　　sort(T[] a, Comparator<? super T> c)       // 见3.从大到小排序
　　根据指定比较器产生的顺序对指定对象数组进行排序。
　　sort(T[] a, int fromIndex, int toIndex, Comparator<? super T> c)
　　根据指定比较器产生的顺序对指定对象数组的指定范围进行排序。
```
**3.从大到小排序**
多了一个Comparator类型的参数而已。
``` java
package test;
import java.util.Arrays;
import java.util.Comparator;

public class Main {
    public static void main(String[] args) {
        //注意，要想改变默认的排列顺序，不能使用基本类型（int,double, char）
        //而要使用它们对应的类
        Integer[] a = {9, 8, 7, 2, 3, 4, 1, 0, 6, 5};
        //定义一个自定义类MyComparator的对象
        Comparator cmp = new MyComparator();
        Arrays.sort(a, cmp);
        for(int i = 0; i < a.length; i ++) {
            System.out.print(a[i] + " ");
        }
    }
}
//Comparator是一个接口，所以这里我们自己定义的类MyComparator要implents该接口
//而不是extends Comparator
class MyComparator implements Comparator<Integer>{
    @Override
    public int compare(Integer o1, Integer o2) {
        //如果n1小于n2，我们就返回正值，如果n1大于n2我们就返回负值，
        //这样颠倒一下，就可以实现反向排序了
        if(o1 < o2) { 
            return 1;
        }else if(o1 > o2) {
            return -1;
        }else {
            return 0;
        }
    }
    
}
```