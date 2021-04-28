# 1.Cookie会话技术

## 1.1 什么是Cookie

+ Cookie翻译过来是`小饼干`的意思
+ 服务器创建了Cookie，并发送给浏览器，由浏览器保存。
+ 客户端有了Cookie之后，每次请求都会发送给服务器

## 1.2 创建Cookie

![image-20210427213623478](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427213623478.png)



**<span style="color:red">创建Cookie的步骤：</span>**

1. 服务器创建Cookie对象,参数中绑定键值对
2. 服务器通知客户端保存Cookie，服务器发送Cookie给客户端
3. 客户端收到Cookie后，保存，再次发送请求时，携带这个Cookie
4. 服务器收到Cookie后，通过getName和getValue得到Cookie的数据



**代码示例：演示Cookie的创建**

1.创建CookieServlet

```java
public class CookieServlet extends BaseServlet{

    protected void createCookie(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {


        //1.服务器创建Cookie对象
        Cookie cookie = new Cookie("key1","value1");

        //2.服务器通知客户端保存cookie值
        response.addCookie(cookie);

        response.getWriter().write("服务器通知客户端保存cookie");
    }
}
```

## 1.3服务器如何获取Cookie

服务器获取客户端的cookie只需要一行代码：Cookie[] cookies = request.getCookies();

![image-20210427221417384](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427221417384.png)

**Cookie工具类：查找指定Cookie**

```java
public class CookieUtils {
/**
* 查找指定名称的Cookie 对象
* @param name
* @param cookies
* @return
*/
public static Cookie findCookie(String name , Cookie[] cookies){
    if (name == null || cookies == null || cookies.length == 0) {
    	return null;
    }
    for (Cookie cookie : cookies) {
        if (name.equals(cookie.getName())) {
        	return cookie;
        }
    }
	return null;
	}
}
```

**服务器获取Cookie的Servlet程序**

```java
protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    Cookie[] cookies = req.getCookies();
        for (Cookie cookie : cookies) {
            // getName 方法返回Cookie 的key（名）
            // getValue 方法返回Cookie 的value 值
            resp.getWriter().write("Cookie[" + cookie.getName() + "=" + cookie.getValue() + "] <br/>");
        }
    Cookie iWantCookie = CookieUtils.findCookie("key1", cookies);
    // for (Cookie cookie : cookies) {
        // if ("key2".equals(cookie.getName())) {
            // iWantCookie = cookie;
            // break;
        // }
    // }	
    // 如果不等于null，说明赋过值，也就是找到了需要的Cookie
    if (iWantCookie != null) {
    	resp.getWriter().write("找到了需要的Cookie");
	}
}
```

## 1.4 Cookie值的修改

+ **方案一：**
  + 先创建一个同名的Cookie对象
  + 再调用response.addCookie(cookie)

```java
		//方案一：
        //1.创建一个同名的cookie
        Cookie cookie = new Cookie("key1", "newValue1");
        //2.调用response.addCookie( Cookie )通知客户端保存Cookie
        response.addCookie(cookie);
        response.getWriter().write("Cookie[" + cookie.getName() + "=" + cookie.getValue() + "]");
```

+ 方案二：
  + 先获取到指定的Cookie
  + 通过cookie.setValue(newValue);
  + 调用response.addCookie(cookie)

```java
  //方案二：
        //1.先获取Cookie
        Cookie[] cookies = request.getCookies();
        for (Cookie c : cookies) {
            if ("key1".equals(c.getName())){
                //2.设置value
                c.setValue("secondNewValue");
                //3.通过response.addCookie(c); 通知客户端保存cookie
                response.addCookie(c);
            }
        }
```

## 1.5 浏览器查看Cookie

+ **谷歌浏览器如何查看Cookie：**

![image-20210427224615851](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427224615851.png)

+ **火狐浏览器查看Cookie**

![image-20210427224623279](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427224623279.png)

## <span style="color:red">1.6 Cookie生命控制（重点）</span>

Cookie的生命控制指的是如何管理Cookie什么时候被销毁（删除）

通过Cookie的setMaxAge()方法**：`(默认值-1)`**

+ 正数：表示在指定的秒数后销毁
+ **<span style="color:red">负数：表示浏览器一关，就被销毁</span>**
+ **<span style="color:red">0：表示立马销毁</span>**

```java
protected void defaultLife(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        Cookie cookie = new Cookie("defaultLife", "defaultLife");

        cookie.setMaxAge(-1);

        response.addCookie(cookie);
    }
    protected void deleteNow(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        Cookie cookie = new Cookie("deleteNow", "deleteNow");

        cookie.setMaxAge(0);

        response.addCookie(cookie);

    }

    protected void life3600(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        Cookie cookie = new Cookie("life3600", "life3600");

        cookie.setMaxAge(60 * 60);

        response.addCookie(cookie);

    }
```

## <span style="color:red">1.7 Cookie有效路径 Path属性(重点)</span>

Cookie的path属性可以有效的过滤哪些Cookie可以发送给服务器，哪些不可以发送给服务器。

path属性是通过请求的地址来进行有效的过滤。

+ **CookieA：path=/工程路径**
+ **CookieB：path=/工程路径/abc**

**请求地址如下：http://ip:port/工程路径/a.html**

+ **CookieA：可以发送**
+ **CookieB：不可以发送**

```java
    protected void testPath(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        Cookie cookieA = new Cookie("path1","path1");

        cookieA.setPath(request.getContextPath() +"/abc");

        response.addCookie(cookieA);
        
    }
```

# 2,Session会话技术

## 2.1.Session的介绍

1. Session是一个接口(HttpSession)
2. Session是服务器的会技术。
3. **<span style="color:red">在一次会话(session)的多次请求间共享数据，将数据保存到服务器端，常用来保存保护登录之后的信息。</span>**

## 2.2Session的创建与获取

+ Session通过request.getSession()创建session对象
+ Session通过request.getSession()获取session对象
+ Session通过session.isNew()来判断当前的Session是创建 还是获取
+ **<span style="color:red">Session通过session.getId()获取唯一标识</span>**

```java
public class CreateGetSession extends HttpServlet {
    
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //获取Session对象,没有就创建,有就获取
        HttpSession session = req.getSession();

        //判断session是创建还是获取
        boolean isNew = session.isNew();

        //获取Session唯一标识 id
        String id = session.getId();

        System.out.println("当前session是否是创建的:" + isNew);
        System.out.println("当前Session的唯一标识:" + id);
    }
}
```

## 2.3Session域数据的存取

+ 通过session.setAttribute(String name, Object value)往session域中设置数据
+ 通过session.getAttribute(String name)获取session域中的数据

**代码示例**

```java
public class SetGetAttributeToSessionServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //获取Session对象,没有就创建,有就获取
        HttpSession session = req.getSession();

        //往session域中存取数据
        session.setAttribute("msg", "往session中存数据");
        //往session域中获取数据
        String msg = (String) session.getAttribute("msg");

        System.out.println(msg);

    }
}
```

## 2.4Session的生命周期控制

+ HttpSession.setMaxInactiveInterval(int interval) ：设置session过期时间，以秒为单位，超过指定时长，session就会被销毁
+ HttpSession.getMaxInactiveInterval()：获取当前Session的超时时长
+ **HttpSession的默认时长是<span style="color:red">30分钟</span>**

### 2.4.1修改当前服务器所有的Session的默认时长

因为在Tomcat服务器的配置文件web.xml中默认有以下的配置，它就表示配置了当前Tomca服务器下所有的Session超时时间，默认为30分钟

```xml
<session-config>
	<session-timeout>30</session-timeout>
</session-config>
```

### 2.4.2修改单个Session的销毁时长

如果想修改单个Session的销毁时长，可以通过**<span style="color:red">HttpSession.setMaxInactiveInterval(int interval)</span>** 设置

interval为正数时：表示到指定时间后销毁该Session

interval为负数时：表示永不销毁

通过**<span style="color:red">HttpSession.invalidate()</span>** 立即销毁Session

### 2.4.3Session会被销毁的几种方式

1. 服务器关闭
2. Session对象调用invalidate();
3. Session默认时长失效时。

## 2.5cookie和session的技术关联内幕

**Session底层是基于Cookie来实现的**

![image-20210428142159898](C:\Users\leowlwang\AppData\Roaming\Typora\typora-user-images\image-20210428142159898.png)



### 2.5.1**客户端关闭之后服务器不关闭，两次获取的Session是同一个吗？**

1. 默认情况下，不是，Cookie消失，其中的Session自然消失。
2. 如果需要相同，进行如下操作

```java
        Cookie cookie = new Cookie("JSESSIONID",session.getId());
        cookie.setMaxAge(60 * 60 );
        resp.addCookie(cookie);
```

### 2.5.2客户端呢不关闭，服务器关闭，两次获取的Session是否为同一个？

不是同一个Session，但是为了保证数据的不丢失，Tomcat服务器自动完成Session钝化和Session的活化。

1. **Session的钝化：**

在服务器正常关闭之前，将Session对象序列化到硬盘上。

2. **Session的活化：**

在服务器启动之后，将硬盘中的Session反序列化为内存中的Session对象

**注意：**也就是说即使获取的不是同一个Session，但是Session中的数据是相同的。

### 2.<span style="color:red">5.3 Session的特点</span>

1. Session用于存储一次会话的多次请求数据，存在服务器端。一次会话只有一个Session对象
2. Session可以存储任意类型，任意大小的数据。

## <span style="color:red">2.6Session和Cookie的区别</span>



1. Session是服务器端的会话技术。Cookie是浏览器端的会话技术。
2. Session存储数据在服务器端，Cookie是由客户端存储的(由服务器创建)。
3. Session数据安全，Cookie数据相对不安全。
4. **Session没有数据大小的限制，Cookie有大小限制(4KB)**
