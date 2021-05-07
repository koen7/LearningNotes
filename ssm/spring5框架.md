# 1、Spring概述

## 1.1 Spring框架的概述

```
Spring是轻量级的开源的JavaEE框架
```

Spring为简化企业级开发而生，使用Spring，JavaBean就可以实现很多以前靠EJB才能实现的功能

<hr>

**Spring有两个核心部分：IOC和AOP**

+ IOC：Inversion of Control，控制反转，把创建对象的过程交给Spring来管理
+ AOP：面向切面，不修改源代码的情况下进行功能增强

```
Spring的特点
```

1. 方便解耦，简化开发
2. AOP编程支持
3. 方便程序测试
4. 方便和其他框架进行整合
5. 方便进行事务操作
6. 降低API开发难度

## 1.2 Spring入门案例

目前已经更新到5.3.6版本了，不过我还是和老师使用同一个版本5.2.6。

### 1.2.1 下载Spring5

```
下载地址：https://repo.spring.io/release/org/springframework/spring/
```

![image-20210503174934853](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210503174934853.png)

### 1.2.2 打开IDEA工具，创建普通Java工程

![image-20210503175019838](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210503175019838.png)

![image-20210503175032702](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210503175032702.png)

![image-20210503175123108](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210503175123108.png)

### 1.2.3导入Spring5相关jar包

![image-20210503175202612](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210503175202612.png)

![image-20210503175206878](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210503175206878.png)

![image-20210503175403157](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210503175403157.png)

### <span style="color:red">1.2.4创建普通类，在这个类创建普通方法</span>

```java
public class User {
    public void add(){
        System.out.println("add.....");
    }
}
```

### <span style="color:red">1.2.5 创建Spring配置文件，在配置文件中配置创建的对象</span>

（1）Spring配置文件使用.xml格式

![image-20210503175710590](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210503175710590.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">


    <!--配置User对象创建
        class属性: 类的全类名
    -->

    <bean id="user" class="com.atguigu.spring5.User"></bean>
</beans>
```

### 1.2.6编写测试代码

```java
    @Test
    public void testAdd(){
        //1.通过ApplicationContext读取spring的配置文件
        //ClassPathXmlApplicationContext(String configLocation)的configLocation参数：是src下的xml配置文件
        ApplicationContext context =
                new ClassPathXmlApplicationContext("bean1.xml");
        //2.通过ApplicationContext的getBean()获取配置的bean对象
        User user = context.getBean("user", User.class);
        System.out.println(user);
        user.add();

    }
```

**测试结果：**

![image-20210503180403922](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210503180403922.png)



# 2.IOC容器和Bean管理

## <span style="color:red">2.1 什么是IOC</span>

+ **<span style="color:red">定义：控制反转，把对象的创建以及对象之间的调用交给Spring来管理</span>**
+ <span style="color:red">使用IOC的目的：为了降低耦合度</span>
+ 入门案例就是IOC的实现

## 2.2 IOC底层原理

IOC的底层原理使用了这三种技术、分别是<span style="color:red"> xml解析、工厂模式、反射</span>

## <span style="color:red">2.3图解方式说明IOC底层原理</span>

![image-20210504104540905](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504104540905.png)

## 2.4 Spring提供IOC容器实现两种方式(两个接口)

1. **<span style="color:red">IOC思想是基于IOC容器的，IOC容器的底层就是对象工厂。</span>**
2. **<span style="color:red">Spring提供IOC容器实现的两种方式，也就是两个接口：</span>**
   1. <span style="color:red">BeanFactory：这是IOC容器的基本实现，是Spring内部使用的接口，不提供开发人员使用。**该接口在加载配置文件的时候不会创建对象，在获取（使用）才会去创建对象。**</span>
   2. <span style="color:red">ApplicationContext：是BeanFactory接口的子接口，提供更多更强大的功能，由开发人员使用。**该接口在加载配置文件的时候就会把配置文件中的对象进行创建**</span>

3. **`ApplicationContext接口`的两个实现类说明**

![image-20210504105344991](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504105344991.png)

(1) `FileSystemXmlApplicationContext`：对应类路径下的XML格式的配置文件

(2)`ClassPathXmlApplicationContext`：对应文件系统下的XML格式的配置文件(带盘符)

## 2.5 IOC容器—Bean管理(基于xml方式)

### <span style="color:red">2.5.1什么是Bean管理</span>

+ Bean管理指的是两个操作
  + 由Spring创建对象
  + 由Spring注入属性

### <span style="color:red">2.5.2Bean管理操作的两种方式</span>

1. 基于xml配置文件方式实现
2. 基于注解方式实现

### 2.5.3IOC操作Bean管理(基于xml配置文件方式)

**<span style="color:red">1.基于xml方式创建对象</span>**

```xml
    <!--1.创建Book对象的配置-->
    <bean id="book" class="com.atguigu.spring5.Book"></bean>
```

(1) 在Spring配置文件中，使用bean标签，bean标签里添加对应的属性，就可以实现对象的创建

(2)在bean标签中有很多属性，常用的属性有：

+ id属性：唯一标识
+ class属性：类的全路径(包名 +  类名)

<span style="color:red">(3)创建对象时，默认是通过无参构造器进行创建对象。</span>

**<span style="color:red">2.基于xml方式注入属性</span>**

+ DI：依赖注入，也就是注入属性

+ **<span style="color:red">第一种注入方式：使用set方法进行注入</span>**

  + <span style="color:red">创建类，定义属性以及添加对应的set方法</span>

  ```java
  public class Book {
  
      private String bname;
      private String bauthor;
  
      public void setBname(String bname) {
          this.bname = bname;
      }
  
      public void setBauthor(String bauthor) {
          this.bauthor = bauthor;
      }
  }
  ```

  + <span style="color:red">在spring配置文件中进行属性注入</span>

    ```xml
        <!--1.创建Book对象的配置-->
        <bean id="book" class="com.atguigu.spring5.Book">
            <!--2.set方法 注入属性
                name属性：类里面属性的名称
                value属性：向属性中注入的值
            -->
            <property name="bname" value="易经经"></property>
            <property name="bauthor" value="达摩老祖"></property>
        </bean>
    ```

+ **<span style="color:red">第二种注入方式：使用有参数的构造器进行注入</span>**
  
  + <span style="color:red">创建类，定义属性，创建属性对应的有参构造方法</span>

```java
public class Order {

    private String oname;
    private String address;

    public Order(String oname, String address) {
        this.oname = oname;
        this.address = address;
    }
}
```

+ 

  + <span style="color:red">在Spring配置文件中进行有参方法注入属性</span>

  ```xml
      <!--1.创建Order对象的配置-->
      <bean id="order" class="com.atguigu.spring5.Order">
          <!--2.有参构造方法的方式 注入属性
              name属性：类里面有参构造方法中的属性名称
              value属性：向属性中注入的值
          -->
          <constructor-arg name="oname" value="电脑"/>
          <constructor-arg name="address" value="中国"/>
      </bean>
  ```

  + <span style="color:red">测试方法</span>

  ```java
   @Test
      public void testOrder(){
          //1.通过ApplicationContext读取spring的配置文件
          //ClassPathXmlApplicationContext(String configLocation)的configLocation参数：是src下的xml配置文件
          ApplicationContext context =
                  new ClassPathXmlApplicationContext("bean1.xml");
          //2.通过ApplicationContext的getBean()获取配置的bean对象
          Order order = context.getBean("order", Order.class);
          System.out.println(order);
          order.testOrder();
  
      }
  ```

  + <span style="color:red">运行结果</span>

  ![image-20210504115211191](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504115211191.png)

+ **<span style="color:red">第三种方式：通过p名称空间注入属性值(了解)</span>**

  + 在配置文件中添加p名称空间

  ![image-20210504120443586](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504120443586.png)

  + 进行属性注入，在bean标签里进行操作(底层还是基于set注入方式)

  ```xml
      <!--通过p名称空间注入-->
      <bean id="book" class="com.atguigu.spring5.Book" p:bname="九阳神功" p:bauthor="无名氏"></bean>
  ```

  + 测试结果：

  ![image-20210504120711951](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504120711951.png)


**<span style="color:red">3.基于xml注入null值和特殊字符</span>**

+ <span style="color:red">null值</span> ：通过`<null>`标签将引用类型字段的值设置为null

```xml
    <!--1.创建Book对象的配置-->
    <bean id="book" class="com.atguigu.spring5.Book">
        <!--2.set方法 注入属性
            name属性：类里面属性的名称
            value属性：向属性中注入的值
        -->
        <property name="bname" value="喜剧之王"></property>
        <property name="bauthor" value="周星驰"></property>
        <!--null值注入-->
        <property name="address">
            <null/>
        </property>
    </bean>
```

​	测试结果：![image-20210504123533882](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504123533882.png)

+ <span style="color:red">属性值包含特殊字符</span>
  + 把<>进行转义 `&lt;&gt;`
  + 把特殊符号内容写到CDATA中

```xml
<!--特殊字符-->
<property name="address">
   <value><![CDATA[<<南京>>]]></value>
</property>
```

运行结果：![image-20210504124134131](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504124134131.png)

### <span style="color:red">2.5.4IOC操作Bean管理(基于xml方式注入外部bean、内部bean、级联赋值)</span>

**<span style="color:red">1.注入属性——外部bean</span>**

(1)创建两个类service和dao类

(2)在service类中调用dao里面的方法

```java
public class UserService {

    private UserDao userDao;
    //生成set方法用于注入属性
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void add(){
        System.out.println("service add....");
        userDao.update();
    }
}
```

(3)在Spring配置文件中进行配置

```xml
    <!--创建service和dao对象-->
    <bean id="userService" class="com.atguigu.spring5.service.UserService">
        <property name="userDao" ref="userDao"/>
    </bean>

    <bean id="userDao" class="com.atguigu.spring5.dao.UserDaoImpl"></bean>
    
```

(4)测试方法

```java
    @Test
    public void testDao(){

        ApplicationContext context = new ClassPathXmlApplicationContext("bean2.xml");

        UserService userService = context.getBean("userService", UserService.class);

        userService.add();
    }
```

(5)运行结果：

![image-20210504131555312](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504131555312.png)



**<span style="color:red">2.注入属性——内部bean</span>**

(1)创建Emp实体类、Dept实体类

一对多关系：部门和员工

一个部门有多个员工，一个员工属于一个部门

部门是一、员工是多

```java
//部门类
public class Dept {
    private String dname;

    public void setDname(String dname) {
        this.dname = dname;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "dname='" + dname + '\'' +
                '}';
    }
}

//员工类
public class Emp {

    private String ename;
    private String gender;
    private Dept dept;

    public void setEname(String ename) {
        this.ename = ename;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public void setDept(Dept dept) {
        this.dept = dept;
    }

    public void testEmp() {
        System.out.println(ename + "::" + gender + "::" + dept);
    }
}
```

(2)在Spring配置文件中进行配置

```xml
    <!--创建Emp对象-->
    <bean id="emp" class="com.atguigu.spring5.bean.Emp">
        <!--设置普通类型属性-->
        <property name="ename" value="lucy"/>
        <property name="gender" value="女"/>
        <!--设置对象类型属性-->
        <property name="dept">
            <!--内部bean-->
            <bean id="dept" class="com.atguigu.spring5.bean.Dept">
                <property name="dname" value="安保部"/>
            </bean>
        </property>
    </bean>
```

(3)测试代码

```java
@Test
    public void testEmp(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean3.xml");

        Emp emp = context.getBean("emp", Emp.class);

        emp.testEmp();

    }
```

(4)测试结果

![image-20210504150321182](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504150321182.png)

**<span style="color:red">3.注入属性——级联赋值</span>**

<span style="color:red">(1)第一种写法</span>：

```xml
    <!--创建Emp对象-->
    <bean id="emp" class="com.atguigu.spring5.bean.Emp">
        <!--设置普通类型属性-->
        <property name="ename" value="lucy"/>
        <property name="gender" value="女"/>
        <!--级联赋值-->
        <property name="dept" ref="dept"></property>
    </bean>

    <bean id="dept" class="com.atguigu.spring5.bean.Dept">
        <property name="dname" value="财务部"/>
    </bean>
```

<span style="color:red">(2)第二种写法：</span>

​	①.在Emp中生成级联属性的get方法

```java
    private Dept dept;
    //生成get方法
    public Dept getDept() {
        return dept;
    }
```

​    ②.在Spring配置文件中配置级联赋值

```xml
    <!--创建Emp对象-->
    <bean id="emp" class="com.atguigu.spring5.bean.Emp">
        <!--设置普通类型属性-->
        <property name="ename" value="lucy"/>
        <property name="gender" value="女"/>
        <!--级联赋值-->
        <property name="dept" ref="dept"></property>
        <property name="dept.dname" value="技术部"/>
    </bean>

    <bean id="dept" class="com.atguigu.spring5.bean.Dept">
    </bean>
```

测试代码：

```java
    @Test
    public void testEmp(){
        ApplicationContext context = new ClassPathXmlApplicationContext("bean4.xml");

        Emp emp = context.getBean("emp", Emp.class);

        emp.testEmp();

    }
```

测试结果：

![image-20210504151428069](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504151428069.png)



### 2.5.5IOC操作Bean管理(注入集合类型属性)

**<span style="color:red">1、注入数组类型属性</span>**

**<span style="color:red">2、注入List集合类型属性</span>**

**<span style="color:red">3、注入Map集合类型属性</span>**

<span style="color:red">(1) 创建类、定义数组、list、map、set类型属性，生成对应的set方法</span>

```java
public class Stu {
    private String[] courses;

    private List<String> list ;

    private Map<String,String> maps;

    private Set<String> sets;

    public void setCourses(String[] courses) {
        this.courses = courses;
    }

    public void setList(List<String> list) {
        this.list = list;
    }

    public void setMaps(Map<String, String> maps) {
        this.maps = maps;
    }

    public void setSets(Set<String> sets) {
        this.sets = sets;
    }
}
```

<span style="color:red">(2)在Spring配置文件注入集合类型属性</span>

```xml
    <!--集合类型属性注入-->
    <bean id="stu" class="com.atguigu.spring5.collectiontype.Stu">
        <!--数组类型属性注入-->
        <property name="courses">
            <array>
                <value>java课程</value>
                <value>大数据课程</value>
            </array>
        </property>

        <!--list集合类型属性注入-->
        <property name="list">
            <list>
                <value>上海</value>
                <value>杭州</value>
            </list>
        </property>

        <!--map类型属性注入-->
        <property name="maps">
            <map>
                <entry key="JAVA" value="java"/>
                <entry key="PHP" value="php"/>
            </map>
        </property>
        <!--set类型属性注入-->
        <property name="sets">
            <set>
                <value>ak47</value>
                <value>m416</value>
            </set>
        </property>
    </bean>

```

(3)测试代码

```java
    @Test
    public void test(){
        ApplicationContext context =
                new ClassPathXmlApplicationContext("bean1.xml");

        Stu stu = context.getBean("stu", Stu.class);
        stu.print();
    }
```

(4)运行结果

![image-20210504155243049](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504155243049.png)



**<span style="color:red">4、在集合属性里面设置对象类型值</span>**

<span style="color:red">(1)创建Stu实体类、创建Course实体类，并生成set方法</span>

```java
//学生类
public class Stu {
    private String[] courses;

    private List<String> list ;

    private Map<String,String> maps;

    private Set<String> sets;
    
    //List集合中放对象
    private List<Course> courseList;
    public void setCourseList(List<Course> courseList) {
        this.courseList = courseList;
    }

    public void setCourses(String[] courses) {
        this.courses = courses;
    }

    public void setList(List<String> list) {
        this.list = list;
    }

    public void setMaps(Map<String, String> maps) {
        this.maps = maps;
    }

    public void setSets(Set<String> sets) {
        this.sets = sets;
    }

    public void print(){
        System.out.println(Arrays.toString(courses));
        System.out.println(list);
        System.out.println(maps);
        System.out.println(sets);
        System.out.println(courseList);
    }
}

//课程类
public class Course {
    private String cname;

    public void setCname(String cname) {
        this.cname = cname;
    }

    @Override
    public String toString() {
        return "Course{" +
                "cname='" + cname + '\'' +
                '}';
    }
}
```

<span style="color:red">(2)在Spring中配置对象类型值</span>

通过在`<property>标签的<ref>标签来引入其他的Bean`

```xml
    <!--创建多个Course对象-->
    <bean id="course1" class="com.atguigu.spring5.collectiontype.Course">
        <property name="cname" value="Spring5框架"/>
    </bean>
    <bean id="course2" class="com.atguigu.spring5.collectiontype.Course">
        <property name="cname" value="Mybatis框架"/>
    </bean>

<property name="courseList">
            <list>
                <!--ref标签：指定对其他bean的引用。-->
                <ref bean="course1"></ref>
                <ref bean="course2"></ref>
            </list>
        </property>
```

(3)测试代码

```java
@Test
public void test(){
    ApplicationContext context =
            new ClassPathXmlApplicationContext("bean2.xml");

    Stu stu = context.getBean("stu", Stu.class);
    stu.print();
}
```

(4)运行结果

![image-20210504162800672](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504162800672.png)



**<span style="color:red">5.把集合注入部分提取出来</span>**

提取List集合类型属性注入 供其他bean使用

<span style="color:red">(1)在Spring配置文件中引入名称空间util</span>

![image-20210504163253444](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504163253444.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">
```

<span style="color:red">(2)使用util标签完成list集合注入提取</span>

```xml
 <!--1 提取list集合类型属性注入-->
    <util:list id="kcList">
        <!--创建多个Course对象-->
        <bean id="course1" class="com.atguigu.spring5.collectiontype.Course">
            <property name="cname" value="Spring5框架"/>
        </bean>
        <bean id="course2" class="com.atguigu.spring5.collectiontype.Course">
            <property name="cname" value="Mybatis框架"/>
        </bean>
    </util:list>

<property name="courseList" ref="kcList"></property>
```

### 2.5.6IOC操作Bean管理(FactoryBean)

**<span style="color:red">1、Spring有两种类型bean，一种普通bean，另外一种工厂bean(FactoryBean)</span>**

**<span style="color:red">2、普通bean：在配置文件中定义bean的类型 就是返回类型</span>**

**<span style="color:red">3、工厂bean：在配置文件中定义bean类型可以和返回类型不一致。</span>**

<span style="color:red">第一步：创建类，让这个类实现FactoryBean接口，作为工厂Bean</span>

<span style="color:red">第二步：实现接口里面的方法，在实现方法中定义返回的bean类型</span>

```java

public class MyBean implements FactoryBean<Course> {
    //返回Bean类型
    @Override
    public Course getObject() throws Exception {
        Course course =new Course();
        course.setCname("spring5");
        return course;
    }

    @Override
    public Class<?> getObjectType() {
        return null;
    }

    @Override
    public boolean isSingleton() {
        return false;
    }
}
```

<span style="color:red">第三步：在Spring配置文件中配置Bean</span>

```XML
<bean id="myBean" class="com.atguigu.spring5.collectiontype.MyBean"/>
```

第四步：测试方法

```java
    @Test
    public void test2(){
        ApplicationContext context =
                new ClassPathXmlApplicationContext("bean3.xml");
        //注意：返回的类型 是Course 
       Course course= context.getBean("myBean", Course.class);
        System.out.println(course);
    }
```

运行结果：

![image-20210504175059235](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504175059235.png)

### 2.5.7 IOC操作Bean管理(Bean作用域)

**<span style="color:red">1、在Spring中，设置创建Bean实例是单实例还是多实例</span>**

**<span style="color:red">2、在Spring的默认情况下，Bean是单实例的。</span>**

![image-20210504180519770](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504180519770.png)

![image-20210504180523301](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504180523301.png)

**<span style="color:red">3、如何设置单实例还是多实例</span>**

(1)在Spring配置文件的Bean标签中的scope属性，是用来设置单实例还是多实例的。

(2)scope属性值：

第一个值：singleton(默认值),表示是单实例

第二个值：prototype，表示是多实例

![image-20210504180739595](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504180739595.png)

![image-20210504180742439](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504180742439.png)

(3) singleton和prototype区别

singleton单实例，prototype多实例

**设置scope=“singleton”的时候，加载Spring配置文件时候就会创建单实例对象**

**设置scope=“prototype”的时候，不是在加载Spring配置文件时创建对象，而是在调用getBean()方法的时候创建多实例对象**

### 2.5.8 IOC操作Bean管理(Bean的生命周期)

**<span style="color:red">1、生命周期</span>**

从Bean对象的创建到销毁的过程。

**<span style="color:red">2、Bean生命周期过程</span>**

<span style="color:red">(1) 通过无参构造器创建bean对象</span>

<span style="color:red">(2)为Bean的属性设置值和对其他Bean引用(通过set方法)</span>

<span style="color:red">(3)调用Bean的初始化方法(需要配置初始化的方法)</span>

<span style="color:red">(4)获取Bean对象</span>

<span style="color:red">(5)调用Bean的销毁方法(需要配置销毁的方法)</span>

**<span style="color:red">3、演示Bean的生命周期</span>**

这里以Order实体类为例。

**Order实体类**

```java
public class Order {

    private String oname;

    public Order() {
        System.out.println("第一步 执行无参构造创建Bean实例");
    }

    public void setOname(String oname) {
        this.oname = oname;
        System.out.println("第二步 通过set方法为Bean属性赋值");
    }

    public void initMethod(){
        System.out.println("第三步 执行初始化的方法");
    }

    public void destroyMethod(){
        System.out.println("第五步 执行销毁的方法");
    }
}
```

**Spring配置文件**

```xml
    <bean id="order" class="com.atguigu.spring5.bean.Order" init-method="initMethod" destroy-method="destroyMethod">

        <property name="oname" value="手机"/>
    </bean>
```

**测试代码**

```java
    @Test
    public void test(){

        ClassPathXmlApplicationContext context =
                new ClassPathXmlApplicationContext("bean4.xml");

        Order order = context.getBean("order", Order.class);
        System.out.println("第四步 获取创建Bean实例对象");

        //通过close() 方法执行销毁
        context.close();

    }
```

**<span style="color:red">4、Bean的后置处理器，Bean完整生命周期有七步。</span>**

<span style="color:red">(1) 通过无参构造器创建bean对象</span>

<span style="color:red">(2)为Bean的属性设置值和对其他Bean引用(通过set方法)</span>

**<span style="color:red">(3)把Bean实例传递给Bean后置处理器的方法postProcessBeforeInitialization</span>**

<span style="color:red">(4)调用Bean的初始化方法(需要配置初始化的方法)</span>

**<span style="color:red">(5)把Bean实例传递给Bean后置处理器的方法postProcessAfterInitialization</span>**

<span style="color:red">(6)获取Bean对象</span>

<span style="color:red">(7)调用Bean的销毁方法(需要配置销毁的方法)</span>



**<span style="color:red">5、演示添加后置处理器效果</span>**

<span style="color:red">(1) 创建类，实现接口BeanPostProcessor，创建后置处理器</span>

```java
public class MyBeanProcessor implements BeanPostProcessor {

    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {

        System.out.println("在初始化之前执行的方法");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("在初始化之后执行的方法");
        return bean;
    }
}
```

<span style="color:red">(2)在Spring配置文件中配置后置处理器</span>

```xml
    <!--配置后置处理器-->
    <bean id="myBeanProcessor" class="com.atguigu.spring5.bean.MyBeanProcessor"></bean>
```

运行效果：

![image-20210504193401567](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504193401567.png)

### 2.5.9 IOC操作Bean管理(xml自动装配)

**<span style="color:red">1、什么是自动装配</span>**

根据指定装配规则(属性名称或者属性类型)，Spring自动将匹配的属性值进行注入。

**<span style="color:red">2、演示自动装配过程</span>**

+ <span style="color:red">第一种自动装配规则：根据属性名称进行装配</span>

```xml
    <!--根据属性名称进行自动装配
        autowire属性：指定自动装配的规则
            byName：根据属性名称自动装配  bean标签的id值必须和类的属性名一致。
            byType：根据属性类型自动装配  要自动装配的属性类型不能有多个，否则自动装配失败
    -->
    <bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byName"></bean>
    <bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
```

+ <span style="color:red">第二种自动装配规则：根据属性类型进行装配</span>

```xml
    <!--根据属性名称进行自动装配
        autowire属性：指定自动装配的规则
            byName：根据属性名称自动装配  bean标签的id值必须和类的属性名一致。
            byType：根据属性类型自动装配  要自动装配的属性类型不能有多个，否则自动装配失败
    -->
    <bean id="emp" class="com.atguigu.spring5.autowire.Emp" autowire="byType"></bean>
    <bean id="dept" class="com.atguigu.spring5.autowire.Dept"></bean>
```



### 2.5.10 IOC操作Bean管理(外部属性文件)

**<span style="color:red">1、引入外部属性文件配置数据库连接池</span>**

<span style="color:red">(1)创建外部属性文件，properties格式文件，写好数据库信息</span>

![image-20210504201941734](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504201941734.png)

<span style="color:red">(2) 把外部properties格式文件引入到Spring配置文件中</span>

**<span style="color:red">*引入context名称空间</span>**

![image-20210504202252463](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504202252463.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
">
```

**<span style="color:red">*在Spring配置文件使用`<context:property-placeholoder>`引入外部属性文件</span>**

```xml
<context:property-placeholder location="classpath:jdbc.properties"></context:property-placeholder>
    <!--配置数据库信息-->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="${prop.driverClass}"/>
        <property name="url" value="${prop.url}"/>
        <property name="username" value="${prop.username}"/>
        <property name="password" value="${prop.password}"/>
    </bean>
```

# 2.6 IOC容器—Bean管理(基于注解方式)

**<span style="color:red">1、什么是注解</span>**

<span style="color:red">(1)注解是代码特殊标记，格式：@注解名称(属性名称=属性值,属性名称=属性值)</span>

<span style="color:red">(2)使用注解的目的：简化xml配置</span>

<span style="color:red">(3)注解作用在类、方法、属性上。</span>

**<span style="color:red">2、Spring针对Bean管理中创建对象提供了注解</span>**

<span style="color:red">(1) @Component</span>

<span style="color:red">(2) @Service</span>

<span style="color:red">(3) @Controller</span>

<span style="color:red">(4) @Repository</span>

**<span style="color:red">上面四个注解功能是一样的，都可以用来创建Bean实例</span>**

**<span style="color:red">3、基于注解方式实现对象创建</span>**

<span style="color:red">(1) 引入依赖 `spring-aop-5.2.6.RELEASE.jar`</span>

![image-20210504232502789](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210504232502789.png)

<span style="color:red">(2) 开启组件扫描</span>

```xml
    <!--开启组件扫描
        1.如果扫描多个包，多个包使用逗号隔开
        2.扫描包上册目录
    -->
    <context:component-scan base-package="com.atguigu"/>
```

<span style="color:red">(3)创建类，在类上面添加创建对象注解</span>

```java
//在注解里面value属性值可以省略不写， 
//默认值是类名称，首字母小写 //UserService -- userService
@Component(value = "userService") //<bean id="userService" class=".."/> 
public class UserService {
    public void add() {
        System.out.println("service add......."); 
    } 
}
```

**<span style="color:red">4、开启组件扫描细节配置</span>**

```xml
    <!--
    use-default-filters:表示现在不使用默认的filter，自己配置filter
    context:include-filter：设置扫描哪些内容
    -->
    <context:component-scan base-package="com.atguigu.spring5" use-default-filters="false">
        <context:include-filter type="annotation"
                                expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>


    <!--
    context:exclude-filter：设置哪些内容不扫描
    -->
    <context:component-scan base-package="com.atguigu.spring5">
        <context:exclude-filter type="annotation"
                                expression="org.springframework.stereotype.Controller"/>
    </context:component-scan>
```

**<span style="color:red">5、基于注解方式实现属性注入</span>**

<span style="color:red">(1)@Autowired：根据属性类型自动注入</span>

第一步：把service和dao对象创建，在service和dao类添加创建对象注解

第二步：在service注入dao对象，在service类添加dao类型属性，在属性上面使用注解

```java
@Service
public class UserService {

    @Autowired //根据属性类型注入
    //不需要添加set方法
    private UserDao userDao;

    public void test(){
        System.out.println("service add.......");
        userDao.add();
    }
}
```

<span style="color:red">(2)@Qualifier：根据属性名称自动装配，这个注解要和@Autowired一起使用</span>

<span style="color:red">(3)@Resource：可以根据类型注入，也可以根据名称注入</span>

<span style="color:red">(4)@Value：注入普通类型属性</span>

```java
@Service
public class UserService {
    
    //注入普通类型属性
    @Value(value = "jerry")
    private String name;

    @Autowired //根据属性类型注入
    //不需要添加set方法
    @Qualifier(value = "udao")//根据属性名称注入
    private UserDao userDao;

    public void test(){
        System.out.println("service add......." + name);
        userDao.add();
    }
}
```



**<span style="color:red">6、完全注解开发</span>**

<span style="color:red">(1)创建配置类，替代xml配置文件</span>

```java
@Configuration//配置类
@ComponentScan(basePackages = {"com.atguigu"})
public class SpringConfig {
    
}
```

<span style="color:red">(2)编写测试类</span>

```java
@Test
public void testService2(){

    ApplicationContext context =
            new AnnotationConfigApplicationContext(SpringConfig.class);

    UserService userService = context.getBean("userService", UserService.class);

    userService.test();
}
```



# 3.AOP

## <span style="color:red">3.1什么是AOP</span>

<span style="color:red">(1) 面向切面编程，利用AOP可以将业务逻辑的各个部分进行隔离，从而使得业务逻辑格部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。</span>

<span style="color:red">(2) 通俗描述：通过不修改源代码的方式，在主干功能里添加新功能。</span>

<span style="color:red">(3)使用登录例子说明AOP</span>

![image-20210505095935686](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505095935686.png)



## 3.2 AOP底层原理

**<span style="color:red">1、AOP底层使用动态代理方式</span>**

**<span style="color:red">2、有两种情况的动态代理</span>**

**<span style="color:red">(1) 有接口情况，使用JDK动态代理</span>**

​	<span style="color:red">创建接口实现类代理对象，增强类的方法</span>

![image-20210505101825803](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505101825803.png)

**<span style="color:red">(2)没有接口情况，使用CGLIB动态代理</span>**

​	<span style="color:red">创建子类代理对象，增强类的方法</span>

![image-20210505101832285](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505101832285.png)

## 3.3 AOP(JDK动态代理)

**<span style="color:red">1、使用JDK动态代理，使用Proxy类里面的`newProxyInstance`方法创建代理对象</span>**

![image-20210505110157384](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505110157384.png)

<span style="color:red">(1) 调用newProxyInstance方法</span>

![image-20210505110212354](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505110212354.png)

**`newProxyInstance`<span style="color:red">三个参数：</span>**

<span style="color:red">第一个参数：类加载器</span>

<span style="color:red">第二个参数：增强方法所在的类，这个类实现的接口，支持多个接口</span>

<span style="color:red">第三个参数：实现这个接口InvocationHandler，创建代理对象，写增强的部分</span>

**<span style="color:red">2、编写JDK动态代理代码</span>**

(1) 创建接口，定义方法

```java
public interface UserDao {

    int add(int a,int b);

    String update(String id);
}
```

(2)创建接口实现类，实现方法

```java
public class UserDaoImpl implements UserDao {
    @Override
    public int add(int a, int b) {
        return a+b;
    }

    @Override
    public String update(String id) {
        return id;
    }
}
```

(3)使用Proxy类创建代理对象

```java
public class JDKProxy {


    public static void main(String[] args) {


        Class<?>[] interfaces = {UserDao.class};
        UserDaoImpl userDao = new UserDaoImpl();
        UserDao dao = (UserDao) Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDao));

        int result = dao.add(1, 2);
    }
    
//UserDaoProxy类
    public class UserDaoProxy implements InvocationHandler{


    private Object obj;

    public UserDaoProxy(Object obj) {
        this.obj = obj;
    }

    //增强的逻辑
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {

        //方法之前
        System.out.println("方法之前执行..." + method.getName() +":传递的参数" + Arrays.toString(args));

        //被增强的方法执行
        Object result = method.invoke(obj, args);
        System.out.println("result:" + result);

        //方法执行之后
        System.out.println("方法之后执行..." + obj);
        return result;
    }
}
```

运行结果：

![image-20210505112413230](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505112413230.png)

## 3.4 AOP术语

**<span style="color:red">1、连接点</span>**

![image-20210505114350133](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505114350133.png)

**<span style="color:red">2、切入点</span>**

![image-20210505114358798](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505114358798.png)

**<span style="color:red">3、通知(增强)</span>**

![image-20210505114406106](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505114406106.png)

![image-20210505114419035](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505114419035.png)

**<span style="color:red">4、切面</span>**

![image-20210505114441275](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505114441275.png)



## 3.5 AOP操作(准备工作)

**<span style="color:red">1、Spring框架一般都是基于AspectJ实现AOP操作</span>**

(1) AspectJ不是Spring的组成部分，它是独立的AOP框架，一般把AspectJ和Srping框架一起使用，进行AOP操作

**<span style="color:red">2、基于AspectJ实现AOP操作</span>**

(1) 基于xml配置文件实现

(2)基于注解方式实现

**<span style="color:red">3、在项目工程中引入AOP相关依赖</span>**

![image-20210505122549307](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505122549307.png)

**<span style="color:red">4、切入点表达式</span>**

+ <span style="color:red">切入点表达式作用：知道对哪个类的哪个方法进行增强</span>
+ <span style="color:red">语法结构：`execution([权限修饰符][返回值类型][类全路径][方法名称]([参数列表]))`</span>

举例1：对com.atguigu.dao.BookDao类里面的add进行增强

execution(* com.atguigu.dao.BookDao.add(..))

举例2：对com.atguigu.dao.BookDao类里面的所有的方法进行增强

execution(* com.atguigu.dao.BookDao.*(..))

举例3：对com.atguigu.dao包里面所有类，类里面所有方法进行增强

`execution(* com.atguigu.dao.*.*(..))`

## 3.6 AOP操作 (AspectJ注解)

**<span style="color:red">1、创建类，在类里面定义方法</span>**

```java
public class User {

    public void add(){
        System.out.println("add.....");
    }
}
```

**<span style="color:red">2、创建增强类，编写增强方法</span>**

```java
public class UserProxy {

    public void before(){
        System.out.println("before....");
    }
}
```

**<span style="color:red">3、进行通知的配置</span>**

<span style="color:red">(1) 在Spring配置文件，开启注解扫描</span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">


    <!--开启注解扫描-->
    <context:component-scan base-package="com.atguigu.spring5"/>

</beans>
```

<span style="color:red">(2)使用注解创建User和UserProxy对象</span>

![image-20210505210531927](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505210531927.png)

![image-20210505210544268](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210505210544268.png)

<span style="color:red">(3) 在增强类上添加注解@Aspect</span>

```java
@Component
@Aspect //生成代理对象
public class UserProxy {

    public void before(){
        System.out.println("before....");
    }
}
```

<span style="color:red">(4)在Spring配置文件中开启生成代理对象</span>

```xml
    <!--开启自动生成代理对象-->
    <aop:aspectj-autoproxy/>
```

**<span style="color:red">4、配置不同类型的通知</span>**

<span style="color:red">(1)在增强类的里面，在作为通知方法上添加通知类型注解，使用切入点表达式配置</span>

```java
@Component
@Aspect //生成代理对象
public class UserProxy {


    //前置通知
    @Before(value = "execution(* com.atguigu.spring5.aopanto.User.add(..))")
    public void before(){
        System.out.println("before....");
    }

    //无论是否有异常，都会执行，类似最终通知
    @After(value = "execution(* com.atguigu.spring5.aopanto.User.add(..))")
    public void after(){

        System.out.println("after......");
    }


    //在返回结果之后调用该方法
    @AfterReturning(value = "execution(* com.atguigu.spring5.aopanto.User.add(..))")
    public void afterReturn(){

        System.out.println("AfterReturning......");
    }

    //异常通知
    @AfterThrowing(value = "execution(* com.atguigu.spring5.aopanto.User.add(..))")
    public void afterThrow(){
        System.out.println("afterThrowing......");
    }


    //环绕通知
    @Around(value = "execution(* com.atguigu.spring5.aopanto.User.add(..))")
    public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕之前.....");
        //被增强方法执行
        proceedingJoinPoint.proceed();

        System.out.println("环绕之后.....");

    }
}
```

**<span style="color:red">5、相同的切入点进行抽取</span>**

```java
	// 相同切入点抽取
    @Pointcut(value = "execution(* com.atguigu.spring5.aopanto.User.add(..))")
    public void pointcut(){
    }

    //前置通知
	//value=抽取切入点的方法名()
    @Before(value = "pointcut()")
    public void before(){
        System.out.println("before....");
    }
```

**<span style="color:red">6、有多个增强类对同一个方法进行增强时，设置增强类优先级</span>**

<span style="color:red">(1) 在增强类上面添加注解@Order(数字型值)，数字型值越小优先级越高。</span>

```java
@Component
@Aspect
@Order(5)
public class PersonProxy {
}
```

**<span style="color:red">7、AOP完全注解开发</span>**

<span style="color:red">(1) 创建配置类，不需要创建xml配置文件</span>

```java
@Configuration
@ComponentScan(basePackages = {"com.atguigu.spring5"})//开启注解扫描
@EnableAspectJAutoProxy//自动生成代理对象
public class AopConfig {
}
```

## 3.7 AOP基于xml配置文件操作

<span style="color:red">(1)创建两个类，Book类和BookProxy类</span>

```java
// 被增强类
public class Book {

    public void buy(){
        System.out.println("buy .....");
    }
}

//增强类
public class BookProxy {

    public void selectBook(){
        System.out.println("select......");
    }
}
```

<span style="color:red">(2)在Spring配置文件中创建两个类对象</span>

```xml
  <!--创建Book对象-->
    <bean id="book" class="com.atguigu.spring5.aopxml.Book"></bean>
    <!--创建BookProxy对象-->
    <bean id="bookProxy" class="com.atguigu.spring5.aopxml.BookProxy"></bean>
```

<span style="color:red">(3)在Spring配置文件中配置切入点</span>

```xml
    <aop:config>
        <!--配置切入点-->
        <aop:pointcut id="p" expression="execution(* com.atguigu.spring5.aopxml.Book.buy(..))"/>

        <!--配置切面:把通知应用到切入点的过程-->
        <aop:aspect ref="bookProxy">
            <aop:before method="selectBook" pointcut-ref="p"/>
        </aop:aspect>
    </aop:config>
```



# 4. JdbcTemplate

## 4.1 概述和准备

**<span style="color:red">1、什么是JdbcTemplate</span>**

<span style="color:red">(1)Spring框架对JDBC进行了封装，使用JdbcTemplate方便对数据库操作。</span>

**<span style="color:red">2、jdbcTemplate环境搭建</span>**

<span style="color:red">(1) 引入相关jar包</span>

![image-20210506175730399](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506175730399.png)

<span style="color:red">(2) 在Spring配置文件中配置数据库连接池</span>

```xml
    <!--配置数据库连接信息-->
    <bean id="ds" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql:///3306/mybatis"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>
```

<span style="color:red">(3) 在Spring配置文件中创建JdbcTemplate对象</span>

```xml
    <!--创建JdbcTemplate对象-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="ds"/>
    </bean>
```

<span style="color:red">(4)创建service类，dao类，在dao注入JdbcTemplate对象</span>

+ 配置文件

```xml
<!--开启注解扫描-->
<context:component-scan base-package="com.atguigu.spring5"/>
```

```java
@Repository
public class BookDaoImpl {

    @Autowired
    private JdbcTemplate jdbcTemplate;
}

@Service
public class BookService {
    @Autowired
    private BookDaoImpl bookDao;
}
```

## 4.2 JdbcTemplate操作数据库(添加操作)

```
JdbcTemplate增删改操作都是使用public int update(String sql, @Nullable Object... args) throws DataAccessException，通过sql指明要执行的sql语句,并通过args指明SQL语句的参数
```

**<span style="color:red">1、对应数据库创建实体类</span>**

```java
public class Student {

    private Integer id;
    private String name;
	// getter/setter省略
}
```

**<span style="color:red">2、编写service和dao</span>**

(1)在dao进行数据库添加操作

(2)调用jdbcTemplate对象里面的update()方法进行添加操作

![image-20210506182526395](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506182526395.png)

+ 两个参数
  + sql：sql语句
  + args：sql语句中的参数

```java
@Repository
public class StudentDaoImpl {

    @Autowired
    private JdbcTemplate jdbcTemplate;

    //添加操作
    public void add(Student student) {
        String sql = "insert into t_student values(?,?)";
        int update = jdbcTemplate.update(sql, student.getId(),student.getName());
        System.out.println(update);
    }
}
```

**<span style="color:red">3、测试方法</span>**

```java
    @Test
    public void testAdd(){
        Student student = new Student();
        student.setId(3);
        student.setName("王一博");
        ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");

        StudentService studentService = context.getBean("studentService", StudentService.class);
        studentService.add(student);

    }
```

## 4.3 JdbcTemplate操作数据库(修改和删除操作)

**<span style="color:red">1、修改操作</span>**

```java
    //修改操作
    public void updateStudent(Student student) {

        String sql = "update t_student set name=? where id = ?";
        Object[] args = {student.getName(),student.getId()};
        int updateCount = jdbcTemplate.update(sql, args);
        System.out.println(updateCount);

    }
```

**<span style="color:red">2、删除操作</span>**

```java
    //删除操作
    public void delete(Integer id) {
        String sql = "delete from t_student where id = ?";
        jdbcTemplate.update(sql, id);
    }
```

## 4.4 JdbcTemplate操作数据库(查询返回某个值)

查询某个值或对象用这个函数：`public <T> T queryForObject(String sql, RowMapper<T> rowMapper, @Nullable Object... args) throws DataAccessException`，通过 `sql` 指明要执行的 SQL 语句，通过 `RowMapper `对象指明从数据库查询出来的参数应该如何封装到指定的对象中，并通过可变长参数 `args `指明 SQL 语句的参数
**<span style="color:red">1、查询表里有多少条记录，返回的是某个值</span>**

**<span style="color:red">2、使用JdbcTemplate实现查询返回某个值代码</span>**

![image-20210506183925883](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506183925883.png)



+ **<span style="color:red">有两个参数：</span>**
  + **<span style="color:red">sql：sql语句</span>**
  + **<span style="color:red">requiredType：表示返回值的类型</span>**

```java
// 查询表记录数
    public int selectCount() {

        String sql = "select count(1) from t_student";
        return jdbcTemplate.queryForObject(sql, Integer.class);
    }
```

## 4.5 JdbcTemplate操作数据库(查询返回对象)

**<span style="color:red">1、使用JdbcTemplate实现查询返回对象</span>**

![image-20210506184732435](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506184732435.png)

+ **<span style="color:red">有三个参数</span>**
  + **<span style="color:red">sql：sql语句</span>**
  + **<span style="color:red">rowMapper：RowMapper是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装</span>**
  + **<span style="color:red">args：sql语句中的参数</span>**

```java
    //查询返回对象
    public Student selectReturnStudent(Integer id) {

        String sql = "select * from t_student where id = ?";
        return jdbcTemplate.queryForObject(sql,new BeanPropertyRowMapper<Student>(Student.class),id);
    }
```



## 4.6 JdbcTemplate操作数据库(查询返回集合)

**<span style="color:red">1、调用jdbcTemplate.query()方法实现查询返回集合</span>**

![image-20210506185358195](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506185358195.png)

+ **<span style="color:red">有三个参数：</span>**
  + **<span style="color:red">sql：sql语句</span>**
  + **<span style="color:red">args：sql语句中的参数</span>**
  + **<span style="color:red">RowMapper是接口，针对返回不同类型数据，使用这个接口里面实现类完成数据封装</span>**

```java
    //查询所有，返回集合
    public List<Student> findAllStudents() {

        String sql = "select * from t_student";
        return jdbcTemplate.query(sql,new BeanPropertyRowMapper<Student>(Student.class));
    }
```

## 4.7 JdbcTemplate操作数据库(批量操作)

**<span style="color:red">1、批量操作：操作表里面多条记录</span>**

**<span style="color:red">2、JdbcTemplate实现批量添加操作</span>**

![image-20210506191530907](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506191530907.png)

+ **<span style="color:red">有两个参数：</span>**
  + **<span style="color:red">sql：sql语句</span>**
  + **<span style="color:red">batchArgs：sql语句中的参数，是个List</span>**

```java
    //批量添加
    public void batchAddStudent() {

        String sql = "insert into t_student values(?,?)";
        List<Object[]> batchArgs = new ArrayList<>();
        Object[] o1 = {7, "芳芳"};
        Object[] o2 = {8, "晓晓"};
        Object[] o3 = {9, "喃喃"};
        batchArgs.add(o1);
        batchArgs.add(o2);
        batchArgs.add(o3);
        jdbcTemplate.batchUpdate(sql, batchArgs);
    }
```

**<span style="color:red">3、JdbcTemplate实现批量修改操作</span>**

```java
   //批量修改
    public void batchUpdateStudnet() {

        String sql = "update t_student set name=? where id = ?";
        List<Object[]> batchArgs = new ArrayList<>();
        batchArgs.add(new Object[]{"ff", 7});
        batchArgs.add(new Object[]{"xx", 8});
        batchArgs.add(new Object[]{"nn", 9});
        jdbcTemplate.batchUpdate(sql, batchArgs);
    }
```

**<span style="color:red">4、JdbcTemplate实现批量删除操作</span>**

```java
    //批量删除操作
    public void batchDelete() {
        String sql = "delete from  t_student  where id = ?";
        List<Object[]> batchArgs = new ArrayList<>();
        batchArgs.add(new Object[]{7});
        batchArgs.add(new Object[]{8});
        batchArgs.add(new Object[]{9});
        jdbcTemplate.batchUpdate(sql, batchArgs);

    }
```

# 5.Spring声明式事务管理

## 5.1 事务的概念

**<span style="color:red">1、什么是事务</span>**

(1)事务是数据库操作最基本的单元，是逻辑上的一组操作，要么都成功，要么都失败。

(2)典型场景：银行转账

+ lucy 转账100元 给mary
+ lucy少100,mary多100

**<span style="color:red">2、事务的四大特性(ACID)</span>**

(1)原子性：表示事务中的所有操作要么都执行，要么都不执行。

(2)一致性：“一致”指的是数据的一致，具体是指：一个事务不管涉及到多少个操作，都必须保证事务执行之前和执行之后的数据仍然是正确的。

(3)隔离性：多个事务在并发执行过程中不会相互影响。

(4)持久性：事务执行完毕之后，对数据的修改永存的保存下来。

## 5.2 Spring事务管理

**<span style="color:red">1、Spring的事务管理有两种实现方式：编程式事务管理和声明式事务管理(现在使用)</span>**

**<span style="color:red">2、Spring使用的是声明式事务管理，声明式事务管理底层使用AOP</span>**

**<span style="color:red">3、Spring声明式事务管理</span>**

​	**<span style="color:red">(1)基于注解方式</span>**

​	<span style="color:red">(2)基于xml配置文件方式</span>

**<span style="color:red">4、Spring事务管理API</span>**

<span style="color:red">(1) 提供一个接口，代表事务管理器，这个接口针对不同的框架提供不同的实现类</span>

## 5.3 基于注解实现声明式事务管理

**<span style="color:red">1、在Spring配置文件中配置事务管理器</span>**

```xml
    <!--配置事务管理器对象-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="ds"/>
    </bean>
```

**<span style="color:red">2、在Spring配置文件中开启事务注解</span>**

<span style="color:red">(1)引入事务名称空间tx</span>

![image-20210506195603825](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506195603825.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                            http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd>
                            http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">

```

<span style="color:red">(2)开启事务注解</span>

```xml
    <!--开启事务注解-->
    <tx:annotation-driven></tx:annotation-driven>
```

**<span style="color:red">3、在service类上面（或者service类的方法上）添加事务注解@Transcational</span>**

<span style="color:red">（1）@Transactional，这个注解添加到类上面，也可以添加方法上面</span>
<span style="color:red">（2）如果把这个注解添加类上面，这个类里面所有的方法都添加事务</span>
<span style="color:red">（3）如果把这个注解添加方法上面，为这个方法添加事务</span>

![image-20210506200806240](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506200806240.png)

## 5.4 事务操作(声明式事务管理参数配置)

**<span style="color:red">1、在service类上添加注解@Transactional，在这个注解里面可以配置事务相关参数</span>**

![image-20210506200944194](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506200944194.png)

**<span style="color:red">2、propagation：事务传播行为</span>**

(1) 多事务方法之间直接调用,这个过程中事务是如何进行(管理)处理的。

![image-20210506201121525](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506201121525.png)

![image-20210506201129836](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506201129836.png)

![image-20210506201136953](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506201136953.png)

**<span style="color:red">3、isolation：隔离级别</span>**

<span style="color:red">(1)事务的隔离性：多事务之间操作不会产生影响。不考虑隔离性会产生很多问题。</span>

<span style="color:red">(2)有三个读问题：脏读、不可重复读、幻读。</span>

<span style="color:red">(3)脏读：一个未提交的事务读取到另一个未提交事务的数据</span>

![image-20210506201342098](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506201342098.png)

<span style="color:red">(4)不可重复读：一个未提交事务读取到另一个提交事务修改数据</span>

![image-20210506201434901](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506201434901.png)

<span style="color:red">(5)幻读：一个未提交事务读取到另一提交事务添加数据</span>

<span style="color:red">(6)解决：通过设置事务隔离级别，解决读问题</span>

![image-20210506201616470](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506201616470.png)

![image-20210506201621965](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506201621965.png)

**<span style="color:red">4、timeout：超时时间</span>**

<span style="color:red">(1) 事务需要在一定时间内进行提交，如果不提交进行回滚。</span>

<span style="color:red">(2)默认值为-1，设置时间以秒为单位。</span>

**<span style="color:red">5、readOnly：是否只读</span>**

<span style="color:red">(1)false：表示既可以查询，也可以增删改操作</span>

<span style="color:red">(2)true：表示只可以查询</span>

**<span style="color:red">6、rollbackFor：回滚</span>**

<span style="color:red">(1) 设置出现哪些异常进行事务回滚</span>

**<span style="color:red">7、noRollbackFor：不回滚</span>**

<span style="color:red">(1)设置哪些异常不进行事务回滚。</span>

## 5.5 基于XML配置文件方式实现声明式事务管理

**1、在Spring配置文件中进行配置**

(1)配置事务管理器

(2)配置通知

(3)配置切面

```xml
    <!--开启注解扫描-->
    <context:component-scan base-package="com.atguigu.spring5"/>

    <!--配置数据库连接信息-->
    <bean id="ds" class="com.alibaba.druid.pool.DruidDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql:///mybatis"/>
        <property name="username" value="root"/>
        <property name="password" value="root"/>
    </bean>

    <!--创建JdbcTemplate对象-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="ds"/>
    </bean>

    <!--配置事务管理器对象-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="ds"/>
    </bean>

    <!--配置通知-->
    <tx:advice id="txadvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="batchAdd"/>
        </tx:attributes>
    </tx:advice>

    <!--配置切面-->
    <aop:config>
        <aop:pointcut id="pt" expression="execution(* com.atguigu.spring5.service.StudentService.batchAdd(..))"></aop:pointcut>

        <aop:advisor advice-ref="txadvice" pointcut-ref="pt"></aop:advisor>
    </aop:config>
```



## 5.6 事务操作（完全注解实现声明式事务管理）

**完全注解用于后面基于SpringBoot框架下完全注解开发**

**<span style="color:red">1、创建配置类，使用配置类替代xml配置文件</span>**

```java
@Configuration
@ComponentScan(basePackages = {"com.atguigu.spring5"})
@EnableTransactionManagement //开启事务注解
public class TxConfig {

    //数据源
    @Bean
    public DataSource getDataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql:///mybatis");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        return dataSource;
    }

    //配置jdbcTemplate
    @Bean
    public JdbcTemplate getJdbcTemplate(DataSource dataSource){
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }

    //事务管理器
    @Bean
    public TransactionManager getTransactionManager(){
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(getDataSource());
        return transactionManager;
    }
}
```



# 6.Spring5框架新功能

**<span style="color:red">1、整个Spring5框架的代码基于Java8，运行时兼容Java9，许多不建议使用的类和方法在代码库中删除了。</span>**

**<span style="color:red">2、Spring5框架自带了通用的日志框架</span>**

(1) Spring5已经移除Log4jConfigListener，官方建议使用Log4j2

**<span style="color:red">(2)Spring5框架整合Log4j2</span>**

<span style="color:red">第一步：引入日志相关jar包</span>

![image-20210506204656777](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506204656777.png)

<span style="color:red">第二步：创建log4j2.xml配置文件</span>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!--日志级别以及优先级排序: OFF > FATAL > ERROR > WARN > INFO > DEBUG > TRACE > ALL -->
<!--Configuration后面的status用于设置log4j2自身内部的信息输出，可以不设置，当设置成trace时，可以看到log4j2内部各种详细输出-->
<configuration status="INFO">
    <!--先定义所有的appender-->
    <appenders>
        <!--输出日志信息到控制台-->
        <console name="Console" target="SYSTEM_OUT">
            <!--控制日志输出的格式-->
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
        </console>
    </appenders>
    <!--然后定义logger，只有定义了logger并引入的appender，appender才会生效-->
    <!--root：用于指定项目的根日志，如果没有单独指定Logger，则会使用root作为默认的日志输出-->
    <loggers>
        <root level="info">
            <appender-ref ref="Console"/>
        </root>
    </loggers>
</configuration>
```

**<span style="color:red">3、Spring5框架核心容器支持@Nullable注解</span>**

<span style="color:red">（1）@Nullable注解可以使用在方法上面，属性上面，参数上面，表示方法返回可以为空，属性值可以为空，参数值可以为空</span>
<span style="color:red">（2）注解用在方法上面，方法返回值可以为空</span>

![image-20210506204857504](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506204857504.png)

<span style="color:red">（3）注解使用在方法参数里面，方法参数可以为空</span>

![image-20210506204918070](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506204918070.png)

<span style="color:red">（4）注解使用在属性上面，属性值可以为空</span>

![image-20210506204932561](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210506204932561.png)

