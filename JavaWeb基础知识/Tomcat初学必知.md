# 1.JavaWeb概念

## 什么是JavaWeb

JavaWeb是指通过Java语言编写的程序 可以通过浏览器来访问的总称，叫JavaWeb。JavaWeb是基于请求和响应开发的。

## 什么是请求

请求是指客户端给服务器发送数据，叫请求Request

## 什么是响应

服务器端回传数据给客户端，叫响应Response

## 请求和响应的关系

请求和响应是**成对出现的**，有请求就有响应。

![image-20210420191215070](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420191215070.png)

# 2.Web资源的分类

web资源按照实现的技术与呈现的效果不同，又分为**静态资源和动态资源**

+ 静态资源：html、css、txt、js、mp4视频、jpg图片
+ 动态资源：jsp、servlet



# 3.常用的web服务器

+ Tomcat：由Apache组织提供的一种web服务器，提供对jsp和servlet的支持。它是一种轻量级的javaWeb容器（服务器），也是当前应用最广泛的javaWeb服务器（免费）
+ jBoss：是一个遵从JavaEE规范的、开放源代码的、纯Java的EJB服务器，它支持所有的JavaEE规范(免费)。
+ GlassFish：由Oracle公司开发的一款JavaWeb服务器，是一款强健的商业服务器，达到产品级质量（应用很少）。
+ Resin：是CAUCHO公司的产品，是一个非常流行的服务器，对servlet和jsp提供了良好的风格，性能也比较优良，resin自身采用Java语言开发(收费、应用比较多)
+ WebLogic：是Oracle公司的产品，是目前应用最广泛的Web服务器，支持JavaEE规范，而且不断的完善适应新的开发要求，适合大型项目（收费，用的不多，适合大公司）

# 4.Tomcat服务器与Servlet版本的对应关系

当前企业常用的版本 7.* / 8.*

![image-20210420192909106](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420192909106.png)

Servlet程序从2.5版本是现在世面上使用最多的版本(xml配置)

到了Servlet3.0之后，就是注解版本的Servlet使用。



# 5.Tomcat的使用

## 安装

找到你需要的Tomcat对应版本的zip压缩包，解压到需要安装的目录即可

## 目录介绍

+ bin：用来存放Tomcat服务器的可执行文件
+ conf：用来存放Tomcat服务器的配置文件
+ lib：用来存放Tomcat服务器的jar包
+ logs：用来存放Tomcat服务器的日志信息
+ temp：用来存放Tomcat服务器运行时产生的临时数据
+ **webapps：用来存放部署的web工程。**
+ **work：用来存放Tomcat服务器运行时jsp翻译成servlet的源码，和session钝化的目录。**

## 如何启动Tomcat服务器

找到Tomcat目录下的bin目录下的startup.bat文件，双击就可以启动Tomcat服务器

**如何测试Tomcat启动成功？**

打开浏览器，在浏览器地址栏中输入以下地址进行测试：

+ http://localhost:8080
+ http://127.0.0.1:8080
+ http://真实ip:8080

当浏览器出现如下界面,表示Tomcat启动成功。

![image-20210420194938400](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420194938400.png)

常见的启动失败的情况有：双击startup.bat 文件，就会出现一个小黑窗口一闪而过，这个时候，**失败的原因基本上都是因为没有配置好JAVA_HOME环境变量**

配置JAVA_HOME环境变量步骤：

![image-20210420195531149](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420195531149.png)



## 另一种启动Tomcat服务器的方式

1. 通过命令行的方式
2. cd到Tomcat的bin目录下
3. **敲入启动命令：`catalina run`**

![image-20210420200445177](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420200445177.png)



## Tomcat服务器的停止

1. 点击Tomcat服务器窗口的 x 关闭按钮
2. 在Tomcat服务器的窗口中按下快捷键 **`ctrl + c`**
3. **<span style='color:red;'>在Tomcat服务器的bin目录下找到shutdown.bat双击，即可停止Tomcat服务器</span>**

## 如何修改Tomcat的端口号

+ Mysql默认的端口号是：3306
+ Tomcat默认的端口号是：8080
+ **<span style='color:red;'>Http协议默认的端口号是：80</span>**

找到Tomcat目录下的conf目录，再找到server.xml配置文件

![image-20210420201947197](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420201947197.png)

平时上百度：http://www.baidu.com:80

HTTP协议默认的端口号是:80

## <span style='color:red;'>如何部署web工程到Tomcat中（重点）</span>

### 第一种部署方法

**只需要把web工程拷贝到Tomca服务器的webapps目录下即可**

### 如何访问Tomcat下的web工程

只需要在浏览器中输入如下格式的地址

http://ip地址:端口号/工程名/目录名/文件名



### 第二种部署方法

1. 找到Tomcat下的conf目录下的Catalina\localhost，创建如下的配置文件

   ![image-20210420204324677](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420204324677.png)

2. **abc.xml配置文件内容如下：**

```xml
<!-- 
	Context表示一个工程上下文
	path表示工程的访问路径:/abc
	docBase:表示工程所在的地址
-->
<Context path="/abc" docBase="E:\book"/>
```

访问这个工程的路径如下：http：//ip:端口号/abc/ 就表示访问E:\book目录



## 手托页面到浏览器和在浏览器输入http://ip:port/工程名/ 访问的区别

+ 手托html页面的原理：

![image-20210420205705769](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420205705769.png)

+ 输入访问地址访问的原理：

![image-20210420205749232](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420205749232.png)

## ROOT工程的访问，以及默认index.html页面的访问

+ 当我们在浏览器地址栏中输入的访问地址为：http://ip:port/,  ----------> 没有工程名,默认访问的是`ROOT工程`

+ 当我们在浏览器地址栏中输入的访问地址为:http://ip:port/工程名/  ----------->  没有资源名,默认访问`index.html`页面

  

# 6.IDEA整合Tomcat服务器

操作的菜单如下：File | Settings | Build, Execution, Deployment | Application Servers

![image-20210420211509141](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420211509141.png)

配置你的 Tomcat 安装目录：

![image-20210420211517314](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420211517314.png)

就可以通过创建一个 Model 查看是不是配置成功！！！

![image-20210420211525246](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420211525246.png)

# 7.IDEA中动态web工程的操作

## 如何创建动态web工程

1.创建一个新模块：

![image-20210420213107836](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420213107836.png)

2.选择你要创建什么类型的模块：

![image-20210420213117603](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420213117603.png)

3、输入你的模块名，点击【Finish】完成创建。

![image-20210420213127882](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420213127882.png)

![image-20210420213131651](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420213131651.png)



## web工程的目录介绍

![image-20210420213238814](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420213238814.png)

## 如何在IDEA中部署工程到Tomcat上运行

1、建议修改 web 工程对应的 Tomcat 运行实例名称：

![image-20210420215409327](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420215409327.png)

2、确认你的 Tomcat 实例中有你要部署运行的 web 工程模块：

![image-20210420215426248](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420215426248.png)

3、你还可以修改你的 Tomcat 实例启动后默认的访问地址：

![image-20210420215432554](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420215432554.png)

## 修改工程访问路径

![image-20210420215458656](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420215458656.png)

## 修改运行的端口号

![image-20210420215508341](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420215508341.png)

## 修改运行使用的浏览器

![image-20210420215521709](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420215521709.png)

## 配置资源热部署

![image-20210420215533933](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210420215533933.png)

