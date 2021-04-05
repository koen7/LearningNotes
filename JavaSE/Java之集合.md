# 一、集合与数组

## 1.集合与数组存储数据概述

集合和数组都是对多个数据进行存储操作的结构，简称**Java容器**。说明：此时的存储，主要指的是内存层面的存储，不涉及到持久化的存储（.txt，.jpg，.avi，数据库）

## 2.数组存储的特点

一旦初始化以后，其长度就固定了。数组一旦定义好，其元素的类型也就确定了。我们也就只能操作指定类型的数据。

比如：`String[] arr；int[] arr1；Object[]  arr2`；

## 3.数组存储的缺点

1. 一旦数组初始化后，其长度就不可修改。
2. 数组中提供的方法非常有限，对于添加、删除、插入数据等操作，非常不便，同时效率不高。
3. 获取数组中实际元素的个数的需求，数组没有线程的属性或者方法可用。
4. **数组存储数据的特点：有序，可重复。**对于无序，不可重复的需求，不能满足

## 4.集合的有点

解决了数组存储数据方面的缺点。

## 5.集合的分类

Java集合分为Collection和Map两种体系

+ Collection接口：单列数据，定义了存取一组对象的方法的集合。它有很多的子接口，其中最大众的两个:Set接口和List接口
  + List接口：有序，可重复集合
  + Set接口：无序，不可重复集合
+ Map接口：双列数据，保持具有映射关系“key-value”对的集合

## 6.集合的框架体系

```
|----Collection接口:单列集合,用来存储一个一个的对象
	|----List接口:存储有序、可重复的数据。(又称为动态数组)
		|----ArrayList:是List接口的主要实现类;线程不安全，效率高;底层使用Object[] elementData
		|----LinkedList:对于频繁插入、删除操作效率要比ArrayList高;底层使用双向链表结构
		|----Vector:是List接口的古老实现类;线程安全,效率低;底层使用Object[] elementData
	|----Set接口:存储无序、不可重复的数据
		|----HashSet:作为Set接口的主要实现类;线程不安全,可以存储null值
			|----LinkedHashSet:是HashSet的子类，遍历其内部时，可以按照添加的顺序遍历
		|----TreeSet:可以按照添加对象的指定属性，进行排序
|----Map接口:双列集合,用来存储一对对"key-value"键值对
	|----HashMap:作为Map接口的主要类，线程不安全，可以存储null的key和value
		|----LinkedHashMap:是HashMap的子类，可以保证遍历数据的顺序与添加的顺序一致。
	|----TreeMap:可以进行排序操作的Map实现类
	|----HashTable:作为古老的Map接口的实现类，线程安全。不可以存储null的key和value
		|----Properties:常用来处理配置文件。key和value都是String类型。
	
```

# 二、Collection接口

+ Collection接口是List、Set和Queue接口的父接口，该接口定义的方法既可以用于操作Set集合，也可用于操作List和Queue集合
+ JDK不提供此接口的任何直接实现，而是提供更具体的子接口实现
+ 在JDK5.0之前，Java集合会丢失容器中所有对象的数据类型，把所有对象的数据类型都当成Object类来处理，从JDK5.0之后，新增了泛型，Java集合可以记住容器中对象的数据类型。

## 1.单列集合（Collection）框架结构

```
|----Collcetion:单列集合，用来存储一个一个的对象
	|----List:存储 有序、可重复的数据
		|----ArrayList:是List接口的主要实现类；线程不安全，效率高。底层使用Object[] elementData存储
		|----LinkedList:对于频繁的插入、删除操作效率要比ArrayList高;底层使用双向链表结构
		|----Vector:是List接口的古老实现类；线程安全，效率低。底层使用Object[] elementData存储
	|----Set:存储 无序的、不可重复的数据
		|----HashSet:作为Set接口的主要实现类;线程不安全;可以添加null值
			|----LinkedHashSet:是HashSet的子类，遍历其内部数据时，可以按照添加顺序遍历
		|----TreeSet:可以按照添加对象的指定属性,进行排序。
```

**图示**

![image-20200427090109218](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200427090111.png)

## 2.Collection接口的常用方法

1. 添加元素

   + add(Object obj):
   + addAll(Collection coll)

2. 获取有效元素的个数

   + size():

3. 清空元素

   + clear()

4. 判断集合是否为空

   + isEmpty()

5. 是否包含某个元素

   + boolean contains(Object obj)：是通过元素的**`equals`方法来判断是否包含的**。
   + boolean contains(Collection coll1)：**<span style='color:red;'>判断形参coll1中的所有元素是否都存在于当前集合中。</span>**

6. 删除元素

   + boolean remove(Object obj)：通过元素的equals()方法判断是否要删除的那个元素。只会删除找到的第一个元素。
   + boolean removeAll(Collcetion coll)：删除集合中的元素，做差集

   ```java
   //coll [AA, BB, 123, Wed Mar 31 23:35:09 CST 2021, DD, day03.Person@2530c12]
   Collection coll2 = Arrays.asList("AA", "BB", 123);
           //9.removeAll(Collection coll):
           coll.removeAll(coll2);
           System.out.println(coll);
   //[Wed Mar 31 23:35:09 CST 2021, DD, day03.Person@2530c12]
   ```

7. 取两个集合的交集

   + boolean retainAll(Collection coll)

8. 集合是否相等

   + boolean equals(Object obj)

9. 集合转换成对象数组

   + Object[] toArray()

10. 数组转成集合

    + Arrays.asList(T... a)
    
11. 遍历

    iterator():返回迭代器对象，用于遍历集合

**代码示例**

```java
@Test
public void test1() {
    Collection collection = new ArrayList();
    //1.add(Object e):将元素添加到集合中
    collection.add("ZZ");
    collection.add("AA");
    collection.add("BB");
    collection.add(123);
    collection.add(new Date());
    //2.size():获取添加元素的个数
    System.out.println(collection.size());//5
    //3.addAll(Collection coll1):将coll1集合中的元素添加到当前集合中
    Collection collection1 = new ArrayList();
    collection1.add("CC");
    collection1.add(213);
    collection.addAll(collection1);
    System.out.println(collection.size());//9
    //调用collection1中的toString()方法输出
    System.out.println(collection);//[ZZ, AA, BB, 123, Tue Apr 28 09:22:34 CST 2020, 213, 213]
    //4.clear():清空集合元素
    collection1.clear();
    System.out.println(collection1.size());//0
    System.out.println(collection1);//[]
    //5.isEmpty():判断当前集合是否为空
    System.out.println(collection1.isEmpty());//true
}

@Test
public void test2() {
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add(new Person("Tom", 23));
    coll.add(new Person("Jarry", 34));
    coll.add(false);
    //6.contains(Object obj):判断当前集合中是否包含obj
    //判断时需要调用obj对象所在类的equals()方法
    System.out.println(coll.contains(123));//true
    System.out.println(coll.contains(new Person("Tom", 23)));//true
    System.out.println(coll.contains(new Person("Jarry", 23)));//false
    //7.containsAll(Collection coll1):判断形参coll1中的元素是否都存在当前集合中
    Collection coll1 = Arrays.asList(123, 4566);
    System.out.println(coll.containsAll(coll1));//flase
    //8.remove(Object obj):从当前集合中移除obj元素
    coll.remove(123);
    System.out.println(coll);//[456, Person{name='Tom', age=23}, Person{name='Jarry', age=34}, false]
    //9.removeAll(Collection coll1):差集：从当前集合中和coll1中所有的元素
    Collection coll2 = Arrays.asList(123, 456, false);
    coll.removeAll(coll2);
    System.out.println(coll);//[Person{name='Tom', age=23}, Person{name='Jarry', age=34}]
}

@Test
public void test3() {
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add(new Person("Tom", 23));
    coll.add(new Person("Jarry", 34));
    coll.add(false);
    //10.retainAll(Collection coll1):交集：获取当前集合和coll1集合的交集，并返回给当前集合
    Collection coll1 = Arrays.asList(123, 345, 456);
    boolean b = coll.retainAll(coll1);
    System.out.println(b);//true
    System.out.println(coll);//[123, 456]
    //11.equals(Object obj):返回true需要当前集合和形参集合的元素相同
    Collection coll2 = new ArrayList();
    coll2.add(123);
    coll2.add(456);
    System.out.println(coll.equals(coll2));//true
    //12.hashCode():返回当前对象的哈希值
    System.out.println(coll.hashCode());//5230
    //13.集合--->数组:toArray()
    Object[] array = coll.toArray();
    for (Object obj : array) {
        System.out.println(obj);
    }
    //14.数组--->集合:调用Arrays类的静态方法asList()
    List<int[]> ints = Arrays.asList(new int[]{123, 345});
    System.out.println(ints.size());//1
    List<String> strings = Arrays.asList("AA", "BB", "CC");
    System.out.println(strings);//[AA, BB, CC]
    //15.iteratoriterator():返回Iterator接口的实例，用于遍历集合元素。
}
```

## 3.Collection集合与数组间的转换

1. Collection集合转换数组：`toArray()`
2. 数组转换Collection集合：`Arrays.asList(T...t)`

**代码示例**

```java
//集合 --->数组：toArray()
Object[] arr = coll.toArray();
for(int i = 0;i < arr.length;i++){
    System.out.println(arr[i]);
}

//拓展：数组 --->集合:调用Arrays类的静态方法asList(T ... t)
List<String> list = Arrays.asList(new String[]{"AA", "BB", "CC"});
System.out.println(list);

List arr1 = Arrays.asList(new int[]{123, 456});
System.out.println(arr1.size());//1

List arr2 = Arrays.asList(new Integer[]{123, 456});
System.out.println(arr2.size());//2
```

```
注意:使用Collection集合存储对象，要求对象所属的类满足：

向Collection接口的实现类的对象中添加数据Obj时，要求obj所在类要重写equals()
```

# 三、Iterator接口与foreach遍历集合

## 1.遍历Collection的两种方式：

①：使用迭代器Iterator；②：使用foreach遍历（增强for循环）

## 2.java.utils包下定义的迭代器接口：iterator

### 1.说明

Iterator对象成为迭代器（设计模式的一种），主要用于遍历Collection集合中的元素。GOF给迭代器模式的定义为：提供一种方法访问一个容器对象中各个元素，而又不需要暴露方法的内部细节。迭代器模式，就是为容器而生。

### 2.作用

主要作用就是遍历集合

### 3.如何获取实例

`集合对象.iterator()`返回一个迭代器实例

### 4.遍历的代码实现

```java
Iterator it = coll.iterator();
// hasNext()：判断是否有下一个元素
while(it.hasNext()){
    //next():①指针下移 ② 返回下移后的元素
    System.out.println(it.next());
}
```

### 5.迭代器的执行原理

![image-20200427150811299](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200427150812.png)

### 6.iterator接口中的remove()方法使用

+ 如果还未调用`next()`就调用`remove()`就会报IllegalStateException
+ 如果在上一次调用`next()`方法之后已经调用了`remove()`方法，再调用`remove`会报IllegalStateException。

**代码示例**

```java
@Test
public void test3(){
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add(new Person("Jerry",20));
    coll.add("Tom"
            );
    coll.add(false);

    //删除集合中"Tom"
    Iterator iterator = coll.iterator();
    while (iterator.hasNext()){
        //            iterator.remove();
        Object obj = iterator.next();
        if("Tom".equals(obj)){
            iterator.remove();
            //                iterator.remove();
        }

    }
    //将指针重新放到头部，遍历集合
    iterator = coll.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}

```

## 3.JDK5.0新特性——增强for循环(foreach循环)

### 1.遍历集合代码实现

```java
@Test
public void test1(){
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(456);
    coll.add(new Person("Jerry",20));
    coll.add(new String("Tom"));
    coll.add(false);

    //for(集合元素的类型 局部变量 : 集合对象)
    
    for(Object obj : coll){
        System.out.println(obj);
    }
}

增强for循环内部依然调用了迭代器
```



### 2.遍历数组代码实现

```java
@Test
public void test2(){
    int[] arr = new int[]{1,2,3,4,5,6};
    //for(数组元素的类型 局部变量 : 数组对象)
    for(int i : arr){
        System.out.println(i);
    }
}
```



# 四、Collection子接口：List接口

## 1.存储的数据特点：

存储是有序的、可重复的数据

+ 鉴于Java中数组用来存储数据的局限性，我们通常使用List替代数组
+ List集合类中元素有序、且可重复，集合中的每个元素都有其对应的顺序索引。
+ List容器中的元素都对应一个整数型的序号记载其在容器中的位置，可以根据序号存取容器中的元素。
+ JDK AP中List接口的实现类常用的有：ArrayList、LinkedList和 Vector.

## 2.List常用方法

List除了从Collection集合继承的方法外，List集合里还添加了一些关于索引的方法

+ **`void add(int index,Object ele)`**：<span style='color:red;'>**在index位置插入ele元素**</span>
+ `boolean addAll(int index,Collection colls)`：在index位置将colls集合中的所有元素添加进来
+ **`Object get(int index)`**：**<span style='color:red;'>获取指定index位置的元素</span>**
+ `int indexOf(Object obj)：`返回obj在集合中首次出现的位置
+ `int lastIndexOf(Object obj)：`返回obj在集合中最后出现的位置
+ **`Object remove(int index)`**：**<span style='color:red;'>移除指定index位置的元素，并返回此元素</span>**
+ **`Object set(int index,Object ele)`**：**<span style='color:red;'>将指定index位置的元素设置为ele</span>**
+ `List subList(int fromIndex,int endIndex)`：返回从fromIndex到toIndex**<span style='color:red;'>左闭右开</span>**的子集合

**总结**

+ 增：`add(Object obj)/addAll(int index,Collection coll)`
+ 删：`remove(int index)`
+ 改：`set(int index,Object ele)`
+ 查：`get(int index)`
+ 长度：`size()`

**代码示例**

```java
@Test
public void test2(){
    ArrayList list = new ArrayList();
    list.add(123);
    list.add(456);
    list.add("AA");
    list.add(new Person("Tom",12));
    list.add(456);
    //int indexOf(Object obj):返回obj在集合中首次出现的位置。如果不存在，返回-1.
    int index = list.indexOf(4567);
    System.out.println(index);

    //int lastIndexOf(Object obj):返回obj在当前集合中末次出现的位置。如果不存在，返回-1.
    System.out.println(list.lastIndexOf(456));

    //Object remove(int index):移除指定index位置的元素，并返回此元素
    Object obj = list.remove(0);
    System.out.println(obj);
    System.out.println(list);

    //Object set(int index, Object ele):设置指定index位置的元素为ele
    list.set(1,"CC");
    System.out.println(list);

    //List subList(int fromIndex, int toIndex):返回从fromIndex到toIndex位置的左闭右开区间的子集合
    List subList = list.subList(2, 4);
    System.out.println(subList);
    System.out.println(list);
}


@Test
public void test1(){
    ArrayList list = new ArrayList();
    list.add(123);
    list.add(456);
    list.add("AA");
    list.add(new Person("Tom",12));
    list.add(456);

    System.out.println(list);

    //void add(int index, Object ele):在index位置插入ele元素
    list.add(1,"BB");
    System.out.println(list);

    //boolean addAll(int index, Collection eles):从index位置开始将eles中的所有元素添加进来
    List list1 = Arrays.asList(1, 2, 3);
    list.addAll(list1);
    //        list.add(list1);
    System.out.println(list.size());//9

    //Object get(int index):获取指定index位置的元素
    System.out.println(list.get(0));

}
```



**小练习**

```java
@Test
public void testListRemove() {
    List list = new ArrayList();
    list.add(1);
    list.add(2);
    list.add(3);
    updateList(list);
    System.out.println(list);//
}
private static void updateList(List list) {
	list.remove(2);
}
```

**<span style='color:red;'>注意</span>**：

```
在集合中调用remove()时，要注意remove()方法的参数：如果参数是基本数据类型a,那么删除的是索引为a的元素，如果参数是Object类类型的a，那么是删除集合中元素为a的数据
```



## 3.常用实现类

```
|----Collection接口：单列集合，用来存储一个一个的对象
  |----List接口：存储序的、可重复的数据。  -->“动态”数组,替换原的数组
      |----ArrayList：作为List接口的主要实现类；线程不安全的,效率高;底层使用Object[] elementData存储
      |----LinkedList：对于频繁的插入、删除操作，使用此类效率比ArrayList高；底层使用双向链表存储
      |----Vector：作为List接口的古老实现类；线程安全的，效率低；底层使用Object[] elementData存储
```

### 1.ArrayList

+ ArrayList是List接口最主要的实现类。
+ ArrayList底层是使用的Object[] elementData数组
+ 通过无参构造器创建ArrayList对象时,JDK7.0与JDK8.0的区别？
  + JDK7.0:通过无参构造器创建ArrayList对象，类似于单例模式的饿汉式，直接创建一个初始容量为10的数组
  + JDK8.0:通过无参构造器创建ArrayList对象时，类似于单例模式的懒汉式，一开始并没有创建数组。

**代码示例**

```java
@Test
public void test1() {
    Collection coll = new ArrayList();
    coll.add(123);
    coll.add(345);
    coll.add(new User("Tom", 34));
    coll.add(new User("Tom"));
    coll.add(false);
    //iterator()遍历ArrayList集合
    Iterator iterator = coll.iterator();
    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }
}
```

### 2.LinkedHashMap

- 对与对于频繁的插入和删除元素操作，建议使用LinkedList类，效率更高
- 新增方法：
  - void addFirst(Object obj)
  - void addLast(Object obj)
  - Object getFirst()
  - Object getlast)()
  - Object removeFirst()
  - Object removeLast()
- Linkedlist：双向链表，内部没有声明数组，而是定义了Node类型的frst和last，用于记录首末元素。同时，定义内部类Node，作为 Linkedlist中保存数据的基本结构。Node除了保存数据，还定义了两个变量：
  - prev变量记录前一个元素的位置
  - next变量记录下一个元素的位置

![image-20200427154920004](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200427154921.png)

**代码示例**

```java
@Test
public void test3(){
    LinkedList linkedList = new LinkedList();
    linkedList.add(123);
    linkedList.add(345);
    linkedList.add(2342);
    linkedList.add("DDD");
    linkedList.add("AAA");
    
    Iterator iterator = linkedList.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}

```



## 4.源码分析(难点)

### 1.ArrayList的源码分析

+ **JDK7.0情况下**

  ```java
  ArrayList list = new ArrayList();//底层创建了长度固定为10的Object[] elementData
  list.add(123);//elementData[0] = new Integer(123);
  ...
  list.add(11);//如果此次的添加导致底层elementData数组容量不够，那么就会扩容
  ```

  	+ 默认情况下,**<span style='color:red;'>扩容为原来容量的1.5倍</span>**，同时需要**将原有数组中的数据复制到新的数组中**
  	+ 结论：建议开发中使用带参的构造器：`ArrayList list = new ArrayList(int capacity)`

+ **<span style='color:red;'>JDK8.0情况下</span>**

  ```java
  ArrayList list = new ArrayList();//底层Object[] elementData 初始化为{},并没有创建长度为10的数组
  list.add(123);//第一次调用add()时,底层才会创建长度为10的数组，并将数据123添加到elementData[0]
  ```

  **<span style='color:red;'>后续的添加和扩容操作与JDK7.0一致。</span>**

+ **总结**

  + **JDK7.0中**ArrayList的对象的创建类似于**单例的饿汉式**
  + **JDK8.0中**ArrayList的对象的创建类似于**单例的懒汉式**，延迟了数组的创建，节省空间

### 2.LinkedList的源码分析

```java
LinkedList list = new LinkedList();//内部声明了Node类型的first和last属性，默认值为null
list.add(123);//将123封装到Node中,创建了Node对象。

//期中，Node定义为：体现了LinkedList的双向链表的说法，因为有next和prev属性
private static class Node<E> {
    E item;
    Node<E> next;
    Node<E> prev;

    Node(Node<E> prev, E element, Node<E> next) {
        this.item = element;
        this.next = next;
        this.prev = prev;
    }
}
```

### 3.Vector的源码分析

+ Vector是一个古老的集合，JDK1.0就有了。大多数操作与ArrayList相同，区别在于Vector是线程安全的。
+ Vector是线程安全的，因此效率低
+ 在JDK7.0和JDK8.0中通过`Vector()构造器`创建对象时，底层都创建了**<span style='color:red;'>长度为10的数组。</span>**
+ 在**扩容方面**，默认扩容为原来容量的**<span style='color:red;'>2倍</span>**

## 5.Collection存储元素的要求

添加的对象所在的类要重写equals()方法

## 6.面试题： ArrayList/LinkedList/Vector的异同

请问 ArrayList/LinkedList/Vector的异同？谈谈你的理解？ArrayList底层是什么？扩容机制？ Vector和 ArrayList的最大区别？

- ArrayList和 Linkedlist的异同：

  二者都线程不安全，相比线程安全的 Vector，ArrayList执行效率高。 此外，ArrayList是实现了基于动态数组的数据结构，Linkedlist基于链表的数据结构。对于随机访问get和set，ArrayList觉得优于Linkedlist，因为Linkedlist要移动指针。对于新增和删除操作add（特指插入）和 remove，Linkedlist比较占优势，因为 ArrayList要移动数据。

- ArrayList和 Vector的区别：

  Vector和ArrayList几乎是完全相同的，唯一的区别在于Vector是同步类(synchronized)，属于强同步类。因此开销就比 ArrayList要大，访问要慢。正常情况下，大多数的Java程序员使用ArrayList而不是Vector，因为同步完全可以由程序员自己来控制。Vector每次扩容请求其大小的2倍空间，而ArrayList是1.5倍。Vector还有一个子类Stack.

# 五、Collection子接口：Set接口

+ Set接口是Collection的子接口，**Set接口没有提供额外的方法**
+ Set接口不允许包含相同的元素，如果有多个相同的元素，只会保存一个。
+ Set接口判断两个对象是否相同，不是根据==运算符，而是根据`equals()`方法

## 1.存储数据的特点

**Set接口**存储数据的特点是：**<span style='color:red;'>无序的、不可重复的。</span>**

+ **无序的：**不等于随机性。存储的数据在底层数组中并非是按照数组索引的顺序添加，而是根据数据的**<span style='color:red;'>哈希值</span>**来决定的。
+ **不可重复的：**<span style='color:red;'>相同元素只能存储一个。</span>保证添加的元素按照equals()判断时，不能返回true

## <span style='color:red;'>2.元素添加的过程（以HashSet为例）</span>

我们向HashSet中添加元素A，首先调用元素A所在类的hashCode()方法，计算元素A的哈希值。此哈希值接着通过某种算法计算出在HashSet底层的数组中的存放位置（即索引位置）。

判断数组此位置上是否已经有元素：

 如果此位置上没有其他元素,则元素A添加成功 		--->  情况1

如果此位置上有其他元素B（或以链表形式存在的多个元素）,则比较元素A和元素B的哈希值：

​	如果哈希值不相同，则元素A添加成功				 ----> 情况2

​	如果哈希值相同，进而需要调用元素a所在类的equals()方法：

​			如果equals()返回true，元素A添加失败

​			如果equals()返回false，元素A添加成功 	----> 情况3

对于添加成功的情况2和情况3而言：元素A与已经存在指定索引位置上数据以链表的方式存储。

JDK7.0：新的元素A添加到数组中，指向原来数组中的元素。

JDK8.0：原来的元素在数组中，指向元素a

==**<span style='color:red;'>总结：七上八下（针对新元素而言）</span>**==

==**<span style='color:red;'>HashSet底层采用数组+链表的形式</span>**==

![image-20200427162158726](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200427162200.png)



## 3.Set常用方法

Set接口中没有额外定义新的方法，使用的都是Collection中声明过的方法

### 1.重写hashCode()方法

+ 在程序运行时，同一个对象多次调用hashCode()方法应该返回相同的值。
+ **当两个对象的equals()方法比较返回true时,这两个对象的hashCode()方法的返回值也应相同。**
+ 对象中用作equals()方法比较的Field,都应该用来计算hashCode值。

### 2.重写equals()方法基本原则

+ 以自定义的Customer类为例，何时需要重写equals()？
+ 当一个类有自己特有的"逻辑相等"概念，当改写equals()的时候，总是要改写hashCode()，根据一个类的equals方法(改写后)，两个截然不同的实例有可能在逻辑上是相等的，但是，根据Object.hashCode()方法，它们仅仅是两个对象。
+ 因此，违反了**“相等的对象必须具有相等的散列码”**
+ **<span style='color:red;'>结论：复写equals()方法的时候一般都需要同时复写hashCode()方法。通常参与计算hashCode的对象的属性也应该参与到equals()中进行计算。</span>**

## 4.Set接口常用实现类

```java
 |----Collection接口：单列集合，用来存储一个一个的对象
      |----Set接口：存储无序的、不可重复的数据   -->高中讲的“集合”
           |----HashSet：作为Set接口的主要实现类；线程不安全的；可以存储null值
                |----LinkedHashSet：作为HashSet的子类；遍历其内部数据时，可以按照添加的顺序遍历，对于频繁的遍历操作，LinkedHashSet效率高于HashSet.
           |----TreeSet：可以按照添加对象的指定属性，进行排序。
```

### 1.HashSet实现类

+ HashSet是Set接口的典型实现，大多数时候使用Set集合时都使用这个实现类。
+ HashSet按Hash算法来存储集合中的元素，因此具有很好的存取、查找、删除性能。
+ **HashSet**具有以下**特点：**
  + **<span style='color:red;'>不能保证元素的排列顺序</span>**
  + **<span style='color:red;'>线程不安全</span>**
  + **<span style='color:red;'>可以存储null值。</span>**
+ HashSet集合判断两个元素相等的标准：两个对象通过hashCode()方法比较相等，并且还要比较两个对象的equals()也相等。
+ 对于存放在Set容器中的对象，对应的类一定要重写equals()和hashCode(Object obj)方法，以实现对象相等规则。即：“相等的对象必须具有相等的散列码

**代码示例**

```java
@Test
//HashSet使用
public void test1(){
    Set set = new HashSet();
    set.add(454);
    set.add(213);
    set.add(111);
    set.add(123);
    set.add(23);
    set.add("AAA");
    set.add("EEE");
    set.add(new User("Tom",34));
    set.add(new User("Jarry",74));

    Iterator iterator = set.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

### 2.LinkedHashSet

+ LinkedHashSet是HashSet的子类
+ LinkedHashSet根据元素的hashCode值来决定元素的存储位置但它同时使用双向链表维护元素的次序，这使得元素看起来是以插入顺序保存的。
+ LinkedHashSet不允许集合元素重复

**图示：**

![image-20200427213038605](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200427213040.png)

**代码示例**

```java
@Test
//LinkedHashSet使用
public void test2(){
    Set set = new LinkedHashSet();
    set.add(454);
    set.add(213);
    set.add(111);
    set.add(123);
    set.add(23);
    set.add("AAA");
    set.add("EEE");
    set.add(new User("Tom",34));
    set.add(new User("Jarry",74));

    Iterator iterator = set.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

### 3.TreeSet

+ TreeSet是SortedSet接口的实现类，TreeSet集合中元素可以排序
+ TreeSet底层使用**红黑树**结构存储数据
+ 新增的方法如下：
  + Comparator comparator()
  + Object first()
  + Object last()
  + Object lower(Object obj)
  + Object higer(Object obj)
  + SortedSet subSet(fromElement,toElement)
  + SortedSet headSet(toElement)
+ TreeSet两种排序方法：自然排序和定制排序。默认情况下,TreeSet采用自然排序

**红黑树图示：**

![image-20200427213936119](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200427213937.png)

红黑树的特点：有序，查询效率比List快

红黑树介绍：[https://www.cnblogs.com/LiaHon/p/11203229.html](https://www.oschina.net/action/GoToLink?url=https%3A%2F%2Fwww.cnblogs.com%2FLiaHon%2Fp%2F11203229.html)



**代码示例**

```java
@Test
public void test1(){
    Set treeSet = new TreeSet();
    treeSet.add(new User("Tom",34));
    treeSet.add(new User("Jarry",23));
    treeSet.add(new User("mars",38));
    treeSet.add(new User("Jane",56));
    treeSet.add(new User("Jane",60));
    treeSet.add(new User("Bruce",58));

    Iterator iterator = treeSet.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```

## 5.存储对象所在类的要求

### 1.HashSet/LinkedHashSet

- 要求：向Set(主要指：HashSet、LinkedHashSet)中添加的数据，其所在的类一定要重写hashCode()和equals()
- 要求：重写的hashCode()和equals()尽可能保持一致性：相等的对象必须具有相等的散列码

```
重写两个方法的小技巧：对象中用作 equals() 方法比较的 Field，都应该用来计算 hashCode 值。
```

### 2.TreeSet

1. 自然排序中，比较两个对象是否相同的标准为：compareTo()返回0.不再是equals().
2. 定制排序中，比较两个对象是否相同的标准为：compare()返回0.不再是equals().

## 6.TreeSet的使用

### 1.使用说明

1. 向TreeSet中添加的数据，要求是相同类的对象。
2. 两种排序方式：自然排序（实现Comparable接口 和 定制排序（Comparator）

### 2.常用的排序方式

方式一：自然排序

- 自然排序：TreeSet会调用集合元素的compareTo(object obj)方法来比较元素之间的大小关系，然后将集合元素按升序（默认情况）排列
- 如果试图把一个对象添加到Treeset时，则该对象的类必须实现Comparable接口。
  - 实现Comparable的类必须实现compareTo(Object obj)方法，两个对象即通过compareTo(Object obj)方法的返回值来比较大小
- Comparable的典型实现:
  - BigDecimal、BigInteger以及所有的数值型对应的包装类：按它们对应的数值大小进行比较
  - Character：按字符的unic！ode值来进行比较
  - Boolean：true对应的包装类实例大于fase对应的包装类实例
  - String：按字符串中字符的unicode值进行比较
  - Date、Time：后边的时间、日期比前面的时间、日期大
- 向TreeSet中添加元素时，只有第一个元素无须比较compareTo()方法，后面添加的所有元素都会调用compareTo()方法进行比较。
- 因为只有相同类的两个实例才会比较大小，所以向 TreeSet中添加的应该是同一个类的对象。 对于TreeSet集合而言，它判断两个对象是否相等的唯一标准是：两个对象通过compareTo(Object obj)方法比较返回值。
- 当需要把一个对象放入TreeSet中，重写该对象对应的equals()方法时，应保证该方法与compareTo(Object obj)方法有一致的结果：如果两个对象通过equals()方法比较返回true，则通过compareTo(object ob)方法比较应返回0。否则，让人难以理解。

```java
@Test
public void test1(){
    TreeSet set = new TreeSet();

    //失败：不能添加不同类的对象
    //        set.add(123);
    //        set.add(456);
    //        set.add("AA");
    //        set.add(new User("Tom",12));

    //举例一：
    //        set.add(34);
    //        set.add(-34);
    //        set.add(43);
    //        set.add(11);
    //        set.add(8);

    //举例二：
    set.add(new User("Tom",12));
    set.add(new User("Jerry",32));
    set.add(new User("Jim",2));
    set.add(new User("Mike",65));
    set.add(new User("Jack",33));
    set.add(new User("Jack",56));


    Iterator iterator = set.iterator();
    while(iterator.hasNext()){
        System.out.println(iterator.next());
    }

}

```

方式二：定制排序

- TreeSet的自然排序要求元素所属的类实现Comparable接口，如果元素所属的类没有实现 Comparable接口，或不希望按照升序（默认情况）的方式排列元素或希望按照其它属性大小进行排序，则考虑使用定制排序。定制排序，通过 Comparator接口来实现。需要重写 compare(T o1，T o2)方法。
- 利用int compare(T o1，T o2)方法，比较o1和o2的大小：如果方法返回正整数，则表示o1大于o2；如果返回0，表示相等；返回负整数，表示o1小于o2。
- 要实现定制排序，需要将实现Comparator接口的实例作为形参传递给TreeSet的构造器。
- 此时，仍然只能向Treeset中添加类型相同的对象。否则发生 ClassCastException异常
- 使用定制排序判断两个元素相等的标准是：通过 Comparator比较两个元素返回了0

```java
@Test
public void test2(){
    Comparator com = new Comparator() {
        //照年龄从小到大排列
        @Override
        public int compare(Object o1, Object o2) {
            if(o1 instanceof User && o2 instanceof User){
                User u1 = (User)o1;
                User u2 = (User)o2;
                return Integer.compare(u1.getAge(),u2.getAge());
            }else{
                throw new RuntimeException("输入的数据类型不匹配");
            }
        }
    };

    TreeSet set = new TreeSet(com);
    set.add(new User("Tom",12));
    set.add(new User("Jerry",32));
    set.add(new User("Jim",2));
    set.add(new User("Mike",65));
    set.add(new User("Mary",33));
    set.add(new User("Jack",33));
    set.add(new User("Jack",56));

    Iterator iterator = set.iterator();
    while(iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```



# 六、Map接口

## 1.常见实现类结构

![image-20200429112012230](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200429112013.png)

```
|----Map：双列数据,存储key-value键值对。 ----类似于高中的函数：y=f(x)
	|----HashMap:是Map接口的主要实现类,线程不安全,效率高。可以存储null的key或者value
		|----LinkedHashMap:是HashMap的一个子类,保证在遍历map元素时,可以按照添加时的顺序遍历
		原因：在原有的HashMap底层结构基础上,添加了一对指针,指向前一个和后一个元素。
		对于频繁的遍历操作,此类执行效率高于HashMap。
	|----TreeMap:可以实现对添加的key-value进行排序,底层使用 红黑树
	|----Hashtable:是Map接口的古老实现类,线程安全,效率低。不可以存储null的key和value
		|----Properties:常用来处理配置文件。key和value都是String类型的。
		
HashMap底层： 数组 + 链表 (jdk7.0以及之前)
			 数组 + 链表 + 红黑树 (jdk8.0以后)
```

### 1.HashMap

+ Map中的key是无序的、不可重复的，使用**Set**存储所有的key ---> key所在的类需要重写`hashCode()和equals()`方法
+ Map中的value是无序的、可以重复的，使用Collection存储所有的value  --->value所在的类需要重写`equals()`
+ **<span style='color:red;'>一个键值对：key-value构成了一个Entry对象</span>**
+ Map中的entry：无序的、不可重复的 ，使用**Set**存储所有的entry
+ **<span style='color:red;'>允许存储null的key和value</span>**
+ HashMap判断两个key是否相等：首先通过`hashCode()`值进行比较，在`hashCode()`值相等的情况下,在去通过`equals()`比较
+ HashMap判断两个value是否相等：两个value通过`equals()`方法返回true。

#### 1.HashMap在JDK7.0中底层实现原理

+ 首先`HashMap map  = new HashMap()`在实例化以后**,<span style='color:red;'>底层创建了一个长度为16的Entry[] table数组</span>**
+ 在通过`map.put(key1,value1)`的方式添加元素，具体实现如下
  + 首先通过`key1`所在类的`hashCode()`方法计算key1的哈希值，此哈希值经过某种算法计算以后，得到在Entry数组中存储的位置；
  + 如果此位置上的数据为空,此时的key1-value1添加成功。   ---- 情况1
  + 如果此位置上的数据不为空，（意味着此位置上存在一个或者多个数据(以链表的形式存在)），那么在去比较key1和此位置上的一个或者多个数据的哈希值：
    + 如果key1的哈希值与已经存在该位置上的数据的哈希值不相等，此时key1-value1添加成功 		---- 情况2
    + 如果key1的哈希值与该位置上的某一个数据的哈希值相等，再调用key1的`equals(key2)`
      + 如果equals()返回false：此时key1-value1添加成功  ---- 情况3
      + 如果equals()返回true：使用value2替换value1
+ 补充：关于情况2和情况3：此时的**key1-value1和原来的数据<span style='color:red;'>以链表的方式存储</span>**。
+ 在不断的添加过程中，会涉及到扩容问题，**默认的扩容方式：<span style='color:red;'>扩容为原来容量的2倍</span>，并将原来的数据复制过来**



#### 2.HashMap在JDK8.0中底层实现原理

jdk8 相较于jdk7在底层实现方面的不同：

1. `new HashMap():`底层没有创建一个长度为16的数组

2. jdk8 底层的数组是:Node[]，而非Entry[]

3. 首次调用put()方法时，底层创建长度为16的数组。

4. jdk7 底层结构只有：数组+链表。 jdk8中底层结构：数组+链表+红黑树

   当数组的某一个索引位置上的元素以链表形式存在的个数 > 8 且 当前数组的长度 > 64时，

   此时此索引位置上的所有数据改为使用红黑树存储。

#### 3.HashMap底层典型属性的说明

1. DEFAULT_INITAL_CAPACITY：HashMap的默认容：16
2. DEFAULT_LOAD_FACTOR：HashMap的默认加载因子：0.75
3. threshold：扩容的临界值 =  容量 * 加载因子 ，HashMap中的有效数据大于临界值时，就需要扩容了
4. TREEIFY_THRESHOLD：Bucket中链表长度大于该默认值8，转化为红黑树
5. MIN_TREEIFY_CAPACITY：Bucket（桶中）的Node被树化时最小的hash表容量：64



### 2.LinkHashMap

+ LinkedHashMap底层使用的结构与HashMap相同，因为LinkedHashMap继承于HashMap
+ **<span style='color:red;'>LinkedHashMap可以保证在遍历元素时的顺序和添加元素时的顺序一致。</span>**

**源码示例**

```java
static class Entry<K,V> extends HashMap.Node<K,V> {
    Entry<K,V> before, after;//能够记录添加的元素的先后顺序
    Entry(int hash, K key, V value, Node<K,V> next) {
        super(hash, key, value, next);
    }
}
```



### 3.TreeMap

+ TreeMap存储key-value对时，需要根据key-value对进行排序。
+ TreeMap的Key的排序:
  - 自然排序： TreeMap的所有的Key必须实现Comparable接口，而且所有的Key应该是同一个类的对象，否则将会抛出ClasssCastEXception()
  - 定制排序：创建 TreeMap时，传入一个 Comparator对象，该对象负责对TreeMap中的所有key进行排序。此时不需要Map的Key实现Comparable接口
+ TreeMap判断两个key相等的标准：两个key通过 compareTo()方法或者compare()方法返回

### 6.Hashtable

- Hashtable是个古老的Map实现类，JDK1.0就提供了。不同于 HashMap，Hashtable是线程安全的.
- Hashtable实现原理和HashMap相同，功能相同。底层都使用哈希表结构，查询速度快，很多情况下可以互用
- 与HashMap.不同，Hashtable不允许使用null作为key和value.
- 与HashMap一样，Hashtable也不能保证其中Key-value对的顺序.
- Hashtable判断两个key相等、两个value相等的标准，与HashMap-致.

### 5.Properties——HashTable的一个子类

**主要作用**：①用来读取配置文件 ②**<span style='color:red;'>该类的key-value都是String类型的</span>**

**使用Properties读取配文件代码示例：**

```java
//Properties:常用来处理配置文件。key和value都是String类型
public static void main(String[] args)  {
    FileInputStream fis = null;
    try {
        Properties pros = new Properties();

        fis = new FileInputStream("jdbc.properties");
        pros.load(fis);//加载流对应的文件

        String name = pros.getProperty("name");
        String password = pros.getProperty("password");

        System.out.println("name = " + name + ", password = " + password);
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if(fis != null){
            try {
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }
    }

}

```



## 2.Map的常用方法

### 1.添加、删除、修改操作

+ Object put(Object key,Object value)：将指定key-value添加到map集合中
+ void putAll(Map m)：将m中的所有key-value添加到当前map集合中。
+ Object remove(Object key)：移除指定key的key-value,返回value。
+ void clear()：清空当前集合中的所有key-value数据

**代码示例**

```java
@Test
public void test1() {
    Map map = new HashMap();
    //Object put(Object key,Object value)：将指定key-value添加到(或修改)当前map对象中
    map.put("AA",123);
    map.put("ZZ",251);
    map.put("CC",110);
    map.put("RR",124);
    map.put("FF",662);
    System.out.println(map);//{AA=123, ZZ=251, CC=110, RR=124, FF=662}

    //Object put(Object key,Object value)：将指定key-value添加到(或修改)当前map对象中
    map.put("ZZ",261);
    System.out.println(map);//{AA=123, ZZ=261, CC=110, RR=124, FF=662}

    //void putAll(Map m):将m中的所有key-value对存放到当前map中
    HashMap map1 = new HashMap();
    map1.put("GG",435);
    map1.put("DD",156);
    map.putAll(map1);
    System.out.println(map);//{AA=123, ZZ=261, CC=110, RR=124, FF=662, GG=435, DD=156}

    //Object remove(Object key)：移除指定key的key-value对，并返回value
    Object value = map.remove("GG");
    System.out.println(value);//435
    System.out.println(map);//{AA=123, ZZ=261, CC=110, RR=124, FF=662, DD=156}

    //void clear()：清空当前map中的所有数据
    map.clear();
    System.out.println(map.size());//0  与map = null操作不同
    System.out.println(map);//{}
}


```



### 2.元素查询操作

+ Object get(Object key)：获取指定key的value值
+ boolean containsKey(Object key)：是否包含指定的key
+ boolean containsValue(Object value)：是否包含指定的value
+ int size()：返回当前map中key-value对的个数
+ boolean isEmpty()：当前map是否为空
+ boolean equals(Object obj)：当前map是否和obj参数对象相等

**代码示例**

```java
@Test
public void test2() {
    Map map = new HashMap();
    map.put("AA", 123);
    map.put("ZZ", 251);
    map.put("CC", 110);
    map.put("RR", 124);
    map.put("FF", 662);
    System.out.println(map);//{AA=123, ZZ=251, CC=110, RR=124, FF=662}
    //Object get(Object key)：获取指定key对应的value
    System.out.println(map.get("AA"));//123

    //boolean containsKey(Object key)：是否包含指定的key
    System.out.println(map.containsKey("ZZ"));//true

    //boolean containsValue(Object value)：是否包含指定的value
    System.out.println(map.containsValue(123));//true

    //int size()：返回map中key-value对的个数
    System.out.println(map.size());//5

    //boolean isEmpty()：判断当前map是否为空
    System.out.println(map.isEmpty());//false

    //boolean equals(Object obj)：判断当前map和参数对象obj是否相等
    Map map1 = new HashMap();
    map1.put("AA", 123);
    map1.put("ZZ", 251);
    map1.put("CC", 110);
    map1.put("RR", 124);
    map1.put("FF", 662);
    System.out.println(map.equals(map1));//true
}
```



### 3.元视图操作

+ Set keySet()：返回所有key构成的Set集合
+ Collection values()：返回所有value构成的Collection集合
+ Set entrySet()：返回所有key-value对构成的集合

**代码示例**

```java
@Test
public void test3() {
    Map map = new HashMap();
    map.put("AA", 123);
    map.put("ZZ", 251);
    map.put("CC", 110);
    map.put("RR", 124);
    map.put("FF", 662);
    System.out.println(map);//{AA=123, ZZ=251, CC=110, RR=124, FF=662}
    //遍历所有的key集:Set keySet()：返回所有key构成的Set集合
    Set set = map.keySet();
    Iterator iterator = set.iterator();
    while (iterator.hasNext()) {
        System.out.println(iterator.next());
    }
    System.out.println("--------------");
    //遍历所有的value集：Collection values()：返回所有value构成的Collection集合
    Collection values = map.values();
    for (Object obj :
         values) {
        System.out.println(obj);
    }
    System.out.println("---------------");
    //Set entrySet()：返回所有key-value对构成的Set集合
    Set entrySet = map.entrySet();
    Iterator iterator1 = entrySet.iterator();
    //方式一：
    while (iterator1.hasNext()) {
        Object obj = iterator1.next();
        //entrySet集合中的元素都是entry
        Map.Entry entry = (Map.Entry) obj;
        System.out.println(entry.getKey() + "-->" + entry.getValue());
    }
    System.out.println("--------------");

    //方式二：
    Set keySet = map.keySet();
    Iterator iterator2 = keySet.iterator();
    while (iterator2.hasNext()) {
        Object key = iterator2.next();
        Object value = map.get(key);
        System.out.println(key + "==" + value);
    }
}
```

### 4.常用方法总结

- 添加：put(Object key,Object value)
- 删除：remove(Object key)
- 修改：put(Object key,Object value)
- 查询：get(Object key)
- 长度：size()
- 遍历：keySet() / values() / entrySet()

## 3.内存结构说明：（难点）

### 1.HashMap在JDK 7.0中实现原理

JDK 7.0及以前的版本：HashMap是数组+链表结构（地址链表法）

JDK 8.0版本以后：HashMap是数组+链表+红黑树实现

![image-20200429140534825](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200429140536.png)

#### 2. 对象创建和添加过程

HashMap map = new HashMap():

在实例化以后，底层创建了长度是16的一维数组Entry[] table。

map.put(key1,value1):

- 首先，调用key1所在类的hashCode()计算key1哈希值，此哈希值经过某种算法计算以后，得到在Entry数组中的存放位置。
- 如果此位置上的数据为空，此时的key1-value1添加成功。 ----情况1
- 如果此位置上的数据不为空，(意味着此位置上存在一个或多个数据(以链表形式存在)),比较key1和已经存在的一个或多个数据的哈希值：
  - 如果key1的哈希值与已经存在的数据的哈希值都不相同，此时key1-value1添加成功。----情况2
  - 如果key1的哈希值和已经存在的某一个数据(key2-value2)的哈希值相同，继续比较：调用key1所在类的equals(key2)方法，比较：
    - 如果equals()返回false:此时key1-value1添加成功。----情况3
    - 如果equals()返回true:使用value1替换value2。

```
补充：关于情况2和情况3：此时key1-value1和原来的数据以链表的方式存储。
```

在不断的添加过程中，会涉及到扩容问题，当超出临界值(且要存放的位置非空)时，扩容。默认的扩容方式：扩容为原来容量的2倍，并将原有的数据复制过来。

#### 3.HashMap的扩容

当HashMap中的元素越来越多的时候，hash冲突的几率也就越来越高，因为数组的长度是固定的。所以为了提高查询的效率，就要对 HashMap的数组进行扩容，而在HashMap数组扩容之后，原数组中的数据必须重新计算其在新数组中的位置，并放进去，这就是 resize。

#### 4. HashMap扩容时机

当HashMap中的元素个数超过数组大小（数组总大小 length，不是数组中个数）* loadFactor时，就会进行数组扩容，loadFactor的默认值(DEFAULT_LOAD_ FACTOR)为0.75，这是一个折中的取值。也就是说，默认情况下，数组大小(DEFAULT INITIAL CAPACITY)为16，那么当 HashMap中元素个数超过16 * 0.75=12（这个值就是代码中的 threshold值，也叫做临界值）的时候，就把数组的大小扩展为2 * 16=32，即扩大一倍，然后重新计算每个元素在数组中的位置，而这是一个非常消耗性能的操作，所以如果我们已经预知 HashMap中元素的个数，那么预设元素的个数能够有效的提高HashMap的性能。

### 2.HashMap在JDK 8.0底层实现原理

HashMap的内部存储结构其实是数组+链表+红黑树的组合。

![image-20200429140606819](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200429140608.png)

#### 1.HashMap添加元素的过程

当实例化一个HashMap时，会初始化 initialCapacity和loadFactor，在put第一对映射关系时，系统会创建一个长度为 initialCapacity的Node数组，这个长度在哈希表中被称为容量（Capacity），在这个数组中可以存放元素的位置我们称之为“桶”（ bucket），每个bucket都有自己的索引，系统可以根据索引快速的查找bucket中的元素。

每个 bucket中存储一个元素，即一个Node对象，但每一个Noe对象可以带个引用变量next，用于指向下一个元素，因此，在一个桶中，就有可能生成一个Node链。也可能是一个一个 TreeNode对象，每一个Tree node对象可以有两个叶子结点left和right，因此，在一个桶中，就有可能生成一个TreeNode树。而新添加的元素作为链表的last，或树的叶子结点。

#### 2.HashMap的扩容机制

- 当HashMapl中的其中一个链的对象个数没有达到8个和JDK 7.0以前的扩容方式一样。
- 当HashMapl中的其中一个链的对象个数如果达到了8个，此时如果 capacity没有达到64，那么HashMap会先扩容解决，如果已经达到了64，那么这个链会变成树，结点类型由Node变成 Tree Node类型。当然，如果当映射关系被移除后，下次resize方法时判断树的结点个数低于6个，也会把树再转为链表。

#### 3.JDK 8.0与JDK 7.0中HashMap底层的变化

1. new HashMap():底层没有创建一个长度为16的数组
2. JDK 8.0底层的数组是：Node[],而非Entry[]
3. 首次调用put()方法时，底层创建长度为16的数组
4. JDK 7.0底层结构只有：数组+链表。JDK 8.0中底层结构：数组+链表+红黑树。
   - 形成链表时，七上八下（jdk7:新的元素指向旧的元素。jdk8：旧的元素指向新的元素）
   - 当数组的某一个索引位置上的元素以链表形式存在的数据个数 > 8 且当前数组的长度 > 64时，此时此索引位置上的所数据改为使用红黑树存储。



### 3.HashMap底层典型属性的属性的说明

- DEFAULT_INITIAL_CAPACITY : HashMap的默认容量，16
- DEFAULT_LOAD_FACTOR：HashMap的默认加载因子：0.75
- threshold：扩容的临界值，= 容量*填充因子：16 * 0.75 => 12
- TREEIFY_THRESHOLD：Bucket中链表长度大于该默认值，转化为红黑树:JDK 8.0引入
- MIN_TREEIFY_CAPACITY：桶中的Node被树化时最小的hash表容量:64

### 4.LinkedHashMap的底层实现原理

- LinkedHashMap底层使用的结构与HashMap相同，因为LinkedHashMap继承于HashMap.
- 区别就在于：LinkedHashMap内部提供了Entry，替换HashMap中的Node.
- 与Linkedhash Set类似，LinkedHashMap可以维护Map的迭代顺序：迭代顺序与Key-value对的插入顺序一致

**HashMap中内部类Node源码**

```java
static class Node<K,V> implements Map.Entry<K,V>{
    final int hash;
    final K key;
    V value;
    Node<K,V> next;
}
```

**LinkedHashM中内部类Entry源码：**

```java
static class Entry<K,V> extends HashMap.Node<K,V> {
    Entry<K,V> before, after;//能够记录添加的元素的先后顺序
    Entry(int hash, K key, V value, Node<K,V> next) {
        super(hash, key, value, next);
    }
}
```

### 5.TreeMap

向TreeMap中添加key-value，要求key必须是由同一个类创建的对象 要照key进行排序：自然排序 、定制排序

**代码示例**

```java
//自然排序
@Test
public void test() {
    TreeMap map = new TreeMap();
    User u1 = new User("Tom", 23);
    User u2 = new User("Jarry", 18);
    User u3 = new User("Bruce", 56);
    User u4 = new User("Davie", 23);

    map.put(u1, 98);
    map.put(u2, 16);
    map.put(u3, 92);
    map.put(u4, 100);

    Set entrySet = map.entrySet();
    Iterator iterator = entrySet.iterator();
    while (iterator.hasNext()) {
        Object obj = iterator.next();
        Map.Entry entry = (Map.Entry) obj;
        System.out.println(entry.getKey() + "=" + entry.getValue());
    }
}

//定制排序：按照年龄大小排
@Test
public void test2() {
    TreeMap map = new TreeMap(new Comparator() {
        @Override
        public int compare(Object o1, Object o2) {
            if (o1 instanceof User && o2 instanceof User) {
                User u1 = (User) o1;
                User u2 = (User) o2;
                return Integer.compare(u1.getAge(), u2.getAge());
            }
            throw new RuntimeException("输入数据类型错误");
        }
    });
    User u1 = new User("Tom", 23);
    User u2 = new User("Jarry", 18);
    User u3 = new User("Bruce", 56);
    User u4 = new User("Davie", 23);

    map.put(u1, 98);
    map.put(u2, 16);
    map.put(u3, 92);
    map.put(u4, 100);

    Set entrySet = map.entrySet();
    Iterator iterator = entrySet.iterator();
    while (iterator.hasNext()) {
        Object obj = iterator.next();
        Map.Entry entry = (Map.Entry) obj;
        System.out.println(entry.getKey() + "=" + entry.getValue());
    }
}

```

### 6.使用Properties读取配置文件

```java
//Properties:常用来处理配置文件。key和value都是String类型
public static void main(String[] args)  {
    FileInputStream fis = null;
    try {
        Properties pros = new Properties();

        fis = new FileInputStream("jdbc.properties");
        pros.load(fis);//加载流对应的文件

        String name = pros.getProperty("name");
        String password = pros.getProperty("password");

        System.out.println("name = " + name + ", password = " + password);
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if(fis != null){
            try {
                fis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }

        }
    }

}
```

### 7.面试题

1. HashMap的底层实现原理？
2. HashMap 和 Hashtable的异同？
3. CurrentHashMap 与 Hashtable的异同？
4. 负载因子值的大小，对HashMap的影响？
   - 负载因子的大小决定了HashMap的数据密度。
   - 负载因子越大密度越大，发生碰撞的几率越高，数组中的链表越容易长，造成査询或插入时的比较次数增多，性能会下降
   - 负载因子越小，就越容易触发扩容，数据密度也越小，意味着发生碰撞的几率越小，数组中的链表也就越短，查询和插入时比较的次数也越小，性能会更高。但是会浪费一定的内容空间。而且经常扩容也会影响性能，建议初始化预设大一点的空间
   - 按照其他语言的参考及研究经验，会考虑将负载因子设置为0.7~0.75，此时平均检索长度接近于常数。

# 七、Collections工具类的使用

## 1.作用

Collections是一个操作Set、List和Map等集合的工具类

Collections中提供了一系列静态的方法对集合元素进行排序、查询和修改等操作，还提供了对集合对象设置不可变、对集合对象实现同步控制等方法

## 2.常用方法

### 1.排序操作

+ void reverse(List)：反转List中元素的顺序
+ shuffle(List)：对List集合元素进行随机排序
+ sort(List)：根据元素的自然顺序对指定List集合元素升序排序
+ sort(List,Comparator)：根据指定的Comparator产生的顺序对List集合元素进行排序
+ swap(List ，int，int)：将指定list集合中的i处元素和j处元素进行交换

**代码示例**

```java
@Test
public void test1() {
    List list = new ArrayList();
    list.add(123);
    list.add(43);
    list.add(765);
    list.add(-97);
    list.add(0);
    System.out.println(list);//[123, 43, 765, -97, 0]

    //reverse(List)：反转 List 中元素的顺序
    Collections.reverse(list);
    System.out.println(list);//[0, -97, 765, 43, 123]

    //shuffle(List)：对 List 集合元素进行随机排序
    Collections.shuffle(list);
    System.out.println(list);//[765, -97, 123, 0, 43]

    //sort(List)：根据元素的自然顺序对指定 List 集合元素按升序排序
    Collections.sort(list);
    System.out.println(list);//[-97, 0, 43, 123, 765]

    //swap(List，int， int)：将指定 list 集合中的 i 处元素和 j 处元素进行交换
    Collections.swap(list,1,4);
    System.out.println(list);//[-97, 765, 43, 123, 0]
}

```



### 2.查找、替换操作

+ Object max(Collection)：根据元素的自然顺序、返回给定集合中的最大元素
+ Object max(Collection，Comparator)：根据 Comparator 指定的顺序，返回给定集合中的最大元素
+ Object min(Collection)
+ Object min(Collection，Comparator)
+ int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
+ void copy(List dest,List src)：将src中的内容复制到dest中
+ boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所旧值

**代码示例**

```java
@Test
public void test2(){
    List list = new ArrayList();
    list.add(123);
    list.add(123);
    list.add(123);
    list.add(43);
    list.add(765);
    list.add(-97);
    list.add(0);
    System.out.println(list);//[123, 43, 765, -97, 0]
    //Object max(Collection)：根据元素的自然顺序，返回给定集合中的最大元素
    Comparable max = Collections.max(list);
    System.out.println(max);//765

    //Object min(Collection)
    Comparable min = Collections.min(list);
    System.out.println(min);//-97

    //int frequency(Collection，Object)：返回指定集合中指定元素的出现次数
    int frequency = Collections.frequency(list,123);
    System.out.println(frequency);//3

    //void copy(List dest,List src)：将src中的内容复制到dest中
    List dest = Arrays.asList(new Object[list.size()]);
    System.out.println(dest.size());//7
    Collections.copy(dest,list);
    System.out.println(dest);//[123, 123, 123, 43, 765, -97, 0]
    //boolean replaceAll(List list，Object oldVal，Object newVal)：使用新值替换 List 对象的所有旧值
}

```



### 3.同步操作

Collections类中提供了多个synchronizedXxx()方法，该方法可将指定集合包装成线程安全的集合，从而可以解决多线程并发访问集合时的线程安全问题

![image-20200429115211668](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200429115213.png)

**代码示例**

```java
@Test
public void test3() {
    List list = new ArrayList();
    list.add(123);
    list.add(123);
    list.add(123);
    list.add(43);
    list.add(765);
    list.add(-97);
    list.add(0);
    System.out.println(list);//[123, 43, 765, -97, 0]
    //返回的list1即为线程安全的List
    List list1 = Collections.synchronizedList(list);
    System.out.println(list1);//[123, 123, 123, 43, 765, -97, 0]
}

```

