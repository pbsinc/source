---
title: java中大数的表示BigInteger和BigDecimal
date: 
categories: java总结
tags: [BigInteger , BigDecimal]
---
首先要 import java.math.BigInteger;和import java.math.BigDecimal;
BigInteger和BigDecimal分别表示不可变的任意精度的整数和不可变的有符号的任意精度的十进制数（浮点数）；
java中的大数使高精度运算变得很简单。
<!-- more -->
**1.声明赋值**
BigInteger:
	BigInteger bi = new BigInteger("100");
或: int a = 100; BigInteger bi = BigInteger.valueOf(a);
数组定义与基本类型类似.

BigDecimal:
	BigDecimal bd = new BigDecimal(100);
或: int a = 100; BigDecimal bd = BigDecimal.valueOf(a);

java.util包中的Scanner类实现了nextBigInteger()和nextBigDecimal()方法,可以用来读入控制台输入的BigInteger和BigDecimal.给个例子:
``` java
Scanner sc = new Scanner(System.in);
while(sc.hasNext()){
    BigInteger bi;
    //BigDecimal bd;
    bi = sc.nextBigInteger();//读入BigInteger , 从控制台输入
    // bd = sc.nextBigDecimal();//读入BigDecimal
    System.out.println(bi.toString());
    //System.out.println(bd.toString());
}
```

**2.运算函数**

BigInteger:
``` java
package Factorial;  
  
import java.math.BigInteger;  
import java.util.Random;  
/** 
 * 测试BigInteger类的一些函数 
 * @author LY 2011-10-27 
 * */  
public class BigIntegerDemo {  
    public static void main(String[] arguments){  
        System.out.println("构造两个BigInteger对象: ");  
        //BigInteger(int numBits, Random rnd)   
        //构造一个随机生成的 BigInteger，它是在 0 到 (2^numBits - 1)（包括）范围内均匀分布的值  
        BigInteger bi1 =  new BigInteger(55,new Random());  
        System.out.println("bi1 = " + bi1);  
          
        //BigInteger(byte[] val)   
        //将包含 BigInteger 的二进制补码表示形式的 byte 数组转换为 BigInteger。  
        BigInteger bi2 = new BigInteger(new byte[]{3,2,3});  
        System.out.println("bi2 = " + bi2);  
          
        //加  
        System.out.println("bi1 + bi2 = " + bi1.add(bi2));  
        //减  
        System.out.println("bi1 - bi2 = " + bi1.subtract(bi2));  
        //乘  
        System.out.println("bi1 * bi2 = " + bi1.multiply(bi2));  
        //指数运算  
        System.out.println("bi1的2次方 = " + bi1.pow(2));  
        //整数商  
        System.out.println("bi1/bi2的整数商: " + bi1.divide(bi2));  
        //余数  
        System.out.println("bi1/bi2的余数: " + bi1.remainder(bi2));  
        //整数商+余数  
        System.out.println("bi1 / bi2 = " + bi1.divideAndRemainder(bi2)[0] +   
                "--" + bi1.divideAndRemainder(bi2)[1]);  
        System.out.println("bi1 + bi2 = " + bi1.add(bi2));  
        //比较大小,也可以用max()和min()  
        if(bi1.compareTo(bi2) > 0)  
  
               System.out.println("bd1 is greater than bd2");  
  
           else if(bi1.compareTo(bi2) == 0)  
  
               System.out.println("bd1 is equal to bd2");  
  
           else if(bi1.compareTo(bi2) < 0)  
  
               System.out.println("bd1 is lower than bd2");  
        //返回相反数  
        BigInteger bi3 = bi1.negate();  
        System.out.println("bi1的相反数: " + bi3);  
        //返回绝对值  
        System.out.println("bi1的绝对值:  " + bi3.abs());  
    }  
  
}  
}

运算结果：

构造两个BigInteger对象:   
bi1 = 8893838204110884  
bi2 = 197123  
bi1 + bi2 = 8893838204308007  
bi1 - bi2 = 8893838203913761  
bi1 * bi2 = 1753180068308949786732  
bi1的2次方 = 79100358000902314326836967261456  
bi1/bi2的整数商: 45118216565  
bi1/bi2的余数: 168389  
bi1 / bi2 = 45118216565--168389  
bi1 + bi2 = 8893838204308007  
bd1 is greater than bd2  
bi1的相反数: -8893838204110884  
bi1的绝对值:  8893838204110884  
```

BigDecimal:

``` java
package Factorial;  
  
import java.math.BigDecimal;;  
/** 
 * 测试BigDecimal类的一些函数 
 * @author LY 2011-10-27 
 * */  
public class BigDecimalDemo {  
    public static void main(String[] arguments){  
        System.out.println("构造两个BigDecimal对象: ");  
        //用char[]数组创建BigDecimal对象,第二个参数为位移offset,  
        //第三个参数指定长度  
        BigDecimal bd1 = new BigDecimal("3464656776868432998434".toCharArray(),2,15);  
        System.out.println("bd1 = " + bd1);  
        //用double类型创建BigDecimal对象  
        BigDecimal bd2 = new BigDecimal(134258767575867.0F);  
        System.out.println("bd2 = " + bd2);  
          
        //加  
        System.out.println("bd1 + bd2 = " + bd1.add(bd2));  
        //减  
        System.out.println("bd1 - bd2 = " + bd1.subtract(bd2));  
        //乘  
        System.out.println("bd1 * bd2 = " + bd1.multiply(bd2));  
        //指数运算  
        System.out.println("bd1的2次方 = " + bd1.pow(2));  
        //取商的整数部分  
        System.out.println("bd1/bd2的整数商: " + bd1.divideToIntegralValue(bd2));  
        //返回余数计算为:this.subtract(this.divideToIntegralValue(divisor).multiply(divisor))  
        //System.out.println(bd1.subtract(bd1.divideToIntegralValue(bd2).multiply(bd2)));  
        System.out.println("bd1/bd2的余数: " + bd1.remainder(bd2));  
        //取商和余,即bd1.divideToIntegralValue(bd2)与bd1.remainder(bd2)  
        System.out.println("bd1 / bd2 = " + bd1.divideAndRemainder(bd2)[0] +   
                "--" + bd1.divideAndRemainder(bd2)[1]);  
        //比较大小,也可以用max()和min()  
        if(bd1.compareTo(bd2) > 0)  
  
               System.out.println("bd1 is greater than bd2");  
  
           else if(bd1.compareTo(bd2) == 0)  
  
               System.out.println("bd1 is equal to bd2");  
  
           else if(bd1.compareTo(bd2) < 0)  
  
               System.out.println("bd1 is lower than bd2");  
        //末位数据精度  
        System.out.println("bd1的末位数据精度:  " + bd1.ulp());  
          
    }  
  
}  

运算结果：

构造两个BigDecimal对象:   
bd1 = 646567768684329  
bd2 = 134258765070336  
bd1 + bd2 = 780826533754665  
bd1 - bd2 = 512309003613993  
bd1 * bd2 = 86807390157840676971865964544  
bd1的2次方 = 418049879501431972683650180241  
bd1/bd2的整数商: 4  
bd1/bd2的余数: 109532708402985  
bd1 / bd2 = 4--109532708402985  
bd1 is greater than bd2  
bd1的末位数据精度:  1  
```

**最后，这个BigInteger系列内容分析的比较透彻：**
http://swiftlet.net/archives/tag/java-biginteger