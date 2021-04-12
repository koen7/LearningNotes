**Java8新特性汇总**

![image-20200507101934984](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507101936.png)

**Java8的改进**

+ 速度更快
+ 代码更少（增加了新的语法：Lambda表达式）
+ 引入更强大的Stream API
+ 便于并行
+ 最大化减少空指针异常：Optional API
+ Nashorn引擎，允许在JVM上运行JS应用
+ 并行流就是把一个内容分成多个数据块，并用不同的线程分别处理每个数据块的流。相比较串行的流，并行流可以很大程度上提高程序的**执行效率**
+ Java8中将并行进行了优化，我们可以很容易的对数据进行并行操作。Stream API可以声明性的通过`pareallel()和sequential()`在并行和顺序流之间进行切换

# 一、Lambda表达式

## 1.Lambda表达式概述

Lambda是一个匿名函数，可以把Lambda表达式理解为是一段可以传递的代码（将代码像数据一样进行传递）。使用它可以写出更简洁、更灵活的代码。作为一种更紧凑的代码风格，使Java的语言表达能力得到了提升。

## 2.使用Lambda表达式举例

**示例一：调用Runnable接口**

```java
@Test
    public void test() {
        //未使用Lambda表达式
        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("我爱北京天安门");
            }
        };

        r1.run();

        System.out.println("***************");
        //使用Lambda表达式
        Runnable r2 = () -> System.out.println("我爱你中国~");
        r2.run();
    }
```

**示例二：调用Comparator接口**

```java
@Test
    public void test2() {
        //未使用Lambda表达式
        Comparator<Integer> com1 = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1, o2);
            }
        };

        int compare1 = com1.compare(12, 24);
        System.out.println(compare1);
        System.out.println("******************");
        //使用Lambda表达式
        Comparator<Integer> com2 = (o1, o2) -> Integer.compare(o1, o2);
        int compare2 = com2.compare(50, 1);
        System.out.println(compare2);

    }
```

## 3.怎么使用Lambda表达式

### 1.Lambda表达式基本语法

1. 举例：`(o1,o2) -> Integer.compare(o1,o2)`
2. **<span style='color:red;'>格式</span>**
   + `->` ：lambda操作符 或 箭头操作符
   + ` ->` 左边：lambda表达式形参列表（**<span style='color:red;'>也就是接口中抽象方法的形参列表</span>**）
   + `->`右边：lambda体（**<span style='color:red;'>也就是接口中抽象方法的方法体</span>**）



### 2.Lambda表达式使用（包含六种情况）

**1.语法格式一：无参，有返回值**

```
Runnable r1 = () -> {System.out.println("hello lambda!")}
```

**2.语法格式二：有一个参数，但没有返回值**

```
Consumer<String> con = (String s) -> {System.out.println(s)};
```

**3.语法格式三：<span style='color:red;'>数据类型可以省略</span>，因为可由编译器推断得出，称为类型推断**

```
Consumer<String> con = (s) -> {System.out.println(s)};
```

**4.语法格式四：Lambda若只有一个参数时，小括号也可以省略**

```
Consumer<String> con = s -> {System.out.println(s)};
```

**5.语法格式五：Lambda需要两个以上的参数，多条执行语句，并且可以有返回值。**

```java
Comparator<Integer> com = (o1,o2) -> {
	System.out.println(o1);
	System.out.println(o2);
	return Integer.compare(o1,o2);
};
```

**6.语法格式六：当lambda体只有一条语句时，return和大括号若都有，都可以省略**

```
Comparator<Integer>com = (o1,o1) ->	Integer.compare(o1,o2);
```

```
注意：若return省略，大括号一定要省略。
	 若大括号省略，return可以不省略。
```

**代码示例**

```java
//语法格式一：无参，有返回值
    @Test
    public void test(){
        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("lambda语法格式一：无参，有返回值");
            }
        };

        r1.run();

        System.out.println("********************");

        Runnable r2 = () -> {
            System.out.println("lambda语法格式一：无参，有返回值");
        };
        r2.run();
    }

    //语法格式二：有一个参数，有返回值
    @Test
    public void test2(){
        Consumer<String> con = new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };

        con.accept("有一个参数，有返回值");

        System.out.println("****************");

        Consumer<String> con2 =(String s) -> {
            System.out.println(s);
        };
        con2.accept("有一个参数，有返回值");
    }


    @Test
    public void test3(){
        Consumer<String> con = new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };

        con.accept("有一个参数时，数据类型可以省略，因为类型推断");

        System.out.println("****************");

        Consumer<String> con2 =(s) -> {
            System.out.println(s);
        };
        con2.accept("有一个参数时，数据类型可以省略，因为类型推断");
    }

    @Test
    public void test4(){
        Consumer<String> con = new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };

        con.accept("有一个参数时，小括号可以省略");

        System.out.println("****************");

        Consumer<String> con2 =s -> {
            System.out.println(s);
        };
        con2.accept("有一个参数时，小括号可以省略");
    }

    @Test
    public void test5(){
        Comparator<Integer> com = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1,o2);
            }
        };
        com.compare(12,50);

        System.out.println("*******************");


        Comparator<Integer> com2 = (o1, o2) -> {
            System.out.println(o1);
            System.out.println(o2);
            return Integer.compare(o1,o2);
        };

        com2.compare(100,10);
    }
```

## 4.Lambda表达式总结

+ `->` 左边：lambda形参列表的参数类型可以省略(类型推断)；如果lambda形参列表只有一个参数，其一对()也可以省略
+ `->` 右边：lambda体应该使用一对{}包裹；如果lambda体只有一条执行语句（可能是return语句），省略这一对{}和return关键
+ Lambda表达式的本质：作为函数式接口的实例
+ 如果一个接口中，只声明了一个抽象方法，则此接口就称为函数式接口。我们可以在一个接口上使用 `@FunctionalInterface` 注解，这样做可以检查它是否是一个函数式接口。
+ 因此以前用匿名实现类表示的现在都可以用Lambda表达式来写。

# 二、函数式接口

## 1.函数式接口概述

+ **<span style='color:red;'>只包含一个抽象方法的接口，称为函数式接口</span>**
+ 可以通过Lambda表达式来创建该接口的对象。（若Lambda表达式抛出一个受检异常，那么该异常需要在目标接口的抽象方法上进行声明）
+ 可以在一个接口上使用`@FunctionalInterface`注解，这样做可以检查它是否是一个函数式接口。同时javadoc也会包含一条声明，说明这个接口是一个函数式接口
+ Lambda表达式的本质：是函数式接口的一个实例。
+ 在`java.util.function`包下定义了java8丰富的函数式接口

**自定义函数式接口**

```java
@FunctionalInterface
public interface MyInterface {
    void method1();
}
```

## 2.Java内置的函数式接口

### 1.四大核心函数式接口

![image-20200507110931093](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507110933.png)

**应用举例**

```java
public class LambdaTest3 {
    //    消费型接口 Consumer<T>     void accept(T t)
    @Test
    public void test1() {
        //未使用Lambda表达式
        Learn("java", new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println("学习什么？ " + s);
            }
        });
        System.out.println("====================");
        //使用Lambda表达
        Learn("html", s -> System.out.println("学习什么？ " + s));

    }

    private void Learn(String s, Consumer<String> stringConsumer) {
        stringConsumer.accept(s);
    }

    //    供给型接口 Supplier<T>     T get()
    @Test
    public void test2() {
        //未使用Lambdabiaodas
        Supplier<String> sp = new Supplier<String>() {
            @Override
            public String get() {
                return new String("我能提供东西");
            }
        };
        System.out.println(sp.get());
        System.out.println("====================");
        //使用Lambda表达
        Supplier<String> sp1 = () -> new String("我能通过lambda提供东西");
        System.out.println(sp1.get());
    }

    //函数型接口 Function<T,R>   R apply(T t)
    @Test
    public void test3() {
        //使用Lambda表达式
        Employee employee = new Employee(1001, "Tom", 45, 10000);

        Function<Employee, String> func1 =e->e.getName();
        System.out.println(func1.apply(employee));
        System.out.println("====================");

        //使用方法引用
        Function<Employee,String>func2 = Employee::getName;
        System.out.println(func2.apply(employee));

    }

    //断定型接口 Predicate<T>    boolean test(T t)
    @Test
    public void test4() {
        //使用匿名内部类
        Function<Double, Long> func = new Function<Double, Long>() {
            @Override
            public Long apply(Double aDouble) {
                return Math.round(aDouble);
            }
        };
        System.out.println(func.apply(10.5));
        System.out.println("====================");

        //使用Lambda表达式
        Function<Double, Long> func1 = d -> Math.round(d);
        System.out.println(func1.apply(12.3));
        System.out.println("====================");

        //使用方法引用
        Function<Double,Long>func2 = Math::round;
        System.out.println(func2.apply(12.6));

    }
}
```

### 2.其他函数式接口

![image-20200507111244577](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507111245.png)

## 3.使用总结

### 1.何时使用lambda表达式

当需要对一个函数式接口实例化的时候，可以使用lambda表达式

### 2.何时使用给定的函数式接口

如果我们开发中需要定义一个函数式接口，首先看看在已有的jdk提供的函数式接口是否提供了满足需求的函数式接口。如果有，则直接调用即可，不需要自己再自定义了。

# 三、方法的引用

## 1.方法引用概述

方法引用可以看做是Lambda表达式深层次的表达。换句话说，**方法引用就是Lambda表达式，**也就是函数式接口的一个实例，通过方法的名字来指向一个方法。

## 2.使用场景

当要传递给Lambda体的操作，已经有实现的方法了，就可以使用方法引用！

## 3.使用格式

**<span style='color:red;'>类（或对象）：：方法名</span>**

## 4.使用情况

+ 情况1	对象`::`非静态方法
+ 情况2    类`::`静态方法
+ 情况3    类`::`非静态方法

## 5.使用要求

+ **<span style='color:red;'>要求接口中的抽象方法的形参列表和返回值类型与方法引用的形参列表和返回值相同</span>**（针对于情况1、情况2）
+ 当函数式接口方法的第一个参数是需要引用方法的调用者，并且第二个参数是需要引用方法的参数（或无参数）时：`ClassName:methodName`（针对于情况3）

## 6.使用建议

如果给函数式接口提供实例，恰好满足方法引用的使用情境，就可以考虑使用方法引用给函数式接口提供实例。如果不熟悉方法引用，那么还可以使用lambda表达式。

## 7.使用举例

```java
public class MethodRefTest {

    // 情况一：对象 :: 实例方法
    //Consumer中的void accept(T t)
    //PrintStream中的void println(T t)
    @Test
    public void test1() {
        //使用Lambda表达
        Consumer<String> con1 = str -> System.out.println(str);
        con1.accept("中国");
        System.out.println("====================");

        //使用方法引用
        PrintStream ps = System.out;
        Consumer con2 = ps::println;
        con2.accept("China");

    }

    //Supplier中的T get()
    //Employee中的String getName()
    @Test
    public void test2() {
        //使用Lambda表达
        Employee emp = new Employee(1001, "Bruce", 34, 600);
        Supplier<String> sup1 = () -> emp.getName();
        System.out.println(sup1.get());
        System.out.println("====================");

        //使用方法引用
        Supplier sup2 = emp::getName;
        System.out.println(sup2.get());


    }

    // 情况二：类 :: 静态方法
    //Comparator中的int compare(T t1,T t2)
    //Integer中的int compare(T t1,T t2)
    @Test
    public void test3() {
        //使用Lambda表达
        Comparator<Integer> com1 = (t1, t2) -> Integer.compare(t1, t2);
        System.out.println(com1.compare(32, 45));
        System.out.println("====================");

        //使用方法引用
        Comparator<Integer> com2 = Integer::compareTo;
        System.out.println(com2.compare(43, 34));
    }

    //Function中的R apply(T t)
    //Math中的Long round(Double d)
    @Test
    public void test4() {
        //使用匿名内部类
        Function<Double, Long> func = new Function<Double, Long>() {
            @Override
            public Long apply(Double aDouble) {
                return Math.round(aDouble);
            }
        };
        System.out.println(func.apply(10.5));
        System.out.println("====================");

        //使用Lambda表达式
        Function<Double, Long> func1 = d -> Math.round(d);
        System.out.println(func1.apply(12.3));
        System.out.println("====================");

        //使用方法引用
        Function<Double, Long> func2 = Math::round;
        System.out.println(func2.apply(12.6));


    }

    // 情况三：类 :: 实例方法
    // Comparator中的int comapre(T t1,T t2)
    // String中的int t1.compareTo(t2)
    @Test
    public void test5() {
        //使用Lambda表达式
        Comparator<String> com1 = (s1, s2) -> s1.compareTo(s2);
        System.out.println(com1.compare("abd", "aba"));
        System.out.println("====================");

        //使用方法引用
        Comparator<String> com2 = String::compareTo;
        System.out.println(com2.compare("abd", "abc"));
    }

    //BiPredicate中的boolean test(T t1, T t2);
    //String中的boolean t1.equals(t2)
    @Test
    public void test6() {
        //使用Lambda表达式
        BiPredicate<String, String> pre1 = (s1, s2) -> s1.equals(s2);
        System.out.println(pre1.test("abc", "abc"));
        System.out.println("====================");

        //使用方法引用
        BiPredicate<String, String> pre2 = String::equals;
        System.out.println(pre2.test("abc", "abd"));

    }

    // Function中的R apply(T t)
    // Employee中的String getName();
    @Test
    public void test7() {
        //使用Lambda表达式
        Employee employee = new Employee(1001, "Tom", 45, 10000);

        Function<Employee, String> func1 =e->e.getName();
        System.out.println(func1.apply(employee));
        System.out.println("====================");

        //使用方法引用
        Function<Employee,String>func2 = Employee::getName;
        System.out.println(func2.apply(employee));
    }
}
```

# 四、构造器和数组的引用

## 1.使用格式

构造器引用：`类::new`

数组引用：`数组类型[]::new`

## 2.使用要求

### 1.构造器引用

和方法引用类似，函数式接口的抽象方法的形参列表和构造器的形参列表一致。抽象方法的返回值类型就是构造器所属的类的类型

### 2.数组引用

可以把数组看做是一个特殊的类，写法与构造器引用一致。

## 3.使用举例

### 1.构造器引用

```java
//构造器引用
//Supplier中的T get()
@Test
public void test1() {
    //使用匿名内部类
    Supplier<Employee> sup = new Supplier<Employee>() {
        @Override
        public Employee get() {
            return new Employee();
        }
    };
    System.out.println(sup.get());
    //使用Lambda表达式
    System.out.println("====================");
    Supplier<Employee> sup1 = () -> new Employee(1001, "Tom", 43, 13333);
    System.out.println(sup1.get());

    //使用方法引用
    Supplier<Employee> sup2 = Employee::new;
    System.out.println(sup2.get());

}

//Function中的R apply(T t)
@Test
public void test2() {
    //使用Lambda表达式
    Function<Integer, Employee> func1 = id -> new Employee(id);
    Employee employee = func1.apply(1001);
    System.out.println(employee);
    System.out.println("====================");

    //使用方法引用
    Function<Integer, Employee> func2 = Employee::new;
    Employee employee1 = func2.apply(1002);
    System.out.println(employee1);

}

//BiFunction中的R apply(T t,U u)
@Test
public void test3() {
    //使用Lambda表达式
    BiFunction<Integer, String, Employee> func1 = (id, name) -> new Employee(id, name);
    System.out.println(func1.apply(1001, "Tom"));
    System.out.println("====================");

    //使用方法引用
    BiFunction<Integer, String, Employee> func2 = Employee::new;
    System.out.println(func2.apply(1002, "Jarry"));
}
```

### 2.数组引用

```java
//数组引用
//Function中的R apply(T t)
@Test
public void test4() {
    //Function的T:代表数组的长度
    Function<Integer, String[]> func1 = length -> new String[length];
    String[] arr1 = func1.apply(5);
    System.out.println(Arrays.toString(arr1));

    System.out.println("====================");

    //使用方法引用
    Function<Integer,String[]>func2=String[]::new;
    String[] arr2 = func2.apply(10);
    System.out.println(Arrays.toString(arr2));
}
```



# 五、StreamAPI

## 1.Stream API概述

+ **Stream关注的是对数据的运算**,与CPU打交道；集合关注的是数据的存储，与内存打交道。
+ Java8提供了一套API，使用这套API可以对内存中的数据进行过滤、排序、映射、归约等操作。类似于SQL对数据库中表的相关操作。
+ Stream是数据渠道,用于操作数据源（集合、数组等）所生成的元素序列。“集合讲的是数据,Stream讲的是计算！”

**使用注意点：**

①Stream不会存储元素

②Stream不会改变源对象。相反，他们会返回一个持有结果的新Stream。

**<span style='color:red;'>③Stream操作是延迟执行的。这意味着他们会等到终止操作结束完，才执行中间操作。</span>**



## 2.Stream使用流程

1. Stream的实例化
2. 一系列的中间操作（过滤、映射...）
3. 终止操作

![image-20200507114714381](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507114715.png)



**使用流程中的注意点：**

+ 一个中间操作链，对数据源的数据进行处理
+ 一旦执行终止操作，就执行中间操作链，并产生结果。之后，不会再被使用。



## 3.使用方法

### 1.步骤一 创建Stream

**1.创建方式一： 通过集合**

Java8的Collection接口被扩展，提供了两个获取流的方法：

+ `default Stream<E> stream()`：返回一个顺序流
+ `default Stream<E> parallelStream()`：返回一个并行流

**2.创建方式二： 通过数组**

Java8中的Arrays的静态方法stream()可以获取数组流

+ 调用Arrays类的`static Stream<T> stream(T[] array)`：返回一个流，类型就是数组的类型
+ 重载形式，能够处理对应基本类型的数组：
  + `public static IntStream stream（int[] array）`
  + `public static LongStream stream（long[] array）`
  + `public static DoubleStream stream（double[] array）`

**3.创建方式三：通过Stream的of()方法**

可以调用Stream类静态方法of()，通过显示值创建一个流。可以用于接收任意数量的参数

+ `static<T> Stream<T> of(T t)`：返回一个流

**4.创建方式四：创建无限流**

+ 迭代：`static<T> Stream<T> iterate(final T seed, final UnaryOperator<T> f)`
+ 生成：`static<T> Stream<T> generate`

**代码示例**：

```java
 //通过集合创建Stream
    @Test
    public void test(){
        List<Employee> employees = EmployeeData.getEmployees();
        //创建串行stream
        Stream<Employee> stream = employees.stream();
        //创建并行流 stream
        Stream<Employee> parallelStream = employees.parallelStream();
    }

    //通过Arrays数组的静态方法stream()
    @Test
    public void test2(){
        Employee e1 = new Employee(1001,"tom");
        Employee e2 = new Employee(1002,"jerry");

        Employee[] employees = new Employee[]{e1,e2};

        Stream<Employee> stream = Arrays.stream(employees);
    }

    //通过Stream的of()方法
    @Test
    public void test3(){
        Stream<String> of = Stream.of("abc","ccc");
    }

  //创建 Stream方式四：创建无限流
    @Test
    public void test4() {

        //迭代
        //public static<T> Stream<T> iterate(final T seed, final UnaryOperator<T> f)
        //遍历前10个偶数
        Stream.iterate(0, t -> t + 2).limit(10).forEach(System.out::println);

        //生成
        //public static<T> Stream<T> generate(Supplier<T> s)
        Stream.generate(Math::random).limit(10).forEach(System.out::println);
    }
```

### 2.步骤二 中间操作

多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理！而在终止操作时一次性全部处理，称为“惰性求值”

#### 1.筛选与切片

![image-20200507115721208](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507115722.png)

**代码示例**

```java
@Test
    public void test() {
        //1.filter(Predicate p):接收Lambda，从流中排除某些元素
        List<Employee> list = EmployeeData.getEmployees();
        //创建流
        Stream<Employee> stream = list.stream();
        //查询工资大于7000的员工
        stream.filter(e -> e.getSalary() > 7000).forEach(System.out::println);
        System.out.println();

        //2.distinct：去重
        list.add(new Employee(1010, "刘强东", 40, 8000));
        list.add(new Employee(1010, "刘强东", 40, 8000));
        list.add(new Employee(1010, "刘强东", 40, 8000));
        list.add(new Employee(1010, "刘强东", 40, 8000));
        list.add(new Employee(1010, "刘强东", 40, 8000));
        list.stream().distinct().forEach(System.out::println);
        System.out.println();

        //3.limit(n):限制流，输出前n个数据
        list.stream().limit(3).forEach(System.out::println);
        System.out.println();

        //4.skip(n)：跳过前n个数据。
        list.stream().skip(3).forEach(System.out::println);
    }
```

#### 2.映射

![image-20200507115738168](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507115739.png)

**代码示例**

```java
//Stream中间操作：映射
    @Test
    public void test2(){
        List<Employee> list = EmployeeData.getEmployees();

        //练习1：获取员工姓名长度大于3的员工的姓名。
        list.stream().map(e -> e.getName()).filter(name -> name.length() > 3).forEach(System.out::println);
        System.out.println();

        //练习2：使用map()中间操作实现flatMap()中间操作方法
        List<String> list2 = Arrays.asList("aa","bb","cc","dd");
        Stream<Stream<Character>> streamStream = list2.stream().map(StreamAPITest1::fromStringtoStream);
        streamStream.forEach(s -> {
            s.forEach(System.out::println);
        });
        System.out.println();
        //flatMap(Function f)——接收一个函数作为参数，将流中的每个值都换成另一个流，然后把所有流连接成一个流。
        Stream<Character> characterStream = list2.stream().flatMap(StreamAPITest1::fromStringtoStream);
        characterStream.forEach(System.out::println);

    }
```

#### 3.排序

![image-20200507115751929](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507115752.png)

**代码示例**

```java
// Stream中间操作：排序
    @Test
    public void test3(){
        //sort()：自然排序
        List<Employee> list = EmployeeData.getEmployees();
        list.stream().sorted().forEach(System.out::println);

        System.out.println();
        //sort(Comparator com):定制排序
        list.stream().sorted((e1,e2) -> {
            int ageValue  = Integer.compare(e1.getAge(), e2.getAge());
            if (ageValue != 0){
                return ageValue;
            }else{
                return -Double.compare(e1.getSalary(),e2.getSalary());
            }

        }).forEach(System.out::println);
    }
```

### 3.步骤三 终止操作

+ 终止操作会从流的流水线生成结果。其结果可以是任何不是流的值，例如：List、Integer，甚至是void
+ 流进行了终止操作后，不能再次使用

#### 1.匹配与查找

![image-20200507120245034](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507120246.png)

![image-20200507120300023](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507120301.png)

**代码示例**

```java
//1-匹配与查找
    @Test
    public void test1(){
        List<Employee> employees = EmployeeData.getEmployees();

        //allMatch(Predicate p)——检查是否匹配所有元素。
        //练习：是否所有的员工的年龄都大于18
        boolean b = employees.stream().allMatch(e -> e.getAge() > 18);
        System.out.println(b);

        //anyMatch(Predicate p)——检查是否至少匹配一个元素。
        //练习：是否存在员工的工资大于 5000
        boolean b1 = employees.stream().anyMatch(e -> e.getSalary() > 5000);
        System.out.println(b1);

        //noneMatch(Predicate p)——检查是否没有匹配的元素。
        //练习：是否存在员工姓“雷”
        boolean b2 = employees.stream().noneMatch(e -> e.getName().startsWith("雷"));
        System.out.println(b2);

        //findFirst——返回第一个元素
        Optional<Employee> first = employees.stream().sorted().findFirst();
        System.out.println(first);

        //findAny——返回当前流中的任意元素
        Optional<Employee> employee = employees.stream().findAny();
        System.out.println(employee);

    }

@Test
    public void test2(){
        List<Employee> employees = EmployeeData.getEmployees();
        // count——返回流中元素的总个数
        long count = employees.stream().count();
        System.out.println(count);

        //max(Comparator c)——返回流中最大值
        //练习：返回最高的工资
        Optional<Double> max = employees.stream().map(e -> e.getSalary()).max((e1, e2) -> Double.compare(e1, e2));
        System.out.println(max);

        //min(Comparator c)——返回流中最小值
        //练习：返回最低工资的员工
        Optional<Employee> min = employees.stream().min((e1, e2) -> Double.compare(e1.getSalary(), e2.getSalary()));
        System.out.println(min);

        //forEach(Consumer c)——内部迭代
        //使用集合的遍历操作
        employees.stream().forEach(System.out::println);

    }
```

#### 2.归约

![image-20200507120318120](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507120320.png)

**代码示例**

```java
//2-归约
    @Test
    public void test3() {
        //reduce(T identity, BinaryOperator)——可以将流中元素反复结合起来，得到一个值。返回 T
        //练习1：计算1-10的自然数的和
        List<Integer> list = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        Integer sum = list.stream().reduce(0, (a, b) -> a + b);
        //Integer sum = list.stream().reduce(0, Integer::sum);
        System.out.println(sum);

        //reduce(BinaryOperator) ——可以将流中元素反复结合起来，得到一个值。返回 Optional<T>
        //练习2：计算公司所有员工工资的总和
        List<Employee> employeeList = EmployeeData.getEmployees();
        Optional<Double> salary = employeeList.stream().map(e -> e.getSalary()).reduce(Double::sum);
        System.out.println(salary);
    }
```

#### 3.收集

![image-20200507120339459](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507120340.png)

Collector接口中方法的实现决定了如何对流执行收集的操作（如收集到List、Set、Map）

Collectors工具类提供了很多静态的方法，可以方便得创建常见收集器实例。具体方法如下

![image-20200507120628702](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507120630.png)

![image-20200507120648175](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200507120649.png)

**代码示例：**

```java
//3-收集
@Test
public void test4(){
    //collect(Collector c)——将流转换为其他形式。接收一个 Collector接口的实现，用于给Stream中元素做汇总的方法
    //练习1：查找工资大于6000的员工，结果返回为一个List或Set
    List<Employee> employees = EmployeeData.getEmployees();
    List<Employee> employeeList = employees.stream().filter(e -> e.getSalary() > 6000).collect(Collectors.toList());

    employeeList.forEach(System.out::println);
    System.out.println();
    Set<Employee> employeeSet = employees.stream().filter(e -> e.getSalary() > 6000).collect(Collectors.toSet());
    employeeSet.forEach(System.out::println);
}
```

# 六、Optional类

## 1.Optional类的概述

+ **为了解决java中的空指针问题而生**
+ Optional<T> 类(java.util.Optional)是一个容器类,它可以保存类型T的值，代表这个值存在。或者仅仅保存null；表示这个值不存在。原来用null表示一个值不存在，现在Optional可以更好的表达这个概念。并且可以避免空指针异常。

## 2.Optional类提供的方法

Optional类提供了很多方法，可以不用在显示的进行空值检验了。

### 1.创建Optional类对象的方法

+ Optional.of(T t)：创建一个Optional实例，t必须非空。
+ Optional.empty()：创建一个空的Optional实例
+ Optional.ofNullable(T t)： t可以为null

### 2.判断Optional容器是否包含对象

+ boolean isPresent()：判断是否包含对象
+ void ifPresent(Consumer<? super Consumer)：如果有值，就这行Consumer接口的实现代码，并且该值会作为参数传递给它。

### 3.获取Optional容器的对象

+ T get()：如果对象包含值，返回该值，否则抛异常
+ T orElse(T orther)：如果对象包含值，返回该值。 否则返回指定的other对象
+ T orElseGet(Supplier <? extends t> other)：如果有值将其返回，否则返回由Supplier接口实现提供的对象
+ T orElseThrow(Supplier<? extends X> exceptionSupplier)：如果有值则将其返回，否则抛出由Supplier接口实现提供的异常。

**搭配使用**

+ `of()和get()`搭配使用，明确对象非空
+ `ofNullable()和orElse()` 搭配使用，不确定对象非空

## 3.应用举例

```java
public class OptionalTest {
    @Test
    public void test1() {
        //empty():创建的Optional对象内部的value = null
        Optional<Object> op1 = Optional.empty();
        if (!op1.isPresent()){//Optional封装的数据是否包含数据
            System.out.println("数据为空");
        }
        System.out.println(op1);
        System.out.println(op1.isPresent());

        //如果Optional封装的数据value为空，则get()报错。否则，value不为空时，返回value.
        System.out.println(op1.get());
    }
    @Test
    public void test2(){
        String str = "hello";
//        str = null;
        //of(T t):封装数据t生成Optional对象。要求t非空，否则报错。
        Optional<String> op1 = Optional.of(str);
        //get()通常与of()方法搭配使用。用于获取内部的封装的数据value
        String str1 = op1.get();
        System.out.println(str1);
    }
    @Test
    public void test3(){
        String str ="Beijing";
        str = null;
        //ofNullable(T t) ：封装数据t赋给Optional内部的value。不要求t非空
        Optional<String> op1 = Optional.ofNullable(str);
        System.out.println(op1);
        //orElse(T t1):如果Optional内部的value非空，则返回此value值。如果
        //value为空，则返回t1.
        String str2 = op1.orElse("shanghai");
        System.out.println(str2);
    }
}
```

使用Optional类避免产生空指针异常

```java
public class GirlBoyOptionalTest {

    //使用原始方法进行非空检验
    public String getGrilName1(Boy boy){
        if (boy != null){
            Girl girl = boy.getGirl();
            if (girl != null){
                return girl.getName();
            }
        }
        return null;
    }
    //使用Optional类的getGirlName()进行非空检验
    public String getGirlName2(Boy boy){
        Optional<Boy> boyOptional = Optional.ofNullable(boy);
        //此时的boy1一定非空,boy为空是返回“迪丽热巴”
        Boy boy1 = boyOptional.orElse(new Boy(new Girl("迪丽热巴")));

        Girl girl = boy1.getGirl();
        //girl1一定非空,girl为空时返回“古力娜扎”
        Optional<Girl> girlOptional = Optional.ofNullable(girl);
        Girl girl1 = girlOptional.orElse(new Girl("古力娜扎"));

        return girl1.getName();
    }

    //测试手动写的控制检测
    @Test
    public void test1(){

        Boy boy = null;
        System.out.println(getGrilName1(boy));

        boy = new Boy();
        System.out.println(getGrilName1(boy));

        boy = new Boy(new Girl("杨幂"));
        System.out.println(getGrilName1(boy));
    }
    //测试用Optional类写的控制检测
    @Test
    public void test2(){
        Boy boy = null;
        System.out.println(getGirlName2(boy));

        boy = new Boy();
        System.out.println(getGirlName2(boy));

        boy = new Boy(new Girl("杨幂"));
        System.out.println(getGirlName2(boy));

    }
}
```

