# 1.异常的定义

在Java语言中，将程序执行中发生的不正常情况称为“异常”。（开发过程中的语法错误和逻辑错误不是异常）

# 2.异常的体系结构

Java程序在执行过程中所发生的**异常可以分为两类**：

1. error：Java虚拟机无法解决的严重问题。如：JVM系统内部错误，资源耗尽等严重情况。比如：**StatckOverflowError**和**OutOfMemory。**一般不编写针对性的代码进行处理
2. Exception：其他因为编程错误或偶然的外在因素导致的一致性问题，可以使用针对性的代码进行处理。例如：
   + 空指针异常
   + 角标越界异常
   + 试图读取不存在的文件
   + 网络连接中断

```
异常的体系结构
	java.lang.Throwable
			|----- java.lang.Error:一般不编写针对性的代码进行处理
			|----- java.lang.Exception:可以使用针对性的代码进行处理
				|----- 编译时异常（checked）不会生成字节码文件
					|-----IOException
						|-----FileNotFoundException
					|-----ClassNotFoundException
				|----- 运行时异常（unchecked，RuntimeException）
					|-----NullPointerException 空指针异常
					|-----ArrayIndexOutOfBoundException 数组角标越界
					|-----ClassCastException 类型转换异常
```

**Java中异常类的继承关系**

![image-20200403161431591](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200403161437.png)

# 3.**按照异常发生的时间**可以分为**两类**

1. 编译时异常：执行javac.exe命名时，可能出现的异常 是指编译器不要求强制处置的异常。一般是指编程时的逻辑错误，是程序员应该积极避免其出现的异常。 java. lang. Runtime Exception类及它的子类都是运行时异常。 对于这类异常，可以不作处理，因为这类异常很普遍，若全处理可能会对程序的可读性和运行效率产生影响

2. 运行时异常(RuntimeException)：执行java.exe命名时，出现的异常 是指编译器要求必须处置的异常。即程序在运行时由于外界因素造成的一般性异常。编译器要求Java程序必须捕获或声明所有编译时异常对于这类异常，如果程序不处理，可能会带来意想不到的结果。

   ![image-20200403162029470](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200403162030.png)

# 4.常见的异常类

```java
//******************以下是运行时异常***************************
	//ArithmeticException
	@Test
	public void test6(){
		int a = 10;
		int b = 0;
		System.out.println(a / b);
	}
	
	//InputMismatchException
	@Test
	public void test5(){
		Scanner scanner = new Scanner(System.in);
		int score = scanner.nextInt();
		System.out.println(score);
		
		scanner.close();
	}
	
	//NumberFormatException
	@Test
	public void test4(){
		
		String str = "123";
		str = "abc";
		int num = Integer.parseInt(str);	
	}
	
	//ClassCastException
	@Test
	public void test3(){
		Object obj = new Date();
		String str = (String)obj;
	}
	
	//IndexOutOfBoundsException
	@Test
	public void test2(){
		//ArrayIndexOutOfBoundsException
//		int[] arr = new int[10];
//		System.out.println(arr[10]);
		//StringIndexOutOfBoundsException
		String str = "abc";
		System.out.println(str.charAt(3));
	}
	
	//NullPointerException
	@Test
	public void test1(){		
//		int[] arr = null;
//		System.out.println(arr[3]);
		
		String str = "abc";
		str = null;
		System.out.println(str.charAt(0));
		
	}

	//******************以下是编译时异常***************************
	@Test
	public void test7(){
//		File file = new File("hello.txt");
//		FileInputStream fis = new FileInputStream(file);
//		
//		int data = fis.read();
//		while(data != -1){
//			System.out.print((char)data);
//			data = fis.read();
//		}
//		
//		fis.close();
		
	}
```



# 5.异常的处理

## 1.Java异常处理的抓抛模型

**过程一：“抛”，**程序在正常的执行过程中，一旦出现异常，就会在异常代码处生成一个对应异常的对象。并将此对象抛出。一旦抛出对象以后，其后的代码就不再执行

​	①：系统自动生成的异常对象

​	②：手动的生成一个异常对象，并抛出（throw）

**过程二：“抓”，**可以理解为异常处理方式：① try-catch-finally ② throws

## 2.异常的处理方式一：try-catch-finally

**使用说明**

```java
try{
    //可能出现异常的代码
}catch(异常类型1 变量名1){
    // 处理异常
}catch(异常类型2 变量名2){
    // 处理异常
}catch(异常类型3 变量名3){
    // 处理异常
}
...
    finally{
        //一定会执行的代码
    }
```

**说明**

	1. finally是可选的
 	2. 使用try将可能出现异常代码包装起来，在执行过程中，一旦出现异常，就会生成一个对应异常类的对象，根据此对象的类型，去catch中进行匹配
 	3. 一旦try中的异常对象匹配到某一个catch时，就进入catch中进行异常处理。一旦处理完成，就跳出try-catch结构（没写finally的情况。）
 	4. catch中的异常类型**如果没子父类关系**，则**谁声明在上，谁声明在下无所谓。**
 	5. catch中的异常类型如果满足子父类关系，则要求子类一定声明在父类的上面。否则，报错
 	6. 常用的异常处理的方式：① String getMessage()    ② e.printStackTrace();
 	7. 在try结构中声明的变量，再出了try结构以后，就不能再被调用
	8. try-catch-finally结构可以嵌套

## 3.finally的在说明

1. finally是可选的
2. finally中声明的是一定会被执行的代码。即使catch中又出现异常了，try中return语句，catch中return语句等情况。
3. 像数据库连接、输入输出流、网络编程Socket等资源，JVM是不能自动的回收的，我们需要自己手动的进行资源的释放。此时的资源释放，就需要声明在finally中。

## 4.异常的处理方式二：throws + 异常类型

"throws + 异常类型"==写在方法的声明处==。指明此方法执行时，可能会抛出的异常类型。 一旦当方法体执行时，出现异常，仍会在异常代码处生成一个异常类的对象，此对象满足throws后异常类型时，就会被抛出。异常代码后续的代码，就不再执行！

## 5.两种方式对比

try-catch-finally是真正在处理异常。throws的方法只是将异常抛给了方法的调用者。==并没有真正的将异常处理掉==

## 6.开发中应该如何选择两种处理方式？

- **如果父类中被重写的方法没throws方式处理异常**，则子类重写的方法也不能使用throws，意味着如果子类重写的方法中异常，必须使用try-catch-finally方式处理。
- 执行的方法a中，先后又调用了另外的几个方法，这几个方法是递进关系执行的。我们建议这几个方法使用throws的方式进行处理。而执行的方法a可以考虑使用try-catch-finally方式进行处理。



# 6.手动抛出异常

## 1.使用说明

在程序执行中，除了自动抛出异常对象的情况之外，我们还可以手动的throw一个异常类的对象。

## 2.经典面试题

**throw 和 throws区别**： throw 表示抛出一个异常类的对象，生成异常对象的过程。声明在方法体内。 throws 属于异常处理的一种方式，声明在方法的声明处。



## 3.代码示例

```java
class Student{
	
	private int id;
	
	public void regist(int id) throws Exception {
		if(id > 0){
			this.id = id;
		}else{
			//手动抛出异常对象
//			throw new RuntimeException("您输入的数据非法！");
//			throw new Exception("您输入的数据非法！");
			throw new MyException("不能输入负数");

		}		
	}

	@Override
	public String toString() {
		return "Student [id=" + id + "]";
	}
		
}
```

# 7.自定义异常类

## 1.如何自定义异常类

1. 继承于现的异常结构：RuntimeException 、Exception
2. 提供全局常量：serialVersionUID（对类的唯一标识）
3. 提供重载的构造器

## 2.代码示例

```java
public class MyException extends Exception{
	
	static final long serialVersionUID = -7034897193246939L;
	
	public MyException(){
		
	}
	
	public MyException(String msg){
		super(msg);
	}
}
```

