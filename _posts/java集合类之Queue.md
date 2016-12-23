---
title: java集合类之Queue
date: 
categories: java总结
tags: java集合类之Queue
---
Queue是一种很常见的数据结构类型，在Java里面Queue是一个接口，它只是定义了一个基本的Queue应该有哪些功能规约。
实际上有多个Queue的实现，有的是采用线性表实现，有的基于链表实现。还有的适用于多线程的环境。
Java中具有Queue功能的类主要有如下几个：

	AbstractQueue, ArrayBlockingQueue, ConcurrentLinkedQueue, LinkedBlockingQueue, DelayQueue, LinkedList, PriorityBlockingQueue, PriorityQueue和ArrayDqueue。
在本文中，我们主要讨论常用的两种实现：LinkedList和ArrayDeque。
<!-- more -->
**Queue**
     Queue本身是一种先入先出的模型(FIFO)，和我们日常生活中的排队模型很类似。根据不同的实现，他们主要有数组和链表两种实现形式。如下图：
![.](http://dl.iteye.com/upload/attachment/0081/3338/30179b73-4b08-34cb-9caf-02c058bf2b6b.jpg)	 
 因为在队列里和我们日常的模型很近似，每次如果要出队的话，都是从队头移除。而如果每次要加入新的元素，则要在队尾加。所以我们要在队列里保存队头和队尾。
    在jdk里几个常用队列实现之间的类关系图如下：
	![.](http://dl.iteye.com/upload/attachment/0081/3334/0fb2f639-61f2-3ad8-81cc-589e459ad02c.jpg)	 
我们可以看到，Deque也是一个接口，它继承了Queue的接口规范。我们要讨论的LinkedList和ArrayDeque都是实现Deque接口，所以，可以说他们俩都是双向队列。具体的实现我们会在后面讨论。
Queue作为一个接口，它声明的几个基本操作无非就是入队和出队的操作，具体定义如下：	
``` java
public interface Queue<E> extends Collection<E> {  
  
    boolean add(E e); // 添加元素到队列中，相当于进入队尾排队。  
  
    boolean offer(E e);  //添加元素到队列中，相当于进入队尾排队.  
  
    E remove(); //移除队头元素  
  
    E poll();  //移除队头元素  
  
    E element(); //获取但不移除队列头的元素  
  
    E peek();  //获取但不移除队列头的元素  
}   
``` 
 有了这些接口定义的规约，我们就可以很容易的在后续的详细实现里察看具体细节。
**Deque**
    按照我们一般的理解，Deque是一个双向队列，这将意味着它不过是对Queue接口的增强。
如果仔细分析Deque接口代码的话，我们会发现它里面主要包含有4个部分的功能定义。

	1. 双向队列特定方法定义。
	2. Queue方法定义。
	3. Stack方法定义。
	4. Collection方法定义。
第3，4部分的方法相当于告诉我们，具体实现Deque的类我们也可以把他们当成Stack和普通的Collection来使用。这也是接口定义规约带来的好处。这里我们就不再赘述。
    我们重点来对Queue相关的定义方法做一下概括：
**add相关**的方法有如下几个：
``` java
	boolean add(E e);  
	  
	boolean offer(E e);  
	  
	void addFirst(E e);  
	  
	void addLast(E e);  
	  
	boolean offerFirst(E e);  
	  
	boolean offerLast(E e);  
``` 	
    这里定义了add, offer两个方法，从doc说明上来看，两者的基本上没什么区别。
之所以定义了这两个方法是因为Deque继承了Collection, Queue两个接口，而这两个接口中都定义了增加元素的方法声明。
他们本身的目的是一样的，只是在队列里头，添加元素肯定只是限于在队列的头或者尾添加。
而offer作为一个更加适用于队列场景中的方法，也有存在的意义。
他们的实现基本上一样，只是名字不同罢了。
**remove相关**的方法：
``` java
	E removeFirst();  
	  
	E removeLast();  
	  
	E pollFirst();  
	  
	E pollLast();  
	  
	E remove();  
	  
	E poll();  
``` 
    这里remove相关的方法poll和remove也很类似，他们存在的原因也和前面一样。
**get元素相关**的方法：
``` java
E getFirst();  
  
E getLast();  
  
E peekFirst();  
  
E peekLast();  
  
E element();  
  
E peek();  
``` 
    peek和element方法和前面提到的差别有点不一样，element方法是在队列为空的时候抛异常，而peek则是返回null。
    ok，有了前面这些对方法操作的分门别类，我们后面分析起具体实现就更方便了。
**ArrayDeque**
    有了我们前面几篇分析的基础，我们可以很容易猜到ArrayDeque的内部实现机制。
    它的内部使用一个数组来保存具体的元素，然后分别使用head, tail来指示队列的头和尾。
    他们的定义如下：
``` java
private transient E[] elements;  
  
private transient int head;  
  
private transient int tail;  
  
private static final int MIN_INITIAL_CAPACITY = 8;  
``` 
     ArrayDeque的默认长度为8，这么定义成2的指数值也是有一定好处的。在后面调整数组长度的时候我们会看到。关于tail需要注意的一点是tail所在的索引位置是null值，在它前面的元素才是队列中排在最后的元素。
调整元素长度
    在调整元素长度部分，ArrayDeque采用了两个方法来分配。一个是allocateElements，还有一个是doubleCapacity。allocateElements方法用于构造函数中根据指定的参数设置初始数组的大小。而doubleCapacity则用于当数组元素不够用了扩展数组长度。
下面是allocateElements方法的实现：
``` java
private void allocateElements(int numElements) {  
    int initialCapacity = MIN_INITIAL_CAPACITY;  
    // Find the best power of two to hold elements.  
    // Tests "<=" because arrays aren't kept full.  
    if (numElements >= initialCapacity) {  
        initialCapacity = numElements;  
        initialCapacity |= (initialCapacity >>>  1);  
        initialCapacity |= (initialCapacity >>>  2);  
        initialCapacity |= (initialCapacity >>>  4);  
        initialCapacity |= (initialCapacity >>>  8);  
        initialCapacity |= (initialCapacity >>> 16);  
        initialCapacity++;  
  
        if (initialCapacity < 0)   // Too many elements, must back off  
            initialCapacity >>>= 1;// Good luck allocating 2 ^ 30 elements  
    }  
    elements = (E[]) new Object[initialCapacity];  
}  
``` 
    这部分代码里最让人困惑的地方就是对initialCapacity做的这一大堆移位和或运算。
首先通过无符号右移1位，再与原来的数字做或运算，然后再右移2、4、8、16位。这么做的目的是使得最后生成的数字尽可能每一位都是1。
而且很显然，如果这个数字是每一位都为1，后面再对这个数字加1的话，则生成的数字肯定为2的若干次方。
**而且这个数字也肯定是大于我们的numElements值的最小2的指数值。**这么说来有点绕。
我们前面折腾了大半天，就为了求一个2的若干次方的数字，使得它要大于我们指定的数字，而且是最接近这个数字的数。
这样子到底是为什么呢？
因为我们后面要扩展数组长度的话，有了它这个基础我们就可以判断这个数字是不是到了2的多少多少次方，它增长下去最大的极限也不过是2的31次方。这样他每次的增长刚好可以把数组可以允许的长度给覆盖了，不会出现空间的浪费。
比如说，我正好有一个数组，它的长度比Integer.MAX_VALUE的一半要大几个元素，如果我们这个时候设置的值不是让它为2的整数次方，那么直接对它空间翻倍就导致空间不够了，但是我们完全可以设置足够空间来容纳的。
    我们现在再来看doubleCapacity方法：
``` java
private void doubleCapacity() {  
    assert head == tail;  
    int p = head;  
    int n = elements.length;  
    int r = n - p; // number of elements to the right of p  
    int newCapacity = n << 1;  
    if (newCapacity < 0)  
        throw new IllegalStateException("Sorry, deque too big");  
    Object[] a = new Object[newCapacity];  
    System.arraycopy(elements, p, a, 0, r);  
    System.arraycopy(elements, 0, a, r, p);  
    elements = (E[])a;  
    head = 0;  
    tail = n;  
}  
``` 
    有了前面的讨论，它只要扩展空间容量的时候左移一位，这就相当于空间翻倍了。如果长度超出了允许的范围，就会发生溢出，返回的结果就会成为一个负数。这就是为什么有 if (newCapacity < 0)这一句来抛异常。
**添加元素**
    我们先看看两个主要添加元素的方法add和offer:
 
``` java
public boolean add(E e) {  
    addLast(e);  
    return true;  
}  
  
public void addLast(E e) {  
    if (e == null)  
        throw new NullPointerException();  
    elements[tail] = e;  
    if ( (tail = (tail + 1) & (elements.length - 1)) == head)  
        doubleCapacity();  
}  
  
public boolean offer(E e) {  
    return offerLast(e);  
}  
  
public boolean offerLast(E e) {  
    addLast(e);  
    return true;  
}  
``` 
    很显然，他们两个方法的底层实现实际上是一样的。这里要注意的一个地方就是我们由于不断的入队和出队，可能head和tail都会移动到超过数组的末尾。
	这个时候如果有空闲的空间，我们会把头或者尾跳到数组的头开始继续移动。所以添加元素并确定元素的下标是一个将元素下标值和数组长度进行求模运算的过程。
	addLast方法通过和当前数组长度减1求与运算来得到最新的下标值。它的效果相当于tail = (tail + 1) % elements.length;
 
``` java
public void addFirst(E e) {  
    if (e == null)  
        throw new NullPointerException();  
    elements[head = (head - 1) & (elements.length - 1)] = e;  
    if (head == tail)  
        doubleCapacity();  
}  
  
public boolean offerFirst(E e) {  
    addFirst(e);  
    return true;  
}  
``` 
    addFirst和offerFirst是在head元素的之前插入元素，所以他们的位置为 (head - 1) & (elements.length - 1)。
**取元素**
     获取元素主要包括如下几个方法：
``` java
public E element() {  
    return getFirst();  
}  
  
public E getFirst() {  
    E x = elements[head];  
    if (x == null)  
        throw new NoSuchElementException();  
    return x;  
}  
  
public E peek() {  
    return peekFirst();  
}  
  
public E peekFirst() {  
    return elements[head]; // elements[head] is null if deque empty  
}  
  
public E getLast() {  
    E x = elements[(tail - 1) & (elements.length - 1)];  
    if (x == null)  
        throw new NoSuchElementException();  
    return x;  
}  
  
public E peekLast() {  
    return elements[(tail - 1) & (elements.length - 1)];  
}  
``` 
    这部分代码算是最简单的，无非就是取tail元素或者head元素的值。
    关于队列的几种运算方法定义的特别杂乱，很容易让人搞混。如果从一个最简单的单向队列角度来看的话，我们可以把Queue中的enqueue方法对应到addLast方法，因为我们每次添加元素就是在队尾增加。deque方法则对应到removeFirst方法。虽然也可以用其他的方法来实现，不过具体的实现细节和他们基本上是一样的。
LinkedList
    现在，我们在来看看**LinkedList**对应Queue的实现部分。在前面一篇文章中，已经讨论过LinkedList里面Node的结构。它本身包含元素值，prev、next两个引用。对链表的增加和删除元素的操作不像数组，不存在要考虑下标的问题，也不需要扩展数组空间，因此就简单了很多。先看查找元素部分：
``` java
public E getFirst() {  
    final Node<E> f = first;  
    if (f == null)  
        throw new NoSuchElementException();  
    return f.item;  
}  
  
public E getLast() {  
    final Node<E> l = last;  
    if (l == null)  
        throw new NoSuchElementException();  
    return l.item;  
}  
``` 
    这里唯一值得注意的一点就是last引用是指向队列最末尾和元素，和前面ArrayDeque的情况不一样。
    **添加元素**的方法如下：
``` java
public boolean add(E e) {  
    linkLast(e);  
    return true;  
}  
  
public boolean offer(E e) {  
    return add(e);  
}  
  
void linkLast(E e) {  
    final Node<E> l = last;  
    final Node<E> newNode = new Node<>(l, e, null);  
    last = newNode;  
    if (l == null)  
        first = newNode;  
    else  
        l.next = newNode;  
    size++;  
    modCount++;  
}  
``` 
    linkLast的方法在前一篇文章里已经分析过，就不再重复。
    **删除元素**的方法主要有remove():
``` java
public boolean remove(Object o) {  
    if (o == null) {  
        for (Node<E> x = first; x != null; x = x.next) {  
            if (x.item == null) {  
                unlink(x);  
                return true;  
            }  
        }  
    } else {  
        for (Node<E> x = first; x != null; x = x.next) {  
            if (o.equals(x.item)) {  
                unlink(x);  
                return true;  
            }  
        }  
    }  
    return false;  
}  
  
E unlink(Node<E> x) {  
    // assert x != null;  
    final E element = x.item;  
    final Node<E> next = x.next;  
    final Node<E> prev = x.prev;  
  
    if (prev == null) {  
        first = next;  
    } else {  
        prev.next = next;  
        x.prev = null;  
    }  
  
    if (next == null) {  
        last = prev;  
    } else {  
        next.prev = prev;  
        x.next = null;  
    }  
  
    x.item = null;  
    size--;  
    modCount++;  
    return element;  
}  
``` 
    这部分的代码看似比较长，实际上是遍历整个链表，如果找到要删除的元素，则移除该元素。这部分的难点在unlink方法里面。我们分别用要删除元素的前面和后面的引用来判断各种当prev和next为null时的各种情况。虽然不是很复杂，但是很繁琐。
总结
    从我们实际中的考量来看，Queue和Deque他们本身不仅定义了作为一个队列需要的基本功能。同时因为队列也是属于整个集合类这一个大族里面的，所以他们也必须要具备集合类的一些常用功能，比如元素查找，删除，迭代器等。我们读一些集合类的代码时，尤其是一些接口的定义，会发现一个比较有意思的事情。就是通常一些子接口把父接口的方法又重新定义了一遍。这样似乎违背了面向对象里继承的原则。后来经过一些讨论，发现主要原因是一些jdk版本的更新，有的新类是后面新增加的。这些新的接口有的是为了保持兼容，有的是为了保证后续生成文档里方便用户知道它也有同样的功能而不需要再去查它的父类，就直接把父类的东西给搬过来了。比较有意思，读代码还读出点历史感了。
参考资料
http://docs.oracle.com/javase/7/docs/api/java/util/Queue.html
http://docs.oracle.com/javase/7/docs/api/java/util/Deque.html
http://blog.donews.com/maverick/archive/2005/08/31/534269.aspx

原文地址：http://shmilyaw-hotmail-com.iteye.com/blog/1700599




























1、获取链表的第一个和最后一个元素
``` java
import java.util.LinkedList;  
  
public class LinkedListTest{  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
  
  
    System.out.println("链表的第一个元素是 : " + lList.getFirst());  
    System.out.println("链表最后一个元素是 : " + lList.getLast());  
  }  
}  
``` 
2、获取链表元素  
``` java
for (String str: lList) {  
      System.out.println(str);  
    }  
``` 
3、从链表生成子表
``` java
List subl = lList.subList(1, 4);  
    System.out.println(subl);  
    lst.remove(2);  
    System.out.println(lst);  
    System.out.println(lList);  
``` 
4、添加元素：添加单个元素
 如果不指定索引的话，元素将被添加到链表的最后.
public boolean add(Object element)
public boolean add(int index, Object element)
也可以把链表当初栈或者队列来处理:
public boolean addFirst(Object element)
public boolean addLast(Object element)
addLast()方法和不带索引的add()方法实现的效果一样.
``` java
import java.util.LinkedList;  
  
public class LinkedListTest{  
  public static void main(String[] a) {  
    LinkedList list = new LinkedList();  
    list.add("A");  
    list.add("B");  
    list.add("C");  
    list.add("D");  
    list.addFirst("X");  
    list.addLast("Z");  
    System.out.println(list);  
  }  
}  
``` 
5、删除元素
``` java
public Object removeFirst()  
public Object removeLast()  
import java.util.LinkedList;  
  
  
public class MainClass {  
  public static void main(String[] a) {  
  
  
    LinkedList list = new LinkedList();  
    list.add("A");  
    list.add("B");  
    list.add("C");  
    list.add("D");  
    list.removeFirst();  
    list.removeLast();  
    System.out.println(list);  
  }  
} 
```  
6、使用链表实现栈效果
``` java
import java.util.LinkedList;  
public class MainClass {  
  public static void main(String[] args) {  
    StackL stack = new StackL();  
    for (int i = 0; i < 10; i++)  
      stack.push(i);  
    System.out.println(stack.top());  
    System.out.println(stack.top());  
    System.out.println(stack.pop());  
    System.out.println(stack.pop());  
    System.out.println(stack.pop());  
  }  
}  
class StackL {  
  private LinkedList list = new LinkedList();  
  public void push(Object v) {  
    list.addFirst(v);  
  }  
  public Object top() {  
    return list.getFirst();  
  }  
  public Object pop() {  
    return list.removeFirst();  
  }  
}  
``` 
7、使用链表来实现队列效果
``` java
import java.util.LinkedList;  
public class MainClass {  
  public static void main(String[] args) {  
    Queue queue = new Queue();  
    for (int i = 0; i < 10; i++)  
      queue.put(Integer.toString(i));  
    while (!queue.isEmpty())  
      System.out.println(queue.get());  
  }  
}  
class Queue {  
  private LinkedList list = new LinkedList();  
  public void put(Object v) {  
    list.addFirst(v);  
  }  
  public Object get() {  
    return list.removeLast();  
  }  
  public boolean isEmpty() {  
    return list.isEmpty();  
  }  
}  
``` 
8、将LinkedList转换成ArrayList

``` java
ArrayList<String> arrayList = new ArrayList<String>(linkedList);  
    for (String s : arrayList) {  
      System.out.println("s = " + s);  
    }  
	``` 
9、删掉所有元素：清空LinkedList

    lList.clear();
10、删除列表的首位元素
``` java
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    System.out.println(lList);  
        //元素在删除的时候，仍然可以获取到元素  
    Object object = lList.removeFirst();  
    System.out.println(object + " has been removed");  
    System.out.println(lList);  
    object = lList.removeLast();  
    System.out.println(object + " has been removed");  
    System.out.println(lList);  
  }  
}  
``` 
11、根据范围删除列表元素
``` java
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    System.out.println(lList);  
    lList.subList(2, 5).clear();  
    System.out.println(lList);  
  }  
}  
``` 
12、删除链表的特定元素
``` java
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    System.out.println(lList);  
    System.out.println(lList.remove("2"));//删除元素值=2的元素  
    System.out.println(lList);  
    Object obj = lList.remove(2);  //删除第二个元素  
    System.out.println(obj + " 已经从链表删除");  
    System.out.println(lList);  
  }  
}  
``` 
13、将LinkedList转换为数组，数组长度为0
``` java
import java.util.LinkedList;  
import java.util.List;  
public class Main {  
  public static void main(String[] args) {  
    List<String> theList = new LinkedList<String>();  
    theList.add("A");  
    theList.add("B");  
    theList.add("C");  
    theList.add("D");  
    String[] my = theList.toArray(new String[0]);  
    for (int i = 0; i < my.length; i++) {  
      System.out.println(my[i]);  
    }  
  }  
}  
``` 
14、将LinkedList转换为数组，数组长度为链表长度
``` java
import java.util.LinkedList;  
import java.util.List;  
public class Main {  
  public static void main(String[] args) {  
    List<String> theList = new LinkedList<String>();  
    theList.add("A");  
    theList.add("B");  
    theList.add("C");  
    theList.add("D");  
    String[] my = theList.toArray(new String[theList.size()]);  
    for (int i = 0; i < my.length; i++) {  
      System.out.println(my[i]);  
    }  
  }  
}  
``` 
15、将LinkedList转换成ArrayList
``` java
import java.util.ArrayList;  
import java.util.LinkedList;  
import java.util.List;  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> myQueue = new LinkedList<String>();  
    myQueue.add("A");  
    myQueue.add("B");  
    myQueue.add("C");  
    myQueue.add("D");  
    List<String> myList = new ArrayList<String>(myQueue);  
    for (Object theFruit : myList)  
      System.out.println(theFruit);  
  }  
}  
``` 
16、实现栈
``` java
import java.util.Collections;  
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] argv) throws Exception {  
    LinkedList stack = new LinkedList();  
    Object object = "";  
    stack.addFirst(object);  
    Object o = stack.getFirst();  
    stack = (LinkedList) Collections.synchronizedList(stack);  
  }  
}  
``` 
17、实现队列
``` java
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] argv) throws Exception {  
    LinkedList queue = new LinkedList();  
    Object object = "";  
    // Add to end of queue  
    queue.add(object);  
    // Get head of queue  
    Object o = queue.removeFirst();  
  }  
}  
``` 
18 、同步方法
``` java
import java.util.Collections;  
import java.util.LinkedList;  
public class Main {  
  public static void main(String[] argv) throws Exception {  
    LinkedList queue = new LinkedList();  
    Object object = "";  
    queue.add(object);  
    Object o = queue.removeFirst();  
    queue = (LinkedList) Collections.synchronizedList(queue);  
  }  
}  
``` 
19、查找元素位置
``` java
import java.util.LinkedList;  
  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    lList.add("2");  
    System.out.println(lList.indexOf("2"));  
    System.out.println(lList.lastIndexOf("2"));  
  }  
}  
``` 
20、替换元素
``` java
import java.util.LinkedList;  
  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    System.out.println(lList);  
    lList.set(3, "Replaced");//使用set方法替换元素，方法的第一个参数是元素索引，后一个是替换值  
    System.out.println(lList);  
  }  
}  
``` 
21、链表添加对象
``` java
import java.util.LinkedList;  
class Address {  
  private String name;  
  private String street;  
  private String city;  
  private String state;  
  private String code;  
  Address(String n, String s, String c, String st, String cd) {  
    name = n;  
    street = s;  
    city = c;  
    state = st;  
    code = cd;  
  }  
  public String toString() {  
    return name + " " + street + " " + city + " " + state + " " + code;  
  }  
}  
  
  
class MailList {  
  public static void main(String args[]) {  
    LinkedList<Address> ml = new LinkedList<Address>();  
    ml.add(new Address("A", "11 Ave", "U", "IL", "11111"));  
    ml.add(new Address("R", "11 Lane", "M", "IL", "22222"));  
    ml.add(new Address("T", "8 St", "C", "IL", "33333"));  
    for (Address element : ml)  
      System.out.println(element + "\n");  
  }  
}  
``` 
22、确认链表是否存在特定元素
``` java
import java.util.LinkedList;  
  
  
public class Main {  
  public static void main(String[] args) {  
    LinkedList<String> lList = new LinkedList<String>();  
    lList.add("1");  
    lList.add("2");  
    lList.add("3");  
    lList.add("4");  
    lList.add("5");  
    if (lList.contains("4")) {  
      System.out.println("LinkedList contains 4");  
    } else {  
      System.out.println("LinkedList does not contain 4");  
    }  
  }  
}  
``` 
23、根据链表元素生成对象数组
``` java
Object[] objArray = lList.toArray();  
for (Object obj: objArray) {  
   System.out.println(obj);  
}  
``` 
24、链表多线程
``` java
import java.util.Collections;  
import java.util.LinkedList;  
import java.util.List;  
class PrepareProduction implements Runnable {  
  private final List<String> queue;  
  PrepareProduction(List<String> q) {  
    queue = q;  
  }  
  public void run() {  
    queue.add("1");  
    queue.add("done");  
  }  
}  
class DoProduction implements Runnable {  
  private final List<String> queue;  
  DoProduction(List<String> q) {  
    queue = q;  
  }  
  public void run() {  
    String value = queue.remove(0);  
    while (!value.equals("*")) {  
      System.out.println(value);  
      value = queue.remove(0);  
    }  
  }  
}  
public class Main {  
  public static void main(String[] args) throws Exception {  
    List q = Collections.synchronizedList(new LinkedList<String>());  
    Thread p1 = new Thread(new PrepareProduction(q));  
    Thread c1 = new Thread(new DoProduction(q));  
    p1.start();  
    c1.start();  
    p1.join();  
    c1.join();  
  }  
}  
``` 
25、优先级链表（来自JBOSS）
``` java
import java.util.ArrayList;  
import java.util.LinkedList;  
import java.util.List;  
import java.util.ListIterator;  
import java.util.NoSuchElementException;  
  
  
public class BasicPriorityLinkedList {  
  
  
  protected LinkedList[] linkedLists;  
  protected int priorities;  
  protected int size;  
  
  
  public BasicPriorityLinkedList(int priorities) {  
    this.priorities = priorities;  
    initDeques();  
  }  
  public void addFirst(Object obj, int priority) {  
    linkedLists[priority].addFirst(obj);  
    size++;  
  }  
  public void addLast(Object obj, int priority) {  
    linkedLists[priority].addLast(obj);  
    size++;  
  }  
  public Object removeFirst() {  
    Object obj = null;  
    for (int i = priorities - 1; i >= 0; i--) {  
      LinkedList ll = linkedLists[i];  
      if (!ll.isEmpty()) {  
        obj = ll.removeFirst();  
        break;  
      }  
    }  
    if (obj != null) {  
      size--;  
    }  
    return obj;  
  }  
  public Object removeLast() {  
    Object obj = null;  
    for (int i = 0; i < priorities; i++) {  
      LinkedList ll = linkedLists[i];  
      if (!ll.isEmpty()) {  
        obj = ll.removeLast();  
      }  
      if (obj != null) {  
        break;  
      }  
    }  
    if (obj != null) {  
      size--;  
    }  
    return obj;  
  }  
  
  
  public Object peekFirst() {  
    Object obj = null;  
    for (int i = priorities - 1; i >= 0; i--) {  
      LinkedList ll = linkedLists[i];  
      if (!ll.isEmpty()) {  
        obj = ll.getFirst();  
      }  
      if (obj != null) {  
        break;  
      }  
    }  
    return obj;  
  }  
  
  
  public List getAll() {  
    List all = new ArrayList();  
    for (int i = priorities - 1; i >= 0; i--) {  
      LinkedList deque = linkedLists[i];  
      all.addAll(deque);  
    }  
    return all;  
  }  
  
  
  public void clear() {  
    initDeques();  
  }  
  
  
  public int size() {  
    return size;  
  }  
  
  
  public boolean isEmpty() {  
    return size == 0;  
  }  
  
  
  public ListIterator iterator() {  
    return new PriorityLinkedListIterator(linkedLists);  
  }  
  
  
  protected void initDeques() {  
    linkedLists = new LinkedList[priorities];  
    for (int i = 0; i < priorities; i++) {  
      linkedLists[i] = new LinkedList();  
    }  
    size = 0;  
  }  
  
  
  class PriorityLinkedListIterator implements ListIterator {  
    private LinkedList[] lists;  
    private int index;  
    private ListIterator currentIter;  
    PriorityLinkedListIterator(LinkedList[] lists) {  
      this.lists = lists;  
      index = lists.length - 1;  
      currentIter = lists[index].listIterator();  
    }  
  
  
    public void add(Object arg0) {  
      throw new UnsupportedOperationException();  
    }  
  
  
    public boolean hasNext() {  
      if (currentIter.hasNext()) {  
        return true;  
      }  
      while (index >= 0) {  
        if (index == 0 || currentIter.hasNext()) {  
          break;  
        }  
        index--;  
        currentIter = lists[index].listIterator();  
      }  
      return currentIter.hasNext();  
    }  
  
  
    public boolean hasPrevious() {  
      throw new UnsupportedOperationException();  
    }  
  
  
    public Object next() {  
      if (!hasNext()) {  
        throw new NoSuchElementException();  
      }  
      return currentIter.next();  
    }  
  
  
    public int nextIndex() {  
      throw new UnsupportedOperationException();  
    }  
  
  
    public Object previous() {  
      throw new UnsupportedOperationException();  
    }  
  
  
    public int previousIndex() {  
      throw new UnsupportedOperationException();  
    }  
  
  
    public void remove() {  
      currentIter.remove();  
      size--;  
    }  
  
  
    public void set(Object obj) {  
      throw new UnsupportedOperationException();  
    }  
  }  
}  
``` 
26、生成list的帮助类（来自google）
``` java
import java.util.ArrayList;  
import java.util.Collections;  
import java.util.LinkedList;  
import java.util.List;  
public class Lists {  
  private Lists() { }  
  public static <E> ArrayList<E> newArrayList() {  
    return new ArrayList<E>();  
  }  
  public static <E> ArrayList<E> newArrayListWithCapacity(int initialCapacity) {  
    return new ArrayList<E>(initialCapacity);  
  }  
  public static <E> ArrayList<E> newArrayList(E... elements) {  
    ArrayList<E> set = newArrayList();  
    Collections.addAll(set, elements);  
    return set;  
  }  
  public static <E> ArrayList<E> newArrayList(Iterable<? extends E> elements) {  
    ArrayList<E> list = newArrayList();  
    for(E e : elements) {  
      list.add(e);  
    }  
    return list;  
  }  
  public static <E> LinkedList<E> newLinkedList() {  
    return new LinkedList<E>();  
  }  
}  
```    