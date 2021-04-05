# 一、泛型简介

## 1.泛型的概念

+ 所谓泛型，就是允许在定义类、接口时通过一个标识来表示类中某个属性的类型或者是某个方法的返回值以及参数类型。
+ 从JDK5.0以后，Java引入了“泛型”，泛型也称为“参数化类型”，允许我们在创建集合时再指定集合元素的类型，正如：`List<String>`表明该List只能保存字符串类型的对象。
+ JDK 5.0改写了集合框架中的全部接口和类，为这些接口、类增加了泛型支持，从而可以在声明集合变量、创建集合对象时传入类型实参。

## 2.泛型的引入背景

合容器类在设计阶段/声明阶段不能确定这个容器到底实际存的是什么类型的对象，所以在JDK1.5之前只能把元素类型设计为Object，JDK1.5之后使用泛型来解决。因为这个时候除了元素的类型不确定，其他的部分是确定的，例如关于这个元素如何保存，如何管理等是确定的，因此此时把元素的类型设计成一个参数，这个类型参数叫做泛型。Collection<E>，List<E>，ArrayList<E> 这个<E>就是类型参数，即泛型。

### 3.引入泛型的目的

1. 解决元素存储的安全性问题，好比商品、药品标签、不会弄错。
2. 解决获取元素时，需要类型强制转换的问题，好比不用每回拿商品、药品都要辨别。

```
Java泛型可以保证程序在编译时不会报错！
```

# 二、泛型在集合中应用

## 1.在集合中没有使用泛型的例子

```java
@Test
public void test1(){
    ArrayList list = new ArrayList();
    //需求：存放学生的成绩
    list.add(78);
    list.add(76);
    list.add(89);
    list.add(88);
    //问题一：类型不安全
    //        list.add("Tom");

    for(Object score : list){
        //问题二：强转时，可能出现ClassCastException
        int stuScore = (Integer) score;

        System.out.println(stuScore);
    }
}
```

**图示**

![image-20200429224753376](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200429224754.png)

## 2.在集合中使用泛型的例子：以ArrayList为例

```java
//在集合中使用泛型，以ArrayList为例
@Test
public void test1(){
    ArrayList<String> list = new ArrayList<>();
    list.add("AAA");
    list.add("BBB");
    list.add("FFF");
    list.add("EEE");
    list.add("CCC");
	//遍历方式一：
    Iterator<String> iterator = list.iterator();
    while (iterator.hasNext()){
        System.out.println(iterator.next());
    }
    System.out.println("-------------");
    //便利方式二：
    for (String str:
         list) {
        System.out.println(str);
    }
}
```

**图示**

![image-20200429224832416](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200429224833.png)

## 3.在集合中使用泛型例子：以HashMap为例

```java
@Test
//在集合中使用泛型的情况：以HashMap为例
public void test2(){
    Map<String,Integer> map = new HashMap<>();//jdk7新特性：类型推断
    map.put("Tom",26);
    map.put("Jarry",30);
    map.put("Bruce",28);
    map.put("Davie",60);
    //嵌套循环
    Set<Map.Entry<String, Integer>> entries = map.entrySet();
    Iterator<Map.Entry<String, Integer>> iterator = entries.iterator();

    while (iterator.hasNext()){
        Map.Entry<String, Integer> entry = iterator.next();
        String key = entry.getKey();
        Integer value = entry.getValue();
        System.out.println(key+"="+value);
    }

}
```

## 4.在集合中使用泛型总结

1. 泛型是在JDK5.0新增的。

2. 在实例化集合类时,可以指名具体的泛型类型

3. 指名完以后，在集合类或接口中凡是定义类或接口时，内部结构（比如：方法、构造器、属性等）使用到类的泛型的位置，都指定为实例化的泛型类型

     比如：add(E e) --->实例化以后：add(Integer e)

4. 注意点：**泛型的类型必须是类**，需要使用基本类型的位置，可以拿包装类替换

5. 如果实例化时，没有指名泛型的类型。默认类型为java.lang.Object类型。



# 三、自定义泛型

泛型类、泛型接口、泛型方法

## 1.泛型的声明

+ `interface List<T>`和`class GenTest<K,V>`其中，T，K，V不代表值，而是表示类型。这里使用任意字母都可以。
+ 常用T表示，是Type的缩写

## 2.泛型的实例化

一定要在类名后面指定类型参数的值（类型）。如：

`List<String>  strList = new ArrayList<String>()；`

`Iterator<Customer>  iterator = customers.iterator();`

+ 把一个集合中的内容限制为一个特定的数据类型，这就是泛型背后的核心思想。

```java
//JDK 5.0以前
Comparable c = new Date();
System.out.println(c.comparaTo("red");
                   
//JDK 5.0以后
Comparable <Date> c = new Date();
System.out.println(c.comparaTo("red");
```

```
总结：使用泛型的主要优点在于能够在编译时发现错误。
```

## 3.自定义泛型类泛型接口的注意点

1. 泛型类可能有多个参数，此时应将多个参数一起放在尖括号内。比如<E1,E2,E3>
2. 泛型类的构造器如下：public GenericClass(){}
3. 实例化后，操作原来泛型位置的结构必须与指定的泛型类型一致
4. 泛型不同的引用不能相互赋值

```
尽管在编译时 ArrayList<String>和ArrayList<Integer>是两种类型，但是，在运行时只有一个ArrayList被加载到JVM中
```

5. 泛型如果不指定，将被擦除，**泛型对应的类型均按照Object处理**，但不等价于Object

```
建议：泛型要使用就全都使用，要不用，全都不要用。
```

6. 如果泛型结构是一个接口或者一个抽象类，则不可以创建泛型类的对象

7. JDK 7.0，泛型的简化操作： ArrayList<Fruit>first= new ArrayList<>();（类型推断）

8. 泛型的指定不能使用基本数据类型，可以使用包装类替换

9. **静态方法中不能使用泛型。**原因：在实例化对象时才指定泛型的具体类型，静态方法是优先于实例化对象的。

10. **异常类不能使用泛型。**原因：泛型是继承Object类的，异常是继承Exception类的

11. 不能使用new E[]。但是可以：E[] elements= (E[])new Object[capacity];

12. 父类有泛型，子类可以选择保留泛型也可以选择指定泛型类型：

    - 子类不保留父类的泛型：按需实现
      - 没有类型---擦除
      - 具体类型
    - 子类保留父类的泛型：泛型子类
      - 全部保留
      - 部分保留
    - 结论：子类必须是“富二代”，子类除了指定或保留父类的泛型，还可以增加自己的泛型

    **代码示例**

    ```java
    class Father<T1, T2> {
    }
    
    /**
     * 定义泛型子类Son
     * 情况一：继承泛型父类后不保留父类的泛型
     */
    //1.没有指明类型  擦除
    class Son1<A, B> extends Father {//等价于class Son1 extends Father<Object,Odject>{}
    }
    
    //2.指定具体类型
    class Son2<A, B> extends Father<Integer, String> {
    }
    
    /**
     * 定义泛型子类Son
     * 情况二：继承泛型父类后保留泛型类型
     */
    //1.全部保留
    class Son3<T1, T2, A, B> extends Father<T1, T2> {
    }
    
    //2.部分保留
    class Son4<T2, A, B> extends Father<Integer,T2>{
    }
    ```

    

    ## 4.自定义泛型结构

    ### 1.自定义泛型类

    **代码示例**

    ```java
    /**
     * 自定义泛型类Order
     */
    class Order<T> {
        private String orderName;
        private int orderId;
        //使用T类型定义变量
        private T orderT;
    
        public Order() {
        }
        //使用T类型定义构造器
        public Order(String orderName, int orderId, T orderT) {
            this.orderName = orderName;
            this.orderId = orderId;
            this.orderT = orderT;
        }
    
        //这个不是泛型方法
        public T getOrderT() {
            return orderT;
        }
        //这个不是泛型方法
        public void setOrderT(T orderT) {
            this.orderT = orderT;
        }
        //这个不是泛型方法
        @Override
        public String toString() {
            return "Order{" +
                    "orderName='" + orderName + '\'' +
                    ", orderId=" + orderId +
                    ", orderT=" + orderT +
                    '}';
        }
    //    //静态方法中不能使用类的泛型。
    //    public static void show(T orderT){
    //        System.out.println(orderT);
    //    }
    
    //    //try-catch中不能是泛型的。
    //    public void show(){
    //        try {
    //
    //        }catch (T t){
    //
    //        }
    //    }
    
        //泛型方法：在方法中出现了泛型的结构，泛型参数与类的泛型参数没有任何关系。
        //换句话说，泛型方法所属的类是不是泛型类都没有关系。
        //泛型方法，可以声明为静态的。
        // 原因：泛型参数是在调用方法时确定的。并非在实例化类时确定。
        public static <E> List<E> copyFromArryToList(E[] arr) {
            ArrayList<E> list = new ArrayList<>();
            for (E e :
                    list) {
                list.add(e);
            }
            return list;
        }
    }
    ```

    ### 2.自定义泛型接口

    **代码示例**

```java
/**
 * 自定义泛型接口
 */
public interface DemoInterface <T> {
    void show();
    int size();
}

//实现泛型接口
public class Demo implements DemoInterface {
    @Override
    public void show() {
        System.out.println("hello");
    }

    @Override
    public int size() {
        return 0;
    }
}

@Test
//测试泛型接口
public void test3(){
    Demo demo = new Demo();
    demo.show();
    
```

### 3.自定义泛型方法

+ 方法，也可以被泛型化，**不管此时定义在其中的类是不是泛型类**。 在泛型方法中可以定义泛型参数，此时，参数的类型就是传入数据的格式
+ **<span style='color:red;'>泛型方法的格式：[访问权限] <泛型>  返回类型  方法名(泛型标识  参数名称)抛出的异常</span>**
+ 泛型方法声明泛型时也可以指定上限

**代码示例**

```java
//泛型方法：在方法中出现了泛型的结构，泛型参数与类的泛型参数没有任何关系。
//换句话说，泛型方法所属的类是不是泛型类都没有关系。
//泛型方法，可以声明为静态的。
// 原因：泛型参数是在调用方法时确定的。并非在实例化类时确定。
public static <E> List<E> copyFromArryToList(E[] arr) {
    ArrayList<E> list = new ArrayList<>();
    for (E e :
         list) {
        list.add(e);
    }
    return list;
}
```

### 4.总结

- 泛型实际上就是标签，声明时不知道类型，再使用时指明
- 定义泛型结构，即：泛型类、接口、方法、构造器时贴上泛型的标签<T>
- 用泛型定义类或借口是<T>放到类名或接口名后面，定义泛型方法时在方法名前加上<T>

## 4.泛型的应用场景

【DAO.java】:定义了操作数据库中的表的通用操作。 ORM思想(数据库中的表和Java中的类对应)

```java
public class DAO<T> {//表的共性操作的DAO

    //添加一条记录
    public void add(T t){

    }

    //删除一条记录
    public boolean remove(int index){

        return false;
    }

    //修改一条记录
    public void update(int index,T t){

    }

    //查询一条记录
    public T getIndex(int index){

        return null;
    }

    //查询多条记录
    public List<T> getForList(int index){

        return null;
    }

    //泛型方法
    //举例：获取表中一共有多少条记录？获取最大的员工入职时间？
    public <E> E getValue(){

        return null;
    }

}
```

**【CustomerDAO.java】:**

```java
public class CustomerDAO extends DAO<Customer>{//只能操作某一个表的DAO
}
```

**【StudentDAO.java】:**

```java
public class StudentDAO extends DAO<Student> {//只能操作某一个表的DAO
}
```

# 四、泛型在继承上的体现

泛型在继承方面的体现：

虽然类A是类B的父类，但是G<A> 和G<B>二者不具备子父类关系，二者是并列关系。

补充：类A是类B的父类，A<G> 是 B<G> 的父类

**代码示例**

```java
@Test
public void test1(){

    Object obj = null;
    String str = null;
    obj = str;

    Object[] arr1 = null;
    String[] arr2 = null;
    arr1 = arr2;
    //编译不通过
    //        Date date = new Date();
    //        str = date;
    List<Object> list1 = null;
    List<String> list2 = new ArrayList<String>();
    //此时的list1和list2的类型不具子父类关系
    //编译不通过
    //        list1 = list2;
    /*
        反证法：
        假设list1 = list2;
           list1.add(123);导致混入非String的数据。出错。

         */

    show(list1);
    show1(list2);
}

public void show1(List<String> list){

}

public void show(List<Object> list){

}

@Test
public void test2(){

    AbstractList<String> list1 = null;
    List<String> list2 = null;
    ArrayList<String> list3 = null;

    list1 = list3;
    list2 = list3;

    List<String> list4 = new ArrayList<>();

}
```

# 五、通配符

## 1.通配符的使用

1. 使用类型通配符：？
   1. 比如：`List<?>,  Map<?>`
   2. `List<?> 是 List<String> ，List<Object>` 等各种泛型List的父类
2. **当通过`list.get(index)`从List<?> 中读取数据时，是可以读取到的，但如果`list.add()`将数据添加到List<?>  中是不可以的。**
3. 唯一能添加元素到List<?>中的只有null`list.add(null)`

**代码示例**

```java
@Test
public void test3(){
    List<Object> list1 = null;
    List<String> list2 = null;

    List<?> list = null;

    list = list1;
    list = list2;
    //编译通过
    //        print(list1);
    //        print(list2);

    //
    List<String> list3 = new ArrayList<>();
    list3.add("AA");
    list3.add("BB");
    list3.add("CC");
    list = list3;
    //添加(写入)：对于List<?>就不能向其内部添加数据。
    //除了添加null之外。
    //        list.add("DD");
    //        list.add('?');

    list.add(null);

    //获取(读取)：允许读取数据，读取的数据类型为Object。
    Object o = list.get(0);
    System.out.println(o);
}

public void print(List<?> list){
    Iterator<?> iterator = list.iterator();
    while(iterator.hasNext()){
        Object obj = iterator.next();
        System.out.println(obj);
    }
}
```

## 2.有限制的通配符

+ <?> ：允许所有泛型的引用调用 [负无穷，正无穷]

+ <? extends A>：使用时指定的类必须是A和A的子类； 即 <= 

+ <? super A>：使用指定的类必须是A和A的父类 ；即>=

+ 举例：

  - <?extends Number>（无穷小， Number]

    只允许泛型为Number及Number子类的引用调用

  - <?super Number>[Number，无穷大）

    只允许泛型为Number及Number父类的引用调用

  - <? extends Comparable>

    只允许泛型为实现 Comparable接口的实现类的引用调用

**代码示例**

```java
@Test
public void test4(){

    List<? extends Person> list1 = null;
    List<? super Person> list2 = null;

    List<Student> list3 = new ArrayList<Student>();
    List<Person> list4 = new ArrayList<Person>();
    List<Object> list5 = new ArrayList<Object>();

    list1 = list3;
    list1 = list4;
    //        list1 = list5;

    //        list2 = list3;
    list2 = list4;
    list2 = list5;

    //读取数据：
    list1 = list3;
    Person p = list1.get(0);
    //编译不通过
    //Student s = list1.get(0);

    list2 = list4;
    Object obj = list2.get(0);
    ////编译不通过
    //        Person obj = list2.get(0);

    //写入数据：
    //编译不通过
    //        list1.add(new Student());

    //编译通过
    list2.add(new Person());
    list2.add(new Student());

}
```

