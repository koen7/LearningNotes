# 一、Java语言概述



## 1.Java语言版本迭代概述

1991年 Green项目，开发语言最初命名为Oak (橡树)

1994年，开发组意识到Oak 非常适合于互联网

1996年，发布JDK 1.0，约8.3万个网页应用Java技术来制作

1997年，发布JDK 1.1，JavaOne会议召开，创当时全球同类会议规模之最

1998年，发布JDK 1.2，同年发布企业平台J2EE

1999年，Java分成J2SE、J2EE和J2ME，JSP/Servlet技术诞生

2004年，发布里程碑式版本：JDK 1.5，为突出此版本的重要性，**==更名为JDK 5.0==**

2005年，J2SE -> JavaSE，J2EE -> JavaEE，J2ME -> JavaME

2009年，Oracle公司收购SUN，交易价格74亿美元

2011年，发布JDK 7.0

2014年，发布JDK 8.0，是继JDK 5.0以来变化最大的版本

2017年，发布JDK 9.0，最大限度实现模块化

2018年3月，发布JDK 10.0，版本号也称为18.3

2018年9月，发布JDK 11.0，版本号也称为18.9



**在迭代部分最重要的是记住 ：**

​					 JDK1.5以后更名为JDK5.0 

​					JDK 1.6更名为JDK6.0

​					JDK1.8更名为==JDK8.0==



## 2.Java的应用场景

​	1.桌面软件的开发

​	2. 网站开发

​	3.安卓、大数据

​										

## 3.Java语言的特点



+ 面向对象

  ​	两个基本概念：类、对象

  ​	三大特性：封装、继承、多态

+ 健壮性

  ①去除了C语言中的指针 ② 自动的垃圾回收机制

+ 跨平台性

  ​	通过Java语言编写的程序在不同的系统平台上都可以运行。**write once  run anywhere** 



# 二、Java开发环境搭建



## 1.什么是JDK、JRE

JDK（Java Development Kit） Java开发工具包

​	JDK是提供给Java开发人员使用的，期中包含了Java的开发工具，也包括了JRE	，所以安装了JDK就不需要在单独安装JRE了。

JRE（Java Runtime Environment）Java运行环境

包括Java虚拟机(JVM Java Virtual Machine）和Java程序所需要的核心类库。如果想要<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>运行</span>一个开发好的Java程序，计算机中只需要安装JRE即可。

## 2.JDK、JRE、JVM的三者关系

![image-20200430120700919](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200430120702.png)

JDK官网下载： https://www.java.com/zh-CN/

安装：傻瓜式安装，JDK、JRE

建议：<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>安装软件的目录不要有空格、中文等</span>

## 3.配置环境变量

为什么要配置环境变量呢？

如果我们希望能在任何目录下执行java指令，那么我们就需要配置环境变量

配置的**步骤**：

​		1.计算机 - 右键 - 属性 - 高级系统设置 -  环境变量 （Win7系统）

​		2.在系统变量中 新建  变量名 JAVA_HOME 变量值：jdk的目录

​		3.在path变量的变量值的最前边添加：<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>%JAVA_HOME%\bin;</span>

**验证配置是否成功**

​	在命令行窗口 输入 javac.exe  java.exe java -version

![image-20210302212903350](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210302212903350.png)

![image-20210302212922339](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210302212922339.png)

![image-20210302212849756](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210302212849756.png)



## 4.第一个Java程序——HelloWorld

### 1. 开发流程

![image-20200430120743708](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200430120744.png)

### 2.编写HelloWorld.java文件

```java
class HelloJava{

	public static void main(String[] args){

		System.out.println("Hello");
	}

}
```



### 3.将源文件(.java)编译成字节码文件(.class)

 + 在cmd窗口：进入源文件目录

 + javac HelloWorld.java

   ![image-20210302221131057](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210302221131057.png)

### 4.运行

java HelloJava

![image-20210302221217243](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210302221217243.png)





## 5.注释

### 1.注释

//单行注释	/*	多行注释	*/		/** 文档注释	*/

**作用：**

①：对所写的程序进行解释说明，增强可读性。方便自己，也方便别人

②：可以调试所写的程序

**特点：**

①单行注释和多行注释，注释了的内容不参与编译。 换句话说，编译以后生成的.class结尾的字节码文件中不包含注释掉的信息

② 注释内容可以被JDK提供的工具 javadoc 所解析，生成一套以网页文件形式体现的该程序的说明文档。

③ 多行注释不可以嵌套使用

**生成注释文档**

```
语法: javadoc -d -author -version 源文件.java
```



### 2.Java API （Application Programming interface）

Java语言提供的类库。类似于《新华词典》



## 6.总结第一个程序

1.Java程序的开发流程是 编写 -  编译  - 运行

+ 编写：我们将编写的java代码保存在以".java"结尾的源文件中
+ 编译：使用javac.exe命令编译我们的源文件 命令：javac 源文件名.java
+ 运行：使用java.exe命令解释运行我们的字节码文件。 命令： java 字节码文件名

2.在一个源文件中可以有声明多个class。但是，最多只能有一个public class。而且要求声明为public的类的类名必须是和源文件名相同。

3.程序的入口是main方法，格式是固定的。

4.输出语句：

​		System.out.println()：先输出数据，后换行

​		System.out.print()：只输出数据

5.每一行的执行语句都以"；"结束

6.编译的过程：编译以后，会生成一个或多个字节码文件，字节码文件的文件名与Java源文件中的类名相同。



# 三、基本语法

## （一）、关键字和标识符

### 1.关键字的使用

定义：被Java赋予了特殊含义，用做专门用途的字符（单词）

特点：所有的关键字都是小写字母，以下是具体的关键字：

![image-20200430110458435](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200430110459.png)

![image-20200430110505797](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200430110507.png)

### 2.保留字（Reserved word）

现在Java版本尚未使用，但以后版本可能会作为关键字使用。具体保留的关键字：goto、const

注意：自己命令标识符时要避免使用这些保留字



### <span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>3.标识符</span>

+ Java对各种<span style='color:blue;background:背景颜色;font-size:文字大小;font-family:字体;'>**变量、方法**</span>和<span style='color:blue;background:背景颜色;font-size:文字大小;font-family:字体;'>**类**</span>等要素命名时使用的字符序列称为标识符
+ <span style='color:blue;background:背景颜色;font-size:文字大小;font-family:字体;'>**技巧：凡是是自己起名的地方都叫标识符**</span>

3.1定义

​	凡是自己可以起名字的地方都叫标识符。

3.2 标识符涉及到的地方

​	变量、类、方法、接口、包名

**3.3规则**  --> **如果不遵守规则，编译不通过！**

​	>	数字、字母、下划线和$组成

​	>	<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**不能以数字开头**</span>

​	>	避免使用关键字、保留字

​	>	Java严格区分大小写

​	>    不能有空格

3.4规范  --> 建议遵守规范、**如果不遵守规范，编译也可以通过**

​	包名：所有字母都是小写: xxxxyyyzzzz

​	类名、接口名：多个单词组成时、所有单词的首字母大写：XxxYyyZzz

​	变量名、方法名：多个单词组成似乎，第一个单词的首字母小写，接下来的单词的首字母大写

​	常量：所有字母都是大写；XXXYYYZZZ

```
注意点：在起名字时，为了提高代码的阅读性，要尽量见名知意
```



## （二）、变量的使用

### 1.变量的定义

```
定义变量的格式:  数据类型 变量名 = 变量值; 
```

### 2.使用变量的注意：

```
1.变量先声明，再赋值，最后才可以使用
2.同一个作用域中不能有同名的变量
3.变量的作用域：其定义所在的一对{}内
4.使用变量名访问区域的数据
```



### 3.变量的分类

1.变量按照**变量类型**分类：

![image-20200430111438466](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200430111439.png)



**变量类型**详细说明：

 + 整型：byte（1字节=8bit）、short（2字节）、int（4字节）、long（8字节）

   ​	① byte范围： ~128 - 127

   ​	② short范围： ~2的15次方 - 2的15次方 - 1

   ​	③ 声明long类型时，需要在结尾加“l”或者“L”

   ​	④ 整型一般默认使用int类型。

 + 浮点型：float（4字节）、double（8字节）

   ​	① 浮点型，表示带小数点的数值

   ​	<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>② float表示数值的范围比long还大</span>

   ​	③ 定义float类型的变量时，<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>变量要以“f”或者“F”结尾</span>

   ​	④ 通常定义浮点型变量时，使用double型。

 + 字符型：char（1字符=2字节）

   ​	①.字符型使用一对 ' '，‘ ’内部中只能使用一个字符

   ​	②. 表示方式：1.声明一个字符 2.声明一个转移字符 3.使用Unicode值来表示字符型常量

 + 布尔型：boolean

   ​	① 只能取两个值之一：true、false

   ​	② 常常在条件判断、循环结构中使用

   


2.变量按照**作用域**分类：

​	成员变量

​	局部变量

![image-20200430111643457](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200430111644.png)

### 4.基本数据类型之间的转换

4.1  涉及到的基本数据类型

除了boolean以外的7种

**4.2 自动类型转换**

	+ 结论：当容量小的数据类型和容量大的数据类型之间做运算时，会自动转换到容量大的数据类型
	+ byte 、short、char -- > int -- > long  -- > float-- > double 
	+ **特别的：当byte 、short、char三种类型的变量做运算时，结果为int型**
	+ 说明：这里说的**容量**是表示数据类型发范围，而不是字节数

4.3 强制类型转换

 + 需要使用强制符：（）

 + <span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**强制类型转换会导致精度损失**</span>

   ![image-20210304131717053](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210304131717053.png)

   

   

**4.4 String与其他8基本数据类型**

	- <span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>String属于引用数据类型</span>，翻译为：字符串
	- String的声明是使用一对“”
	- String可以和8种基本类型做运算，且只能是加法运算 ，代表的是字符串的连接
	- 运算的结果仍然是String类型

```java
class StringTest{
	
	public static void main(String[] args){
		
		// 练习1
		char c ='a';
		int num= 10;
		String str="hello";
		System.out.println(c + num + str); //107hello
		System.out.println(c + str + num);//ahello10
		System.out.println(c + (num + str));//a10hello
		System.out.println((c + num )+ str);//107hello
		System.out.println(str + num + c);//hello10a
		
		
		// 练习2
		//*	*
		System.out.println("*	*");
		System.out.println('*' + '\t' + '*');
		System.out.println('*' + "\t" + '*');
		System.out.println('*' + '\t' + "*");
		System.out.println('*' + ('\t' + "*"));
	}
}
```



## （三）、进制之间的转换<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>（了解）</span>



### 	1.计算机中常见进制

在计算机底层中，都是以二进制的形式存在的。

对于整数，有四种表示方法：

			+	二进制：0,1 ，满2进1，以<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>0b</span>或者<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>0B</span>开头
			+	十进制：0-9，满十进1。
			+	八进制：0-7，满8进1，以数字<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>0</span>开头
			+	十六进制：0-9，A-F，满16进1，以<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>0x</span>或者<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>0X</span>开头表示。此处的A-F不区分大小写

```java
class BinaryTest{
	
	public static void main(String[] args){
		int num1 = 123; //十进制
		int num2 = 0B110;//二进制
		int num3 = 0123;//八进制
		int num4 = 0xab110;//十六进制
		System.out.println(num1);
		System.out.println(num2);
		System.out.println(num3);
		System.out.println(num4);
	}
}
```

### 2.十进制 二进制互转

![image-20200430113138334](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200430113139.png)

		+ <span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>二进制转十进制，乘以2的幂数</span>
		+ <span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>十进制转二进制，除于2取余的逆</span>



## （四）、 运算符

Java中的运算符主要有 算术运算符、赋值运算符、比较运算符、逻辑运算符、位运算符、三元运算符

### 1.算术运算符

![image-20210304160904954](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210304160904954.png)



```java
class AriTest{
	
	public static void main(String[] args){
		
		//除号： /
		int num1 = 12;
		int num2 = 5;
		int result1 = num1 / num2 ;
		System.out.println(result1); //2
		
		// %:取余运算
		// 结果的符号与被模数的符号相同
		
		int m1 = 12;
		int n1 = 5;
		System.out.println("m1 % n1 =" + m1 % n1);
		
		int m2 = -12;
		int n2 = 5;
		System.out.println("m2 % n2 =" + m2 % n2);
		
		int m3 = 12;
		int n3 = -5;
		System.out.println("m3 % n3 =" + m3 % n3); 
		
		int m4 = -12;
		int n4 = -5;
		System.out.println("m4 % n4 =" + m4 % n4);
		
		//（前）++ ：先自增1，后运算
		int a1 = 10;
		int b1 = ++a1;
		System.out.println("a1 = " + a1 +",b1 = " + b1) ;
		//（后）++  先运算，后自增1
		int a2 = 10;
		int b2 = a2++;
		System.out.println("a2 = " + a2 +",b2 = " + b2) ;
		
		//注意点：
		short s1 = 10;
		//s1 = s1 + 1;//编译失败
		//s1 =(short)(s1 + 1);//正确的
		s1++;
		
		//（前）--：先自减1，再运算
		//（后）--: 先运算，再自减1
		
		
	}
}
```

```java
class AriExer{
	
	public static void main(String[] args){
		/*
		练习:随意给出一个三位数的整数，打印显示它的个位数、十位数、百位数的值
		格式如下：
		数字xxx的情况如下：
		个位数：
		十位数：
		百位数：
		
		例如：
		数字153的情况如下：
		个位数：3
		十位数：5
		百位数：1
		
		*/
		
		int num = 187;
		int bai = num  / 100;
		int shi = num % 100 / 10;
		int ge = num % 10;
		System.out.println("百位数:" + bai);
		System.out.println("百位数:" + shi);
		System.out.println("百位数:" + ge);
		
	}
}
```

### 2.赋值运算符

常见的赋值运算符有：  = ，+= ，%=，/=等等

```java
class SetValueTest{
	
	public static void main(String[] args){
		
		//赋值符号: =
		int a1 = 10;
		int b1 = 20;
		
		//连续赋值
		int a2,b2;
		a2= b2 = 20;
		
		int a3 = 10,b3= 20;
		
		// 赋值运算： +=
		int num1 = 10;
		num1 += 2;
		System.out.println(num1);
		
		//赋值运算:  %=，*=，/= 和+=类似，不再举例 
		
		short s1 = 10;
		s1 = s1 + 2;// 编译失败
		s1 += 2;// 不会改变变量本身的数据类型
		
		// 开发中，如果希望变量实现+2的操作，有几种方法
		// 方式一： num = num + 2;
		// 方式二： num += 2;（推荐，不会改变数据类型）
	}
}
```

### 3.比较运算符

![image-20210304173422676](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210304173422676.png)

<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**注意：Java中比较是否相等 是==，不是=**</span>

```java
int i = 10;
int j = 20;

System.out.println(i == j);//false
System.out.println(i = j);//20

boolean b1 = true;
boolean b2 = false;
System.out.println(b2 == b1);//false
System.out.println(b2 = b1);//true
```

### 4.逻辑运算符

&——逻辑与， |——逻辑或	！——逻辑非

&&——短路与，|| ——短路或 ，^——逻辑异或

```java
class LogicTest{
	
	public static void main(String[] args){
		
		boolean b1 = false;
		int num1 = 10 ;
		if(b1 & (num1++ > 0)){
			
			System.out.println("今天大太阳");
		}else{
		
			System.out.println("今天下雨");
		}
		System.out.println("num1 = " +num1);
		
		
		boolean b2 = false;
		int num2 = 10 ;
		if(b2 && (num2++ > 0)){
			
			System.out.println("今天大太阳");
		}else{
			System.out.println("今天下雨");
		}
		System.out.println("num2 = " +num2);
		
		
		
		boolean b3 = true;
		int num3 = 10 ;
		if(b3 | (num3++ > 0)){
			
			System.out.println("今天大太阳");
		}else{
		
			System.out.println("今天下雨");
		}
		System.out.println("num3 = " +num3);
		
		
		boolean b4 = true;
		int num4 = 10 ;
		if(b4 || (num4++ > 0)){
			
			System.out.println("今天大太阳");
		}else{
			System.out.println("今天下雨");
		}
		System.out.println("num4 = " +num4);
	}
}
```

<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**特别说明：**</span>

	+ 逻辑运算符的两边需要是boolean类型
	+ 逻辑运算符的结果也是boolean类型

<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**&和&&的相同点和不同点**</span>

+ 当&和&&运算符的左边都为true时，两者都会执行运算符右边。
+ &和&&的运算结果相同
+ 当&和&&运算符的左边为flase时，&会继续执行右边的操作，而&&不会执行右边操作
+ 开发中推荐使用&&

<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**（| 和||的相同点和不同点）**</span>

+ 当|和||运算符的左边都为false时，两者都会执行运算符右边
+ |和||的运算结果相同
+ 当|和||运算符的左边为true时，|会继续执行运算符右边的操作，而||不会执行运算符右边操作
+ 开发中推荐使用||

### 5.位运算符 （了解）

![image-20210304185540308](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210304185540308.png)

```java
class BitTest{
	
	public static void main(String[] args){
		int i = 21;
		// 每左移一次，* 2
		System.out.println("i << 2 :" + (i << 2));
		
		// 每右移一次，/ 2
		System.out.println("i >> 2 :" + (i >> 2));
		
		i = -21;
		System.out.println("i << 2 :" + (i << 2));
		System.out.println("i >> 2 :" + (i >> 2));
	}
}
```

【运行结果】：

![image-20210304185945311](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210304185945311.png)

【特别说明：】

 + 位运算符操作的都是整型数值
 + << :<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>在一定范围内</span>，每左移1位，相当于✖ 2；
 + ">>":<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>在一定范围内</span>，每右移1位，相当于➗ 2；
 + & 、|、^、~运算如下图所示，跟逻辑与、逻辑或、逻辑异或运算规则相同

![image-20210304190703328](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210304190703328.png)

【经典题目】：交换两个变量的值

​	①借用临时变量

```java
int num1 = 10;
int num2 = 20;
int temp = num1;
num1 = num2;
num2 = temp;
```

​	②加减交换:节省内存，但是会超出存储范围，而且只能适用于数值型运算，

```java
num1 = num1 + num2;
num2 = num1 - num2;
num1 = num1 - num2;
```

​	③使用位运算符：只适用于数值型运算

```;java
num1 = num1 ^ num2;
num2 = num1 ^ num2;
num1 = num1 ^ num2;
```

### 6 三元运算符

+ 格式：（条件表达式）? 表达式1：表达式2

+ 条件表达式为true，执行表达式1，条件表达式为false，执行表达式2


【代码】：

```java
// 1.获取两个整数的较大值
		int a = 3;
		int b = 6;
		int c = 9;
		int max = (a > b)? a : c ;
		System.out.println(t);

// 2.获取三个数的最大值
		int temp = (a > b)? a : c ;
		int max2 = (max > c) ? max : c;
		System.out.println(max2);
```

【特别说明】：

	+ 条件表达式的值是boolean类型
	+ 条件表达式若真，执行表达式1，若条件表达式为假，执行表达式2。
	+ 三元运算符可以嵌套使用
	+ 凡是可以用到三元运算符的地方也可以使用三元运算符，反之不一定
	+ 如果程序既可以使用三元运算符，又可以使用if-else结构，那么优先选择三元运算符。原因：简洁、执行效率高。



## （五）、流程控制

### 1.分支结构IF

```java
IF有三种结构
结构一
    if(条件表达式){
        条件表达式为true,执行这里
    }

结构二： 二选一
    if(条件表达式){
        条件表达式为true，执行该{}内
    }else{
        条件表达式为false，执行该{}内
    }

结构三：多选一
    if(条件表达式){
        执行表达式1
    }else if(条件表达式2){
        执行表达式2
    }else if(条件表达式3){
        执行表达式3
    }else{
        都不满足，执行表达式n
    }
```

【特别说明】：

+ else结构是可选的

 + 针对于条件表达式
   	+ 如果多个条件表达式是互斥关系（没有交集的部分），那么哪个判断和执行语句声明在上面还是下面，无所谓
   	   	+ 如果多个条件表达式是有交集关系的，那么需要根据实际情况，考虑清楚应该将哪个放在上面
   	+ if-else 可以嵌套
   	+ 当if-else中只有一行时｛｝ 可以省略，但一般不建议省略！

```
生成(a,b]之间的随机数
公式： int(Math.random() * (b - a + 1) + a);
```

### 2.分支结构switch-case 

格式：

```java
switch(表达式){
	case 常量1：
        执行语句1;
        //break;
    case 常量2：
        执行语句2;
        //break;
    case 常量3:
        执行语句3;
        //break;
    default:
        执行语句n;
        //break;
}
```

【说明】：

	+ 根据switch中表达式的值，依次去匹配case中的常量，如果匹配到，就进入相关的case中执行相关语句。<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>当调用完相关语句会仍然继续执行接下来的其他case中的执行语句，直到遇到break关键字或此switch-case结构末尾为止。</span>
	+ break，可以在switch-case结构中使用，表示一旦执行到break关键字，就跳出switch-case结构
	+ 表达式的类型只能是：byte、short、int、char、enum（JDK5.0新增）、String（JDK7.0新增）
	+ <span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>case之后只能声明常量，不能是范围</span>
	+ break关键字是可选的。
	+ default 相当于if -else中的else，default结构是可选的。
	+ 当多个case的执行语句相同时，可以考虑合并执行语句。

### 3、循环结构

Java中规定了三种循环结构，for循环、while循环、do-while循环

**循环语句的四个组成部分**

![image-20210305153838242](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210305153838242.png)



	+ 初始化部分
	+ 循环条件部分
	+ 循环体部分
	+ 迭代部分

#### 1.for循环结构

一、循环结构的4个部分

①：初始化部分   -- > 自始至终只执行一次

②：循环条件部分   -->   布尔类型

③：循环体部分

④：迭代部分

```
for(①;②;④){
    ③
}

执行过程：  1️⃣ --  2️⃣  -- 3️⃣  -- 4️⃣  -- 2️⃣  -- 3️⃣ -- 4️⃣  .... -- 2️⃣ 
```

```java
import java.util.Scanner;
class ForTest2{

	/*
		输入两个正整数m和n，求其最大公约数和最小公倍数
		比如：12和20的最大公约数4，最小公倍数60
	
	*/
	public static void main(String[] args){
	
		Scanner sc = new Scanner(System.in);
		System.out.println("请输入第一个正整数");
		int m = sc.nextInt();
		
		System.out.println("请输入第二个正整数");
		int n = sc.nextInt();
		
		int min  = (m < n) ? m : n;
		for(int i = min;i >=1;i--){
				
				if(m % i == 0 && n % i ==0){
					System.out.println("最大公约数为：" + i);
					break;
				}
		}
	
		int max = (m > n )? m: n;
		for(int i = max;i <= m*n;i++){
			if(i % n == 0 && i % m == 0){
				System.out.println("最大公倍数为：" + i);
					break;
			}
		}
	}
}
```

#### 2.while循环结构

①：初始化部分   -- > 自始至终只执行一次

②：循环条件部分   -->   布尔类型

③：循环体部分

④：迭代部分

```
①
while(②){
    
    ③
    ④
}

执行过程：  1️⃣ --  2️⃣  -- 3️⃣  -- 4️⃣  -- 2️⃣  -- 3️⃣ -- 4️⃣  .... -- 2️⃣ 
```

【说明】

	+ 写while循环千万小心不要丢了迭代部分。一旦丢了，就可能导致死循环
	+ 尽量避免死循环
	+ for和while循环可以相互转换的！



#### 3.do-while循环

①：初始化部分   -- > 自始至终只执行一次

②：循环条件部分   -->   布尔类型

③：循环体部分

④：迭代部分

```
// 格式
①
do{
    ③
    ④
}while(②)

执行过程：1️⃣  -  3️⃣  - 4️⃣  -  2️⃣ - 3️⃣  - 4️⃣ - 2️⃣  -..... -2️⃣
```

【说明】

	+ do-while循环至少执行一次！
	+ 在开发中我们用while和for居多

```java
/*
		题目：从键盘读入个数不确定的整数，并判断读入的正数和负数的个数，输入为0时结束程序
*/

import java.util.Scanner;	

class ForWhileTest{
	
	public static void main(String[] args){
	
		Scanner scan = new Scanner(System.in);
		
		int postiviteNumber = 0;
		int negativeNumber = 0;
		while(true){
			
			int num = scan.nextInt();
			if(num > 0) {
				postiviteNumber++;
			}else if(num < 0){
				negativeNumber++;
			}else{
				break;
			}
		}
		
		System.out.println("正数的个数为:" + postiviteNumber);
		System.out.println("负数的个数为:" + negativeNumber);
	}
}
```

#### 4.嵌套循环

1.定义：将一个循环结构A声明在另一个循环结构B的循环体中，就构成了嵌套循环

2.

外层循环：循环结构B

内层循环：循环结构A

【说明】

	+ 假设外层循环需要执行m次，内层循环需要执行n次，此时内层循环的循环体一共需要执行m*n次。
	+ 内层循环遍历一遍，相当于外层循环循环体执行一次。

【题目】

```java

/*
	题目:求100以内的质数
	质数: 也称为素数，只能被1和它本身除的数 例如: 3,7
	最小的质数是2
	
	对PrimeNumberTest.java中质数输出问题的优化

*/
class PrimeNumberTest2{
	
	public static void main(String[] args){
		
		for(int i = 2; i <= 100; i++ ){
			
			boolean flag = true;
			// 优化二:对本身是质数的自然数是有效的
			for(int j = 2; j <= Math.sqrt(i); j++){
			//for(int j = 2; j < i; j++){
				
				if(i % j == 0){
					
					flag = false;//不是质数
					break;//优化一： 只对非质数的自然数是有效的。
				
				}
			}
			if(flag == true){
				System.out.println(i);
			}
		}
	}
}

```



## 4.关键字 break和continue

```
				使用范围			作用					相同点

break:			switch-case		结束当前循环			break后面不能声明执行语句

continue:		循环结构体中		结束当次循环			continue后面不能声明执行语句
```

return在后面的面向对象的方法中讲



## 项目一：家庭收支记账软件

工程组成部分： Utility工具类 + FamilyAccount.java

**Utility工具类**

```java
import java.util.Scanner;
/**
Utility工具类：
将不同的功能封装为方法，就是可以直接通过调用方法使用它的功能，而无需考虑具体的功能实现细节。
*/
public class Utility {
    private static Scanner scanner = new Scanner(System.in);
    /**
	用于界面菜单的选择。该方法读取键盘，如果用户键入’1’-’4’中的任意字符，则方法返回。返回值为用户键入字符。
	*/
	public static char readMenuSelection() {
        char c;
        for (; ; ) {
            String str = readKeyBoard(1);
            c = str.charAt(0);
            if (c != '1' && c != '2' && c != '3' && c != '4') {
                System.out.print("选择错误，请重新输入：");
            } else break;
        }
        return c;
    }
	/**
	用于收入和支出金额的输入。该方法从键盘读取一个不超过4位长度的整数，并将其作为方法的返回值。
	*/
    public static int readNumber() {
        int n;
        for (; ; ) {
            String str = readKeyBoard(4);
            try {
                n = Integer.parseInt(str);
                break;
            } catch (NumberFormatException e) {
                System.out.print("数字输入错误，请重新输入：");
            }
        }
        return n;
    }
	/**
	用于收入和支出说明的输入。该方法从键盘读取一个不超过8位长度的字符串，并将其作为方法的返回值。
	*/
    public static String readString() {
        String str = readKeyBoard(8);
        return str;
    }
	
	/**
	用于确认选择的输入。该方法从键盘读取‘Y’或’N’，并将其作为方法的返回值。
	*/
    public static char readConfirmSelection() {
        char c;
        for (; ; ) {
            String str = readKeyBoard(1).toUpperCase();
            c = str.charAt(0);
            if (c == 'Y' || c == 'N') {
                break;
            } else {
                System.out.print("选择错误，请重新输入：");
            }
        }
        return c;
    }
	
	
    private static String readKeyBoard(int limit) {
        String line = "";

        while (scanner.hasNext()) {
            line = scanner.nextLine();
            if (line.length() < 1 || line.length() > limit) {
                System.out.print("输入长度（不大于" + limit + "）错误，请重新输入：");
                continue;
            }
            break;
        }

        return line;
    }
}

```

**FamilyAccount.java**

```java

class FamilyAccount{
	
	
	public static void main(String[] args){

		//用于控制循环开始结束的标记
		boolean flag = true;
		
		//用于显示收入支出明细信息
		String inOutRecord = "";
		
		//账户初始余额
		int balance = 10000;
		while(flag){
		
			System.out.println("-----------------家庭收支记账软件-----------------\n");
			System.out.println("                  1 收支明细");
			System.out.println("                  2 登记收入");
			System.out.println("                  3 登记支出");
			System.out.println("                  4 退    出\n");
			System.out.print("                    请选择(1-4)：\n");
			
			char selection = Utility.readMenuSelection();
			
			switch(selection){
				case '1':
					System.out.println("-----------------当前收支明细记录-----------------");
					System.out.println("收支\t\t账户金额\t\t收支金额\t\t说    明");
					System.out.println(inOutRecord);
					break;
				case '2':
					System.out.println("本次收入金额：");
					int inMoney = Utility.readNumber();
					System.out.println("本次收入说明：");
					String inMoneyDescription = Utility.readString();
					//账户余额处理
					balance += inMoney;
					
					inOutRecord += "收入\t\t" + balance + "\t\t\t" + inMoney + "\t\t\t" + inMoneyDescription + "\n";
					System.out.println("---------------------登记完成---------------------");
					
					break;
				case '3':
					System.out.println("本次支出金额：");
					int outMoney = Utility.readNumber();
					System.out.println("本次支出说明：");
					String outMoneyDescription = Utility.readString();
					//账户余额处理
					balance -= outMoney;
					
					inOutRecord += "支出\t\t" + balance + "\t\t\t" + outMoney + "\t\t\t" + outMoneyDescription;
					System.out.println("---------------------登记完成---------------------");
					break;
				case '4':
					System.out.println("确认是否退出(Y/N)：");
					char isExit = Utility.readConfirmSelection();
					if(isExit == 'Y'){
						flag = false;
					}
					//break;
			}
		}
	}
}
```

