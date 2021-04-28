# 1.什么是JSP，它有什么作用？

+ jsp的全程是java server pages。 java服务器页面
+ jsp的主要作用是代替Servlet程序回传html页面的数据，因为Servlet程序回传html页面数据是一件非常繁琐的事情。开发成本和维护成本都非常高。

**代码示例**：Servlet回传html页面数据

```java
public class PringHtml extends HttpServlet {
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    // 通过响应的回传流回传html 页面数据
    resp.setContentType("text/html; charset=UTF-8");
    PrintWriter writer = resp.getWriter();
    writer.write("<!DOCTYPE html>\r\n");
    writer.write(" <html lang=\"en\">\r\n");
    writer.write(" <head>\r\n");
    writer.write(" <meta charset=\"UTF-8\">\r\n");
    writer.write(" <title>Title</title>\r\n");
    writer.write(" </head>\r\n");
    writer.write(" <body>\r\n");
    writer.write(" 这是html 页面数据\r\n");
    writer.write(" </body>\r\n");
    writer.write("</html>\r\n");
    writer.write("\r\n");
	}
}
```

**代码示例**：jsp回传一个简单的html页面

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
    <head>
    <title>Title</title>
    </head>
<body>
	这是html 页面数据
</body>
</html>
```

## JSP页面如何访问

jsp页面和html页面一样，都是存放在web目录下。访问也和访问html页面一样。

比如：

web 目录
a.html 页面访问地址是----------------->>>>>> http://ip:port/工程路径/a.html
b.jsp 页面访问地址是------------------->>>>>> http://ip:port/工程路径/b.jsp



# 2.jsp的本质是什么

+ **<span style="color:red">jsp页面的本质就是一个Servlet</span>**
+ 当我们第一次访问jsp页面的时候，Tomcat服务器会帮我们把jsp页面翻译成一个java源代码。并且对它进行编译.class成字节码文件，我们打开java源代码不难发现其里面的内容为：

![image-20210424180606190](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424180606190.png)

我们跟踪源代码发现，`HttpJspBase类`继承了HttpServlet类。也就是说。jsp翻译出来的java类，它间接的继承了HttpServlet类。也就是说，翻译出来的是一个Servlet程序。

![image-20210424180741780](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424180741780.png)

总结：通过翻译的 java 源代码我们就可以得到结果：jsp 就是 Servlet 程序。



# 3.jsp的三种语法

## jsp头部的page指令

jsp的page指令可以修改jsp页面中一些重要的属性，或者行为。

`<%@ page contentType="text/html;charset=UTF-8" language="java" %>`

+ language属性： 表示jsp翻译后是什么语言的文件，暂时只支持java
+ **contentType属性：表示jsp返回的数据类型是什么。也是源码中response.setContentType()参数值**
+ pageEncoding属性：表示当前jsp页面文件本身的字符集
+ import属性：用于导包，导类
+ **<span style="color:red">autoFlush属性：设置当out输出流缓冲区满了之后，是否自动刷新缓冲区。默认值是true</span>**
+ **<span style="color:red">buffer属性：设置out缓冲区的大小。默认是8kb</span>**

**缓冲区满了之后出现的异常：**

![image-20210424184011327](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424184011327.png)

+ errorPage属性：当jsp页面出现错误之后，自动跳转去的错误页面路径。**<span style="color:red">这个路径一般都是以斜杠打头，它表示请求地址为http://ip:port/工程路径/</span>**
+ isErrorPage属性：设置当前jsp页面是否是错误信息页面。默认是false，**如果为true，可以获取异常信息对象。**
+ session属性：设置当前jsp页面，是否创建HttpSession对象，默认是true。
+ extends属性：设置jsp页面翻译出来的java类默认继承谁。

## jsp中常用的脚本

### 声明脚本（<span style="color:red">极少使用</span>）

**<span style="color:red">格式</span>：<%!  声明的java代码   %>**

**作用：**可以给jsp翻译出来的java类定义属性和方法，甚至是静态代码块。内部类等

练习：

1. 声明类属性
2. 声明 static 静态代码块
3. 声明类方法
4. 声明内部类

```jsp
<body>
<%!
    //声明类属性
    private Integer id;
    private String name;
    private static Map<String,String> map = null;


    //声明 static 静态代码块
    static {
        map = new HashMap<>();
        map.put("key1","value1");
        map.put("key2","value2");
        map.put("key3","value3");
    }

    //声明类方法
    public int sum(){
        return 12;
    }
%>
</body>
</html>
```

声明脚本代码翻译对照：

![image-20210424190823741](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424190823741.png)

### 表达式脚本（<span style="color:red">常用</span>）

+ **表达式脚本的格式**：`<%= 表达式 %>`
+ **表达式脚本的作用**：在jsp页面输出数据。
+ **表达式脚本的特点：**
  + 所有的表达式脚本都会被翻译到_jspService()方法中
  + 表达式脚本都会被翻译成out.print()输出到页面中
  + 由于表达式脚本都会被翻译到_jspService()方法中，所以_jspService()方法中的对象都可以使用。
  + **<span style="color:red">表达式脚本中的表达式不能以分号结束。</span>**
+ 练习：
  + 输出整型
  2.	输出浮点型
  3.	输出字符串
  4.	输出对象
+ **代码示例**

```jsp
<%=12%>
<%=11.11%>
<%="我是一个好人"%>
<%=map%>
<%=request.getParameter("username")%>
```

翻译对照：

![image-20210424192307886](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424192307886.png)

### 代码脚本

+ **代码脚本的格式**：`<%  java语句  %>`
+ **代码脚本的作用：可以在jsp页面中，编写我们需要的功能(java语句)**
+ **代码脚本的特点：**
  + 代码脚本翻译之后都在_jspService()方法中
  + 代码脚本由于翻译到_jspService()方法中，所以在_jspService()方法中的对象都可以直接使用。
  + 可以由多个代码脚本块组合完成一个完整的java语句
  + 代码脚本还可以和表达式脚本一起使用，在jsp页面中输出数据。
+ 练习：
  + 代码脚本----if 语句
  2.	代码脚本----for 循环语句
  3.	翻译后 java 文件中_jspService 方法内的代码都可以写

翻译之后的对比：

![image-20210424194340029](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424194340029.png)

## jsp中的三种注释

### html注释

` <!-- 这是html注释 -->`

html注释会被翻译到java源代码中。在_jspService()方法里，以out.writer输出到客户端

### java注释

`// 单行注释`

`/*多行注释 */`

java 注释会被翻译到 java 源代码中。

### jsp注释

`<%-- 这是jsp注释  --%>`

jsp 注释可以注掉，jsp 页面中所有代码。



# 4.jsp九大内置对象

![image-20210424195919229](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210424195919229.png)

+ request：请求对象
+ response：响应对象
+ pageContext：jsp的上下文对象
+ session：会话对象
+ application：ServletContext对象
+ config：ServletConfig对象
+ out：jsp输出流对象
+ page：指向当前jsp的对象
+ exception：异常对象

# 5.jsp四大域对象

四个域对象分别是：

+ pageContenxt：	(pageContextImpl类)    当前jsp页面范围内有效
+ request：(HttpServletReuqest)    一次请求内有效
+ session：(HttpSession)    一次会话内有效（打开浏览器访问服务器，直到关闭浏览器）
+ application：(ServletContext)  整个web工程范围内有效(只要web工程不停止，数据都在)

域对象是可以像Map一样存取数据对象。四个域对象功能一样。不同的是它们对数据的存取范围。

虽然四个域对象都可以存取数据。但在使用上它们是有先后顺序的。

**<span style="color:red">四个域在使用的时候，优先顺序是：</span>**

**<span style="color:red">pageContext       --------->      request    ---------->   session  ----------->      application</span>**

**scope.js页面**

```jsp
<body>
<h1>scope.jsp 页面</h1>
<% // 往四个域中都分别保存了数据
    pageContext.setAttribute("key", "pageContext");
    request.setAttribute("key", "request");
    session.setAttribute("key", "session");
    application.setAttribute("key", "application"); %>
pageContext 域是否有值：<%=pageContext.getAttribute("key")%> <br>
request 域是否有值：<%=request.getAttribute("key")%> <br>
session 域是否有值：<%=session.getAttribute("key")%> <br>
application 域是否有值：<%=application.getAttribute("key")%><br>
<% request.getRequestDispatcher("/scope2.jsp").forward(request, response); %>
</body>
```

**scope2.js页面**

```jsp
<body>
    <h1>scope2.jsp 页面</h1>
    pageContext 域是否有值：<%=pageContext.getAttribute("key")%> <br>
    request 域是否有值：<%=request.getAttribute("key")%> <br>
    session 域是否有值：<%=session.getAttribute("key")%> <br>
    application 域是否有值：<%=application.getAttribute("key")%> <br>
</body>
```

# 6.jsp的out输出和response.getWriter输出的异同

+ **相同点**
  + response表示响应，用于给客户端(浏览器)返回内容
  + out同样也是给客户端(浏览器)输出内容
+ 不同点：
  + **<span style="color:red">即使代码中out在前面，但最后输出。</span>**
  + **<span style="color:red">当jsp页面中所有代码执行完成后会有以下两个操作：</span>**
    + 执行out.flush()操作,会把out缓冲区中的数据**追加写入**到response缓冲区末尾
    + 会执行response的刷新操作。把全部数据写给客户端。

![img](https://img-blog.csdnimg.cn/20200811125645948.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80OTM0MzE5MA==,size_16,color_FFFFFF,t_70)

+ **注意**：由于官方将jsp翻译成java类的代码中都使用的是out进行输出，故一般使用out进行输出，out又分为write()和print()两个方法：
  + out.print()：会将任何内容转换成字符串并且调用out.write()进行输出
  + out.write()：输出字符串没有问题，但输出int型的数据时会将int转换成char输出，这就导致输出的并非是我们想要的数字，而是数字对应的ASCII码
  + **<span style="color:red">结论：jsp页面的代码脚本中任何要输出在浏览器的内容均使用out.print()</span>**

# 7.jsp常用标签

## 7.1静态包含

使用场景：

![img](https://img-blog.csdnimg.cn/20200811125657868.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80OTM0MzE5MA==,size_16,color_FFFFFF,t_70)

**使用方式：**

**`<% @include file="" %>`**

其中file属性设置要包含的jsp页面, 以斜杠`/`打头，斜杠`/`代表：http://ip:port/工程路径/，**对应web目录**

**项目结构**

![image-20210425093255708](C:\Users\leowlwang\AppData\Roaming\Typora\typora-user-images\image-20210425093255708.png)



**代码示例：**在web目录下创建foot.jsp

```jsp
<body>
    页脚信息 <br>
</body>
```

**代码示例：在main.jsp页面中包含foot.jsp**

```jsp
<body>
    头部信息<br/>
    主体信息<br>
    <%@include file="/include/foot.jsp"%>
</body>
```

+ **静态包含的特点：**
  + 静态包含不会被包含的jsp页面翻译成.java 和.class文件
  + 静态包含是把被包含的页面的代码拷贝到main.jsp翻译成的Java文件的对应位置执行输出。

## 7.2动态包含

**使用方式：**

`<jsp:include page=""></jsp:include>`

其中page属性设置要包含的jsp页面,与静态包含一致。

+ **动态包含的特点：**

  + 动态包含将被包含的jsp页面翻译成.java和.class文件
  + 动态包含还可以传递参数
  + 动态包含底层使用如下代码调用被包含的jsp页面执行输出：

  `org.apache.jasper.runtime.JspRuntimeLibrary.include(request, response, “/foot.jsp”, out, false);`

  代码示例1：在web目录下创建main.jsp

  ```jsp
  <body>
      头部信息 <br>
      主体信息 <br>
      <jsp:include page="/include/foot.jsp">
         <jsp:param name="username" value="jay"/>
         <jsp:param name="password" value="root"/>
      </jsp:include>
  </body>
  ```

  注意：

  ​	**<span style="color:red">1. 设置参数的标签要写在动态包含标签之中。</span>**

   	2. 出现`Expecting “jsp:param” standard action with “name” and “value” attributes`异常，有两个原因：
   	  	1. 动态包含中未设置参数时，没有把`<jsp:include page=""></jsp:include>`放在一行上
   	  	2. 动态包含中加了注释

  代码示例2：在web目录的main.jsp页面中调用参数

  ```jsp
  <body>
      <%=request.getParameter("username")%>
  </body>
  ```

  **<span style="color:red">动态包含的底层原理：</span>**

![img](https://img-blog.csdnimg.cn/2020081112590031.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80OTM0MzE5MA==,size_16,color_FFFFFF,t_70)

## 7.3 请求转发标签

**使用方式**：

`<jsp:forward page=""/>`

page属性就是请求转发的路径

# 8.ServletContextListener监听器

## 1.Listener监听器的介绍

(1)Listener监听器是JavaWeb的三大组件之一

(2)Listener监听器是JavaEE的规范(接口)

(3)Listener监听器的作用是监听某个事物的变化，然后通过回调函数反馈给程序做一些处理。



## 2.ServletContextListener监听器

ServletContextListener监听器可以监听ServletContext对象的创建和销毁（web工程启动时创建，停止时销毁），监听到创建和销毁之后都会调用ServletContextListener监听器的方法进行反馈。

```java
 //在ServletContext对象创建之后调用该方法
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("ServletContext对象被创建了..");
    }
    //在ServletContext对象销毁之后调用该方法
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("ServletContext对象被销毁了..");
    }
```

## 3.ServletContextListener监听器的使用步骤

1. 编写一个类实现ServletContextListener接口
2. 重写`contextInitialized(ServletContextEvent sce)和contextDestroyed(ServletContextEvent sce)`方法
3. 在web.xml文件中配置监听器

**代码示例1：创建一个类**

```java
public class MyServletContextListener implements ServletContextListener {

    //在ServletContext对象创建之后调用该方法
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("ServletContext对象被创建了..");
    }
    //在ServletContext对象销毁之后调用该方法
    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("ServletContext对象被销毁了..");
    }
}
```

**代码示例2：在web.xml配置监听器**

```xml
  <listener>
        <listener-class>com.auo.MyServletContextListener</listener-class>
    </listener>
```

**运行结果**

ServletContext对象被创建了..

ServletContext对象被销毁了..



# 9.查找jsp被翻译后的生成的.java和.class文件方法

1. 查看翻译后的Java源文件的方法：启动Tomcat服务器访问到JSP页面之后在控制台输出的信息的前端找到Using CATALINA_BASE中的路径，在硬盘中打开此目录，点击work --> Catalina --> localhost，找到对应的工程文件夹寻找即可
2. 访问JSP页面其实是在执行对应的翻译后的Java代码的_jspService方法：翻译后的Java类中没有service方法，而是重写了父类的_jspService方法，这个方法会被父类的service方法调用
