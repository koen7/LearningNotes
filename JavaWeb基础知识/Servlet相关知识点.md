# 1.Servlet技术

## 什么是Servlet

1. Servlet是JavaEE的规范之一，规范就是接口
2. Servlet是JavaWeb的三大组件之一。三大组件分别是Servlet、Filter过滤器、Listener监听器。
3. Servlet是运行在服务器上的一个Java小程序，**<span style='color:red;'>它可以接收客户端发送来的请求，并响应数据给客户端。</span>**

## <span style='color:red;'>手动实现Servlet程序</span>

1. 编写一个类实现`Servlet接口`
2. 实现service方法,处理请求，做出响应
3. 到web.xml中配置Servlet程序的访问地址

**Servlet程序的示例代码**

```java
public class HelloServlet implements Servlet {
    @Override
    public void init(ServletConfig servletConfig) throws ServletException {

    }

    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("服务器(servlet)做出响应。");
    }

    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

**web.xml中的配置**

```xml
<servlet>
        <!--servlet-name标签:给servlet起一个别名(一般都是类名)-->
        <servlet-name>HelloServlet</servlet-name>
        <!--servlet-class标签:指定servlet全类名-->
        <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <!--servlet-name标签的作用是告诉服务器，我当前配置的地址给哪个Servlet程序使用-->
        <servlet-name>HelloServlet</servlet-name>
        <!--url-pattern标签:配置访问地址-->
        <!--
               / 斜杠在服务器解析的时候,表示地址为:http://ip:port/工程名/
               /hello 表示地址为:http://ip:port/工程名/hello
        -->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
```



常见的错误1：url-pattern中配置的路径没有以斜杠打头

![image-20210420232321710](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420232321710.png)

常见的错误2：servlet-name 配置的值不存在

![image-20210420232344737](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420232344737.png)

常见错误3：servlet-class标签的全类名配置错误

![image-20210420232421702](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420232421702.png)

## url地址到Servlet程序的访问

![image-20210420235310659](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420235310659.png)



## Servlet的生命周期

1. **执行Servlet构造器方法：<span style='color:red;'>第一次访问会被调用,且只调用一次</span>**
2. **执行init()方法：<span style='color:red;'>第一次访问的时候,会被调用,且只调用一次。</span>**
3. **执行service()方法：每次访问都会被调用**
4. 执行destory()方法：在web工程停止的时候会被调用。



# 2.ServletConfig类

ServletConfig类从类名上来看,就知道是Servlet程序的配置信息类。

Servlet程序和ServletConfig都是由Tomcat负责创建，我们负责使用。

Servlet程序默认是第一次访问的时候创建，ServletConfig是每个Servlet程序创建时，就创建一个对应的ServletConfig对象。



## ServletConfig类的三大作用

1. 可以获取Servlet程序的别名servlet-name的值
2. 可以获取初始化参数init-param
3. 可以获取ServletContext对象

**代码演示：web.xml中的配置**

```xml
<servlet>
        <!--servlet-name标签:给servlet起一个别名(一般都是类名)-->
        <servlet-name>HelloServlet</servlet-name>
        <!--servlet-class标签:指定servlet全类名-->
        <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>

        <!--初始化参数-->
        <init-param>
            <!--参数名-->
            <param-name>username</param-name>
            <!--参数值-->
            <param-value>root</param-value>
        </init-param>

        <init-param>
            <param-name>url</param-name>
            <param-value>jdbc:mysql://localhost:3306</param-value>
        </init-param>
    </servlet>

    <servlet-mapping>
        <!--servlet-name标签的作用是告诉服务器，我当前配置的地址给哪个Servlet程序使用-->
        <servlet-name>HelloServlet</servlet-name>
        <!--url-pattern标签:配置访问地址-->
        <!--
               / 斜杠在服务器解析的时候,表示地址为:http://ip:port/工程名/
               /hello 表示地址为:http://ip:port/工程名/hello
        -->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
```

**代码演示：Servlet中的代码**

```java
@Override
    public void init(ServletConfig servletConfig) throws ServletException {
        System.out.println("2. 初始化方法");

        System.out.println("Servlet程序的别名是:" + servletConfig.getServletName());

        System.out.println("Servlet程序的初始化参数username的值为:" + servletConfig.getInitParameter("username"));

        System.out.println("Servlet程序的ServletContext对象:" + servletConfig.getServletContext());
    }
```

**<span style='color:red;'>注意点：</span>**

![image-20210421200551792](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210421200551792.png)

# 3.ServletContext类

## 什么是ServletContext

1. ServletContext是一个接口，它表示Servlet上下文对象。
2. 一个web工程，只有一个ServletContext对象
3. ServletContext对象是一个域对象
4. **<span style='color:red;'>ServletContext是在web工程部署启动的时候创建。在web工程停止的时候销毁。</span>**

**什么是域对象?**

域对象，是可以像Map一样存取数据的对象,叫做域对象。

这里的域指的是存取数据的操作范围，**<span style='color:red;'>ServletContext的操作范围是整个web工程</span>**

```
			存数据			取数据					删除数据
Map			put()		  get()					remove()
域对象		setAttribute()  getAttribute() 		removeAttribute()
```



## ServletContext类的四个作用

1. 获取web.xml中配置的上下文参数context-param
2. 获取当前的工程路径，格式为：/工程路径
3. 获取工程部署后在服务器硬盘上的绝对路径
4. 像Map一样存取数据



**代码示例：ServletContext像Map一样存取数据**

**ServletContext1代码：** 

```java
public class ContextServlet1 extends HttpServlet {
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws
ServletException, IOException {
// 获取ServletContext 对象
    ServletContext context = getServletContext();
    System.out.println(context);
    System.out.println("保存之前: Context1 获取key1 的值是:"+ context.getAttribute("key1"));
    context.setAttribute("key1", "value1");
    System.out.println("Context1 中获取域数据key1 的值是:"+ context.getAttribute("key1"));
    }
}
```

**ContextServlet2 代码：**

```JAVA
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException,
IOException {
	ServletContext context = getServletContext();
	System.out.println(context);
		System.out.println("Context2 中获取域数据key1 的值是:"+ context.getAttribute("key1"));
}
```



# 4.HTTP协议

## 什么是HTTP协议

+ 协议：协议就是指双方、或者多方、大家约定好都需要共同遵守的规则，就叫协议
+ HTTP协议：就是指 客户端和服务器之间进行通信时需要遵守的规则，就叫HTTP协议
+ 报文：HTTP协议中的数据。

## 请求的HTTP协议格式

客户端给服务器发送数据叫请求

服务器回传数据给客户端叫响应

请求又分为` GET请求 和POST请求`两种

### GET请求

GET请求有：

 1. 请求行

     	1. 请求的方式			GET
     	2. 请求的资源路径[ + ? + 请求参数]
     	3. 请求的协议版本号      HTTP/1.1

 2. 请求头

    key：value	组成		不同的键值对，表示不同的含义

![image-20210421230653035](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210421230653035.png)



### POST请求

1. 请求行
   1. 请求的方式
   2. 请求的资源路径
   3. 请求的协议版本号
2. 请求头
   1. key：value		不同的请求头，有不同的含义
3. **<span style='color:red;'>空行</span>**
4. 请求体  ---------------》  就是发送给服务器的数据

![image-20210421230858254](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210421230858254.png)



### 常用的请求头说明

+ Accept：告诉服务器，客户端可以接收的数据类型
+ Accept-Language：告诉服务器，客户端可以接收的语言类型
+ User-Agent：客户端（浏览器）的信息
+ **<span style='color:red;'>Host：表示请求的服务器ip和端口号port</span>**

### 哪些是GET请求，哪些是POST请求

**GET请求：**

​	form标签的method= get

​	a标签

​	link标签引入css

​	script标签引入js文件

​	img标签引入图片

​	iframe引入html页面

​	**在浏览器地址栏中输入地址后敲回车**

**POST请求：**

​	form标签 method= post



## 响应的HTTP协议格式

1. 响应行
   1. 响应的协议和版本号
   2. 响应状态码
   3. 响应状态描述符
2. 响应头
   1. key：value   不同的响应头，具有不同的含义
3. **<span style='color:red;'>空行</span>**
4. 响应体   ----------》   就是回传给客户端的数据

![image-20210421234247759](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210421234247759.png)

## 常用的响应状态码说明

+ 200：表示请求成功
+ 302：表示请求重定向
+ **404**：表示请求服务器已经收到了，但是你要的数据不存在（**请求地址错误**）
+ 500：服务器内部错误（代码错误）

## MIME类型说明

MIME是HTTP协议中数据类型。

MIME的英文全称是“Multipurpose Internet Mail Extension” 多功能Internet邮件扩充服务。MIME类型的格式是“大类型/小类型”，并与某一种文件的扩展名相对应。

**常见的MIME数据类型：**

![image-20210421234616955](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210421234616955.png)

![image-20210421234625175](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210421234625175.png)



**谷歌浏览器如何查看HTTP协议：**

![image-20210421234651803](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210421234651803.png)

**火狐浏览器如何查看HTTP 协议：**

![image-20210421234706158](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210421234706158.png)



# 5.HttpServletRequest类

## base标签的作用

![image-20210423114612475](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210423114612475.png)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <!--base标签设置 页面相对路径工作时参照的地址-->
    <base href="http://localhost:8080/06_servlet/a/b/c.html"/>
</head>
<body>
这是a/b/c.html
<a href="../../index.html">回首页</a>
</body>
</html>
```

## Web中的相对路径和绝对路径

在JavaWeb中，路径分为相对路径和绝对路径两种：

+ 相对路径
  + `.	`		表示当前目录
  + `..`       表示上一级目录
  + 资源名 表示当前目录/资源名
+ 绝对路径
  + http://ip:port/工程名/
+ **在实际开发中，路径都是使用绝对路径，而不是简单的使用相对路径**
  + 绝对路径
  + base+相对路径

## web中/斜杠的不同意义

**在web中，/斜杠就是一种绝对路径**

+ /如果被浏览器解析，得到的地址是：http://ip:port/

  + `<a href="/">斜杠</a>`

+ /如果被服务器解析，得到的地址是：http://ip:port/工程路径

  1、`<url-pattern>/servlet1</url-pattern> `

  2、servletContext.getRealPath(“/”);
  3、request.getRequestDispatcher(“/”);

+ **<span style="color:red">特殊情况</span>**：**response.sendRedirect("/") 把斜杠发送给浏览器解析，得到http://ip:port/**



# 6.HttpServletResponse类

## HttpServletResponse类的作用

HttpServletResponse类和HttpServletRequest类一样。每次请求进来，Tomcat服务器都会创建一个Response对象传递给Servlet程序去使用。HttpServletRequest表示请求过来的信息，**HttpServletResponse表示所有的响应信息。**

如果我们需要设置返回给客户端的信息，都可以通过HttpServletResponse对象来进行设置。



## 两个输出流的说明

+ 字节流：getOutputStream()		常用于下载(传递二进制数据)
+ 字符流：getWriter()                       常用于回传字符串
+ **注意：**两个流只能使用一个。使用了字节流，就不能使用字符流，反之亦然。否则会报错。

![image-20210423122334067](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210423122334067.png)

## 如何往客户端回传数据

```java
public class Servlet1 extends HttpServlet{

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        PrintWriter writer = resp.getWriter();
        writer.write("response's content");
    }
}
```

## 响应的乱码问题解决

解决中文乱码方式一(**<span style="color:red">不推荐使用</span>**)

```java
//设置服务器字符集为UT-8
resp.setCharacterEncoding("UTF-8");
//通过响应头，设置浏览器也使用UTF-8字符集
resp.setHeader("Content-type","text/html;charset=utf-8")
```



解决中文乱码方式二:

```java
//它会同时设置服务器和浏览器都使用UTF-8字符集,还设置了响应头
//此方法一定要写在获取流对象之前 才能生效！！！
resp.setContentType("text/html;charset=utf-8");
```



## 请求的重定向

重定向：是指客户端给服务器发送请求，然后服务器告诉客户端，我给你一个新地址，你去新地址访问。叫重定向（因为之前的地址可能已经被废弃了）

![image-20210423131608294](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210423131608294.png)

请求重定向的方式一(不推荐使用)：

```JAVA
// 设置响应状态码 302 ，表示重定向，（已搬迁）
resp.setStatus(302);
// 设置响应头，说明 新的地址在哪里
resp.setHeader("Location", "http://localhost:8080");
```

请求重定向的第二种方案（推荐使用）：

```java
resp.sendRedirect("http://localhost:8080");
```



**<span style="color:red">重定向的特点：</span>**

1. **浏览器地址栏会发生改变**
2. **发送两次请求**
3. **不共享Request域中的数据**
4. **不可以访问WEB-INF下面的资源文件**
5. **可以访问工程外的资源**