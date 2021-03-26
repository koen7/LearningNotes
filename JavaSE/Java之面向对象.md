# 一、面向对象与面向过程

+ 面向对象：强调的是具备行为的对象，以类/对象为最小单位，考虑谁来做
+ 面向过程：强调的是功能行为，以函数为最小单位，考虑怎么做

# 二、类和对象

## 1.类和对象的概念

+ <span style='color:red;'>**类（class）**</span> 和**<span style='color:red;'>对象（object）</span>**是面向对象中的核心概念

  + 类：是对一类事物的描述，是抽象的、被定义的。
  + 对象：是实际存在的每个个体，因此也被称为实例（instance）

+ 一切万物皆是对象

+ 面向对象程序设计，也就是设计类。

+ 设计类，也就是设计类的**成员**

  成员具备：

  	+ 属性 = 成员变量 = field
  	+ 方法 = 成员方法 = 函数 = method 

## 2.类和对象的使用（面向对象的落地实现）

**步骤**：

			1. 创建类，设计类的成员
			2. 实例化（创建）类的对象
			3. 通过“对象.属性” 和“对象.方法”调用对象的结构

```java
// 1. 创建类，设计类的成员
class Person{

    // 类的属性
     String name;
     boolean isMale;
     int age;

     //类的成员方法
    public void slepp(){
        System.out.println("人可以睡觉..");
    }

    public void eat(){
        System.out.println("人可以吃饭~");
    }
}
```

实例化对象：

```java
// 实例化Person的对象 （也叫做创建Person的对象）
        Person p1 = new Person();
```

调用成员的属性和方法：

```java
		// 调用成员方法
        p1.eat();
        // 调用成员属性
        p1.name="tom";
```

## 3.对象的创建以及对象的内存解析

```java
	// 实例化Person的对象 （也叫做创建Person的对象）
	Person p1 = new Person();
    /* 如果一个类创建了多个对象，则每个对象都会拥有一套独立的成员属性。
       这意味着，如果我们修改对象a的属性，不会影响对象b的属性。
    */
    Person p2 = new Person();
    Person p3 = p1;
```

![image-20210308103436322](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210308103436322.png)



## 4.JVM的内存结构

![20200327145207.png](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200327145207.png)



虚拟机栈：就是我们经常所说的栈，<span style='color:red;'>**存放局部变量**</span>

堆（heap）：我们将<span style='color:red;'>**new出来的结构（数组、对象）**</span>存放到堆中，**对象的属性（非static）**也存放在堆中

方法区：<span style='color:red;'>**存放类信息，static域，常量池**</span>

## 5."万事万物皆对象"

+ 在Java语言范畴中，我们都将功能、结构封装为一个类，并通过类的对象调用功能结构
  + Scanner，String等
  + 文件：File
  + 网络资源：URL
+ 不管是前端Html页面，还是后端的数据库，在与Java进行交互时都要体现为类、对象

## 6.匿名对象的使用

	+ 理解：当我们new一个对象时，没有显示的将其赋值给一个变量，这就叫匿名对象
	+ 特征：匿名对象只能调用一次。
	+ 使用：当我们需要传递引用类型的形参时，这时可以使用匿名对象

# 三、类的结构

## <span style='color:red;'>1.成员变量（属性） 与局部变量的区别</span>

### 1.1**相同点**

+ 变量的声明都是： 数据类型 变量名 =  变量的值
+ **都是先声明，后使用**
+ 都有其对应的作用域

### 1.2**不同点**

1. **在类中声明的位置**
 + 成员变量（属性）：直接定义在类的一对｛｝中
 + 局部变量：方法的形参、定义在方法中、构造器形参、构造器中、代码块内
2. **关于权限修饰符的不同**
 + 属性（成员变量）：可以在声明属性时，为其赋予权限修饰符
 + 局部变量：不可以赋予权限修饰符
 + 常见的权限修饰符：public、private、缺省（default）、protected
3. **在内存中存放的位置不同**
+ 属性（成员变量）存放在堆中
+ 局部变量：非static的局部变量存放在栈中
4. **默认初始值的情况不同**
+ 属性（成员变量）的默认值
  + 数值型（byte、short、int）：0
  + 浮点型（float、double）：0.0
  + 字符型（char）：0或者'\u000'
  + 布尔型（boolean）：false
  + <span style='color:red;'>**引用数据类型（类（class）、数组、接口）：null**</span>
+ **局部变量的默认值：没有默认值**
  + 意味着，我们在调用局部变量之前，要显示的先赋值
  + 特别的：形参在调用时，我们赋值即可

### 1.3 变量的分类

 + 按照数据类型

   ![20200327145208.png](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200327145208.png)

	+ 按照在类中声明的位置

![20200327145209.png](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200327145209.png)



## 2.方法

### 1.方法的声明

```java
语法： 权限修饰符  返回值类型  [static/final/abstract] 方法名(形参列表){
    	方法体;
}

注意：static、final、abstract 是修饰方法的，后面再说
```

### 2.方法的权限修饰符

Java中规定了4种权限修饰符：public、private、缺省、protected

详细的在封装那块再讲

### 3.方法返回值类型

1. 有返回值
   + 如果方法有返回值，那么在声明方法的时候要指定返回值的类型，并且在方法体中，要通过return 返回相关类型的值。
2. 没有返回值
   + Java中没有返回值用void来表示。通常，没有返回值的方法，在方法体中就不需要使用return了。但是，如果使用的话，只能使用**return;**表示结束此方法的意思。
3. 方法中该不该定义返回值
   1. 看题目要求
   2. 凭经验

### 4.方法名

+  属于标识符，遵循标识符的规则和规范,“见名知意”
+  方法名应该遵循小驼峰命令规范。

### 5.关于形参列表

+ 方法的参数可以声明0，1个或者多个
+ 格式：数据类型1 形参1   数据类型2 形参2

### 6.方法的使用

+ 同类中的方法可以直接调用当前类中的属性或方法，不同类中通过类的实例化对象来调用方法

+  特殊的：方法A调用了自身——递归方法

### 7.方法的重载

#### 1.重载的概念

在同一个类中，允许存在一个以上的同名方法，只要它们的参数个数不同或参数类型不同即可

<span style='color:red;'>**“两同一不同”**</span>

	+ 两同：同一个类中，同一个方法名
	+ 参数列表不同：不同的参数类型或者不同的参数个数

**【构成重载的例子】**

```java
 	public int getSum(){
        return 0;
    }

    public int getSum(int m){
        return 1;
    }

    public int getSum(int m,int j){
        return -1;
    }

	// 注意该方法的参数与最后一个方法的参数的区别 ，这也是能构成重载的
    public void getSum(int m,String s){

    }

    public int getSum(String s,int m){
        return 0;
    }
```

**【不能构成重载的例子】**

```java
// 如下的3个方法不能与上述4个方法构成重载
public int getSum(int i,int j){
    return 0;
}
    
public void getSum(int m,int n){
        
}
    
private void getSum(int i,int j){
    
}
```

#### <span style='color:red;'>**2.重载方法的判断？**</span>

+ 严格按照定义判断：两同一不同
+ <span style='color:red;'>**重载跟权限修饰符、返回值的类型、形参变量名、方法体都没有关系**</span>



### 8.可变个数形参方法

#### 1.使用说明

 + JDK5.0后新增的内容

 + JDK5.0之前，采用数组类型作为方法形参，传入多个同一个类型变量

   ```
   public static void test(int a,String[] names){}
   ```

 + JDK5.0以后，采用可变个数作为方法形参，传入多个同一个类型变量

   ```
   public static void test(int a,String ... names);
   ```

#### 2.具体使用

 + 可变个数形参格式：  数据类型 ...  变量名

 + 当调用可变个数形参的方法时，**传入的参数**可以是<span style='color:red;'>**0个**</span>、1个、2个、多个。

 + 可变个数形参的方法与本类中方法名相同，形参不同的方法之间构成重载

 + 可变个数形参的方法与本类中方法名相同，与形参类型相同的数组之间**不构成重载**。<span style='color:red;'>**换句话说，可变个数形参与同类型的数组不能共存。**</span>

 + 若方法的**形参列表中有可变个数形参和其他参数，**那么可变个数形参<span style='color:red;'>**必须声明在末尾。**</span>

 + 方法形参中可变个数形参最多只能声明一个。

   

**【举例说明】**

```java
public void show(int i){}

public void show(String s){
    System.out.println("show(String)");
}

public void show(String ... strs){
    System.out.println("show(String ... strs)");

    for(int i = 0;i < strs.length;i++){
        System.out.println(strs[i]);
    }
}
// 不能与上一个方法同时存在
//  public void show(String[] strs){
//      
//  }
// 调用时：可变形参与数组类似
test.show("hello");
test.show("hello","world");
test.show();

test.show(new String[]{"AA","BB","CC"});
```

### 9.Java的值传递机制【重点】

#### 1.针对方法内变量的赋值理解

```java
    //针对基本数据类型变量
        System.out.println("********针对基本数据类型********");
        int m = 10;
        int n = m;
        System.out.println("m = " + m +",n = " + n);


        //针对引用数据类型变量
        System.out.println("********针对引用数据类型变量");
        Order o1 = new Order();
        o1.id = 1;

        Order o2 = o1;
        System.out.println(o2.id);

        o2.id = 9;
        System.out.println(o1.id);
```

<span style='color:red;'>**值传递的规则：**</span>

	+ 如果形参的**变量是<span style='color:red;'>基本数据</span>类型**，此时传递的是数据的**<span style='color:red;'>变量值</span>**
	+ 如果形参的变量是**<span style='color:red;'>引用数据</span>类型**，此时传递的是变量的**<span style='color:red;'>地址值</span>**



**形参和实参的定义**

```
实参：方法调用时，实际传递给形参的数据
形参：方法定义时，（）内的变量
```



#### 2.针对基本数据类型的内存解析

```java
public static void main(String[] args) {
        int x = 3;

        System.out.println("修改之前x = " + x); //3

        change(x);

        System.out.println("修改之后x = " + x);// 3
    }

    private static void change(int x) {

        System.out.println("change:修改之前x " + x); //3

        x = 5;

        System.out.println("change:修改之后x = " + x);// 5
    }
```

![image-20210309171155902](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210309171155902.png)



#### 3.针对引用数据类型的内存解析

```java
public class RefDateTypeValueTransferTest {

    public static void main(String[] args) {
        Person p = new Person();
        p.age = 18;

        System.out.println("修改之前age = " + p.age);

        change(p);

        System.out.println("修改之后x = " + p.age);
    }

    private static void change(Person person) {

        System.out.println("change:修改之前age " + person.age);

        person.age = 32;

        System.out.println("change:修改之前age = " + person.age);
    }
}

class Person{
    int age;
}
```



![image-20210309172535218](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210309172535218.png)



### 10.递归方法

递归方法：一个方法体内调用其自身

```
方法递归包含了一种隐式循环，他会重复执行某段代码，这种重复执行无需循环控制。
一般方法递归，要往已知的方向递归，否则就变成了无穷递归，类似于死循环
```



**【递归举例】**

```java
public static void main(String[] args) {

        // 递归例题：求1-100连续自然数的和‘
        // 方法一： for循环
        int sum = 0;
        for (int i = 0; i < 100; i++) {
            sum += i;
        }

        //方式二
        RecursionTest rt = new RecursionTest();
        System.out.println(rt.getSum(100));

    }

    public int getSum(int n) {
        if (n == 0) {
            return 1;
        } else {
            return n + getSum(n - 1);
        }
    }
```

### 11.方法的重写（override）

#### 1.什么是方法的重写

子类继承父类后，可以对父类同名同形参列表的方法进行覆盖，这就叫重写

#### 2.重写的应用

重写以后，当创建子类对象时，通过子类调用父类中同名同形参列表的方法，实际上执行的是子类重写父类的方法

#### 3.重写举例

```java
// 父类
class Circle{
public double findArea(){}//求面积
}
// 子类
class Cylinder extends Circle{
public double findArea(){}//求表面积
}
**********************************************
// 父类
class Account{
public boolean withdraw(double amt){}
}
// 子类
class CheckAccount extends Account{
public boolean withdraw(double amt){}
}
```

#### 4.重写的规则

**方法的声明**

```
权限修饰符 返回值类型 方法名（形参列表）{
	//方法体
}
```

**约定俗成的**：子类中的叫重写的方法，父类中的叫被重写的方法

1. 子类重写的方法的方法名和参数列表与父类中被重写的方法名和参数列表相同
2. 子类中重写的方法的权限修饰符要**大于等于**父类中被重写的方法的修饰符

```
特别的：父类中private修饰的方法，子类中不能重写该类方法
```

3. 返回值
   + 父类中的被重写的方法的返回值是void，那么子类重写的方法的返回值也必须是void
   + 父类中被重写的方法的返回值类型是**A类型**，那么子类重写的方法的返回值类型**可以是A或者A的子类**
   + 父类被重写的方法的返回值类型是基本数据类型（比如：double），则子类重写的方法的返回值类型必须与父类的返回值相同
4. 子类重写的方法抛出的异常类型小于等于父类被重写的方法抛出的异常类型

```
子类和父类中重写的方法要么都声明 非 static的（这样才有可能构成重写）
```



## 3.构造器（或构造方法）：Constructor

### 1.构造器的作用

1. 创建对象
2. 为属性初始化

### 2.使用说明：

+ 如果没有显示的定义类的构造器时，则系统会默认提供一个无参构造。
+ 定义构造器的格式：权限修饰符 类名（形参列表）｛｝
+ 一个类中可以定义多个构造器，彼此构成重载
+ **<span style='color:red;'>一旦我们显示的定义了构造方法，系统就不再提供空参的构造器</span>**
+ 一个类中，**至少有一个构造器**

### 3.属性赋值顺序

总结：属性赋值的先后顺序

① 默认初始化

② 显示初始化

③构造器中初始化

④ 通过“对象.方法”或“对象.属性”的方式，赋值

以上操作的先后顺序：① - ② - ③ - ④ 



### 4.JavaBean的概念

所谓的JavaBean，是指符合如标准的Java类：

+ 类是公共（public）的
+ 具有一个空参的构造器
+ 属性都是私有化的，且有对应的set、get方法





## 4.代码块

### 1.代码块的作用

初始化类和对象的信息

### 2.代码块的分类

代码块如果要用修饰符修饰，**==只能使用static进行修饰==**，因此代码块分为 **静态代码块 和非静态代码块**

### 3.静态代码块

+ 内部可以输出语句
+ ==<span style='color:red;'>静态代码快**随着类的加载而执行**，并且**只执行一次**</span>==
+ 静态代码块中**只能调用静态属性或方法**，不能调用非静态属性和非静态方法
+ 如果一个类中有多个静态代码块，那么根据静态代码块的声明的先后顺序执行（自上而下）
+ 静态代码块优先执行于非静态代码块

### 4.非静态代码块

+ 内部可以输出语句
+ ==<span style='color:red;'>**非静态代码块**随着**对象的创建而执行**，并且**可以执行多个**</span>==
+ 每创建一个对象，执行一次非静态代码块
+ 作用：可以在创建对象时，对对象的属性等进行初始化
+ 如果一个类中有多个非静态代码块，那么根据非静态代码块声明的先后顺序执行（自上而下）
+ 非静态代码块中可以调用静态变量和静态方法，也可以调用普通变量和普通方法



**练习**

`在实例化子类对象时，涉及到父类、子类中静态代码块、非静态代码块、构造器的加载顺序：由父及子，静态先行，非静态代码块优先执行，构造器紧跟其他`

```java
class Root{
	static{
		System.out.println("Root的静态初始化块");
	}
	{
		System.out.println("Root的普通初始化块");
	}
	public Root(){
		System.out.println("Root的无参数的构造器");
	}
}
class Mid extends Root{
	static{
		System.out.println("Mid的静态初始化块");
	}
	{
		System.out.println("Mid的普通初始化块");
	}
	public Mid(){
		System.out.println("Mid的无参数的构造器");
	}
	public Mid(String msg){
		//通过this调用同一类中重载的构造器
		this();
		System.out.println("Mid的带参数构造器，其参数值："
			+ msg);
	}
}
class Leaf extends Mid{
	static{
		System.out.println("Leaf的静态初始化块");
	}
	{
		System.out.println("Leaf的普通初始化块");
	}	
	public Leaf(){
		//通过super调用父类中有一个字符串参数的构造器
		super("尚硅谷");
		System.out.println("Leaf的构造器");
	}
}
public class LeafTest{
	public static void main(String[] args){
		new Leaf(); 
		//new Leaf();
	}
}

运行结果：
    Root的静态初始化块
    Mid的静态初始化块
    Leaf的静态初始化块
    Root的普通初始化块
    Root的无参数的构造器
    Mid的普通初始化块
    Mid的无参数的构造器
    Mid的带参数构造器，其参数值：尚硅谷
    Leaf的普通初始化块
    Leaf的构造器
```

练习2：

```java
class Father {
    static {
        System.out.println("11111111111");
    }

    {
        System.out.println("22222222222");
    }

    public Father() {
        System.out.println("33333333333");

    }

}

public class Son extends Father {
    static {
        System.out.println("44444444444");
    }

    {
        System.out.println("55555555555");
    }

    public Son() {
        System.out.println("66666666666");
    }


    public static void main(String[] args) { // 由父及子 静态先行
        System.out.println("77777777777");
        System.out.println("************************");
        new Son();
        System.out.println("************************");

        new Son();
        System.out.println("************************");
        new Father();
    }

}

运行结果：
    11111111111
    44444444444
    77777777777
    ************************
    22222222222
    33333333333
    55555555555
    66666666666
    ************************
    22222222222
    33333333333
    55555555555
    66666666666
    ************************
    22222222222
    33333333333
```



### 5.对属性可以赋值的顺序

①：默认初始化

②：显示初始化

③：构造器中初始化

④：可以通过“对象.属性”或“对象.方法”进行初始化

⑤：代码块中初始化

**执行顺序**：①    -    ② / ⑤	-	③	-	④

`特别的，2和5的顺序 是看谁写在上面 谁先进行初始化`



## 5.类的内部成员之 ——内部类

Java中允许将一个类A声明在另一个类B中，则类A就是**内部类**，类B就是**外部类**

### **1.内部类的分类： 成员内部类（静态和非静态） 和 局部内部类（方法内、构造器内、代码块内）**

```java
public class InnerClassTest {

    public static void main(String[] args) {

    }
}

class Animal{

    // 静态的成员内部类
    static class tiger{

    }


    // 非静态的成员内部类
    class pet{

    }

    public Animal(){

        //定义在构造器中的局部内部类
        class dog{

        }
    }

    // 定义在方法中的局部内部类
    public void method(){
        class cat{

        }
    }

    // 定义在代码块中的局部内部类
    {
        class bird{

        }
    }
}
```



### 2.成员内部类的理解

+ 一方面，可以作为外部类的成员
  + 可以被4种权限修饰符修饰（public、缺省、protected、private）
  + 可以被static修饰
  + 调用外部类的结构
+ 另一方面，可以作为一个类：
  + 类的内部可以定义属性、方法、构造器等结构
  + **类可以被final修饰**，表示此类不能被继承。言外之意，不使用final，就可以被继承
  + 可以被abstract修饰

### 3.成员内部类

#### 1.如何实例化成员内部类对象

```java
class Animal{

    // 静态的成员内部类
    static class tiger{

        public void shout(){
            System.out.println("老虎会叫~");
        }
    }

    // 非静态的成员内部类
     class pet{
        String name;
        public pet(){}
        public void gogo(){
            System.out.println("猫还吃鱼");
        }
    }
}

public class InnerClassTest {

    public static void main(String[] args) {

        // 实例化静态的成员内部类
        Animal.tiger t = new Animal.tiger();
        t.shout();

        // 实例化非静态的成员内部类
        Animal animal =new Animal();
        Animal.pet p = animal.new pet();
        p.gogo();

    }
}
```

#### 2.如何在成员内部类中区分调用外部类的结构

```java
class Person{
    String name = "小明";
    public void eat(){
    }
    //非静态成员内部类
    class Bird{
        String name = "杜鹃";
        public void display(String name){
            System.out.println(name);//方法的形参
            System.out.println(this.name);//内部类的属性
            System.out.println(Person.this.name);//外部类的属性
            //Person.this.eat();
        }
    }
}
```

### 4.局部内部类

```java
//返回一个实现了Comparable接口的类的对象
public Comparable getComparable(){

    //创建一个实现了Comparable接口的类:局部内部类
    //方式一：
    //      class MyComparable implements Comparable{
    //
    //          @Override
    //          public int compareTo(Object o) {
    //              return 0;
    //          }
    //          
    //      }
    //      
    //      return new MyComparable();

    //方式二：
    return new Comparable(){

        @Override
        public int compareTo(Object o) {
            return 0;
        }
    };
}
```

**注意点：**

在局部内部类的方法中如果调用局部内部类所声明的方法中的局部变量的话，要求此局部变量声明为final的。

原因：局部内部类也会生成字节码文件，在调用所属类的局部变量时，因为是两个类，所以不能修改所属类的属性，因此所属类将属性设置为final的为内部类调用提供一个副本，而内部类不能进行修改。

- jdk 7及之前版本：要求此局部变量显式的声明为final的
- jdk 8及之后的版本：可以省略final的声明

总结：成员内部类和局部内部类，在编译以后，都会生成字节码文件。 格式：成员内部类：`外部类$内部类名.class` 局部内部类：`外部类$数字 内部类名.class`