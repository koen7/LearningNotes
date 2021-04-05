# 一、常用类——String类

是在java.lang包下的

## 1.String类的概述

+ `String`表示字符串，用一对“” 引起来。
+ `String`是一个final类，表示**<span style='color:red;'>不可被继承</span>**
+ `String`实现了`Serializable`接口，表示字符串是支持序列化的。又实现了`Comparable接口`，表示String是可比较大小的。
+ `String`代表不可变的字符序列。简称：**<span style='color:red;'>不可变性</span>**
  + 当对字符串重新赋值时，需要重新指定内存区域赋值，不能使用原来的值进行赋值。
  + 当对字符串进行连接操作时，需要重新指定内存区域赋值，同样不能使用原来的值进行赋值。
  + 当调用String的`replace()`修改字符或者字符串时，需要重新指定内存区域赋值，不能使用原来的值进行赋值。
+ 通过**<span style='color:red;'>字面量</span>**的方式**赋值**，此时<span style='color:red;'>字符串的值声明在字符串常量池</span>中（**区别通过new的方式给字符串赋值**）
+ **字符串常量池**中是**不会存储<span style='color:red;'>相同内容</span>**（相同内容指的是通过`equals()`比较，返回true的字符串）s

## 2.String的实例化的方法

String的实例化方法有两种，一种是通过**<span style='color:red;'>字面量</span>**定义，另一种是通过 **<span style='color:red;'>new + 构造器</span>**定义。

+ 方式一：通过字面量的方式实例化
+ 方式二：通过`new + 构造器 ` 方式 实例化。

### **面试题1**：String s2 = new String("abc") 和 String s1 = "abc"的区别？

s2的“abc” 是存储在方法区中的字符串常量池中

s1的"abc"是存储在堆空间中。

**s1和s2的内存解析图**

![image-20200417150138496](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200417150139.png)

**更具体的图示**

![image-20200417150527547](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200417150528.png)

### 面试题2：String s = new String("abc");方式创建对象，在内存中创建了几个对象？

两个：一个是堆空间中new的结构，另一个是char[]对应的常量池中的数据"abc"

**代码示例**

```java
//通过字面量定义的方式：此时的s1和s2的数据javaEE声明在方法区中的字符串常量池中。
String s1 = "javaEE";
String s2 = "javaEE";
//通过new + 构造器的方式:此时的s3和s4保存的地址值，是数据在堆空间中开辟空间以后对应的地址值。
String s3 = new String("javaEE");
String s4 = new String("javaEE");

System.out.println(s1 == s2);//true
System.out.println(s1 == s3);//false
System.out.println(s1 == s4);//false
System.out.println(s3 == s4);//false
```



## 3.字符串拼接方式赋值对比

### **<span style='color:red;'>1.结论</span>**

1. **<span style='color:red;'>常量与常量</span>**的**拼接结果**存放在**<span style='color:red;'>字符串常量池中</span>**。且常量池中不会存在相同内容的常量
2. **<span style='color:red;'>常量与变量</span>**的**拼接结果**存放在**<span style='color:red;'>堆空间中。</span>**
3. 如果拼接的结果**调用了`intern()`**，那么该结果存放**<span style='color:red;'>在常量池中</span>**

### 2.代码示例

```java
 @Test
    public void test3(){
        String s1 = "JavaEE";
        String s2 = "hadoop";
        String s3 = "JavaEE" + "hadoop";
        String s4 = s1 + "hadoop";
        String s5 = "JavaEE" + s2;
        String s6 = s1 + s2;
        String s7 = s4.intern();
        System.out.println(s3 == s4);//false
        System.out.println(s3 == s5);//false
        System.out.println(s4 == s5);//false
        System.out.println(s3 == s6);//false
        System.out.println(s4 == s6);//false
        System.out.println(s3 == s7);//true
        System.out.println(s5 == s7);//false
    }
```

### 3.内存解析

![image-20200417151140368](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200417151142.png)



## 4.String类的常用方法

### 1.字符串常用操作方法

1. int length()：返回字符串的长度： return value.length
2. char charAt(int index)： 返回某索引处的字符return value[index]
3. boolean isEmpty()：判断是否是空字符串：return value.length == 0
4. String toLowerCase()：使用默认语言环境，将 String 中的所字符转换为小写
5. String toUpperCase()：使用默认语言环境，将 String 中的所字符转换为大写
6. String trim()：返回字符串的副本，忽略前导空白和尾部空白
7. boolean equals(Object obj)：比较字符串的内容是否相同
8. boolean equalsIgnoreCase(String anotherString)：与equals方法类似，忽略大小写
9. String concat(String str)：将指定字符串连接到此字符串的结尾。 等价于用“+”
10. int compareTo(String anotherString)：比较两个字符串的大小
11. String substring(int beginIndex)：返回一个新的字符串，它是此字符串的从beginIndex开始截取到最后的一个子字符串。
12. String substring(int beginIndex, int endIndex) ：返回一个新字符串，它是此字符串从beginIndex开始截取到endIndex(不包含)的一个子字符串。

**代码示例**

```java
@Test
    public void test(){
        String s1 = "HelloWorld";
        System.out.println(s1);
        System.out.println(s1.length());

        //返回某索引的字符
        System.out.println(s1.charAt(0));
        System.out.println(s1.charAt(9));
        //java.lang.StringIndexOutOfBoundsException
        //System.out.println(s1.charAt(10));

        System.out.println(s1.isEmpty());
        //s1 = null;
        //java.lang.NullPointerException
        //System.out.println(s1.isEmpty());


        String s2 = s1.toUpperCase();
        System.out.println(s1);
        System.out.println(s2);
        String s3 = s1.toLowerCase();
        System.out.println(s3);
        String s4 = "--------" + "   He   llo world      " + "-------";
        String s5 = s4.trim();
        System.out.println(s5);

        String s6 = "helloworld";
        System.out.println(s1.equals(s6));
        System.out.println(s1.equalsIgnoreCase(s6));

        String s7 = s6.concat("777");
        System.out.println(s7);

        String s8 = "abc";
        String s9 = "abf";
        System.out.println(s8.compareTo(s9));

        String s10 ="尚硅谷真不错";
        String s11 = s10.substring(3);
        System.out.println(s11);
        String s12 = s10.substring(0, 3); //左闭又开
        System.out.println(s12);
    }
```

### 2.字符串的判断操作方法

1. boolean endsWith(String suffix)：测试此字符串是否以指定的后缀结束
2. boolean startsWith(String prefix)：测试此字符串是否以指定的前缀开始
3. boolean startsWith(String prefix, int toffset)：测试此字符串从指定索引开始的子字符串是否以指定前缀开始

**代码示例**

```java
 		String s1 = "helloWorld";
        System.out.println(s1.endsWith("ld"));
        System.out.println(s1.endsWith("rl"));

        System.out.println(s1.startsWith("hel"));
        System.out.println(s1.startsWith("Hel"));

        System.out.println(s1.startsWith("hel", 5));

        System.out.println(s1.contains("ow"));
        System.out.println(s1.contains("oW"));
```

### 3.查找字符串中的字符

1. boolean contains(CharSequence s)：当且仅当此字符串包含指定的 char 值序列时，返回 true
2. int indexOf(String str)：返回指定子字符串在此字符串中第一次出现处的索引
3. int indexOf(String str, int fromIndex)：返回指定子字符串在此字符串中第一次出现处的索引，从指定的索引开始
4. int lastIndexOf(String str)：返回指定子字符串在此字符串中最右边出现处的索引
5. int lastIndexOf(String str, int fromIndex)：返回指定子字符串在此字符串中最后一次出现处的索引，从指定的索引开始反向搜索

```
注意：indexOf和lastIndexOf方法如果没有找到，返回-1
```

**代码示例**

```java
		String s1 = "helloWorld";
		//返回lo在s1中第一次出现的索引
        System.out.println(s1.indexOf("lo"));
        //从索引5开始 返回l 第一次出现的索引
        System.out.println(s1.indexOf("l", 5));
        //从右往前找 返回W 第一次出现的索引位置
        System.out.println(s1.lastIndexOf("W"));
        // 返回从索引7的左边开始 找o第一次出现的索引位置
        System.out.println(s1.lastIndexOf("o", 7));
```

### 4.字符串操作方法

1. 替换：
   - String replace(char oldChar, char newChar)：返回一个新的字符串，它是通过用 newChar 替换此字符串中出现的所 oldChar 得到的。
   - String replace(CharSequence target, CharSequence replacement)：使用指定的字面值替换序列替换此字符串所匹配字面值目标序列的子字符串。
   - String replaceAll(String regex, String replacement)：使用给定的 replacement 替换此字符串所匹配给定的正则表达式的子字符串。
   - String replaceFirst(String regex, String replacement)：使用给定的 replacement 替换此字符串匹配给定的正则表达式的第一个子字符串。
2. 匹配:
   - boolean matches(String regex)：告知此字符串是否匹配给定的正则表达式。
3. 切片：
   - String[] split(String regex)：根据给定正则表达式的匹配拆分此字符串。
   - String[] split(String regex, int limit)：根据匹配给定的正则表达式来拆分此字符串，最多不超过limit个，如果超过了，剩下的全部都放到最后一个元素中。

**代码示例**

```java
@Test
public void test4() {
    String str1 = "北京你好，你好北京";
    String str2 = str1.replace('北', '南');

    System.out.println(str1);//北京你好，你好北京
    System.out.println(str2);//南京你好，你好南京

    String str3 = str1.replace("北京", "上海");
    System.out.println(str3);//上海你好，你好上海

    System.out.println("*************************");
    String str = "12hello34world5java7891mysql456";
    //把字符串中的数字替换成,，如果结果中开头和结尾有，的话去掉
    String string = str.replaceAll("\\d+", ",").replaceAll("^,|,$", "");
    System.out.println(string);//hello,world,java,mysql

    System.out.println("*************************");
    str = "12345";
    //判断str字符串中是否全部有数字组成，即有1-n个数字组成
    boolean matches = str.matches("\\d+");
    System.out.println(matches);//true
    String tel = "0571-4534289";
    //判断这是否是一个杭州的固定电话
    boolean result = tel.matches("0571-\\d{7,8}");
    System.out.println(result);//true


    System.out.println("*************************");
    str = "hello|world|java";
    String[] strs = str.split("\\|");
    for (int i = 0; i < strs.length; i++) {
        System.out.println(strs[i]);//依次输出hello word java
    }
    System.out.println();
    str2 = "hello.world.java";
    String[] strs2 = str2.split("\\.");
    for (int i = 0; i < strs2.length; i++) {
        System.out.println(strs2[i]);//依次输出hello word java
    }
}
```

##  5.String与其他结构转换

### 1.String与基本数据类型、包装类进行转换

**String --->    基本数据类型、包装类**：调用包装类的静态方法`parseXxx(String str)`

**基本数据类型、包装类   --- >    String**：调用String重载的`valueOf(xxx)`方法

**代码示例**

```java
  @Test
    public void test(){
        String str = "123";

        // String -->  基本数据类型
        int i = Integer.parseInt(str);
        System.out.println(i);

        // 基本数据类型 --> String
        String s1 = String.valueOf(i);
        System.out.println(s1);
    }
```

### 2.String与char[]数组之间的转换

**String -->  char[] ：** 通过调用String类的`toCharArray()`

**char[] -->  String：**通过调用String的一个构造器即可

**代码示例**

```java
 	/*
        string和char[]之间的转换
    */
    @Test
    public void test2(){
        String str = "hello";
        char[] charArray = str.toCharArray();
        for (int i = 0; i < charArray.length; i++) {
            System.out.println(charArray[i]);
        }

        char[] arr = new char[]{'w','s','g','b'};
        String s1 = new String(arr);
        System.out.println(s1);
    }
```

### 3.String与byte[]（字节数组）数组之间的转换

编码：String --> byte[]:调用String的getBytes()

解码：byte[] --> String:调用String的构造器

编码：字符串 -->字节 (看得懂 --->看不懂的二进制数据)

解码：编码的逆过程，字节 --> 字符串 （看不懂的二进制数据 ---> 看得懂

说明：解码时，要求解码使用的字符集必须与编码时使用的字符集一致，否则会出现乱码。

```java
@Test
public void StringToByteTest() throws UnsupportedEncodingException {
    String s1 ="你好java世界";
    byte[] bytesArray = s1.getBytes();//使用默认字符集编码
    System.out.println(Arrays.toString(bytesArray));//[-28, -67, -96, -27, -91, -67, 106, 97, 118, 97, -28, -72, -106, -25, -107, -116]

    byte[] gbks = s1.getBytes("gbk");//使用gbk编码集合
    System.out.println(Arrays.toString(gbks));//[-60, -29, -70, -61, 106, 97, 118, 97, -54, -64, -67, -25]

    System.out.println("--------------------------------");

    String str1=new String(bytesArray);//使用默认字符进行解码
    System.out.println(str1);//你好java世界

    String str2 = new String(gbks);//使用默认字符对gbk编码进行解码
    System.out.println(str2);//���java����解码错误，出现中文乱码,原因：编码和解码不一致

    String str3 = new String(gbks,"gbk");//使用gbk格式进行解码
    System.out.println(str3);//你好java世界，解码正确，原因：编码和解码一致
}
```

### 4.String与StringBuffer和StringBuilder之间的转换

StringBuffer和StringBuilder 转换为 String：①.通过调用String的构造器实现 ② 通过`toString()`

String转换为StringBuffer和StringBuilder：通过StringBuffer和StringBuilder的构造器实现

**代码示例：**

```java
@Test
    public void test4(){

        String str = "hello!";
        // String转换成StringBuffer或StringBuilder
        StringBuffer sb = new StringBuffer(str);
        System.out.println(sb);

        //StringBuffer和StringBuilder转换成String
        //方式一：通过toString()
        String s2 = sb.toString();
        System.out.println(s2);
        //方式二：通过String构造器
        String s3 = new String(sb);

    }
```



# 二、StringBuffer和StringBuilder

## 1.StringBuffer和StringBuilder概述

`StringBuffer`是在**JDK1.0**中声明的，==**<span style='color:red;'>线程安全</span>**==，代表**<span style='color:red;'>可变化的字符序列</span>**

`StringBuilder`是在**JDK1.5**中声明的，==**<span style='color:red;'>线程不安全</span>**==，同样代表**<span style='color:red;'>可变化的字符序列</span>**

+ StringBuffer和StringBuilder与String类不同，他们两个的对象必须通过构造器生成。
+ StringBuffer和StringBuilder是**<span style='color:red;'>可变的字符序列</span>**，而String是**<span style='color:red;'>不可变的字符序列</span>**
+ StringBuffer的无参构造器`new StringBuffer()，默认初始化容量为16的字符串缓冲区`
+ `new StringBuffer(int size)`：构造指定容量大小的字符串缓冲区
+ 如果StringBuffer的**容量不够时**，会**自动扩容**，==**<span style='color:red;'>扩容之后的容量 =  (原来容量 * 2)  + 2</span>**==



## 2.StringBuffer和StringBuilder的常用方法

StringBuffer和StringBuilder的常用方法类似，这边以StringBuffer为例。

1. StringBuffer append(xxx)：提供了很多的append()方法，用于进行字符串拼接
2. StringBuffer delete(int start,int end)：删除指定位置的内容
3. StringBuffer replace(int start, int end, String str)：把[start,end)位置替换为str
4. StringBuffer insert(int offset, xxx)：在指定位置插入xxx
5. StringBuffer reverse() ：把当前字符序列逆转

- public int indexOf(String str)：返回子串的下标
- public String substring(int start,int end):返回一个从start开始到end索引结束的左闭右开区间的子字符串
- public int length()：获取字符串的长度
- public char charAt(int n )：返回指定位置的字符
- public void setCharAt(int n ,char ch)：设置指定位置的字符

**总结方法：**

	+ 增：append(xxx)
	+ 删：delete(xxx)
	+ 改：setCharAt(int n ,char ch) / replace(int start, int end, String str)
	+ 查：charAt(int n )
	+ 插：insert(int offset, xxx)
	+ 长度： length()
	+ 遍历：for()

**代码示例**

```java
@Test
public void stringBufferMethodTest(){
    StringBuffer s1 = new StringBuffer("abc");
    System.out.println(s1);

    System.out.println(s1.append("1"));//abc1
    System.out.println(s1.delete(0, 1));//bc1
    System.out.println(s1.replace(0, 1, "hello"));//helloc1
    System.out.println(s1.insert(3, "v"));//helvloc1
    System.out.println(s1.reverse());//1colvleh
}
```



## 3.String、StringBuffer和StringBuilder三者的对比

+ String：不可变的字符序列；底层使用char[]存储；占用内存（会不断的创建和回收对象）
+ StringBuffer：可变的字符序列；**线程安全的，效率低**；底层使用char[]存储；
+ StringBuilder：可变的字符序列；jdk5.0新增的，**线程不安全的，效率高；**底层使用char[]存储

```

注意：作为参数传递的话，方法内部Stng不会改变其值， StringBuffer和 StringBuilder会改变其值。

```

**选用的标准**：开发中建议大家使用：StringBuffer和StringBuilder。**那StringBuffer和StringBuilder之间的选择主要是依据是否涉及多线程的安全问题**



### 1.**三者执行效率的对比**

从高到低排列：StringBuilder > StringBuffer > String

```java
@Test
public void test3(){
    //初始设置
    long startTime = 0L;
    long endTime = 0L;
    String text = "";
    StringBuffer buffer = new StringBuffer("");
    StringBuilder builder = new StringBuilder("");
    //开始对比
    startTime = System.currentTimeMillis();
    for (int i = 0; i < 20000; i++) {
        buffer.append(String.valueOf(i));
    }
    endTime = System.currentTimeMillis();
    System.out.println("StringBuffer的执行时间：" + (endTime - startTime));

    startTime = System.currentTimeMillis();
    for (int i = 0; i < 20000; i++) {
        builder.append(String.valueOf(i));
    }
    endTime = System.currentTimeMillis();
    System.out.println("StringBuilder的执行时间：" + (endTime - startTime));

    startTime = System.currentTimeMillis();
    for (int i = 0; i < 20000; i++) {
        text = text + i;
    }
    endTime = System.currentTimeMillis();
    System.out.println("String的执行时间：" + (endTime - startTime));

}
```



# 三、JDK8.0以前的 日期时间API

## 1.java.lang.System类

System类提供了`public static long currentTimeMillis()` 用来返回当前时间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差。也称为**时间戳。**

**代码示例**：

```java
//获取系统当前时间：System类中的currentTimeMillis()
long time = System.currentTimeMillis();
//返回当前时间与1970年1月1日0时0分0秒之间以毫秒为单位的时间差。
//称为时间戳
System.out.println(time);
```

## 2.Java.util.Date类

JDK8.0之前的日期API。 

Java.sql.Date是Java.util.Date类的子类

### 1.构造器

+ Date()：使用无参构造器创建对象，可以获取本地当前时间
+ Date(long date)：参数是Long类型的时间戳

### 2.常用方法

+ toString()：将Date类对象转换成String类型。
+ getTime()：获取当前时间自1970年1月1日0时0分0秒以来的时间戳（毫秒值）

## 3.Java.sql.Date类

#### 1.构造器

+ Date()：创建Java.sql.Date类的对象。
+ Date(long date)：创建指定时间戳的java.sql.Date类对象

#### 2.常用方法

+ getTime()：获取当前Date对象对应的毫秒值。
+ **<span style='color:red;'>toString()：返回对象的年月日 ，格式：yyyy-MM-dd</span>**



## 4.Java.util.Date和Java.sql.Date类之间的转换

**思路：**

	1. 获取java.util.Date类的时间戳
	2. 将java.util.Date类的时间戳作为参数传递给java.sql.Date类的构造器即可

**代码示例**

```java
@Test
public void dateTest(){
    //构造器一：Date()：创建一个对应当前时间的Date对象
    Date date1 = new Date();
    System.out.println(date1.toString());//Sun Apr 19 13:35:12 CST 2020
    System.out.println(date1.getTime());//1587274512876

    //构造器二：创建指定毫秒数的Date对象
    Date date2 = new Date(15872745176L);
    System.out.println(date2.toString());
    System.out.println("-----------------------");

    //创建java.sql.Date对象
    java.sql.Date date3 = new java.sql.Date(1587274512876L);
    System.out.println(date3.toString());

    //如何将java.util.Date对象转换为java.sql.Date对象
    Date date4 = new Date();
    //第一种方式，存在问题：java.util.Date cannot be cast to java.sql.Date
    //        java.sql.Date date6 = (java.sql.Date) date4;
    //        System.out.println(date6);
    //第二种方式
    java.sql.Date date5 = new java.sql.Date(date4.getTime());
    System.out.println(date5);
}
```



## 5.java.text.SimpleDataFormat类

Date类不易于国际化，大部分方法过期。java.text.SimpleDataFormat类是一个不与语言环境有关的方式来格式化和解析日期的具体类。

它主要有**<span style='color:red;'>两个作用</span>**： **<span style='color:red;'>① 格式化</span>**：日期 --> 字符串   **<span style='color:red;'>②解析</span>**： 字符串 --> 日期

由于格式化和解析两个方法不是静态的，所以需要通过`SimpleDataFormat对象`调用。

### 1.SimpleDataFormat的实例化

SimpleDataFormat的构造方法主要有如下几个：

+ `SimpleDateFormat()`：无参，采用默认格式创建对象
+ `SimpleDateFormat(String pattern)`：有参，可以指定对应的格式创建对象

**举例**

```java
SimpleDateFormat sdf1 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
```



![image-20200419141139894](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200419141141.png)



### 2.SimpleDataFormat的格式化与解析操作

+ **格式化**：通过`SimpleDataFormat对象.format(Date date)`来进行格式化。

+ **解析**：通过`SimpleDataFormat对象.parse(String source)`：解析source字符串，返回Date对象。

```

注意：parse(source)方法中的source字符串的格式务必要和调用parse的SimpleDataFormat类中指定的格式一致，否则会解析失败（报错！）

```



**代码示例**

```java
@Test
    public void test() throws ParseException {
        Date date = new Date();
        System.out.println(date);

        //1.实例化SimpleDateFormat对象
        SimpleDateFormat sdf = new SimpleDateFormat();
        //2.格式化日期
        String format = sdf.format(date);
        System.out.println(format);


        //指定格式的实例化SimpleDateFormat对象
        SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        String format1 = sdf2.format(date);
        System.out.println(format1);


        //3.解析字符串，生成date对象：这里需要注意，parse(source)中的参数source的格式必须和SimpleDateFormat类指定的格式一致，否则解析失败
        Date parse = sdf2.parse(format1);
        System.out.println(parse);
    }
```

## 6.Calendar类：日历类

Calendar是一个**抽象基类**，主用用于完成日期字段之间相互操作的功能

+ 获取Calendar实例的方式有：①使用Calendar.getInstance()方法 ② 调用它的子类GregorianCalendar的构造器
+ 一个Calendar的实例是系统时间的抽象表示，通过get(int field)方法来取得想要的时间信息。比如YEAR、MONTH、DAY_OF_WEEK、HOUR_OF_DAY、MINUTE、SECOND 
+ public void set(int field,int value)
+ public void add(int field,int amount) 
+  public final Date get Time()
+ public final void set Time(Date date)
+ 获取月份时：一月是从**<span style='color:red;'>0开始的</span>**

### 1.实例化Calendar对象的方式

+ 方式一：创建其子类（GregonrianCalendar）的对象
+ 方式二：调用静态方法`getInstance()`

```java
Calendar c = Calendar.getInstance();
```

### 2.Calendar的常用方法

+ get():获取日期
+ set():设置日期
+ add():添加、修改日期
+ getTime:日历类-->Date
+ setTime:Date-->日历类

```java
Calendar calendar = Calendar.getInstance();
//        System.out.println(calendar.getClass());

//2.常用方法
//get()
int days = calendar.get(Calendar.DAY_OF_MONTH);//获取本月第几天
System.out.println(days);
System.out.println(calendar.get(Calendar.DAY_OF_YEAR));//获取本年第几天

//set()
//calendar可变性
calendar.set(Calendar.DAY_OF_MONTH,22);//设置本月第几天
days = calendar.get(Calendar.DAY_OF_MONTH);
System.out.println(days);

//add()
calendar.add(Calendar.DAY_OF_MONTH,-3);
days = calendar.get(Calendar.DAY_OF_MONTH);
System.out.println(days);

//getTime():日历类---> Date
Date date = calendar.getTime();
System.out.println(date);

//setTime():Date ---> 日历类
Date date1 = new Date();
calendar.setTime(date1);
days = calendar.get(Calendar.DAY_OF_MONTH);
System.out.println(days);
```





# 四、JDK8.0中的新的日期时间类

## 1.日期时间API的迭代

第一代：jdk1.0 Date类

第二代：jdk1.1 Calendar类，一定成都上替换Date类

第三代：jdk1.8提出了新的一套API。

## 2.前两代的问题

+ 可变性：像日期和时间这样的类应该是不可变的。
+ 偏移性：Date中的年份是从1900开始的，而月份都从0开始。
+ 格式化：**格式化只对Date类有用**，**Calendar类则不行。**此外，它们也不是线程安全的；不能处理闰秒等
+ Java8.0引入了新的API， 是java.time包下的
  + Java8.0吸收了Joda-Time的精华，以一个新的开始为Java创建优秀的API。新的java.time中包含了**本地日期（LocalDate），本地时间（LocalTime），本地日期时间（LocalDateTime），时区（ZonedDate time）和持续时间（Duration）的类**。历史悠久的**Date类**也新增了**toInstant()方法**用于**把Date转换成新的表示形式。**这些新增的本地化时间日期API大大简化了日期时间和本地化的管理。



## 3.Java8.0中新的日期时间API涉及的包

![image-20200421111833248](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200421111835.png)

## 4.本地日期、本地时间、本地日期时间的使用

+ 本地日期：LocalDate
+ 本地时间：localTime
+ 本地日期时间：LocalDateTime

### 1.说明



### 2.常用方法

![20200421112139.png](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200421112139.png)



**代码示例**

```java
 @Test
    public void test(){

        //now():获取系统默认的当前日期、当前时间、当前日期时间
        LocalDate localDate = LocalDate.now();
        LocalTime localTime = LocalTime.now();
        LocalDateTime localDateTime = LocalDateTime.now();

        System.out.println(localDate);//2021-03-30
        System.out.println(localTime);//20:01:44.061
        System.out.println(localDateTime);//2021-03-30T20:01:44.062

        //of():设置指定的年、月、日、时、分、秒。没有偏移量
        LocalDateTime localDateTime1 = LocalDateTime.of(2020, 5, 8, 10, 23, 24);
        System.out.println(localDateTime1);//2020-05-08T10:23:24

        //getXxx()；获取相关的属性
        System.out.println(localDateTime1.getYear());
        System.out.println(localDateTime1.getDayOfMonth());
        System.out.println(localDateTime1.getMonthValue());//3
        System.out.println(localDateTime1.getMonth());//March

        //体现不可变性
        //withXxx():设置相关的属性
        LocalDateTime localDateTime2 = localDateTime.withDayOfMonth(1);
        System.out.println(localDateTime1);
        System.out.println(localDateTime2);


        //体现不可变性
        LocalDateTime localDateTime3 = localDateTime.plusHours(10);
        System.out.println(localDateTime);
        System.out.println(localDateTime3);

        LocalDateTime localDateTime4 = localDateTime.minusDays(5);
        System.out.println(localDateTime);
        System.out.println(localDateTime4);


    }
```



## 5.瞬时点：Instant

### 1.说明

① 时间线上的一个瞬时点。概念上讲，他只是简单的表示从1970年0时0分0秒（UTC开始的秒数）

② 类似于java.util.Date类

### 2.常用方法

![image-20200421112259386](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200421112300.png)



**代码示例**

```java
 @Test
    public void test2(){
        //now():获取本初子午线对应的标准时间
        Instant instant = Instant.now();
        System.out.println(instant);
        //添加时间偏移量
        OffsetDateTime offsetDateTime = instant.atOffset(ZoneOffset.ofHours(8));
        System.out.println(offsetDateTime);

        //toEpochMilli()：获取自1970年1月1日0时0分0秒（UTC）开始的毫秒数  ---> Date类的getTime()
        long milli = instant.toEpochMilli();

        //ofEpochMilli(): 通过指定的毫秒值，获取Instant实例  ---> Date(long millis)
        Instant instant1 = Instant.ofEpochMilli(123871283781L);
        System.out.println(instant1);
    }
```



## 6.日期时间格式化类：DateTimeFormatter

### 1.说明

①：主要的作用：格式化和解析日期、时间

②：类似于SimpleDateFormat

### 2.DateTimeFormatter的使用

#### 1.实例化方式

​	通过预定义的标准格式或者自定义格式来实例化对象

+ 预定义的标准格式：ISO_LOCAL_DATE、ISO_LOCAL_DATE_TIME、ISO_LOCAL_TIME
+ **自定义的格式**：**<span style='color:red;'>ofPattern("yyyy-MM-dd HH:mm:ss")</span>**

#### 2.常用方法

![image-20200421112839437](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200421112840.png)



**代码示例**

```java
@Test
public void test3(){
    //        方式一：预定义的标准格式。
    //        如：ISO_LOCAL_DATE_TIME;ISO_LOCAL_DATE;ISO_LOCAL_TIME
    DateTimeFormatter formatter = DateTimeFormatter.ISO_LOCAL_DATE_TIME;
    //格式化:日期-->字符串
    LocalDateTime localDateTime = LocalDateTime.now();
    String str1 = formatter.format(localDateTime);
    System.out.println(localDateTime);//2020-04-21T19:13:13.530
    System.out.println(str1);//2020-04-21T19:13:13.53

    //解析：字符串 -->日期
    TemporalAccessor parse = formatter.parse("2000-04-21T19:13:13.53");
    System.out.println(parse);//{},ISO resolved to 2000-04-21T19:13:13.530
    //        方式二：
    //        本地化相关的格式。如：ofLocalizedDateTime()
    //        FormatStyle.LONG / FormatStyle.MEDIUM / FormatStyle.SHORT :适用于LocalDateTime
    DateTimeFormatter formatter1 = DateTimeFormatter.ofLocalizedDateTime(FormatStyle.LONG);
    //格式化
    String str2 = formatter1.format(localDateTime);
    System.out.println(str2);//2020年4月21日 下午07时16分57秒

    //      本地化相关的格式。如：ofLocalizedDate()
    //      FormatStyle.FULL / FormatStyle.LONG / FormatStyle.MEDIUM / FormatStyle.SHORT : 适用于LocalDate
    DateTimeFormatter formatter2 = DateTimeFormatter.ofLocalizedDate(FormatStyle.MEDIUM);
    //格式化
    String str3 = formatter2.format(LocalDate.now());
    System.out.println(str3);//2020-4-21

    //       重点： 方式三：自定义的格式。如：ofPattern(“yyyy-MM-dd hh:mm:ss”)
    DateTimeFormatter formatter3 = DateTimeFormatter.ofPattern("yyyy-MM-dd hh:mm:ss");
    String Str4 = formatter3.format(LocalDateTime.now());
    System.out.println(Str4);//2020-04-21 07:24:04

    TemporalAccessor accessor = formatter3.parse("2020-02-03 05:23:06");
    System.out.println(accessor);//{SecondOfMinute=6, HourOfAmPm=5, NanoOfSecond=0, MicroOfSecond=0, MinuteOfHour=23, MilliOfSecond=0},ISO resolved to 2020-02-03
}
```

## 7.其他日期时间相关API的使用

![image-20200421113038985](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200421113040.png)

### 1.带时区的日期时间

ZonedDateTime/ZoneId

**代码示例**

```java
    @Test
    public void test5(){
        //now():获取本时区的ZonedDateTime对象
        ZonedDateTime zonedDateTime = ZonedDateTime.now();
        System.out.println(zonedDateTime);
        //now(ZoneId id):获取指定时区的ZonedDateTime对象
        ZonedDateTime now = ZonedDateTime.now(ZoneId.of("Asia/Shanghai"));
        System.out.println(now);
    }


    @Test
    public void test4(){
        //查看所有的时区
        Set<String> zoneIds = ZoneId.getAvailableZoneIds();
        for (String zoneId : zoneIds) {
            System.out.println(zoneId);
        }
        //获取“Asia/Tokyo”时区对应的时间
        LocalDateTime localDateTime = LocalDateTime.now(ZoneId.of("Asia/Shanghai"));
        System.out.println(localDateTime);
    }
```

### 2.时间间隔：Duration

Duration——用于计算两个**“时间”**间隔，以秒和纳秒为基准。

![image-20200421113320241](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200421113321.png)



**代码示例**

```java
@Test
public void test3(){
    LocalTime localTime = LocalTime.now();
    LocalTime localTime1 = LocalTime.of(15, 23, 32);
    //between():静态方法，返回Duration对象，表示两个时间的间隔
    Duration duration = Duration.between(localTime1, localTime);
    System.out.println(duration);

    System.out.println(duration.getSeconds());
    System.out.println(duration.getNano());

    LocalDateTime localDateTime = LocalDateTime.of(2016, 6, 12, 15, 23, 32);
    LocalDateTime localDateTime1 = LocalDateTime.of(2017, 6, 12, 15, 23, 32);

    Duration duration1 = Duration.between(localDateTime1, localDateTime);
    System.out.println(duration1.toDays());

}
```

### 3.日期间隔：Period

period——用于计算两个**“日期”**间隔，以年、月、日衡量

![image-20200421113353331](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200421113354.png)

**代码示例**

```java
@Test
public void test4(){
    LocalDate localDate = LocalDate.now();
    LocalDate localDate1 = LocalDate.of(2028, 3, 18);

    Period period = Period.between(localDate, localDate1);
    System.out.println(period);

    System.out.println(period.getYears());
    System.out.println(period.getMonths());
    System.out.println(period.getDays());

    Period period1 = period.withYears(2);
    System.out.println(period1);

}
```

### 4.日期时间校正器：TemporalAdjuster

**代码示例**

```java
@Test
public void test5(){
    //获取当前日期的下一个周日是哪天？
    TemporalAdjuster temporalAdjuster = TemporalAdjusters.next(DayOfWeek.SUNDAY);

    LocalDateTime localDateTime = LocalDateTime.now().with(temporalAdjuster);
    System.out.println(localDateTime);

    //获取下一个工作日是哪天？
    LocalDate localDate = LocalDate.now().with(new TemporalAdjuster(){

        @Override
        public Temporal adjustInto(Temporal temporal) {
            LocalDate date = (LocalDate)temporal;
            if(date.getDayOfWeek().equals(DayOfWeek.FRIDAY)){
                return date.plusDays(3);
            }else if(date.getDayOfWeek().equals(DayOfWeek.SATURDAY)){
                return date.plusDays(2);
            }else{
                return date.plusDays(1);
            }

        }

    });

    System.out.println("下一个工作日是：" + localDate);
}
```

### 5.新的日期API与原来API的转换问题

![image-20200421113620480](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200421113621.png)





# 五、Java比较器 Comparable和Comparator接口

## 1.Java比较器的使用北京

+ Java中的对象，正常情况下，只能进行比较：== 或！=。不能使用>或<的。
+ 但是在开发场景中，我们需要对多个对象进行排序，言外之意，就需要比较对象的大小。
+ 如何实现？**使用Comparbale（自然排序）或者Comparator（定制排序）接口**

## 2.自然排序：使用Comparable接口

### 1.说明

1. 像String、包装类等实现了Comparable接口，重写了compareTo(obj)方法，给出了比较两个对象大小的方式。
2. 实现Comparable接口的String、包装类，是进行从小到大的排列。
3. 重写compareTo(obj)的规则： 如果当前对象this大于形参对象obj，则返回**正整数**， 如果当前对象this小于形参对象obj，则返回**负整数**， 如果当前对象this等于形参对象obj，则**返回零。**



### 2.Comparbale方式实现自然排序代码示例

```java
public class Goods implements  Comparable{

    private String name;
    private double price;

    //指明商品比较大小的方式:照价格从低到高排序,再照产品名称从高到低排序
    @Override
    public int compareTo(Object o) {
        //        System.out.println("**************");
        if(o instanceof Goods){
            Goods goods = (Goods)o;
            //方式一：
            if(this.price > goods.price){
                return 1;
            }else if(this.price < goods.price){
                return -1;
            }else{
                //                return 0;
                return -this.name.compareTo(goods.name);
            }
            //方式二：
            //           return Double.compare(this.price,goods.price);
        }
        //        return 0;
        throw new RuntimeException("传入的数据类型不一致！");
    }
    // getter、setter、toString()、构造器：省略
}
```

## 3.使用Comparator接口完成定制排序

### 1.说明

1. 背景：

当元素的类型没实现java.lang.Comparable接口而又不方便修改代码，或者实现了java.lang.Comparable接口的排序规则不适合当前的操作，那么可以考虑使用 Comparator 的对象来排序

2. 重写compare(Object o1,Object o2)方法，比较o1和o2的大小：

- 如果方法返回正整数，则表示o1大于o2；
- 如果返回0，表示相等；
- 返回负整数，表示o1小于o2。

### 2.代码示例

```java
Comparator com = new Comparator() {
    //指明商品比较大小的方式:照产品名称从低到高排序,再照价格从高到低排序
    @Override
    public int compare(Object o1, Object o2) {
        if(o1 instanceof Goods && o2 instanceof Goods){
            Goods g1 = (Goods)o1;
            Goods g2 = (Goods)o2;
            if(g1.getName().equals(g2.getName())){
                return -Double.compare(g1.getPrice(),g2.getPrice());
            }else{
                return g1.getName().compareTo(g2.getName());
            }
        }
        throw new RuntimeException("输入的数据类型不一致");
    }
}
```

## 4.两种排序方式对比

- Comparable接口的方式是一定的，保证Comparable接口实现类的对象在任何位置都可以比较大小。
- Comparator接口属于临时性的比较。

# 六、其他常用类

## 1.System类

- System类代表系统，系统级的很多属性和控制方法都放置在该类的内部。该类位于java.lang包。
- 由于该类的构造器是private的，所以无法创建该类的对象，也就是无法实例化该类。其内部的成员变量和成员方法都是static的，所以也可以很方便的进行调用。

**成员方法：**

- native long currentTimeMillis()：

  该方法的作用是返回当前的计算机时间，时间的表达格式为当前计算机时间和GMT时间（格林威治时间）1970年1月1号0时0分0秒所差的毫秒数。

- void exit(int status)

  该方法的作用是退出程序。其中 status的值为0代表正常退出，非零代表异常退出。使用该方法可以在图形界面编程中实现程序的退出功能等

- void gc()

  该方法的作用是请求系统进行垃圾回收。至于系统是否立刻回收，则取决于系统中垃圾回收算法的实现以及系统执行时的情况。

- String getProperty(String key)

  该方法的作用是获得系统中属性名为key的属性对应的值。系统中常见的属性名以及属性的作用如下表所示：

  ![image-20200421114933166](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200421114934.png)

  **代码示例：**

  ```java
  @Test
  public void test1() {
      String javaVersion = System.getProperty("java.version");
      System.out.println("java的version:" + javaVersion);
  
      String javaHome = System.getProperty("java.home");
      System.out.println("java的home:" + javaHome);
  
      String osName = System.getProperty("os.name");
      System.out.println("os的name:" + osName);
  
      String osVersion = System.getProperty("os.version");
      System.out.println("os的version:" + osVersion);
  
      String userName = System.getProperty("user.name");
      System.out.println("user的name:" + userName);
  
      String userHome = System.getProperty("user.home");
      System.out.println("user的home:" + userHome);
  
      String userDir = System.getProperty("user.dir");
      System.out.println("user的dir:" + userDir);
  
  }
  ```

## 2.Math类

java.lang.Math提供了一系列静态方法用于科学计算。其方法的参数和返回值类型一般为double型。

![image-20200421115050704](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200421115052.png)

## 3.BigInteger类和BigDecimal类

### 1.BigInteger

java.math包的BigInteger可以表示不可变的任意精度的整数。

BigInteger提供所有Java的基本整数操作符的对应物，并提供 java.lang.Math的所有相关方法。另外，BigInteger还提供以下运算：模算术、GCD计算、质数测试、素数生成、位操作以及一些其他操作。

构造器： BigInteger(String val)：根据字符串构建 BigInteger对象

![image-20200421115521295](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200421115522.png)

**代码举例：**

```java
@Test
public void test2() {
    BigInteger bi = new BigInteger("1243324112234324324325235245346567657653");
    BigDecimal bd = new BigDecimal("12435.351");
    BigDecimal bd2 = new BigDecimal("11");
    System.out.println(bi);
    //         System.out.println(bd.divide(bd2));
    System.out.println(bd.divide(bd2, BigDecimal.ROUND_HALF_UP));
    System.out.println(bd.divide(bd2, 25, BigDecimal.ROUND_HALF_UP));

}
```

### 2.BigDecimal

要求数字精度比较高，用到java.math.BigDecimal类

BigDecimal类支持不可变的、任意精度的有符号十进制定点数。

**构造器：**

public BigDecimal(double val)

public BigDecimal(String val)

**常用方法：**

public BigDecimal add(BigDecimal augend)

public BigDecimal subtract(BigDecimal subtrahend)

public BigDecimal multiply(BigDecimal multiplicand)

public BigDecimal divide(BigDecimal divisor， int scale， int rounding Mode)

**代码举例：**

```java
@Test
public void test2() {
    BigInteger bi = new BigInteger("1243324112234324324325235245346567657653");
    BigDecimal bd = new BigDecimal("12435.351");
    BigDecimal bd2 = new BigDecimal("11");
    System.out.println(bi);
    //         System.out.println(bd.divide(bd2));
    System.out.println(bd.divide(bd2, BigDecimal.ROUND_HALF_UP));
    System.out.println(bd.divide(bd2, 25, BigDecimal.ROUND_HALF_UP));

}
```



