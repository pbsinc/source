---
title: long与Long的区别，long型最后有无L的区别
date: 
categories: java总结
tags: [long与Long , long加不加L的区别]
---
1. long 与 Long 的区别
2. long 9876543211 与 long 9876543211L ，long加不加L的区别
<!-- more -->
**1. long 与 Long 的区别:**
1.long是基础数据类型，Long是long的封装类型，也叫包装类；
2.什么叫包装类？在java中有时候的运算必须是两个类对象之间进行的，不充许对象与数字之间进行运算。所以需要有一个对象，这个对象把数字进行了一下包装，这样这个对象就可以和另一个对象进行运算了。
3.long的默认值是0；Long默认值是null；
4.基本类型：long,int,byte,float,double,char,short,boolean
5.基础数据类型、封装类型及默认值
``` java
    long(0)               Long(null)
    int(0)                  Integer(null)
    byte(0)               Byte(null)
    float(0.0)            Float(null)
    double(0.0)        Double(null)
    short(0)              Short(null)
    char(0)                Character(null)
    boolean(false)    Boolean(null)
```	
到底是选择Long 还是long这个还得看具体环境，如果你认为这个属性不能为null,那么就用long，因为它默认初值为0，如果这个字段可以为null，那么就应该选择Long。

**2. long加不加L的区别:**


不加L默认是int，int转为long是安全的，所以会自动转，能编译通过

 long 9876543211  报错，因为超过int 的范围
 long 9876543211L 正确写法

浮点数不加F默认是double类型，double转float可能损失精度，因为不会自动转，编译是通不过的
