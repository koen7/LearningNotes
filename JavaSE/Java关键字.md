# 一、this的使用

## 1.this关键字的概述

1. this 可以用来修饰属性、方法、构造器

2. **<span style='color:red;'>修饰属性和方法时</span>**：this表示**<span style='color:red;'>当前对象</span>**或正在创建的对象

## 2.this关键字的使用

### 2.1 this调用属性、方法

+ 在类的方法中，我们可以使用“this.属性”或者“this.方法”的方式，调用当前对象属性或方法。通常情况下，我们都选择省略“this”。特殊情况下，**如果方法的形参和属性名一样时**，我们就必须**显示**的使用**“<span style='color:red;'>this.属性</span>”**的方式，表名这是属性，而非形参

+ 在类的方法中，我们可以使用“this.属性”或者“this.方法”的方式，调用当前对象属性或方法。通常情况下，我们都选择省略“this”。特殊情况下，**如果构造器的形参和属性名一样时**，我们就必须**显示**的使用**“<span style='color:red;'>this.属性</span>”**的方式，表名这是属性，而非形参

### 2.2.this调用 构造器

①. 我们在类的构造器中，可以使用“this(形参列表)” ，调用本类中**指定的<span style='color:red;'>其他构造器</span>**

②. 构造器中不能使用this调用自身构造器。

③. **规定：this必须在构造器中的<span style='color:red;'>首行。</span>**

④. 如果一个类中有n个构造器，则最多有n-1构造器中使用了"this（形参列表）"

⑤. 构造器内部，最多声明一个“this（形参列表）”，用来调用其他的构造器



# 二、package关键字

## 1.package关键字概述

+ 包，属于标识符，遵循标识符的命名规则、规范（xxxyyyzzz），“见名知意”！小写
+ 为了更好的对类进行管理，提供了包的概念
+ 使用package声明类或接口所属的包，声明在源文件的**<span style='color:red;'>首行</span>**
+ 每“.”一次，就代表一层文件目录

## 2.MVC设计模式

![20200327145213.png](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200327145213.png)

## 3.import关键字的使用

import：导入

1. 声明在package和类之间
2. 使用import导入指定包下的类
3. 如果使用的类或者接口是java.lang包下定义的，则可以省略import结构
4. 如果使用的类或接口是本包下定义的，则可以省略import结构
5. 使用“xxx.*”方式表名导入xxx下的所有包。但是如果使用的是xxx子包下的结构，则仍需要显式导入
6. import static：导入指定类或接口中的静态结构：属性或方法
7. 如果导入的类是不同包的同名类，那么其中一个需要以全类名的形式显式

# 三、super关键字

## 1.super关键字概述

super关键字可以理解为：父类的，可以用来调用**属性、方法、构造器**

+ 尤其当子父类出现同名成员时，可以使用super表明调用的是父类中的成员
+ super的追溯不仅限于直接父类
+ super和this的用法很像，**this**代表**本类对象**的**引用**，**super**代表**父类对象**的**引用**

## 2.super关键字使用

### ①super调用属性、方法

与this关键字使用方式相同，只不过调用的是父类的属性

### ②在子类的方法或构造器中使用

通过使用“super.属性”或“super.方法”的方式，显示的调用父类中声明的属性和方法。但是，通常情况下，我们习惯省略“super”

**特殊情况**

	+ 当子类和父类中定义了同名的属性，我们要想在子类中调用父类的属性时，则必须显式的使用“super.属性”的方式，表名调用的是父类中声明的属性
	+ 当子类重写了父类中的方法以后，我们想在子类的方法中调用父类中被重写的方法时，则必须显式的使用"super.方法"的方式，表明调用的是父类中被重写的方法。

## 3.super调用构造器

+ 我们可以在子类的构造器中显式的使用“super（形参）”的方式，调用父类中声明的指定构造器
+ “super（形参）”的使用，**必须声明在子类构造器的第一行。**
+ 我们在类的构造器中，针对于“this（形参列表）”或“super（形参列表）”只能二选一，不能同时出现
+ 在构造器的首行，**没有显式的声明“this（形参列表）”或者“super（形参列表）**”，**则默认调用的是父类中空参的构造器：super（）；**
+ 在类的多个构造器中，至少有一个类的构造器中使用了“super（形参列表）”，调用父类中的构造器

## 4.this和super的区别

```
区别点			this								super

访问属性		调用的是本类中的属性				直接调用父类中的属性
			如果没有就从父类中继续查找

调用方法		调用的是本类中的方法				直接调用父类中的方法
			如果没有就从父类中继续查找

调用构造器		调用的是本类中的构造器				调用父类的构造器，必须放在子类构造器的首行
				必须放在构造器的首行
```



# 四、static关键字

## 1.static关键字简述

staic是静态的

static关键字可以修饰 属性、方法、代码块、内部类

## 2.static修饰属性

static按照**是否是静态**的可以将属性分为：**静态变量 和 非静态变量**（类变量或者实例变量）

+ 实例变量(非静态变量)：我们创建了类的多个对象，每个对象都拥有一套独立的类中非静态变量。一个对象中的非静态变量值的改变不会影响另一个对象中同名的非静态变量值。

+ 静态变量：我们创建了类的多个对象，==多个对象共享同一个变量==。当通过**某一个对象修改静态变量时**，会导致**其他对象调用此对象**时，是修改过了的。

**static修饰属性的其他说明**

+ 静态变量随着类的加载而加载。可以通过`类.非静态变量`调用
+ 静态变量的加载要**早于**实例变量的加载
+ **由于类只会加载一次，所以静态变量只会存在一份，存在于方法区中的静态域。**

|      | 类变量 | 实例变量 |
| ---- | ------ | -------- |
| 类   | yes    | no       |
| 对象 | yes    | yes      |

**静态变量内存解析**

![image-20210318224559277](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210318224559277.png)



## 3.static修饰方法：静态方法（类方法）

+ 随着类的加载而加载，可以使用`类.静态方法`的方式进行调用
+ **==静态方法中，只能调用静态的方法或属性==**
+ ==非静态方法中，既可以调用非静态的方法或属性，也可以调用调用静态的方法或属性==

|      | 静态方法 | 非静态方法 |
| ---- | -------- | ---------- |
| 类   | yes      | no         |
| 对象 | yes      | yes        |

**使用的注意点：**

+ **静态方法**中**不能使用this、super关键字。**
+ 关于静态属性和静态方法的使用，大家都从生命周期的角度去理解



## 4.开发中，如何确定一个属性是否要声明为静态的

+ 属性是可以被多个对象共有的
+ 不会随着对象的改变而改变
+ 类中的常量常常修饰static的

## 5.开发中，如何确定一个方法是否要声明为静态的

+ ==**一般的工具类中的方法是静态的**==
+ 静态属性生成的get、set方法通常是静态的



**使用举例：**记录创建圆的个数

```java
class Circle{
	
	private double radius;
	private int id;//自动赋值
	
	public Circle(){
		id = init++;
		total++;
	}
	
	public Circle(double radius){
		this();
//		id = init++;
//		total++;
		this.radius = radius;
		
	}
	
	private static int total;//记录创建的圆的个数
	private static int init = 1001;//static声明的属性被所对象所共享
	
	public double findArea(){
		return 3.14 * radius * radius;
	}

	public double getRadius() {
		return radius;
	}

	public void setRadius(double radius) {
		this.radius = radius;
	}

	public int getId() {
		return id;
	}

	public static int getTotal() {
		return total;
	}

}

```



# 五、单例模式

## 1.设计模式的说明

设计模式是在大量的实践中总结和理论之后的优化的代码结构、编程风格、以及解决问题的思考方式 

**常用的设计模式——23种经典的设计模式**

+ 创建型模式，共5种：工厂方法模式、抽象工厂模式、单例模式、建造者模式、原型模式
+ 结构性模式，共7种：适配器模式、装饰器模式、代理模式、外观模式、桥接模式、组合模式、享元模式
+ 行为型模式，共11种：策略模式、模板方法模式、观察者模式、迭代器模式、责任链模式、命令模式、备忘录模式、状态模式、访问者模式、中介者模式、解释器模式。

## 2.单例模式

所谓类的单例设计模式，就是采取一定的方法保证在整个的软件系统中，**对某个类只能存在一个对象实例**。

### 2.1 单例模式——饿汉式

**代码**

```java
/*
   单例模式——饿汉式

*/
public class SingletonTest1 {

    public static void main(String[] args) {
        Bank bank1 = Bank.getInstance();
        Bank bank2 = Bank.getInstance();
        System.out.println(bank1 == bank2);
    }
}

class Bank{

    // 1.构造器私有化
    private Bank(){

    }

    // 2.在类的内部实例化对象
    static Bank  instance = new Bank();
    // 3.使用方法返回对象
    public static Bank getInstance(){
        return instance;
    }
}
```

### 2.2单例模式——懒汉式

**代码：**

```java
/*
    单例模式——懒汉式
*/
public class SingletonTest2 {
    public static void main(String[] args) {
        Order order1 = Order.getInstance();
        Order order2 = Order.getInstance();

        System.out.println(order1 == order2);
    }
}

class Order{

    // 1.构造器私有化
    private Order(){}

    // 2. 在类的内部实例化对象
    static Order instance = null;

    // 3. 调用方法返回对象
    public static Order getInstance(){
        //判断 是否存在instance对象
        if ( instance == null){
            instance = new Order();
        }
        return instance;
    }
}

```

### 2.3 区分饿汉式和懒汉式

+ 饿汉式：上来就创建对象
  + **优点：线程是安全的**
  + 缺点：对象加载的时间过长。
+ 懒汉式：什么时候用什么时候造对象
  + 优点：延迟对象的创建
  + 缺点：目前的写法是线程不安全的。 ---> 到多线程内容时，再修改



# **补充** 理解main方法的语法

+ main方法作为程序的入口
+ main方法可以有多个
+ main方法可以作为与控制台交互的方式



# 六、final关键字——最终的

## 1.可以用来修饰：类、方法、属性

1.final用来修饰一个类：表示此类不能被其他类所继承

​	比如：String类、System类

2.final用来修饰一个方法：表示此方法不能被重写

​	比如：Object类中的getClass（）

3.final修饰用来一个属性：表示此属性是一个常量

	+ final修饰属性：可以考虑修饰的位置：显示初始化、构造器初始化、代码块初始化
	+ final修饰局部变量：尤其是使用final修饰形参时，表明此形参是一个常量。当我们调用此方法时，给常量形参赋一个实参。一旦赋值后，就只能在方法体内使用此参数，但不能被重新赋值。
	+ **static final**：表示**全局常量**



# 七、abstract关键字——抽象的

## 1.abstract可以修饰类和方法

## 2.abstract修饰类

+ 当abstract修饰类时，==明这个类**不能实例化**==
+ 抽象类中一定有构造器，便于子类实例化时调用（涉及：子类实例化全过程）
+ 开发中，都会提供抽象类的子类，让子类对象实例化，完成相关的操作——> **抽象的使用前提：继承性**

## 3.abstract修饰方法

+ abstract修饰方法时，该方法是抽象方法，抽象方法只有声明，**没有方法体**；
+ ==**包含抽象方法的类一定是抽象类**，反之，**抽象类**中**不一定要有抽象方法**==
+ **若子类重写了父类中的抽象方法，此子类才可以实例化**
+ **若子类没有重写父类中的抽象方法，那么此子类也需要声明为抽象类，需要使用abstract修饰**

举例一：

```java
public abstract class Vehicle{//抽象类
    public abstract double calcFuelEfficiency();//计算燃料效率的抽象方法
    public abstract double calcTripDistance();//计算行驶距离的抽象方法
}
public class Truck extends Vehiclel{
    public double calcFuelEfficiency( ){//写出计算卡车的燃料效率的具体方法}
    public double calcTripDistance( ){//写出计算卡车行驶距离的具体方法}
}
public class River Barge extends Vehicle{
    public double calcFuelEfficiency( ){//写出计算驳船的燃料效率的具体方法}
    public double calcTripDistance( ){//写出计算驳船行驶距离的具体方法}
}
```



举例二：

```java
abstract class GeometricObject{//抽象类
	public abstract double findArea();
}
class Circle extends GeometricObject{
    private double radius;
    public double findArea(){
            return 3.14 * radius * radius;
    }
}
```

## 4.abstract使用时的注意点

+ abstract不能修饰：属性、构造器等结构
+ abstract不能修饰私有方法、静态方法（静态方法不能被重写，）、final的方法、final的类

## 5.抽象类的匿名子类实现

```java
abstract class Person{
    String name;
    int age;

    // 抽象类一定有构造器
    public Person() {
    }

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }


    public abstract void eat();

    /* public  void eat(){
         System.out.println("吃");
     }*/
    
    public void walk() {
        System.out.println("走");
    }
}

public class AbstractTest {
    public static void main(String[] args) {

        //当类是抽象类时，不能实例化对象
        //Person person = new Person();

        //创建抽象类的匿名子类
        Person p = new Person() {
            @Override
            public void eat() {
                System.out.println("好好吃饭");
            }
        };
        method1(p);
    }

    public static void method1(Person p){
        p.eat();
        p.walk();
    }
}
```

作用：方便写

缺点：可读性差



# 八、模板方法的设计模式

将代码中易变的抽离出来，单独实现，其他相对固定的代码就当成模板。

**举例：**

```java
public class TemplateTest {
    public static void main(String[] args) {
        method();
    }

    public static void method() {
        //现在距离1970年的时间的毫秒值
        long start = System.currentTimeMillis();
        TiTa tiTa = new TiTa();
        tiTa.code();

        long end = System.currentTimeMillis();
        System.out.println("花费的时间为:" + (end - start));


    }
}

abstract class Tempalte {


    public abstract void code();
}

class TiTa extends Tempalte {


    @Override
    public void code() {

        //求1000以内的质数
        for (int i = 2; i < 1000; i++) {
            //标记
            boolean flag = true;
            for (int j = 2; j < Math.sqrt(i); j++) {

                if (i % j == 0) {
                    flag = false;
                }

            }

            if (flag) {
                System.out.println(i);
            }
        }
    }
}
```

# 九、接口——interface

## 1.接口interface的简述

1. 接口是用interface来定义的
2. Java中接口和类是并列结构

## 2.interface接口的使用

1. 如何定义接口：定义接口中的成员

2. **接口中不能定义构造器**！意味着接口不可以实例化

3. Java开发中，接口通过类去实现（implements）的方式来使用

   + 如果实现类覆盖了接口中的所有抽象方法，则此实现类可以实例化
   + 如果实现类没有覆盖接口中的抽象方法，则此实现类仍然为一个抽象类

4. **Java类可以实现多个接口** ---> 弥补Java单继承的局限性

   格式：Class A implements BB,CC,DD

5. 接口与接口之间可以继承，而且可以多继承

6. 接口的具体使用，体现多态性

7. 接口，实际上可以看做是一个规范

**使用举例**

```java
class Computer{
	
	public void transferData(USB usb){//USB usb = new Flash();
		usb.start();
		
		System.out.println("具体传输数据的细节");
		
		usb.stop();
	}
	
	
}

interface USB{
	//常量：定义了长、宽、最大最小的传输速度等
	
	void start();
	
	void stop();
	
}

class Flash implements USB{

	@Override
	public void start() {
		System.out.println("U盘开启工作");
	}

	@Override
	public void stop() {
		System.out.println("U盘结束工作");
	}
	
}

class Printer implements USB{
	@Override
	public void start() {
		System.out.println("打印机开启工作");
	}

	@Override
	public void stop() {
		System.out.println("打印机结束工作");
	}
	
}
```



**使用总结**

 	1. 接口使用上也满足多态性
 	2. 接口，实际上就是定义了一种规范
 	3. 开发中，体会面向接口编程！

## 3.Java8中关于接口的新特性

JDK7以及之前：只能定义全局常量和抽象方法

	+ 全局常量：public static final 可以省略
	+ 抽象方法：public abstract可以省略

**JDK8之后**：**除了可以定义全局常量和抽象方法，还可以定义静态方法和default方法**

	+ Java8中，可以为接口添加静态方法和默认方法。从技术角度来说，这是完全合法的，只是它看起来违反了接口作为一个抽象定义的理念。
	+ 静态方法：用static修饰
	+ 默认方法：**用default修饰**
	+ 可以通过==**“接口.静态方法”直接调用静态方法。**==
	+ ==可以通过“**实现类对象.默认方法”直接调用默认方法**==

**JDK8后接口使用总结**

+ `知识点1`：**接口中定义的静态方法，只能通过“接口.静态方法”来调用**

+ `知识点2`：通过实现类的对象，可以调用默认方法，如果实现类重写了接口中的默认方法，调用时，仍然调用的是重写以后的方法

+ `知识点3`：**如果子类（实现类）继承的父类和实现的接口中声明的是同名同参数的默认方法，那么子类在没重写此方法的情况下，默认调用的是父类中的同名同参数的默认方法。---> 类优先原则**

+ `知识点4`：如果实现类实现了多个接口，并且这多个接口中都有同名同参数的默认方法，那么在实现类没有重写此方法的情况下，<span style='color:red;'>**报错。**  **--> 接口冲突**</span>

+ `知识点5`：如何在子类（实现类）的方法中调用父类、接口中被重写的方法

+ ```java
  public void myMethod(){
  		method3();//调用自己定义的重写的方法
  		super.method3();//调用的是父类中声明的
  		//调用接口中的默认方法
  		CompareA.super.method3();
  		CompareB.super.method3();
  }
  ```

  

## 4.接口和抽象类的异同

+ **相同点：**都不能被实例化，都可以被继承，都可以包含抽象方法
+ **不同点**：
  + 抽象类有构造器，接口没有构造器
  + 接口可以多实现，类只能单继承
  + 把抽象类和接口(java7,java8,java9)的定义、内部结构解释说明

## 5.接口的应用——代理模式

代理模式是Java开发中使用较多的一种设计模式。代理设计就是为其他对象提供一种代理以控制对这个对象的访问。

**代码示例：**

```java
/*
    接口的应用：代理模式
*/
public class ProxyTest {
    public static void main(String[] args) {

        NetWork netWork = new ProxyServer(new RealServer());
        netWork.browse();

    }
}

interface NetWork {

    public abstract void browse();
}

// 被代理类
class RealServer implements NetWork {


    @Override
    public void browse() {
        System.out.println("真实服务器联网");
    }
}

// 代理类
class ProxyServer implements NetWork {
    private NetWork netWork;

    public ProxyServer(NetWork netWork) {
        this.netWork = netWork;
    }

    public void check() {
        System.out.println("联网之前的检查工作");
    }

    @Override
    public void browse() {
        check();
        netWork.browse();
    }
}
```

### 5.1.代理模式应用场景

- 安全代理：屏蔽对真实角色的直接访问
- 远程代理：通过代理类处理远程方法调用（RM）
- 延迟加载：先加载轻量级的代理对象，真正需要再加载真实对象 比如你要开发一个大文档查看软件，大文档中有大的图片，有可能一个图片有100MB，在打开文件时，不可能将所有的图片都显示出来，这样就可以使用代理模式，当需要查看图片时，用 proxy来进行大图片的打开。

### 5.2代理模式分类

- 静态代理（静态定义代理类）
- 动态代理（动态生成代理类）JDK自带的动态代理，需要反射等知识

## 6.接口的应用——工厂模式

**解决的问题**

实现了创建者与调用者的分离，即将创建对象的具体过程屏蔽隔离起来，达到提高灵活性的目的。

**具体模式：**

- 简单工厂模式：用来生产同一等级结构中的任意产品。（对于增加新的产品，需要修改已有代码）
- 工厂方法模式：用来生产同一等级结构中的固定产品。（支持增加任意产品)
- 抽象工厂模式：用来生产不同产品族的全部产品。（对于增加新的产品，无能为力；支持增加产品族)