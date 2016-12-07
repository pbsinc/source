---
title: java堆栈Stack类简介
date: 
categories: java总结
tags: java堆栈Stack类简介
---
Stack是一个后进先出（last in first out，LIFO）的堆栈，在Vector类的基础上扩展5个方法而来
Deque（双端队列）比起Stack具有更好的完整性和一致性，应该被优先使用
！[](http://img.blog.csdn.net/20130722161253281?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvYTE5ODgxMDI5/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
<!-- more -->

``` java
E push(E item)   
         把项压入堆栈顶部。   
E pop()   
         移除堆栈顶部的对象，并作为此函数的值返回该对象。   
E peek()   
         查看堆栈顶部的对象，但不从堆栈中移除它。   
boolean empty()   
         测试堆栈是否为空。    
int search(Object o)   
         返回对象在堆栈中的位置，以 1 为基数。  
```
Stack本身通过扩展Vector而来，而Vector本身是一个可增长的对象数组（ a growable array of objects）那么这个数组的哪里作为Stack的栈顶，哪里作为Stack的栈底？

答案只能从源代码中寻找，jdk1.6：
``` java
public class Stack<E> extends Vector<E> {
    /**
     * Creates an empty Stack.
     */
    public Stack() {
    }

    /**
     * Pushes an item onto the top of this stack. This has exactly
     * the same effect as:
     * <blockquote><pre>
     * addElement(item)</pre></blockquote>
     *
     * @param   item   the item to be pushed onto this stack.
     * @return  the <code>item</code> argument.
     * @see     java.util.Vector#addElement
     */
    public E push(E item) {
	addElement(item);

	return item;
    }

    /**
     * Removes the object at the top of this stack and returns that
     * object as the value of this function.
     *
     * @return     The object at the top of this stack (the last item
     *             of the <tt>Vector</tt> object).
     * @exception  EmptyStackException  if this stack is empty.
     */
    public synchronized E pop() {
	E	obj;
	int	len = size();

	obj = peek();
	removeElementAt(len - 1);

	return obj;
    }

    /**
     * Looks at the object at the top of this stack without removing it
     * from the stack.
     *
     * @return     the object at the top of this stack (the last item
     *             of the <tt>Vector</tt> object).
     * @exception  EmptyStackException  if this stack is empty.
     */
    public synchronized E peek() {
	int	len = size();

	if (len == 0)
	    throw new EmptyStackException();
	return elementAt(len - 1);
    }

    /**
     * Tests if this stack is empty.
     *
     * @return  <code>true</code> if and only if this stack contains
     *          no items; <code>false</code> otherwise.
     */
    public boolean empty() {
	return size() == 0;
    }

    /**
     * Returns the 1-based position where an object is on this stack.
     * If the object <tt>o</tt> occurs as an item in this stack, this
     * method returns the distance from the top of the stack of the
     * occurrence nearest the top of the stack; the topmost item on the
     * stack is considered to be at distance <tt>1</tt>. The <tt>equals</tt>
     * method is used to compare <tt>o</tt> to the
     * items in this stack.
     *
     * @param   o   the desired object.
     * @return  the 1-based position from the top of the stack where
     *          the object is located; the return value <code>-1</code>
     *          indicates that the object is not on the stack.
     */
    public synchronized int search(Object o) {
	int i = lastIndexOf(o);

	if (i >= 0) {
	    return size() - i;
	}
	return -1;
    }

    /** use serialVersionUID from JDK 1.0.2 for interoperability */
    private static final long serialVersionUID = 1224463164541339165L;
}
```
通过peek()方法注释The object at the top of this stack (the last item of the Vector object，可以发现数组（Vector）的最后一位即为Stack的栈顶

pop、peek以及search方法本身进行了同步

push方法调用了父类的addElement方法

empty方法调用了父类的size方法

Vector类为线程安全类

综上，Stack类为线程安全类(多个方法调用而产生的数据不一致问题属于原子性问题的范畴)
``` java
public class Test {  
    public static void main(String[] args) {  
        Stack<String> s = new Stack<String>();  
        System.out.println("------isEmpty");  
        System.out.println(s.isEmpty());  
        System.out.println("------push");  
        s.push("1");  
        s.push("2");  
        s.push("3");  
        Test.it(s);  
        System.out.println("------pop");  
        String str = s.pop();  
        System.out.println(str);  
        Test.it(s);  
        System.out.println("------peek");  
        str = s.peek();  
        System.out.println(str);  
        Test.it(s);  
        System.out.println("------search");  
        int i = s.search("2");  
        System.out.println(i);  
        i = s.search("1");  
        System.out.println(i);  
        i = s.search("none");  
        System.out.println(i);  
    }  
      
    public static void it(Stack<String> s){  
        System.out.print("iterator:");  
        Iterator<String> it = s.iterator();  
        while(it.hasNext()){  
            System.out.print(it.next()+";");  
        }  
        System.out.print("\n");  
    }  
}  
```
结果：
``` java
------isEmpty  
true            
------push  
iterator:1;2;3;    
------pop  
3       --栈顶是数组最后一个  
iterator:1;2;  
------peek  
2       --pop取后删掉，peek只取不删  
iterator:1;2;  
------search      
1       --以1为基数，即栈顶为1  
2       --和栈顶见的距离为2-1=1  
-1      --不存在于栈中  
```
Stack并不要求其中保存数据的唯一性，当Stack中有多个相同的item时，调用search方法，
只返回与查找对象equal并且离栈顶最近的item与栈顶间距离（见源码中search方法说明）

 