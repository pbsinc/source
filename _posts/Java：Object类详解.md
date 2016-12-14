---
title:  Java：Object类详解
date: 
categories: java总结
tags:  Java：Object类详解
---
转：Java 问答：终极父类
Java的一些特性会让初学者感到困惑，但在有经验的开发者眼中，却是合情合理的。
例如，新手可能不会理解Object类。这篇文章分成三个部分讲跟Object类及其方法有关的问题。
<!-- more -->
**上帝类**
**问：什么是Object类？**
答：Object类存储在java.lang包中，是所有java类(Object类除外)的终极父类。当然，数组也继承了Object类。然而，接口是不继承Object类的，原因在这里指出：Section 9.6.3.4 of the Java Language Specification:“Object类不作为接口的父类”。
Object类中声明了以下函数，我会在下文中作详细说明。
``` java
protected Object clone()
boolean equals(Object obj)
protected void finalize()
Class< > getClass()
int hashCode()
void notify()
void notifyAll()
String toString()
void wait()
void wait(long timeout)
void wait(long timeout, int nanos)
``` 
java的任何类都继承了这些函数，并且可以覆盖不被final修饰的函数。例如，没有final修饰的toString()函数可以被覆盖，但是final wait()函数就不行。

**问：可以声明要“继承Object类”吗？**
答：可以。在代码中明确地写出继承Object类没有语法错误。参考代码清单1。
代码清单1:明确的继承Object类
``` java
importjava.lang.Object;
publicclassEmployee extendsObject {
privateString name;
publicEmployee(String name) {
this.name = name;
}
publicString getName() {
returnname;
}
publicstaticvoidmain(String[] args) {
Employee emp = newEmployee("John Doe");
System.out.println(emp.getName());
}
}
``` 
你可以试着编译代码1(javac Employee.java)，然后运行Employee.class(java Employee)，可以看到John Doe 成功的输出了。

因为编译器会自动引入java.lang包中的类型，即 import java.lang.Object； 没必要声明出来。Java也没有强制声明“继承Object类”。

如果这样的话，就不能继承除Object类之外别的类了，因为java不支持多继承。然而，即使不声明出来，也会默认继承了Object类，参考代码清单2。
代码清单2:默认继承Object类
``` java
publicclassEmployee
{
privateString name;
publicEmployee(String name)
{
this.name = name;
}
publicString getName()
{
returnname;
}
publicstaticvoidmain(String[] args)
{
Employee emp = newEmployee("John Doe");
System.out.println(emp.getName());
}
}
``` 
就像代码清单1一样，这里的Employee类继承了Object，所以可以使用它的函数。

**克隆Object类**
**问：clone()函数是用来做什么的？**
答：clone()可以产生一个相同的类并且返回给调用者。
**问：clone()是如何工作的？**
答：Object将clone()作为一个本地方法来实现，这意味着它的代码存放在本地的库中。当代码执行的时候，将会检查调用对象的类(或者父类)是否实现了java.lang.Cloneable接口(Object类不实现Cloneable)。如果没有实现这个接口，clone()将会抛出一个检查异常()——java.lang.CloneNotSupportedException,如果实现了这个接口，clone()会创建一个新的对象，并将原来对象的内容复制到新对象，最后返回这个新对象的引用。
**问：怎样调用clone()来克隆一个对象？**
答：用想要克隆的对象来调用clone()，将返回的对象从Object类转换到克隆的对象所属的类，赋给对象的引用。这里用代码清单3作一个示例。
代码清单3:克隆一个对象
``` java
publicclassCloneDemo implementsCloneable {
intx;
publicstaticvoidmain(String[] args) throwsCloneNotSupportedException {
CloneDemo cd = newCloneDemo();
cd.x = 5;
System.out.printf("cd.x = %d%n", cd.x);
CloneDemo cd2 = (CloneDemo) cd.clone();
System.out.printf("cd2.x = %d%n", cd2.x);
}
}
``` 
代码清单3声明了一个继承Cloneable接口的CloneDemo类。这个接口必须实现，否则，调用Object的clone()时将会导致抛出异常CloneNotSupportedException。

CloneDemo声明了一个int型变量x和主函数main()来演示这个类。其中，main()声明可能会向外抛出CloneNotSupportedException异常。

Main()先实例化CloneDemo并将x的值初始化为5。然后输出x的值，紧接着调用clone() ，将克隆的对象传回CloneDemo。最后，输出了克隆的x的值。

编译代码清单3(javac CloneDemo.java)然后运行(java CloneDemo)。你可以看到以下运行结果：

	1
	2
	cd.x = 5
	cd2.x = 5
**问：什么情况下需要覆盖clone()方法呢？**
答：上面的例子中，调用clone()的代码是位于被克隆的类(即CloneDemo类)里面的，所以就不需要覆盖clone()了。

但是，如果调用别的类中的clone()，就需要覆盖clone()了。否则，将会看到“clone在Object中是被保护的”提示，
因为clone()在Object中的权限是protected。(译者注：protected权限的成员在不同的包中，只有子类对象可以访问。

代码清单3的CloneDemo类和代码清单4的Data类是Object类的子类，所以可以调用clone()，但是代码清单4中的CloneDemo类就不能直接调用Data父类的clone())。

代码清单4在代码清单3上稍作修改来演示覆盖clone()。

代码清单4:从别的类中克隆对象
``` java
classData implementsCloneable {
intx;
@Override
publicObject clone() throwsCloneNotSupportedException {
returnsuper.clone();
}
}
publicclassCloneDemo {
publicstaticvoidmain(String[] args) throwsCloneNotSupportedException {
Data data = newData();
data.x = 5;
System.out.printf("data.x = %d%n", data.x);
Data data2 = (Data) data.clone();
System.out.printf("data2.x = %d%n", data2.x);
}
}
``` 
代码清单4声明了一个待克隆的Data类。
这个类实现了Cloneable接口来防止调用clone()的时候抛出异常CloneNotSupportedException，声明了int型变量x，覆盖了clone()方法。

这个方法通过执行super.clone()来调用父类的clone()(这个例子中是Object的)。通过覆盖来避免抛出CloneNotSupportedException异常。

代码清单4也声明了一个CloneDemo类来实例化Data，并将其初始化，输出示例的值。然后克隆Data的对象，同样将其值输出。

编译代码清单4(javac CloneDemo.java)并运行(java CloneDemo)，你将看到以下运行结果：

	1
	2
	data.x = 5
	data2.x = 5
**问：什么是浅克隆？**
A:浅克隆(也叫做浅拷贝)仅仅复制了这个对象本身的成员变量，该对象如果引用了其他对象的话，也不对其复制。

代码清单3和代码清单4演示了浅克隆。新的对象中的数据包含在了这个对象本身中，不涉及对别的对象的引用。

如果一个对象中的所有成员变量都是原始类型，并且其引用了的对象都是不可改变的(大多情况下都是)时，使用浅克隆效果很好！

但是，如果其引用了可变的对象，那么这些变化将会影响到该对象和它克隆出的所有对象！代码清单5给出一个示例。

代码清单5：演示浅克隆在复制引用了可变对象的对象时存在的问题
``` java
classEmployee implementsCloneable {
privateString name;
privateintage;
privateAddress address;
Employee(String name, intage, Address address) {
this.name = name;
this.age = age;
this.address = address;
}
@Override
publicObject clone() throwsCloneNotSupportedException {
returnsuper.clone();
}
Address getAddress() {
returnaddress;
}
String getName() {
returnname;
}
intgetAge() {
returnage;
}
}
classAddress {
privateString city;
Address(String city) {
this.city = city;
}
String getCity() {
returncity;
}
voidsetCity(String city) {
this.city = city;
}
}
publicclassCloneDemo {
publicstaticvoidmain(String[] args) throwsCloneNotSupportedException {
Employee e = newEmployee("John Doe", 49, newAddress("Denver"));
System.out.printf("%s: %d: %s%n", e.getName(), e.getAge(),
e.getAddress().getCity());
Employee e2 = (Employee) e.clone();
System.out.printf("%s: %d: %s%n", e2.getName(), e2.getAge(),
e2.getAddress().getCity());
e.getAddress().setCity("Chicago");
System.out.printf("%s: %d: %s%n", e.getName(), e.getAge(),
e.getAddress().getCity());
System.out.printf("%s: %d: %s%n", e2.getName(), e2.getAge(),
e2.getAddress().getCity());
}
}
``` 
代码清单5给出了Employee、Address和CloneDemo类。

Employee声明了name、age、address成员变量，是可以被克隆的类；Address声明了一个城市的地址并且其值是可变的。CloneDemo类驱动这个程序。

CloneDemo的主函数main()创建了一个Employee对象并且对其进行克隆，然后，改变了原来的Employee对象中address值城市的名字。

因为原来的Employee对象和其克隆出来的对象引用了相同的Address对象，所以两者都会提现出这个变化。

编译 (javac CloneDemo.java) 并运行 (java CloneDemo)代码清单5，你将会看到如下输出结果：
 
	John Doe: 49: Denver
	John Doe: 49: Denver
	John Doe: 49: Chicago
	John Doe: 49: Chicago
**问：什么是深克隆？**

答：深克隆(也叫做深复制)会复制这个对象和它所引用的对象的成员变量，如果该对象引用了其他对象，深克隆也会对其复制。

例如，代码清单6在代码清单5上稍作修改演示深克隆。同时，这段代码也演示了协变返回类型和一种更为灵活的克隆方式。

代码清单6：深克隆成员变量address
``` java
classEmployee implementsCloneable
{
privateStringname;
privateintage;
privateAddress address;
Employee(Stringname, intage, Address address)
{
this.name = name;
this.age = age;
this.address = address;
}
@Override
publicEmployee clone() throws CloneNotSupportedException
{
Employee e = (Employee) super.clone();
e.address = address.clone();
returne;
}
Address getAddress()
{
returnaddress;
}
StringgetName()
{
returnname;
}
intgetAge()
{
returnage;
}
}
classAddress
{
privateStringcity;
Address(Stringcity)
{
this.city = city;
}
@Override
publicAddress clone()
{
returnnewAddress(newString(city));
}
StringgetCity()
{
returncity;
}
voidsetCity(Stringcity)
{
this.city = city;
}
}
publicclassCloneDemo
{
publicstaticvoidmain(String[] args) throws CloneNotSupportedException
{
Employee e = newEmployee("John Doe", 49, newAddress("Denver"));
System.out.printf("%s: %d: %s%n", e.getName(), e.getAge(),
e.getAddress().getCity());
Employee e2 = (Employee) e.clone();
System.out.printf("%s: %d: %s%n", e2.getName(), e2.getAge(),
e2.getAddress().getCity());
e.getAddress().setCity("Chicago");
System.out.printf("%s: %d: %s%n", e.getName(), e.getAge(),
e.getAddress().getCity());
System.out.printf("%s: %d: %s%n", e2.getName(), e2.getAge(),
e2.getAddress().getCity());
}
}
``` 
Java支持协变返回类型，代码清单6利用这个特性，在Employee类中覆盖父类clone()方法时，将返回类型从Object类的对象改为Employee类型。

这样做的好处就是，Employee类之外的代码可以不用将这个类转换为Employee类型就可以对其进行复制。

Employee类的clone()方法首先调用super().clone()，对name,age,address这些成员变量进行浅克隆。

然后，调用成员变量Address对象的clone()来对其引用Address对象进行克隆。

从Address类中的clone()函数可以看出，这个clone()和我们之前写的clone()有些不同：

Address类没有实现Cloneable接口。因为只有在Object类中的clone()被调用时才需要实现，而Address是不会调用clone()的，所以没有实现Cloneable()的必要。

这个clone()函数没有声明抛出CloneNotSupportedException。这个检查异常只可能在调用Object类clone()的时候抛出。clone()是不会被调用的，因此这个异常也就没有被处理或者传回调用处的必要了。

Object类的clone()没有被调用(这里没有调用super.clone())。因为这不是对Address的对象进行浅克隆——只是一个成员变量复制而已。

为了克隆Address的对象，需要创建一个新的Address对象并对其成员进行初始化操作。最后将新创建的Address对象返回。

编译(javac CloneDemo.java)代码清单6并且运行这个程序，你将会看到如下输出结果(java CloneDemo)：

	John Doe: 49: Denver
	John Doe: 49: Denver
	John Doe: 49: Chicago
	John Doe: 49: Denver
**Q:如何克隆一个数组？**

A:对数组类型进行浅克隆可以利用clone()方法。对数组使用clone()时，不必将clone()的返回值类型转换为数组类型，代码清单7示范了数组克隆。

代码清单7：对两个数组进行浅克隆
``` java
classCity {
privateString name;
City(String name) {
this.name = name;
}
String getName() {
returnname;
}
voidsetName(String name) {
this.name = name;
}
}
publicclassCloneDemo {
publicstaticvoidmain(String[] args) {
double[] temps = { 98.6, 32.0, 100.0, 212.0, 53.5};
for(doubletemp : temps)
System.out.printf("%.1f ", temp);
System.out.println();
double[] temps2 = temps.clone();
for(doubletemp : temps2)
System.out.printf("%.1f ", temp);
System.out.println();
System.out.println();
City[] cities = { newCity("Denver"), newCity("Chicago") };
for(City city : cities)
System.out.printf("%s ", city.getName());
System.out.println();
City[] cities2 = cities.clone();
for(City city : cities2)
System.out.printf("%s ", city.getName());
System.out.println();
cities[0].setName("Dallas");
for(City city : cities2)
System.out.printf("%s ", city.getName());
System.out.println();
}
}
``` 
代码清单7声明了一个City类存储名字，还有一些有关城市的数据(比如人口)。CloneDemo类提供了主函数main()来演示数组克隆。

main()函数首先声明了一个双精度浮点型数组来表示温度。在输出数组的值之后，克隆这个数组——注意没有运算符。之后，输出克隆的完全相同的数据。

紧接着，main()声明了一个City对象的数组，输出城市的名字，克隆这个数组，输出克隆的这个数组中城市的名字。为了证明浅克隆完成(比如，这两个数组引用了相同的City对象)，main()最后改变了原来的数组中第一个城市的名字，输出第二个数组中所有城市的名字。我们马上就可以看到，第二个数组中的名字也改变了。

编译 (javac CloneDemo.java)并运行 (java CloneDemo)代码清单7，你将会看到如下输出结果：

	98.6 32.0 100.0 212.0 53.5
	98.6 32.0 100.0 212.0 53.5
	Denver Chicago
	Denver Chicago
	Dallas Chicago

**Equality**
**问：euqals()函数是用来做什么的？**
答：equals()函数可以用来检查一个对象与调用这个equals()的这个对象是否相等。
**问：为什么不用“==”运算符来判断两个对象是否相等呢？**
答：虽然“==”运算符可以比较两个数据是否相等，但是要来比较对象的话，恐怕达不到预期的结果。就是说，“==”通过是否引用了同一个对象来判断两个对象是否相等，这被称为“引用相等”。这个运算符不能通过比较两个对象的内容来判断它们是不是逻辑上的相等。
**问：使用Object类的equals()方法可以用来做什么样的对比？**
答：Object类默认的eqauls()函数进行比较的依据是：调用它的对象和传入的对象的引用是否相等。也就是说，默认的equals()进行的是引用比较。如果两个引用是相同的，equals()函数返回true；否则，返回false.
**问：覆盖equals()函数的时候要遵守那些规则？**
答：覆盖equals()函数的时候需要遵守的规则在Oracle官方的文档中都有申明：
**自反性：**对于任意非空的引用值x，x.equals(x)返回值为真。
**对称性：**对于任意非空的引用值x和y，x.equals(y)必须和y.equals(x)返回相同的结果。
**传递性：**对于任意的非空引用值x,y和z,如果x.equals(y)返回真，y.equals(z)返回真，那么x.equals(z)也必须返回真。
**一致性：**对于任意非空的引用值x和y，无论调用x.equals(y)多少次，都要返回相同的结果。在比较的过程中，对象中的数据不能被修改。
对于任意的非空引用值x，x.equals(null)必须返回假。
**问：能提供一个正确覆盖equals()的示例吗？**
答：当然，请看代码清单8。
代码清单8：对两个对象进行逻辑比较
``` java
classEmployee
{
privateString name;
privateintage;
Employee(String name, intage)
{
this.name = name;
this.age = age;
}
@Override
publicbooleanequals(Object o)
{
if(!(o instanceofEmployee))
returnfalse;
Employee e = (Employee) o;
returne.getName().equals(name) && e.getAge() == age;
}
String getName()
{
returnname;
}
intgetAge()
{
returnage;
}
}
publicclassEqualityDemo
{
publicstaticvoidmain(String[] args)
{
Employee e1 = newEmployee("John Doe", 29);
Employee e2 = newEmployee("Jane Doe", 33);
Employee e3 = newEmployee("John Doe", 29);
Employee e4 = newEmployee("John Doe", 27+2);
// 验证自反性。
System.out.printf("Demonstrating reflexivity...%n%n");
System.out.printf("e1.equals(e1): %b%n", e1.equals(e1));
// 验证对称性。
System.out.printf("%nDemonstrating symmetry...%n%n");
System.out.printf("e1.equals(e2): %b%n", e1.equals(e2));
System.out.printf("e2.equals(e1): %b%n", e2.equals(e1));
System.out.printf("e1.equals(e3): %b%n", e1.equals(e3));
System.out.printf("e3.equals(e1): %b%n", e3.equals(e1));
System.out.printf("e2.equals(e3): %b%n", e2.equals(e3));
System.out.printf("e3.equals(e2): %b%n", e3.equals(e2));
// 验证传递性。
System.out.printf("%nDemonstrating transitivity...%n%n");
System.out.printf("e1.equals(e3): %b%n", e1.equals(e3));
System.out.printf("e3.equals(e4): %b%n", e3.equals(e4));
System.out.printf("e1.equals(e4): %b%n", e1.equals(e4));
// 验证一致性。
System.out.printf("%nDemonstrating consistency...%n%n");
for(inti = 0; i < code>5; i++)
{
System.out.printf("e1.equals(e2): %b%n", e1.equals(e2));
System.out.printf("e1.equals(e3): %b%n", e1.equals(e3));
}
// 验证传入非空集合时，返回值为false。
System.out.printf("%nDemonstrating null check...%n%n");
System.out.printf("e1.equals(null): %b%n", e1.equals(null));
}
}
``` 
代码清单8声明了一个包含名字、年龄成员变量的Employee对象。这个对象覆盖了equals()函数来对Employee对象进行适当的对比。
ps：覆盖hashCode()函数
当覆盖equals()函数的时候，就相当于覆盖了hashCode()函数，我将在下篇文章讨论hashCode()的时候详细说明。

equals()函数首先要检查传入的确实是一个Employee对象。如果不是，返回false。这个检查是靠instanceof运算来判断的，当传入null值的时候，同样也返回false。

因此，遵守了“对于任意的非空引用值x，x.equals(null)必须返回假”这个规则。

这样，我们就保证了传入的对象是Employee类型。因为之前的instanceof判断保证了传入值必须是Employee类型的对象，所以在这里我们就不必担心抛出ClassCastException异常了。

接下来，equals()方法对两个对象的name和age的值进行了比较。

编译(javac EqualityDemo.java)并运行(Java EqualityDemo)代码清单8，你可以看到以下输出结果：

	Demonstrating reflexivity...
	e1.equals(e1): true
	Demonstrating symmetry...
	e1.equals(e2): false
	e2.equals(e1): false
	e1.equals(e3): true
	e3.equals(e1): true
	e2.equals(e3): false
	e3.equals(e2): false
	Demonstrating transitivity...
	e1.equals(e3): true
	e3.equals(e4): true
	e1.equals(e4): true
	Demonstrating consistency...
	e1.equals(e2): false
	e1.equals(e3): true
	e1.equals(e2): false
	e1.equals(e3): true
	e1.equals(e2): false
	e1.equals(e3): true
	e1.equals(e2): false
	e1.equals(e3): true
	e1.equals(e2): false
	e1.equals(e3): true
	Demonstrating null check...
	e1.equals(null): false

**equals()和继承**
当Employee类被继承的时候，代码清单8就存在一些问题。例如，SaleRep类继承了Employee类，这个类中也有基于字符串类型的变量，equals()可以对其进行比较。

假设你创建的Employee对象和SaleRep对象都有相同的“名字”和“年龄”。但是，SaleRep中还是添加了一些内容。
假设你在Employee对象中调用equals()方法并且传入了一个SaleRep对象。

由于SaleRep对象继承了Employee，也是一种Employee的对象，instanceof判断会通过，并且执行equals()方法来判断名字和年龄。因为这两个对象有完全相同的名字和年龄，所以equals()方法返回true。

如果拿SaleRep对象中Employee的部分来和Employee比较的话，返回true值是正确的，但是，如果拿整个SaleRep对象来和Employee对象比较，返回true值就不妥了。

现在假设在SaleRep对象中调用equals()方法并将Employee传入。

因为Employee不是SaleRep类型的对象（否则的话，你可以访问Employee对象中不存在的Region域，这会导致虚拟机崩溃），无法通过instanceof判断，equals()方法返回false。

综上，equals()在一种判断中为true却在另一判断中为false，违背了“对称性原则”。

Joshua Bloch在《Effective Java Programming Language Guide》第七版中指出：我们无法扩展可被实例化的类（例如Employee）并向其中增加一个域（如Region域），而同时维持equals()方法的对称性。

尽管也有办法来维持对称性，但代价是破坏传递性。Bloch指出解决这个问题需要在继承上支持组合：不是让SaleRep来扩展Employee，SaleRep应该引用一个私有的Employee值。

获得更多信息可以参考Bloch的书。

**问：可以使用equals()函数来判断两个数组是否相等吗？**
答：可以调用equals()函数来比较数组的引用是否相等。但是，由于在数组对象中无法覆盖equals()，所以只能对数组的引用进行比较，因为不是很常用。参见代码清单9。

代码清单9：尝试通过equals()函数来比较两个数组
``` java
publicclassEqualityDemo
{
publicstaticvoidmain(String[] args)
{
intx[] = { 1, 2, 3};
inty[] = { 1, 2, 3};
System.out.printf("x.equals(x): %b%n", x.equals(x));
System.out.printf("x.equals(y): %b%n", x.equals(y));
}
}
``` 
代码清单9的main()函数中声明了一对类型与内容完全相等的数组。然后尝试对第一个数组和它自己、第一个数组和第二个数组分别进行比较。

由于equals()对数组来说比较的仅仅是引用，而不比较内容，所以x.equals(x)返回true（因为自反性——一个对象与它自己相等），但是x.equals(y)返回false。

编译(javac EqualityDemo.java) 并运行(java EqualityDemo)代码清单9，你将会看到以下输出结果：

	1
	2
	x.equals(x): true
	x.equals(y): false
如果你想要比较的是两个数组的内容，也不要绝望。

可以使用java.util.Arrays 类中声明的 static boolean deepEquals(Object[] a1, Object[] a2) 方法来实现。代码清单10演示了这个方法。

代码清单10：通过deepEquals()函数来比较两个数组
``` java
importjava.util.Arrays;
publicclassEqualityDemo
{
publicstaticvoidmain(String[] args)
{
Integer x[] = { 1, 2, 3};
Integer y[] = { 1, 2, 3};
Integer z[] = { 3, 2, 1};
System.out.printf("x.equals(x): %b%n", Arrays.deepEquals(x, x));
System.out.printf("x.equals(y): %b%n", Arrays.deepEquals(x, y));
System.out.printf("x.equals(z): %b%n", Arrays.deepEquals(x, z));
}
}
``` 
由于deepEquals()方法要求传入的数组元素必须是对象，所以之前在代码清单9中的元素类型要从int[]改为Integer[]。

Java语言的自动封装特性会把integer常量转换成Integer对象存放在数组中。接下来要将数组传入到deepEquals()就是小事一桩了。

编译(javac EqualityDemo.java)并运行(java EqualityDemo)代码清单10，你将看到以下输出结果。

	x.equals(x): true
	x.equals(y): true
	x.equals(z): false
用deepEquals()方法比较的相等是“深度”的相等：这要求每个元素对象所包含的的成员、对象相等。

成员对象如果还包含了对象，也要相等，以此类推，才算是“相等”（另外，两个空的数组引用也是“深度”的相等，因此Arrays.deepEquals(null, null)返回true）。

**终止**
**问：finalize()方法是用来做什么的？**
答：finalize()方法可以被子类对象所覆盖，然后作为一个终结者，当GC被调用的时候完成最后的清理工作（例如释放系统资源之类）。这就是终止。默认的finalize()方法什么也不做，当被调用时直接返回。
对于任何一个对象，它的finalize()方法都不会被JVM执行两次。如果你想让一个对象能够被再次调用的话（例如，分配它的引用给一个静态变量），注意当这个对象已经被GC回收的时候，finalize()方法不会被调用第二次。
**问： 有人说要避免使用finalize()方法，这是真的吗？**
答： 通常来讲，你应该尽量避免使用finalize()。相对于其他JVM实现，终结器被调用的情况较少——可能是因为终结器线程的优先级别较低的原因。如果你依靠终结器来关闭文件或者其他系统资源，可能会将资源耗尽，当程序试图打开一个新的文件或者新的系统资源的时候可能会崩溃，就因为这个缓慢的终结器。
**问： 应该使用什么来替代终结器？**
答： 提供一个明确的用来销毁这个对象的方法（例如，java.io.FileInputStream的void close()方法），并且在代码中使用try - finally结构来调用这个方法，以确保无论有没有异常从try中抛出，都会销毁这个对象。参考下面释放锁的代码：
``` java
Lock l = ...; // ... is a placeholder for the actual lock-acquisition code
l.lock();
try
{
// access the resource protected by this lock
}
finally
{
l.unlock();
}
这段代码保证了无论try是正常结束还是抛出异常都会释放锁。
**问： 什么情况下适合使用终结器？**
答： 终结器可以作为一个安全保障，以防止声明的终结方法（像是java.io.FileOutputStream对象的close()方法或者java.util.concurrent.Lock对象的Lock()方法）没有被调用。万一这种情况出现，终结器可以在最后被调用，释放临街资源。
**问： 怎么写finalize()？**
答： 可以遵循下面这个模式写finalize()方法：
``` java
@Override
protectedvoidfinalize() throwsThrowable
{
try
{
// Finalize the subclass state.
// ...
}
finally
{
super.finalize();
}
}
``` 
子类终结器一般会通过调用父类的终结器来实现。当被调用时，先执行try模块，然后再在对应的finally中调用super.finalize()；这就保证了无论try会不会抛出异常父类都会被销毁。
**问： 如果finalize()抛出异常会怎样？**
答： 当finalize()抛出异常的时候会被忽略。而且，对象的终结将在此停止，导致对象处在一种不确定的状态。如果另一个进程试图使用这个对象的话，将产生不确定的结果。通常抛出异常将会导致线程终止并产生一个提示信息，但是从finalize()中抛出异常就不会。
**问： 我想实践一下finalize()方法，能提供一个范例吗？**
答： 参考代码清单1.
``` java
classLargeObject
{
byte[] memory = newbyte[1024*1024*4];
@Override
protectedvoidfinalize() throwsException
{
System.out.println("finalized");
}
}
publicclassFinalizeDemo
{
publicstaticvoidmain(String[] args)
{
while(true)
newLargeObject();
}
}
``` 
代码清单1:实践finalize()

代码清单1中的代码写了一个FinalizeDemo程序，重复地对largeObject类实例化。每一个Largeobject对象将产生4M的数组。

在这种情况下，由于没有指向该对象的引用，所以LargeObject对象将被GC回收。

GC会调用对象的finalize()方法来回收对象。LargeObject重载的finalize()方法被调用的时候会想标准输出流打印一条信息。它没有调用父类的finalize()方法，因为它的父类是Object，即父类的finalize()方法什么也不做。

编译(javac FinalizeDemo.java)并运行(java FinalizeDemo)代码清单1.当我在我的环境下（64位win7平台）使用JDK7u6来编译运行的时候，我看到一列finalized的信息。

但是在JDK8的环境下时，在几行finalized之后抛出了java.lang.OutOfMemoryError。

因为finalize()方法对于虚拟机来说不是轻量级的程序，所以不能保证你一定会在你的环境下观察到输出信息。

**得到对象的类**

**问：gerClass()方法是用来做什么的？**

答： 通过gerClass()方法可以得到一个和这个类有关的java.lang.Class对象。返回的Class对象是一个被static synchronized方法封装的代表这个类的对象；例如，static sychronized void foo(){}。这也是指向反射API。因为调用gerClass()的对象的类是在内存中的，保证了类型安全。

**问： 还有其他方法得到Class对象吗？**

答： 获取Class对象的方法有两种。可以使用类字面常量，它的名字和类型相同，后缀位.class；例如，Account.class。另外一种就是调用Class的foeName()方法。类字面常量更加简洁，并且编译器强制类型安全；如果找不到指定的类编译就不会通过。通过forname()可以动态地通过指定包名载入任意类型地引用。但是，不能保证类型安全，可能会导致Runtime异常。

**问： 实现equals()方法的时候，getClass()和instanceof哪一个更好？**

答： 使用getClass()还是instanceof的话题一直都是Java社区争论的热点，Angelika Langer的Secrets of equals – Part 1这片文章可以帮助你做出选择。关于正确覆盖equals()方法（例如保证对称性）的讨论，Lang的这篇文章可以作为一个很好的参考手册。

**哈希码**

**问：hashCode()方法是用来做什么的？**

答：hashCode()方法返回给调用者此对象的哈希码（其值由一个hash函数计算得来）。这个方法通常用在基于hash的集合类中，像java.util.HashMap,java.until.HashSet和java.util.Hashtable.

**问： 在类中覆盖equals()的时候，为什么要同时覆盖hashCode()？**

答： 在覆盖equals()的时候同时覆盖hashCode()可以保证对象的功能兼容于hash集合。这是一个好习惯，即使这些对象不会被存储在hash集合中。

**问：hashCode()有什么一般规则？**

答：hashCode()的一般规则如下：

在同一个Java程序中，对一个相同的对象，无论调用多少次hashCode()，hashCode()返回的整数必须相同，因此必须保证equals()方法比较的内容不会更改。但不必在另一个相同的Java程序中也保证返回值相同。

如果两个对象用equals()方法比较的结果是相同的，那么这两个对象调用hashCode()应该返回相同的整数值。

当两个对象使用equals()方法比较的结果是不同的，hashCode()返回的整数值可以不同。然而，hashCode()的返回值不同可以提高哈希表的性能。

**问： 如果覆盖了equals()却不覆盖hashCode()会有什么后果？**

答： 当覆盖equals()却不覆盖hashCode()的时候，在hash集合中存储对象时就会出现问题。例如，参考代码清单2.

代码清单2：当hash集合只覆盖equals()时的问题
``` java
importjava.util.HashMap;
importjava.util.Map;
finalclassEmployee
{
privateString name;
privateintage;
Employee(String name, intage)
{
this.name = name;
this.age = age;
}
@Override
publicbooleanequals(Object o)
{
if(!(o instanceofEmployee))
returnfalse;
Employee e = (Employee) o;
returne.getName().equals(name) && e.getAge() == age;
}
String getName()
{
returnname;
}
intgetAge()
{
returnage;
}
}
publicclassHashDemo
{
publicstaticvoidmain(String[] args)
{
Map map = newHashMap<>();
Employee emp = newEmployee("John Doe", 29);
map.put(emp, "first employee");
System.out.println(map.get(emp));
System.out.println(map.get(newEmployee("John Doe", 29)));
}
}
``` 
代码清单2声明了一个Employee类，覆盖了equals()方法但是没有覆盖hashCode()。同时声明了一个一个HashDemo类，来演示将Employee作为键存储时时产生的问题。

main()函数首先在实例化Employee之后创建了一个hashmap，将Employee对象作为键，将一个字符串作为值来存储。然后它将这个对象作为键来检索这个集合并输出结果。

同样地，再通过新建一个具有相同内容的Employee对象作为键来检索集合，输出信息。

编译(javac HashDemo.java)并运行(java HashDemo)代码清单2，你将看到如下输出结果：

	first employee
	null
如果hashCode()方法被正确的覆盖，你将在第二行看到first employee而不是null，因为这两个对象根据equals()方法比较的结果是相同的，根据上文中提到的规则2：如果两个对象用equals()方法比较的结果是相同的，那么这两个对象调用hashCode()应该返回相同的整数值。

**问： 如何正确的覆盖hashCode()？**

答： Joshua Bloch的《Effective Java》第八版中给出了一个四步法来正确的覆盖hashCode()。下面的步骤和Bloch的方法类似。

声明一个int型的变量，命名为result（或者其他你喜欢的名字），然后初始化为一个不为零的常量（比如31）。使用一个不为零的常量会影响到所有的初始的哈希值（步骤2.1的结果）为零的值。
【A nonzero value is used so that it will be affected by any initial fields whose hash value (computed in Step 2.1) is zero. 】如果初始的result为0的话，最后的哈希值不会被它影响到，所以冲突的几率会增加。这个非零result值是任意的。
对每一个对象中有意义的具体值（在equals（）中所涉及的值），f，进行以下步骤的处理：

按照以下步骤计算f的基于int型的哈希值hc：
对于一个boolean型变量，hc = f？ 0 ： 1；。
对于一个byte,char,short,或者int型变量,hc = (int)f;.
对于一个long型变量，hc = (int) (f ^ (f <<< 32));.这个表达式是将long型变量作为32位（long型最多有32位）来计算的；
对于一个float型变量，hc = Float.floatToIntBits(f);.
对于一个double型变量，long l = Double.doubleToLongBits(f); hc = (int) (l ^ (l <<< 32));.
对于引用类型的变量，如果类中的equals()方法递归的调用equals()类比较成员变量，那么就递归调用hashCode()；如果需要更复杂的比较，就计算这个值的“标准表示”来脚酸标准的哈希值；如果引用类型的值为null，f = 0.
对于一个数组类型的引用，将每一个元素视为单独的变量，对于每一个有意义的值，调用对应的方法计算其哈希值，最后如步骤2.2的描述那样将所有的哈希值合并。

计算result = 37*result+hc，将所有的hc合并到哈希值中。乘法使哈希值取决于它的值的规则，当一个类中存在多种相似的值时，就增加了哈希表的离散性。
返回result。

完成hashCode()之后，要确保相同的对象调用hashCode()得到相同的哈希值。

举例说明上面这个方法，代码清单3是代码清单2的第二个版本，它的Employee类重写了hashCode()。

代码清单3：正确地覆盖hashCode()
``` java
importjava.util.HashMap;
importjava.util.Map;
finalclassEmployee
{
privateString name;
privateintage;
Employee(String name, intage)
{
this.name = name;
this.age = age;
}
@Override
publicbooleanequals(Object o)
{
if(!(o instanceofEmployee))
returnfalse;
Employee e = (Employee) o;
returne.getName().equals(name) && e.getAge() == age;
}
String getName()
{
returnname;
}
intgetAge()
{
returnage;
}
@Override
publicinthashCode()
{
intresult = 31;
result = 37*result+name.hashCode();
result = 37*result+age;
returnresult;
}
}
publicclassHashDemo
{
publicstaticvoidmain(String[] args)
{
Map map = newHashMap<>();
Employee emp = newEmployee("John Doe", 29);
map.put(emp, "first employee");
System.out.println(map.get(emp));
System.out.println(map.get(newEmployee("John Doe", 29)));
}
}
``` 
代码清单3的Employee类中声明了两个在hashCode()都涉及到的值。覆盖的hashCode()方法首先初始化result为31，然后将String类型的name变量和int型的age变量的哈希值合并到result中，随后返回result。

编译(javac HashDemo.java)并运行(java HashDemo)代码清单3，你将看到如下输出结果：

	first employee
	first employee

**字符串形式的表现**
**Q1：toString() 方法实现了什么功能？**
A1：toString() 方法将根据调用它的对象返回其对象的字符串形式，通常用于debug。
**Q2：当 toString() 方法没有被覆盖的时候，返回的字符串通常是什么样子的？**
A2：当 toString() 没有被覆盖的时候，返回的字符串格式是 类名@哈希值，哈希值是十六进制的。举例说，假设有一个 Employee 类，toString() 方法返回的结果可能是 Empoyee@1c7b0f4d。
**Q3：能提供一个正确覆盖 toString() 方法的例子吗？**
A3：见代码清单1：
``` java
publicclassEmployee
{
privateString name;
privateintage;
publicEmployee(String name, intage)
{
this.name = name;
this.age = age;
}
@Override
publicString toString()
{
returnname + ": "+ age;
}
}
``` 
代码清单1：返回一个非默认的字符串形式
代码清单1声明了 Employee 类，被私有修饰符修饰的 name 和 age 变量，构造器将其初始化。该类覆盖了 toString() 方法，并返回一个包含对象值和一个冒号的 String 对象。
字符串和 StringBuilder
当编译器遇到 name + ": " + age 的表达时，会生成一个 java.lang.StringBuilder 对象，并调用 append() 方法来对字符串添加变量值和分隔符。最后调用 toString() 方法返回一个包含各个元素的字符串对象。
**Q4：如何得到字符串的表达形式？**
A4：根据对象的引用，调用引用的 toString() 。例如，假设 emp 包含了一个 Employee 引用，调用 emp.toString() 就会得到这个对象的字符串形式。
**Q5：System.out.println(o.toString()); 和 System.out.println(o) 的区别是什么？**
A5：System.out.println(o.toString()); 和 System.out.println(o) 两者的输出结果中都包含了对象的字符串形式。区别是，System.out.println(o.toString()); 直接调用toString() 方法，而System.out.println(o) 则是隐式调用了 toString()。
等待和唤醒
**Q6：wait()，notify() 和 notifyAll() 是用来干什么的？**
A6：wait()，notify() 和 notifyAll() 可以让线程协调完成一项任务。例如，一个线程生产，另一个线程消费。生产线程不能在前一产品被消费之前运行，而应该等待前一个被生产出来的产品被消费之后才被唤醒，进行生产。同理，消费线程也不能在生产线程之前运行，即不能消费不存在的产品。所以，应该等待生产线程执行一个之后才执行。利用这些方法，就可以实现这些线程之间的协调。从本质上说，一个线程等待某种状态（例如一个产品被生产），另一个线程正在执行，知道产生了某种状态（例如生产了一个产品）。
**Q7：不同的 wait() 方法之间有什么区别？**
A7：没有参数的 wait() 方法被调用之后，线程就会一直处于睡眠状态，直到本对象（就是 wait() 被调用的那个对象）调用 notify() 或 notifyAll() 方法。相应的wait(long timeout)和wait(long timeout, int nanos)方法中，当等待时间结束或者被唤醒时（无论哪一个先发生）将会结束等待。
**Q8：notify() 和 notifyAll() 方法有什么区别？**
A8：notify() 方法随机唤醒一个等待的线程，而 notifyAll() 方法将唤醒所有在等待的线程。
**Q9：线程被唤醒之后会发生什么？**
A9：当一个线程被唤醒之后，除非本对象（调用 notify() 或 notifyAll() 的对象）的同步锁被释放，否则不会立即执行。唤醒的线程会按照规则和其他线程竞争同步锁，得到锁的线程将执行。所以notifyAll()方法执行之后，可能会有一个线程立即运行，也可能所有的线程都没运行。
**Q10：为什么在使用等待、唤醒方法时，要放在同步代码中？**
A10:：将等待和唤醒方法放在同步代码中是非常必要的，这样做是为了避免竞争条件。鉴于要等待的线程通常在调用wait()之前会确认一种情况存在与否（通常是检查某一变量的值），而另一线程在调用notify()`之前通常会设置某种情况（通常是通过设置一个变量的值）。以下这种情况引发了竞争条件：
线程一检查了情况和变量，发现需要等待。
线程二设置了变量。
线程二调用了notify()。此时，线程一还没有等待，所以这次调用什么用都没有。
线程一调用了wait()。这下它永远不会被唤醒了。
**Q11：如果在同步代码之外使用这些方法会怎么样呢？**
A11：如果在同步代码之外使用了这些情况，就会抛出java.lang.IllegalMonitorStateException异常。
**Q12：如果在同步代码中调用这些方法呢？**
A12：当 wait() 方法在同步代码中被调用时，会根据同步代码中方法的优先级先后执行。在wait()方法返回值之前，该同步代码一直持有锁，这样就不会出现竞争条件了。在wait()方法可以接受唤醒之前，锁一直不会释放。
**Q13：为什么要把wait()调用放在while循环中，而不是if判断中呢？**
A13：为了防止假唤醒，可以在 stackoverflow上了解有关这类现象的更多信息——假唤醒真的会发生吗？。
**Q14：能提供一个使用等待与唤醒方法的范例吗？**
A14：见代码清单2：
``` java
publicclassWaitNotifyDemo
{
publicstaticvoidmain(String[] args)
{
classShared
{
privateString msg;
synchronizedvoidsend(String msg)
{
while(this.msg != null)
try
{
wait();
}
catch(InterruptedException ie)
{
}
this.msg = msg;
notify();
}
synchronizedString receive()
{
while(msg == null)
try
{
wait();
}
catch(InterruptedException ie)
{
}
String temp = msg;
msg = null;
notify();
returntemp;
}
}
finalShared shared = newShared();
Runnable rsender;
rsender = newRunnable()
{
@Override
publicvoidrun()
{
for(inti = 0; i < code>10; i++)
{
shared.send("A"+i);
try
{
Thread.sleep((int)(Math.random()*200));
}
catch(InterruptedException ie)
{
}
}
shared.send("done");
}
};
Thread sender = newThread(rsender);
Runnable rreceiver;
rreceiver = newRunnable()
{
@Override
publicvoidrun()
{
String msg;
while(!(msg = shared.receive()).equals("done"))
{
System.out.println(msg);
try
{
Thread.sleep((int)(Math.random()*200));
}
catch(InterruptedException ie)
{
}
}
}
};
Thread receiver = newThread(rreceiver);
sender.start();
receiver.start();    
}
}
``` 
代码清单2：发送与接收信息

代码清单2声明了一个WaitNotifyDemo类。其中，main()方法有一对发送和接收信息的线程。

main()方法首先声明了Shard本地类，包含接收和发送信息的任务。Share声明了一个String类型的smg私有成员变量来存储要发送的信息，同时声明了同步的 send()和receive()方法来执行接收和发送动作。

发送线程调用的是send()。因为上一次调用send()的信息可能还没有被接收到，所以这个方法首先要通过计算this.msg != null的值来判断信息发送状态。

如果返回值为true，那么信息处于被等待发送的状态，就会调用 wait() 。一旦信息被接收到，接受的线程就会给msg赋值为null并存储新信息，调用notify()唤醒等待的线程。

接收线程调用的是receive()因为可能没有信息处于被接收状态，这个方法首先会通过计算mas == null的值来验证信息有没有等待被接收的状态。如果表达式返回值为true，就表示没有信息等待被接收，此线程就要调用 wait() 方法。

如果有信息发送，发送线程就会给 msg 分配值并且调用notify()唤醒接收线程。

编译（javac WaitNotifyDemo.java）并运行（java WaitNotifyDemo）源代码，将会看到以下输出结果：

	A0
	A1
	A2
	A3
	A4
	A5
	A6
	A7
	A8
	A9
**Q15：我想更加深入的学习等待和唤醒的机制，能提供一些资源吗？**
A15：可以在artima参考Bill Venners的书《Inside the Java Virtual Machine（深入理解 Java 虚拟机）》中第20章 Chapter 20: Thread Synchronization。
Object接口和Java8
**Q16：在第一部分中提到过接口是不继承Object的。然而，我发现有些接口中声明了Object中的方法。比如java.util.Comparator接口有boolean.equals(Object.obj)。为什么呢？**
A16：Java语言规范的9.6.3.4部分中清楚说明了，接口有相当于 Object 中成员那样的公共抽象成员。此外，如果接口中声明了Object中的成员函数（例如，声明的函数相当于覆盖 Object中的public方法），则认为是接口覆盖了他们，可以用 @Override注释。
为什么要在接口中声明非final的publicObject方法（可能还带有 @Override）呢？举例来说，Comparator接口中就有boolean equals(Object obj)声明，这个方法在接口中声明就是为了此接口的特殊情况。
此外，这个方法只有在传入的类是一个比较规则相同的比较器的时候，才能返回 true。
因为这种情况是可选的，所以并不强制实现 Comparator。这取决于有没有equals，只有在遇到一个比较规则相同的比较器的时候才返回true的需求。尽管类并不要求覆盖equals，但是文档中却支持这样做来提高性能。
注意，不覆盖 Object.equals(Object)是安全的。但是，覆盖这个方发可能在一些情况下提高性能，比如让程序判断两个不同的比较器是不是用的相同规则。
**Q17：哪一个Employee方法被覆盖了？是Object中的，还是接口中的？**
A17：更早的文档中说，被覆盖的方法是在Object中的。
**Q18:Java 8支持接口中的默认方法。可以在接口中默认实现Employee方法或者Object中的其他方法吗？**
A18：不可以。Object中的任何public的非final方法都是不允许在接口中默认实现的。这个限制的基本原理在Brian Goetz的允许默认方法覆盖Object中的方法一文中有说明。
**Q19：能提供更多关于接口中 Object 方法的学习资源吗？**
A19：可以参考这篇接口继承了Object类吗？。

原文链接： Javaworld 翻译： ImportNew.com - 赖 信涛
译文链接： http://www.importnew.com/10304.html