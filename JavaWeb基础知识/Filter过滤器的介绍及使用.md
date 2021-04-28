# 1.什么是过滤器Filter

1. Filter过滤器是JavaWeb的三大组件之一。
2. Filter过滤器它是JavaEE的规范。也就是接口
3. Filter过滤器它的作用是：**<span style="color:red">拦截请求</span>**，过滤响应

过滤器常见的应用场景：

	1.  权限检查
	2.  日记操作
	3.  事务管理等等



# 2.Filter的快速入门

需求：在你的web工程下，有一个admin目录。这个admin目录下的所有资源（html页面，jpg图片，jsp文件，等等）都必须是用户登录之后才允许访问。

思考：我们知道，在当用户登录之后都会把用户登录的信息保存到Session域中。所以要检查用户是否登录，可以判断Session中是否包含有用户登录的信息即可！

```java
  <%
    Object user = session.getAttribute("user");
    if (user == null){
        // ==null  用户未登录
        request.getRequestDispatcher("/login.jsp").forward(request,response);
        return;
    }
  %>
```

## 2.1Filter的工作流程

![image-20210428183927697](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210428183927697.png)



主要的步骤：

1. 创建一个类，实现Filter接口
2. 重写doFilter方法

**Filter代码：**

```java
public class AdminFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

    }

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest httpServletRequest = (HttpServletRequest) servletRequest;
        //1.获取Session对象
        HttpSession session = httpServletRequest.getSession();

        //2.通过session对象获取session域中登录用户的信息
        Object user = session.getAttribute("user");
        if (user == null) {
            //未登录,进行跳转
            servletRequest.getRequestDispatcher("/login.jsp").forward(httpServletRequest, servletResponse);
        } else {

            //通过filterChain.doFilter执行默认操作
            filterChain.doFilter(servletRequest, servletResponse);

        }
    }

    @Override
    public void destroy() {

    }
}
```

**web.xml中配置Filter代码**

```xml
    <filter>
        <filter-name>AdminFilter</filter-name>
        <filter-class>com.atguigu.filter.AdminFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>AdminFilter</filter-name>
        <!--url-parrtern：表示要过滤的路径-->
        <url-pattern>/admin/*</url-pattern>
    </filter-mapping>
</web-app>
```

注意：url-pattern过滤的路径要加* ，否则不起作用。

## 2.2 完整的用户登录

**登录页面login.jsp**

```html
这是登录页面。login.jsp 页面<br>
<form action="http://localhost:8080/15_filter/loginServlet" method="get">
用户名：<input type="text" name="username"/> <br>
密码：<input type="password" name="password"/> <br>
<input type="submit" />
</form>
```

**代码示例：LoginServlet**

```java
public class LoginServlet extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        resp.setContentType("text/html;charset=utf-8");
        Object username = req.getAttribute("username");
        Object password = req.getAttribute("password");

        if ("admin".equals(username) && "admin".equals(password)) {
            //将用户登录的信息存入到session中
            req.getSession().setAttribute("user", username);
            resp.getWriter().write("登录成功");
        } else {
            req.getRequestDispatcher("/login.jsp").forward(req, resp);
        }
    }
}
```



# 3.Filter的生命周期

Filter的生命周期包含几个方法

1. 构造器方法：在web工程启动的时候执行(Filter已经创建好)
2. init()方法：在web工程启动的时候执行
3. doFilter()过滤方法：每次拦截到请求，就会执行
4. destroy()销毁方法：在停止web工程的时候，就会执行（停止web工程，也会销毁Filter过滤器）

# 4.FilterConfig类

FilterConfig类见名知意，它是Filter过滤器的配置文件类

Tomcat服务器每次创建Filter的时候，也会同时创建一个FilterConfig类，这里包含了Filter在web.xml中的配置信息

**FilterConfig类的作用：获取filter过滤器的配置内容**

1. 获取Filter的名称filter-name的内容
2. 获取在Filter中配置的init-param初始化参数
3. 获取ServletContext对象

```java
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {

        System.out.println("2.Filter 的init(FilterConfig filterConfig)初始化");
        
        // 1、获取Filter 的名称filter-name 的内容
        System.out.println("filter-name 的值是：" + filterConfig.getFilterName());
        
        // 2、获取在web.xml 中配置的init-param 初始化参数
        System.out.println("初始化参数username 的值是：" + filterConfig.getInitParameter("username"));
        System.out.println("初始化参数url 的值是：" + filterConfig.getInitParameter("url"));
        
        // 3、获取ServletContext 对象
        System.out.println(filterConfig.getServletContext());
    }
```

**web.xml配置**

```xml
<!--filter 标签用于配置一个Filter 过滤器-->
<filter>
    <!--给filter 起一个别名-->
    <filter-name>AdminFilter</filter-name>
    <!--配置filter 的全类名-->
    <filter-class>com.atguigu.filter.AdminFilter</filter-class>
    <init-param>
        <param-name>username</param-name>
        <param-value>root</param-value>
    </init-param>
	<init-param>
        <param-name>url</param-name>
        <param-value>jdbc:mysql://localhost3306/test</param-value>
    </init-param>
</filter>
```

# 5.FilterChain 过滤器链

FilterChain过滤器链，就是多个过滤器一起工作

![image-20210428204900456](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210428204900456.png)

## 5.1FilterChain.doFilter()的作用

1. 执行下一个Filter过滤器(如果有Filter的话)
2. 执行目标资源（如果没有过滤器的话）

## 5.2多个Filter的执行顺序说明（重点）

**<span style="color:red">多个Filter过滤器的执行顺序是由他们在web.xml中从上往下配置的顺序有关的！！</span>**

## 5.3多个Filter过滤器执行的特点

1. 所有的filter和目标资源默认都执行在同一个线程中
2. 多个Filter共同执行的时候,它们都使用同一个request对象。

# 6.Filter的拦截路径

## 6.1 精确匹配

`<url-pattern>/taget.jsp</url-pattern>`

以上配置的路径，表示请求地址必须是：http://ip:port/工程路径/target.jsp，才会被拦截。

## 6.2 目录匹配

`<url-pattern>/admin/*</url-pattern>`

以上配置的路径，表示请求地址必须是：http://ip:port/工程路径/admin/*的时候才会被拦截。

## 6.3后缀名匹配

`<url-pattern>*.do</url-pattern>`

以上配置的路径，表示请求地址必须以.do结尾的时候会被拦截

`<url-pattern>*.html</url-pattern>`

以上配置的路径，表示请求地址必须以.html结尾的时候会被拦截

`<url-pattern>*.action</url-pattern>`

以上配置的路径，表示请求地址必须以.action结尾的时候会被拦截



**<span style="color:red">Filter 过滤器它只关心请求的地址是否匹配，不关心请求的资源是否存在！！！</span>**