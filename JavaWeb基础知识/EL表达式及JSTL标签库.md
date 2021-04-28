

# 一、EL表达式简介

## 1.什么是EL表达式，EL表达式的作用

1. EL表达式全程：Expression Language 表达式语言
2. EL表达式作用：代替JSP页面中表达式脚本，进行数据的输出
3. EL表达式要比jsp的表达式脚本简洁很多
4. **<span style="color:red">EL表达式格式：${表达式}。</span>**
5. 注意：EL表达式写在jsp页面中,表达式一般是域对象的key。
6. **<span style="color:red">当EL表达式输出null值的时候,输出的是空串。jsp表达式脚本输出null值的时候,输出的是null字符串。</span>**

```html
<body>
<%
    request.setAttribute("key","request");
%>

表达式脚本输出key的值：<%=request.getAttribute("key")%><br/>
EL表达式输出key的值:${key};
</body>
```

## 2.EL表达式搜索域数据的顺序

EL表达式主要是在jsp页面中输出数据，主要的输出的是**域对象中的数据**

当四个域对象中都有相同key的时候，**EL表达式会按照四个域<span style="color:red">从小到大的顺序</span>去进行搜索,找到就输出。**

```jsp
<body>
    <%
        request.setAttribute("key","request");
        pageContext.setAttribute("key","pageContext");
        session.setAttribute("key","session");
        application.setAttribute("key","application");
    %>
</body>
```

## 3.EL表达式输出Bean的普通属性、数组属性、List集合等属性,map集合等属性

需求——输出 Person 类中普通属性，数组属性。list 集合属性和 map 集合属性。

**Person类**

```java
public class Person {
    private String name;
    private int age;

    private String[] phones ={"18812341234","19910001000"};

    private List<String> cities = Stream.of("北京","上海","深圳").collect(Collectors.toList());

     Map<String,String> map = new HashMap<>();
}
```

输出的代码：

```jsp
<%
    Person person  =new Person();
    person.setAge(12);
    Map<String,String> map = new HashMap<>();
    map.put("key1","value1");
    map.put("key2","value2");
    map.put("key3","value3");
    person.setMap(map);
    request.setAttribute("p",person);
%>
输出Person:${p}<br/>
输出Person的age属性:${p.age}<br/>
输出Person的phone数组属性：${p.phones[0]}<br/>
输出Person的cities集合中的元素值：${p.cities[0]}
输出Person的map:${p.map}<br/>
输出Person的map中某个key的值:${p.map.key3}
```

## 4.EL表达式——运算

**语法格式：`{运算表达式}`**  EL表达式支持如下运算符

### 4.1 关系运算

![image-20210425192824140](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210425192824140.png)

### 4.2 逻辑运算

![image-20210425193236759](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210425193236759.png)



### 4.3 算术运算

![img](https://img-blog.csdnimg.cn/20200821220413104.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80OTM0MzE5MA==,size_16,color_FFFFFF,t_70#pic_center)

### 4.4 empty运算

empty运算可以判断一个数据是否为空，如果为空,则输出true,否则输出false

有以下几种情况为空：

1. 值为null的时候,为空
2. 值为空串的时候,为空
3. 值是Object类型数组,长度为0的时候，为空
4. list集合，元素个数为0的时候,为空
5. map集合，元素个数为0的时候为空

```jsp
<body>
<%
// 1、值为null 值的时候，为空
request.setAttribute("emptyNull", null);
// 2、值为空串的时候，为空
request.setAttribute("emptyStr", "");
// 3、值是Object 类型数组，长度为零的时候
request.setAttribute("emptyArr", new Object[]{});
// 4、list 集合，元素个数为零
List<String> list = new ArrayList<>();
// list.add("abc");
request.setAttribute("emptyList", list);
// 5、map 集合，元素个数为零
Map<String,Object> map = new HashMap<String, Object>();
// map.put("key1", "value1");
request.setAttribute("emptyMap", map);
%>
${ empty emptyNull } <br/>
${ empty emptyStr } <br/>
${ empty emptyArr } <br/>
${ empty emptyList } <br/>
${ empty emptyMap } <br/>
</body>
```

### 4.5 三元运算符

表达式1？表达式2：表达式3

如果表达式1 的值为真，返回表达式2 的值，如果表达式1 的值为假，返回表达式3 的值。

### 4.6“.”运算和[]中括号运算

.运算可以输出Bean对象中某个属性的值

[]中括号运算,可以输出有序集合中某个元素的值。

并且[]中括号运算,还可以输出map集合中key里含有特殊字符的key的值。

```jsp
<body>
<%
    Map<String,Object> map = new HashMap<>();
    map.put("a.a.a","aaaValue");
    map.put("b+b+b","bbbValue");
    map.put("c-c-c","cccValue");
    request.setAttribute("map",map);

%>
${map['a.a.a']}<br/>
${map["b+b+b"]}<br/>
${map["c-c-c"]}
</body>
```

## 5.EL表达式的11个隐含对象

EL表达式中11个隐含对象,是EL表达式中自己定义的，可以直接使用。

|       变量       |         类型         |                        作用                        |
| :--------------: | :------------------: | :------------------------------------------------: |
|   pageContext    |   PageContextImpl    |           它可以获取jsp中的九大内置对象            |
|    pageScope     |  Map<String,Object>  |          它可以获取pageContext域中的数据           |
|   requestScope   |  Map<String,Object>  |            它可以获取request域中的数据             |
|   sessionScope   |  Map<String,Object>  |            它可以获取session域中的数据             |
| applicationScope |  Map<String,Object>  |         它可以获取ServletContext域中的数据         |
|      param       |  Map<String,String>  |               它可以获取请求参数的值               |
|   paramValues    | Map<String,String[]> |    它可以获取请求参数的值。获取多个值的时候使用    |
|      header      |  Map<String,String>  |               它可以获取请求头的信息               |
|   headerValues   | Map<String,String[]> |   它可以获取请求头的信息,它可以获取多个值的情况    |
|      cookie      |  Map<String,Cookie>  |           它可以获取当前请求的Cookie信息           |
|    initParam     |  Map<String,String>  | 它可以获取在web.xml中配置的`<context-param>`上下文 |

### 5.1 获取四个特定域中的属性

+ pageScope：对应pageContext域
+ requestScope：对应requestScope域
+ responseScope：对应responseScope域 
+ applicationScope：对应ServletContext域

```jsp
<%
    request.setAttribute("key1","request");
    session.setAttribute("key1","session");
    pageContext.setAttribute("key1","pageContext");
    application.setAttribute("key1","application");
%>

${sessionScope.key1}
```

### 5.2 pageContext对象的使用

1. 协议
2. 服务器ip
3. 服务器端口
4. **<span style="color:red">获取工程路径</span>**
5. 获取请求方式
6. 获取客户端ip地址
7. 获取会话的id编号：

```jsp
<body>
<%--
request.getScheme(): 获取请求的协议
request.getServerName():获取服务器端口
request.getServerPort():获取服务器的端口号
request.getServletContext():获取当前工程路径
request.getMethod():获取当前请求的方式
request.getRemoteHost():获取客户端的ip地址
session.getId():获取会话的id

--%>

<%=session.getId()%><br/>

1. 协议:${pageContext.request.scheme}<br/>
2. 服务器ip:${pageContext.request.serverName}<br/>
3. 服务器端口:${pageContext.request.serverPort}<br/>
4. 获取工程路径:${pageContext.request.contextPath}<br/>
5. 获取请求方式:${pageContext.request.method}<br/>
6. 获取客户端ip地址:${pageContext.request.remoteHost}<br/>
7. 获取会话的id编号：:${pageContext.session.id}<br/>
</body>
```

**运行结果**

![image-20210425211117663](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210425211117663.png)

### 5.3 EL表达式其他隐含对象的使用

|    变量     |         类型         |                      作用                      |
| :---------: | :------------------: | :--------------------------------------------: |
|    param    |  Map<String,String>  |             它可以获取请求参数的值             |
| paramValues | Map<String,String[]> | 它可以获取请求参数的值。获取多个值的时候使用。 |

**示例代码：**

```jsp


<body>
    请求地址:http://localhost:8080/09_el_jstl/EL%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%85%B6%E4%BB%96%E9%9A%90%E5%90%AB%E5%AF%B9%E8%B1%A1.jsp?username=jay&job=music&hobby=java&hobby=c
    
输出请求参数username的值：${param.username}<br/>
输出请求参数job的值：${param.job}<br/>
输出请求参数hobby的值【多个参数hobby】：${paramValues.hobby[0]}
输出请求参数hobby的值【多个参数hobby】：${paramValues.hobby[1]}
</body>
```



|     变量     |         类型         |                      作用                      |
| :----------: | :------------------: | :--------------------------------------------: |
|    header    |  Map<String,String>  |             它可以获取请求头的信息             |
| headerValues | Map<String,String[]> | 它可以获取请求头的信息。获取多个值的时候使用。 |

**代码示例**

```jsp
输出请求头【User-Agent】的值：${ header['User-Agent'] } <br>
输出请求头【Connection】的值：${ header.Connection } <br>
输出请求头【User-Agent】的值：${ headerValues['User-Agent'][0] } <br>
```



|  变量  |        类型        |              作用               |
| :----: | :----------------: | :-----------------------------: |
| cookie | Map<String,Cookie> | 它可以获取当前请求的Cookie 信息 |

**示例代码**

```jsp
输出cookie的name参数：${cookie.JSESSIONID.name}
输出cookie的value参数：${cookie.JSESSIONID.value}
```



|   变量    |        类型        |                         作用                          |
| :-------: | :----------------: | :---------------------------------------------------: |
| initParam | Map<String,String> | 它可以获取在web.xml 中配置的<context-param>上下文参数 |

**web.xml配置：**

```xml
<context-param>
    <param-name>username</param-name>
    <param-value>root</param-value>
</context-param>
<context-param>
    <param-name>url</param-name>
    <param-value>jdbc:mysql:///test</param-value>
</context-param>
```

**示例代码：**

```jsp
输出&lt;Context-param&gt;username 的值：${ initParam.username } <br>
输出&lt;Context-param&gt;url 的值：${ initParam.url } <br>
```



# 二、JSTL标签库

**定义：**JSTL，全称是jsp Standard tag library jsp标准标签库。

EL表达式主要是为了替换jsp中的表达式脚本，而标签库则是为了替换代码脚本。这样使得整个jsp页面变得更加简洁。

JSTL由五个不同功能的标签库组成。

|                      功能范围                      |                             URI                              | 前缀 |
| :------------------------------------------------: | :----------------------------------------------------------: | :--: |
| **<span style="color:red">核心标签库-重点</span>** | **<span style="color:red">http://java.sun.com/jsp/jstl/core</span>** |  c   |
|                       格式化                       |               http://java.sun.com/jsp/jstl/fmt               | fmt  |
|                        函数                        |            http://java.sun.com/jsp/jstl/functions            |  fn  |
|   数据库(<span style="color:red">不使用</span>)    |               http://java.sun.com/jsp/jstl/sql               | sql  |
|     XML(<span style="color:red">不使用</span>)     |               http://java.sun.com/jsp/jstl/xml               |  x   |

**<span style="color:red">在jsp页面中使用taglib指令引入标签库</span>**

+ CORE 标签库
  + <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
+ XML 标签库
  + <%@ taglib prefix="x" uri="http://java.sun.com/jsp/jstl/xml" %>
+ FMT 标签库
  + <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
+ SQL 标签库
  + <%@ taglib prefix="sql" uri="http://java.sun.com/jsp/jstl/sql" %>
+ FUNCTIONS 标签库
  + <%@ taglib prefix="fn" uri="http://java.sun.com/jsp/jstl/functions" %>

## 1.JSTL标签库的使用步骤

1. 先导入jstl标签库的jar包

   taglibs-standard-impl-1.2.1.jar
   taglibs-standard-spec-1.2.1.jar

2. 第二步：使用taglib指令引入标签库

   <%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>

## 2.core核心库使用

### 2.1`<c:set/>`

作用：set标签可以往域中存数据

```jsp
<body>
<%--
    <c:set/>
    scope属性：指定哪个域
    var属性:指定key
    value:指定value
--%>
保存之前：${requestScope.abc}<br/>
<c:set scope="request" var="abc" value="i am the best !"/>
保存之后：${requestScope.abc}
</body>
```

### 2.2 `<c:if/>`

if标签用来做if判断

```jsp
<body>
<%--
    ii. <c:if />
    if 标签用来做if 判断。
    test属性：用来表示判断的条件 （使用EL表达式输出）
--%>
<c:if test="${12 > 5}">
    <h1>12大于5</h1>
</c:if>
</body>
```

### 2.3choose、when、other标签

+ 作用：多路判断。跟switch...case...default非常接近

```jsp
<%--
    iii.<c:choose> <c:when> <c:otherwise>标签
    作用：多路判断。跟switch ... case .... default 非常接近
    choose 标签开始选择判断
    when 标签表示每一种判断情况
    test 属性表示当前这种判断情况的值
    otherwise 标签表示剩下的情况
    <c:choose> <c:when> <c:otherwise>标签使用时需要注意的点：
    1、标签里不能使用html 注释，要使用jsp 注释
    2、when 标签的父标签一定要是choose 标签
--%>
<%
	request.setAttribute("height", 180);
%>
<c:choose>
    <%-- 这是html 注释--%>
    <c:when test="${ requestScope.height > 190 }">
    <h2>小巨人</h2>
    </c:when>
    <c:when test="${ requestScope.height > 180 }">
    <h2>很高</h2>
    </c:when>
    <c:when test="${ requestScope.height > 170 }">
    <h2>还可以</h2>
    </c:when>
 	<c:otherwise>
        <c:choose>
            <c:when test="${requestScope.height > 160}">
            <h3>大于160</h3>
            </c:when>
            <c:when test="${requestScope.height > 150}">
            <h3>大于150</h3>
            </c:when>
            <c:when test="${requestScope.height > 140}">
            <h3>大于140</h3>
            </c:when>
            <c:otherwise>
            	其他小于140
            </c:otherwise>
        </c:choose>
        </c:otherwise>
</c:choose>
```

### 2.4 forEach标签

#### 2.4.1. 遍历1 到10，输出

+ var属性：表示循环的变量（也是当前正在遍历到的数据）
+ begin属性：表示开始的索引
+ end属性：表示结束的索引

**示例代码**

```jsp
<table>
    <c:forEach var="i" begin="1" end="10">
        <tr>
            <td>第${i}行</td>
        </tr>
    </c:forEach>
</table>
```

#### 2.4.2 遍历Object数组

+ items属性：表示要遍历的数据源（要遍历的数组）
+ var属性：表示当前遍历到的数据

**代码示例**

```jsp
<%request.setAttribute("arr",new java.lang.String[]{"aa","bb","cc"});%>

<%--
    items属性：表示要遍历的数组
    var属性：表示当前的变量
--%>
<c:forEach items="arr" var="item">
    ${item}
</c:forEach>
```

#### 2.4.3遍历Map集合

**示例代码**

```jsp
<%
    Map<String,Object> map = new HashMap<String, Object>();
    map.put("key1", "value1");
    map.put("key2", "value2");
    map.put("key3", "value3");
    // for ( Map.Entry<String,Object> entry : map.entrySet()) {
    // }
    request.setAttribute("map", map);
%>
<c:forEach items="${ requestScope.map }" var="entry">
	<h1>${entry.key} = ${entry.value}</h1>
</c:forEach>
```

#### 2.4.4 遍历List 集合---list 中存放Student 类，有属性：编号，用户名，密码，年龄，电话

**Student类**

```java
public class Student {

    //编号，用户名，密码，年龄，电话
    private Integer id;
    private String username;
    private String password;
    private Integer age;
    private String phone;
    	......
   // get,set,构造器,toString省略.. 
```

**示例代码：**

```jsp
遍历List集合
<%
    List<Student> studentList = new ArrayList<>();
    for (int i = 1; i <= 10; i++) {
        studentList.add(new Student(i,"name" + i,"pass" + i,18+i,"phone " + i));
    }
    request.setAttribute("stus",studentList);
%>

<table>
    <tr>
        <th>编号</th>
        <th>姓名</th>
        <th>密码</th>
        <th>年龄</th>
        <th>电话</th>
        <th>操作</th>
    </tr>

    <c:forEach items="${requestScope.stus}" var="stu">
        <tr>
            <td>${stu.id}</td>
            <td>${stu.username}</td>
            <td>${stu.password}</td>
            <td>${stu.age}</td>
            <td>${stu.phone}</td>
            <td>删除、修改、添加</td>
        </tr>
    </c:forEach>
</table>
```





**代码示例2**

```jsp
<body>
    <%
        List<Student> studentList = new ArrayList<>();
        for (int i = 1; i <= 10; i++) {
            studentList.add(new Student(i,"username"+i ,"pass"+i,18+i,"phone"+i));
        }
        request.setAttribute("stus", studentList);
    %>
    <%--
        items 表示遍历的数据源
        var 表示遍历到的数据
        begin表示遍历的开始索引值(起始为0)，不写begin代表从第一个开始
        end 表示结束的索引值，不写end代表遍历到最后一个
        step 属性表示遍历的步长值，默认是1
        varStatus 属性表示当前遍历到的数据的状态
    --%>
    <c:forEach items="${requestScope.stus}" var="stu" begin="2"
                        end="7" step="2" varStatus="status">
            ${stu.id} <br>
            ${stu.username} <br>
            ${stu.password} <br>
            ${stu.age} <br>
            ${stu.phone} <br>
            ${status.step} <br> <%--还可获取更多状态，见下图--%>
    </c:forEach>
    <%--运行结果：从3输出到8，每隔两个输出，即只有3、5、7--%>
</body>
```

**varStatus属性：还可以获取如下的值**

![img](https://img-blog.csdnimg.cn/20200821222124807.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80OTM0MzE5MA==,size_16,color_FFFFFF,t_70#pic_center)

