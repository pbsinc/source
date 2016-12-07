---
title: 集合Collections工具类用法
date: 
categories: java总结
tags: Collections工具类
---
Collections工具类提供了大量针对Collection/Map的操作，总体可分为四类，都为静态（static）方法：
<!-- more -->
**1. 排序操作（主要针对List接口相关）**
reverse(List list)：反转指定List集合中元素的顺序
shuffle(List list)：对List中的元素进行随机排序（洗牌）
sort(List list)：对List里的元素根据自然升序排序
sort(List list, Comparator c)：自定义比较器进行排序
swap(List list, int i, int j)：将指定List集合中i处元素和j出元素进行交换
rotate(List list, int distance)：将所有元素向右移位指定长度，如果distance等于size那么结果不变
``` java

        原始顺序：[b张三, d孙六, a李四, e钱七, c赵五]
        
        Collections.reverse(list);
        reverse后顺序：[c赵五, e钱七, a李四, d孙六, b张三]

        Collections.shuffle(list);
        shuffle后顺序：[b张三, c赵五, d孙六, e钱七, a李四]
        
        Collections.swap(list, 1, 3);
        swap后顺序：[b张三, e钱七, d孙六, c赵五, a李四]

        Collections.sort(list);
        sort后顺序：[a李四, b张三, c赵五, d孙六, e钱七]

        Collections.rotate(list, 1);
        rotate后顺序：[e钱七, a李四, b张三, c赵五, d孙六]
    
```
**2. 查找和替换（主要针对Collection接口相关）**
binarySearch(List list, Object key)：使用二分搜索法，以获得指定对象在List中的索引，前提是集合已经排序
max(Collection coll)：返回最大元素
max(Collection coll, Comparator comp)：根据自定义比较器，返回最大元素
min(Collection coll)：返回最小元素
min(Collection coll, Comparator comp)：根据自定义比较器，返回最小元素
fill(List list, Object obj)：使用指定对象填充
frequency(Collection Object o)：返回指定集合中指定对象出现的次数
replaceAll(List list, Object old, Object new)：替换
``` java
public void testSearch() {
        System.out.println("给定的list：" + list);
        System.out.println("max：" + Collections.max(list));
        System.out.println("min：" + Collections.min(list));
        System.out.println("frequency：" + Collections.frequency(list, "a李四"));
        Collections.replaceAll(list, "a李四", "aa李四");
        System.out.println("replaceAll之后：" + list);
        
        // 如果binarySearch的对象没有排序的话，搜索结果是不确定的
        System.out.println("binarySearch在sort之前：" + Collections.binarySearch(list, "c赵五"));
        Collections.sort(list);
        // sort之后，结果出来了
        System.out.println("binarySearch在sort之后：" + Collections.binarySearch(list, "c赵五"));

        Collections.fill(list, "A");
        System.out.println("fill：" + list);
    }
	
	输出

给定的list：[b张三, d孙六, a李四, e钱七, c赵五]
max：e钱七
min：a李四
frequency：1
replaceAll之后：[b张三, d孙六, aa李四, e钱七, c赵五]
binarySearch在sort之前：-4
binarySearch在sort之后：2
fill：[A, A, A, A, A]
```
**3. 同步控制**
Collections工具类中提供了多个synchronizedXxx方法，该方法返回指定集合对象对应的同步对象，从而解决多线程并发访问集合时线程的安全问题。HashSet、ArrayList、HashMap都是线程不安全的，如果需要考虑同步，则使用这些方法。这些方法主要有：synchronizedSet、synchronizedSortedSet、synchronizedList、synchronizedMap、synchronizedSortedMap。
特别需要指出的是，在使用迭代方法遍历集合时需要手工同步返回的集合。
``` java
Map m = Collections.synchronizedMap(new HashMap());
      ...
  Set s = m.keySet();  // Needn't be in synchronized block
      ...
  synchronized (m) {  // Synchronizing on m, not s!
      Iterator i = s.iterator(); // Must be in synchronized block
      while (i.hasNext())
          foo(i.next());
  }
```
**4. 设置不可变集合**
Collections有三类方法可返回一个不可变集合：
emptyXxx()：返回一个空的不可变的集合对象
singletonXxx()：返回一个只包含指定对象的，不可变的集合对象。
unmodifiableXxx()：返回指定集合对象的不可变视图
``` java
public void testUnmodifiable() {
        System.out.println("给定的list：" + list);
        List<String> unmodList = Collections.unmodifiableList(list);
        
        unmodList.add("再加个试试！"); // 抛出：java.lang.UnsupportedOperationException
        
        // 这一行不会执行了
        System.out.println("新的unmodList：" + unmodList);
    }
```
**5. 其它**
disjoint(Collection<?> c1, Collection<?> c2) - 如果两个指定 collection 中没有相同的元素，则返回 true。
addAll(Collection<? super T> c, T... a) - 一种方便的方式，将所有指定元素添加到指定 collection 中。示范： 
Collections.addAll(flavors, "Peaches 'n Plutonium", "Rocky Racoon");
Comparator<T> reverseOrder(Comparator<T> cmp) - 返回一个比较器，它强行反转指定比较器的顺序。如果指定比较器为 null，则此方法等同于 reverseOrder()（换句话说，它返回一个比较器，该比较器将强行反转实现 Comparable 接口那些对象 collection 上的自然顺序）。
``` java
public void testOther() {
        List<String> list1 = new ArrayList<String>();
        List<String> list2 = new ArrayList<String>();
        
        // addAll增加变长参数
        Collections.addAll(list1, "大家好", "你好","我也好");
        Collections.addAll(list2, "大家好", "a李四","我也好");
        
        // disjoint检查两个Collection是否的交集
        boolean b1 = Collections.disjoint(list, list1);
        boolean b2 = Collections.disjoint(list, list2);
        System.out.println(b1 + "\t" + b2);
        
        // 利用reverseOrder倒序
        Collections.sort(list1, Collections.reverseOrder());
        System.out.println(list1);
    }
	
	输出

true false
[我也好, 大家好, 你好]
```