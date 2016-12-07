---
title: Set、List、Map的遍历
date: 
categories: java总结
tags: Set、List、Map的遍历
---
本文实例讲述了Java集合Set、List、Map的遍历方法，分享给大家供大家参考。具体方法如下:
<!-- more -->

``` java
public class TestCollection {
  
  public static void print(Collection<? extends Object> c){
    Iterator<? extends Object> it = c.iterator();
    while (it.hasNext()) {
      Object object = (Object) it.next();
      System.out.println(object);
    }
  }
  
  @Test
  
/*-----------------------Set----------------------*/   
 
  public void demo1(){
    Set<String> set = new HashSet<String>();
    set.add("AAA");
    set.add("BBB");
    set.add("CCC");
    print(set);
	
    //Set的第一种遍历方式:利用Iterator
    Iterator<String> it1 = set.iterator();
    for (String ss : set) {
      System.out.println(ss);
    }
	
    //Set的第一种遍历方式:利用foreach
    for (String sss : set) {
      System.out.println(sss);
    }
    
/*-----------------------List----------------------*/  
	
    List<String> list = new ArrayList<String>();
    list.add("DDDDD");
    list.add("EEEEE");
    list.add("FFFFF");
    print(list);
    
    //List的第一种遍历方式:因为list有顺序，利用size()和get()方法获取
    for (int i = 0; i < list.size(); i++) {
      System.out.println(list.get(i));
    }
	
    //List的第二种遍历方式：利用Iterator
    Iterator<String> it = list.iterator();
    while (it.hasNext()) {
      System.out.println(it.next());
    }
	
    //List的第三种遍历方式:利用foreach
    for (String s2 : list) {
      System.out.println(s2);
    }
 
/*-----------------------Map----------------------*/  
 
    Map<String,String> map = new TreeMap<String, String>();
    map.put("Jerry", "10000");
    map.put("shellway", "20000");
    map.put("Kizi", "30000");
    print(map.entrySet());
	
    //Map的第一种遍历方式：在for-each循环中遍历keys或values,如果只需要map中的键或者值，你可以通过keySet或values来实现遍历，而不是用entrySet。
    //遍历map中的键 
	for (Integer key : map.keySet()) { 
		System.out.println("Key = " + key); 
	} 
	//遍历map中的值 
	for (Integer value : map.values()) { 
		System.out.println("Value = " + value); 
	} 
	//该方法比entrySet遍历在性能上稍好（快了10%），而且代码更加干净。
	
    //Map的第二种遍历方式：获得键值对
    for (Map.Entry<String, String> entry : map.entrySet()) {
      System.out.println(entry.getKey()+" : "+entry.getValue());
    }
  }
}

```
这里使用泛型对集合对象进行类型安全检查和遍历。