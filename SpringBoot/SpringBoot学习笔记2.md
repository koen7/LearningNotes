# 37、web实验-后台管理系统基本功能

## 项目创建

使用IDEA的Spring Initializr进行项目创建，需要的依赖如下：

thymeleaf、web-starter、devtools、lombok

## 登录页面

+ `/static目录`放js，css等静态资源

+ `/templates/login.html`登录页

+ `/templates/main.html`登录之后的主页面

+ thymeleaf的内联写法(没有标签的时候使用)：

  ```html
  <p>Hello, [[${session.user.name}]]!</p>
  ```



```html
<!-- 要加thymeleaf名称空间，才可以使用thymeleaf -->
<html lang="en" xmlns:th="http://www.thymeleaf.org">

<form class="form-signin" action="index.html" method="post" th:action="@{/login}">

    ...
    
    <!-- 消息提醒 -->
    <label style="color: red" th:text="${msg}"></label>
    
    <input type="text" name="userName" class="form-control" placeholder="User ID" autofocus>
    <input type="password" name="password" class="form-control" placeholder="Password">
    
    <button class="btn btn-lg btn-login btn-block" type="submit">
        <i class="fa fa-check"></i>
    </button>
    
    ...
    
</form>
```

## 登录和首页操作

```java
@Controller
public class IndexController {

    @GetMapping("/")
    public String index(){

        return "login";
    }

    @PostMapping("/login")
    public String login(User user, Model model, HttpSession session){
        if (StringUtils.hasText(user.getUserName()) && "123".equals(user.getPasswod())){
            session.setAttribute("loginUser",user);
            return "redirect:/main.html";
        }else{
            model.addAttribute("msg","用户名密码不正确！");
            return "login";
        }
    }

    @GetMapping("/main.html")
    public String main(HttpSession session,Model model){
        Object user = session.getAttribute("loginUser");
        if (user != null){
            return "main";
        }else{
            model.addAttribute("msg","未登录,请先登录！");
            return "login";
        }
    }
}
```

## 实体类User

```java
@Data
public class User {

    private String userName;
    private String passwod;

}
```

# 38 、web实验-抽取公共页面

官方文档地址：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#template-layout

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org"><!--注意要添加xmlns:th才能添加thymeleaf的标签-->
<head th:fragment="commonheader">
    <!--common-->
    <link href="css/style.css" th:href="@{/css/style.css}" rel="stylesheet">
    <link href="css/style-responsive.css" th:href="@{/css/style-responsive.css}" rel="stylesheet">
    ...
</head>
<body>
<!-- left side start-->
<div id="leftmenu" class="left-side sticky-left-side">
	...

    <div class="left-side-inner">
		...

        <!--sidebar nav start-->
        <ul class="nav nav-pills nav-stacked custom-nav">
            <li><a th:href="@{/main.html}"><i class="fa fa-home"></i> <span>Dashboard</span></a></li>
            ...
            <li class="menu-list nav-active"><a href="#"><i class="fa fa-th-list"></i> <span>Data Tables</span></a>
                <ul class="sub-menu-list">
                    <li><a th:href="@{/basic_table}"> Basic Table</a></li>
                    <li><a th:href="@{/dynamic_table}"> Advanced Table</a></li>
                    <li><a th:href="@{/responsive_table}"> Responsive Table</a></li>
                    <li><a th:href="@{/editable_table}"> Edit Table</a></li>
                </ul>
            </li>
            ...
        </ul>
        <!--sidebar nav end-->
    </div>
</div>
<!-- left side end-->


<!-- header section start-->
<div th:fragment="headermenu" class="header-section">

    <!--toggle button start-->
    <a class="toggle-btn"><i class="fa fa-bars"></i></a>
    <!--toggle button end-->
	...

</div>
<!-- header section end-->

<div id="commonscript">
    <!-- Placed js at the end of the document so the pages load faster -->
    <script th:src="@{/js/jquery-1.10.2.min.js}"></script>
    <script th:src="@{/js/jquery-ui-1.9.2.custom.min.js}"></script>
    <script th:src="@{/js/jquery-migrate-1.2.1.min.js}"></script>
    <script th:src="@{/js/bootstrap.min.js}"></script>
    <script th:src="@{/js/modernizr.min.js}"></script>
    <script th:src="@{/js/jquery.nicescroll.js}"></script>
    <!--common scripts for all pages-->
    <script th:src="@{/js/scripts.js}"></script>
</div>
</body>
</html>

```

`/templates/table/basic_table.html`

```html
<!DOCTYPE html>
<html lang="en" xmlns:th="http://www.thymeleaf.org">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
  <meta name="description" content="">
  <meta name="author" content="ThemeBucket">
  <link rel="shortcut icon" href="#" type="image/png">

  <title>Basic Table</title>
    <div th:include="common :: commonheader"> </div><!--将common.html的代码段 插进来-->
</head>

<body class="sticky-header">

<section>
<div th:replace="common :: #leftmenu"></div>
    
    <!-- main content start-->
    <div class="main-content" >

        <div th:replace="common :: headermenu"></div>
        ...
    </div>
    <!-- main content end-->
</section>

<!-- Placed js at the end of the document so the pages load faster -->
<div th:replace="common :: #commonscript"></div>

</body>
</html>
```

**`th:replace 、th:include、th：insert之间的区别`**

地址：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#template-layout

# 39、web实验-遍历数据与页面修改bug

**遍历数据控制层代码：**

```java
@GetMapping("/dynamic_table")
public String dynamic_table(Model model){
    //表格内容的遍历
    List<User> users = Arrays.asList(new User("zhangsan", "123456"),
                                     new User("lisi", "123444"),
                                     new User("haha", "aaaaa"),
                                     new User("hehe ", "aaddd"));
    model.addAttribute("users",users);

    return "table/dynamic_table";
}

```

**页面代码：**

```html
<table class="display table table-bordered" id="hidden-table-info">
    <thead>
        <tr>
            <th>#</th>
            <th>用户名</th>
            <th>密码</th>
        </tr>
    </thead>
    <tbody>
        <tr class="gradeX" th:each="user,stats:${users}">
            <td th:text="${stats.count}">Trident</td>
            <td th:text="${user.userName}">Internet</td>
            <td >[[${user.password}]]</td>
        </tr>
    </tbody>
</table>
```

# 40、视图解析-【源码分析】-视图解析器与视图

视图解析原理流程：

1. 目标方法处理的过程中(阅读`DispatchServlet`源码)，所有数据都会放在`ModelAndViewContainer`里面，其中包括数据和视图地址。
2. 方法的参数是一个自定义类型对象(从请求参数中确定的)，把他重新放在`ModelAndViewContainer`。
3. 任何目标方法执行完成以后都会返回`ModelAndView`(数据和视图地址)
4. `processDispatcherResult`处理派发结果
   1. `render(mv,request,response)`：进行页面渲染
      1. 根据方法的`String`返回值得到`View`对象【定义了页面渲染的逻辑】
      2. 得到了`redirect:/main.html` -- > `Thymeleaf new RedirectView()`
      3. `ContentNegotiationViewResolover`里面包含了下面所有的视图解析器，内部还是利用下面所有视图解析器得到视图对象。
      4. `View.render(mv.getModelInternal(),request,response)`：视图对象调用自定义的render进行页面渲染工作。
   2. `RedirectView`如何渲染【重定向一个页面】
   3. 获取目标url地址
   4. `response.sendRedirect(encodeURL)`

视图解析：

+ 返回值以`forward:`开始：`new InternalResourceView(forwardURL);`   -->  转发`request.getRequestDispatcher(path).forward(request, response);`

+ 返回值以`redirect:`开始：`new RedirectView()` ---> render就是重定向
+ 返回值是普通字符串：`new ThymeleafView()`

# 41、拦截器-登录检查与静态资源放行

1. **<span style="color:red">编写一个类实现`HandlerInterceptor`接口</span>**
2. **<span style="color:red">编写一个配置类实现`WebMvcConfigure`接口</span>**
3. **<span style="color:red">指定拦截规则(注意，如果拦截所有，静态资源也会被拦截)</span>**

编写一个类实现`HandlerInterceptor`接口

```java
/**
 * 登录检查
 */
public class LoginInterceptor implements HandlerInterceptor {


    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //1.获取session对象
        HttpSession session = request.getSession();
        //2.从session中获取登录的用户
        Object user = session.getAttribute("loginUser");
        if (user != null){
            //放行
            return true;
        }
        request.setAttribute("msg","请先登录！");
        //转发
        request.getRequestDispatcher("/").forward(request,response);
        return false;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {

    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {

    }
}
```

编写一个配置类实现WebMvcConfigure接口，并且指定拦截规则

```java
@Configuration
public class AdminWebConfig implements WebMvcConfigurer {

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(new LoginInterceptor())
                //对所有请求都进行拦截，包括静态资源
                .addPathPatterns("/**")
                //排除静态资源
                .excludePathPatterns("/","/login","css/**","fonts/**","images/**","js/**");
    }
}
```

# 42、拦截器-【源码分析】-拦截器的执行时机与原理

1. 根据当前请求找到`HandlerExecutionChain`(可以处理请求的handler以及所有的拦截器)
2. 先来**<span style="color:red">顺序执行</span>**所有拦截器的`preHandler()`方法
   1. 如果当前拦截器的`preHandler()`返回true，则执行下一个拦截器
   2. 如果当前拦截器的`preHandler()`返回false。直接**<span style="color:red">倒序执行</span>**所有已经执行了的拦截器的`afterCompletion()`
3. 如果任何一个拦截器返回`false`，直接跳出不执行目标方法。
4. 所有拦截器都返回`true`，才执行目标方法
5. 倒序执行所有拦截器的`postHandler()`方法
6. 前面的步骤有任何异常都会直接触发`afterCompletion()`
7. 页面渲染成功完成以后，也会倒序触发`afterCompletion()`

![img](SpringBoot%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B02.assets/20210205011212637.png)

`DispatcherServlet`中涉及到的`HandlerInterceptor`的地方

```java
public class DispatcherServlet extends FrameworkServlet {
    
    ...
    
	protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
		HttpServletRequest processedRequest = request;
		HandlerExecutionChain mappedHandler = null;
		boolean multipartRequestParsed = false;

		WebAsyncManager asyncManager = WebAsyncUtils.getAsyncManager(request);

		try {
			ModelAndView mv = null;
			Exception dispatchException = null;

            	...
            
                //该方法内调用HandlerInterceptor的preHandle()
				if (!mappedHandler.applyPreHandle(processedRequest, response)) {
					return;
				}

				// Actually invoke the handler.
				mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

            	...
                //该方法内调用HandlerInterceptor的postHandle()
				mappedHandler.applyPostHandle(processedRequest, response, mv);
			}			
        	processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
		}
		catch (Exception ex) {
            //该方法内调用HandlerInterceptor接口的afterCompletion方法
			triggerAfterCompletion(processedRequest, response, mappedHandler, ex);
		}
		catch (Throwable err) {
            //该方法内调用HandlerInterceptor接口的afterCompletion方法
			triggerAfterCompletion(processedRequest, response, mappedHandler,
					new NestedServletException("Handler processing failed", err));
		}
		finally {
			...
		}
	}

	private void triggerAfterCompletion(HttpServletRequest request, HttpServletResponse response,
			@Nullable HandlerExecutionChain mappedHandler, Exception ex) throws Exception {

		if (mappedHandler != null) {
            //该方法内调用HandlerInterceptor接口的afterCompletion方法
			mappedHandler.triggerAfterCompletion(request, response, ex);
		}
		throw ex;
	}

	private void processDispatchResult(HttpServletRequest request, HttpServletResponse response,
			@Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv,
			@Nullable Exception exception) throws Exception {

        ...

		if (mappedHandler != null) {
            //该方法内调用HandlerInterceptor接口的afterCompletion方法
			// Exception (if any) is already handled..
			mappedHandler.triggerAfterCompletion(request, response, null);
		}
	}
}
```

```java
public class HandlerExecutionChain {
    
    ...
    
	boolean applyPreHandle(HttpServletRequest request, HttpServletResponse response) throws Exception {
		for (int i = 0; i < this.interceptorList.size(); i++) {
			HandlerInterceptor interceptor = this.interceptorList.get(i);
            //HandlerInterceptor的preHandle方法
			if (!interceptor.preHandle(request, response, this.handler)) {
                
				triggerAfterCompletion(request, response, null);
				return false;
			}
			this.interceptorIndex = i;
		}
		return true;
	}
    
   	void applyPostHandle(HttpServletRequest request, HttpServletResponse response, @Nullable ModelAndView mv)
			throws Exception {

		for (int i = this.interceptorList.size() - 1; i >= 0; i--) {
			HandlerInterceptor interceptor = this.interceptorList.get(i);
            
            //HandlerInterceptor接口的postHandle方法
			interceptor.postHandle(request, response, this.handler, mv);
		}
	}
    
    void triggerAfterCompletion(HttpServletRequest request, HttpServletResponse response, @Nullable Exception ex) {
		for (int i = this.interceptorIndex; i >= 0; i--) {
			HandlerInterceptor interceptor = this.interceptorList.get(i);
			try {
                //HandlerInterceptor接口的afterCompletion方法
				interceptor.afterCompletion(request, response, this.handler, ex);
			}
			catch (Throwable ex2) {
				logger.error("HandlerInterceptor.afterCompletion threw exception", ex2);
			}
		}
	}  
} 
```

# 43、文件上传-单文件上传与多文件上传

**页面代码`/static/form/form_layouts.html`：**

Form表单需要添加：`enctype="multipart/form-data"`，并且请求是POST

```html
<form role="form" th:action="@{/upload}" method="post" enctype="multipart/form-data">
    <div class="form-group">
        <label for="exampleInputEmail1">邮箱</label>
        <input type="email" name="email" class="form-control" id="exampleInputEmail1" placeholder="Enter email">
    </div>
    
    <div class="form-group">
        <label for="exampleInputPassword1">名字</label>
        <input type="text" name="username" class="form-control" id="exampleInputPassword1" placeholder="Password">
    </div>
    
    <div class="form-group">
        <label for="exampleInputFile">头像</label>
        <input type="file" name="headerImg" id="exampleInputFile">
    </div>
    
    <div class="form-group">
        <label for="exampleInputFile">生活照</label>
        <input type="file" name="photos" multiple>
    </div>
    
    <div class="checkbox">
        <label>
            <input type="checkbox"> Check me out
        </label>
    </div>
    <button type="submit" class="btn btn-primary">提交</button>
</form>
```

**控制层代码：**

```java
@PostMapping("/upload")
    public String upload(@RequestParam("email") String email,
                         @RequestParam("name") String name,
                         @RequestPart("headImg") MultipartFile headImg,
                         @RequestPart("photos") MultipartFile[] photos) throws IOException {


        String originalFilename = headImg.getOriginalFilename();//带后缀名
        String name1 = headImg.getName();//不带后缀名
        log.info("originalFilename={},name={}", originalFilename, name1);

        if (!headImg.isEmpty()) {
            headImg.transferTo(new File("E:\\" + originalFilename));
        }

        if (photos.length > 0) {
            for (MultipartFile photo : photos) {
                if (!photo.isEmpty()) {
                    String photoName = photo.getOriginalFilename();
                    photo.transferTo(new File("E:\\" + photoName));
                }
            }
        }
        log.info("邮箱={},姓名={},头像大小={},生活照={}", email, name, headImg, photos);

        return "main";

    }
```

设置文件上传相关参数

```properties
spring.servlet.multipart.max-file-size=10MB
spring.servlet.multipart.max-request-size=100MB
```

# 44、文件上传-【源码分析】-文件上传参数解析器

文件上传相关的自动配置类`MultipartAutoConfiguration`有创建文件上传解析器`StandardServletMultipartResoler`

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnClass({ Servlet.class, StandardServletMultipartResolver.class, MultipartConfigElement.class })
@ConditionalOnProperty(prefix = "spring.servlet.multipart", name = "enabled", matchIfMissing = true)
@ConditionalOnWebApplication(type = Type.SERVLET)
@EnableConfigurationProperties(MultipartProperties.class)
public class MultipartAutoConfiguration {

	private final MultipartProperties multipartProperties;

	public MultipartAutoConfiguration(MultipartProperties multipartProperties) {
		this.multipartProperties = multipartProperties;
	}

	@Bean
	@ConditionalOnMissingBean({ MultipartConfigElement.class, CommonsMultipartResolver.class })
	public MultipartConfigElement multipartConfigElement() {
		return this.multipartProperties.createMultipartConfig();
	}

	@Bean(name = DispatcherServlet.MULTIPART_RESOLVER_BEAN_NAME)
	@ConditionalOnMissingBean(MultipartResolver.class)
	public StandardServletMultipartResolver multipartResolver() {
        //配置好文件上传解析器
		StandardServletMultipartResolver multipartResolver = new StandardServletMultipartResolver();
		multipartResolver.setResolveLazily(this.multipartProperties.isResolveLazily());
		return multipartResolver;
	}
}
```

```java
//文件上传解析器
public class StandardServletMultipartResolver implements MultipartResolver {

	private boolean resolveLazily = false;

	public void setResolveLazily(boolean resolveLazily) {
		this.resolveLazily = resolveLazily;
	}


	@Override
	public boolean isMultipart(HttpServletRequest request) {
		return StringUtils.startsWithIgnoreCase(request.getContentType(), "multipart/");
	}

	@Override
	public MultipartHttpServletRequest resolveMultipart(HttpServletRequest request) throws MultipartException {
		return new StandardMultipartHttpServletRequest(request, this.resolveLazily);
	}

	@Override
	public void cleanupMultipart(MultipartHttpServletRequest request) {
		if (!(request instanceof AbstractMultipartHttpServletRequest) ||
				((AbstractMultipartHttpServletRequest) request).isResolved()) {
			// To be on the safe side: explicitly delete the parts,
			// but only actual file parts (for Resin compatibility)
			try {
				for (Part part : request.getParts()) {
					if (request.getFile(part.getName()) != null) {
						part.delete();
					}
				}
			}
			catch (Throwable ex) {
				LogFactory.getLog(getClass()).warn("Failed to perform cleanup of multipart items", ex);
			}
		}
	}
}
```

```java
public class DispatcherServlet extends FrameworkServlet {
    
    @Nullable
	private MultipartResolver multipartResolver;
    
	private void initMultipartResolver(ApplicationContext context) {
		...
        
        //这个就是配置类配置的StandardServletMultipartResolver文件上传解析器
		this.multipartResolver = context.getBean(MULTIPART_RESOLVER_BEAN_NAME, MultipartResolver.class);
		...
	}
    
	protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
		HttpServletRequest processedRequest = request;
		HandlerExecutionChain mappedHandler = null;
		boolean multipartRequestParsed = false;//最后finally的回收flag
		...
		try {
			ModelAndView mv = null;
			Exception dispatchException = null;

			try {
                //做预处理,如果有上传文件 就new StandardMultipartHttpServletRequest包装类
				processedRequest = checkMultipart(request);
				multipartRequestParsed = (processedRequest != request);
				// Determine handler for the current request.
				mappedHandler = getHandler(processedRequest);
				
                ...

				// Determine handler adapter for the current request.
				HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

				...

				// Actually invoke the handler.
				mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
                
            }
            ....
            
		finally {

            ...
            
            if (multipartRequestParsed) {
                cleanupMultipart(processedRequest);
            }
		}
	}

	protected HttpServletRequest checkMultipart(HttpServletRequest request) throws MultipartException {
		if (this.multipartResolver != null && this.multipartResolver.isMultipart(request)) {
            ...
			return this.multipartResolver.resolveMultipart(request);
            ...
		}
    }

	protected void cleanupMultipart(HttpServletRequest request) {
		if (this.multipartResolver != null) {
			MultipartHttpServletRequest multipartRequest =
					WebUtils.getNativeRequest(request, MultipartHttpServletRequest.class);
			if (multipartRequest != null) {
				this.multipartResolver.cleanupMultipart(multipartRequest);
			}
		}
	}
}
```

# 45、错误处理-SpringBoot默认错误处理机制

```
SpringBoot官方错误机制文档：https://docs.spring.io/spring-boot/docs/2.4.2/reference/htmlsingle/#boot-features-error-handling
```

**<span style="color:red">默认规则：</span>**

+ 默认情况下，SpringBoot提供`/error`处理所有错误的映射
+ 机器客户端，它将生成JSON响应，其中包含错误，HTTP状态和异常信息的详细信息。对于浏览器客户端，响应一个"Whitelabel Error Page"错误页面。
+ 机器客户端(Postman)响应的结果：

```
{
  "timestamp": "2020-11-22T05:53:28.416+00:00",
  "status": 404,
  "error": "Not Found",
  "message": "No message available",
  "path": "/asadada"
}
```

+ 要对其进行自定义，添加`View`解析为`error`
+ 以完全替换默认行为，可以实现`ErrorController`并且注册该类型的Bean定义，或添加`ErrorAttributes类型的组件`以使用现有机制替换其内容
+ **<span style="color:red">在templates的error目录中，放4xx，5xx页面会被自动解析</span>**

![image-20210521181941699](SpringBoot%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B02.assets/image-20210521181941699.png)

![image-20210521182114183](SpringBoot%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B02.assets/image-20210521182114183.png)

# 46、错误处理-【源码分析】-底层组件功能分析

+ `ErrorMvcAutoConfiguration`自动配置异常处理规则
+ **容器中的组件**：类型：`DefaultErrorAttributes` -> id：`errorAttributes`
+ `public class DefaultErrorAttributes implements ErrorAttributes,HandlerExceptionResolver`
  + `DefaultErrorAttributes`：定义错误页面中可以包含数据（异常明细，堆栈信息等）
+ **容器中的组件**：类型：`BasicErrorController` --> id：`basicErrorController`(json+ 白页适配响应)
+ **处理默认`/error`路径的请求**：页面响应`new ModelAndView("error",model)`；
  + 容器中有组件`View` --> id是error：(响应默认错误页)
  + 容器中放组件`BeanNameViewResolver(视图解析器)`：按照返回的视图名作为组件的id去找容器中`View`对象。
+ 容器中组件：`DefaultErrorViewResolver`--> id：`conventionErrorViewResolver`
+ **如果发生异常错误，会以HTTP的状态码 作为视图页地址（viewName），找到真正的页面**（主要作用）。
  - error/404、5xx.html
  - 如果想要返回页面，就会找error视图（`StaticView`默认是一个白页）。

# 47、错误处理-【源码分析】-异常处理流程

譬如写一个会抛出异常的控制层：

```java
@Slf4j
@RestController
public class HelloController {

    @RequestMapping("/hello")
    public String handle01(){

        int i = 1 / 0;//将会抛出ArithmeticException

        log.info("Hello, Spring Boot 2!");
        return "Hello, Spring Boot 2!";
    }
}
```

当浏览器发送`/hello`请求，`DispatchServlet`的`doDIspatch（）`的`mv  = ha.handle(processedRequest, response, mappedHandler.getHandler())`将会抛出**ArithmeticException**。

```java
public class DispatcherServlet extends FrameworkServlet {
    ...
	protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
		...
				// Actually invoke the handler.
            	//将会抛出ArithmeticException
				mv = ha.handle(processedRequest, response, mappedHandler.getHandler());

				applyDefaultViewName(processedRequest, mv);
				mappedHandler.applyPostHandle(processedRequest, response, mv);
			}
			catch (Exception ex) {
                //将会捕捉ArithmeticException
				dispatchException = ex;
			}
			catch (Throwable err) {
				...
			}
    		//捕捉后，继续运行
			processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
		}
		catch (Exception ex) {
			triggerAfterCompletion(processedRequest, response, mappedHandler, ex);
		}
		catch (Throwable err) {
			triggerAfterCompletion(processedRequest, response, mappedHandler,
					new NestedServletException("Handler processing failed", err));
		}
		finally {
			...
		}
	}

	private void processDispatchResult(HttpServletRequest request, HttpServletResponse response,
			@Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv,
			@Nullable Exception exception) throws Exception {

		boolean errorView = false;

		if (exception != null) {
			if (exception instanceof ModelAndViewDefiningException) {
				...
			}
			else {
				Object handler = (mappedHandler != null ? mappedHandler.getHandler() : null);
				//ArithmeticException将在这处理
                mv = processHandlerException(request, response, handler, exception);
				errorView = (mv != null);
			}
		}
		...
	}

	protected ModelAndView processHandlerException(HttpServletRequest request, HttpServletResponse response,
			@Nullable Object handler, Exception ex) throws Exception {

		// Success and error responses may use different content types
		request.removeAttribute(HandlerMapping.PRODUCIBLE_MEDIA_TYPES_ATTRIBUTE);

		// Check registered HandlerExceptionResolvers...
		ModelAndView exMv = null;
		if (this.handlerExceptionResolvers != null) {
            //遍历所有的 handlerExceptionResolvers，看谁能处理当前异常HandlerExceptionResolver处理器异常解析器
			for (HandlerExceptionResolver resolver : this.handlerExceptionResolvers) {
				exMv = resolver.resolveException(request, response, handler, ex);
				if (exMv != null) {
					break;
				}
			}
		}
		...
	
        //若只有系统的自带的异常解析器（没有自定义的），异常还是会抛出
		throw ex;
	}
}
```

系统自带的**异常解析器：**

![img](SpringBoot%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B02.assets/20210205011338251.png)

`DefaultErrorAttributes`：会先来处理异常，他主要功能把异常信息保存到request域中，并且返回null

```java
public class DefaultErrorAttributes implements ErrorAttributes, HandlerExceptionResolver, Ordered {
    ...
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        this.storeErrorAttributes(request, ex);
        return null;
    }

    private void storeErrorAttributes(HttpServletRequest request, Exception ex) {
        request.setAttribute(ERROR_ATTRIBUTE, ex);//把异常信息保存到request域
    }
    ...
   
}    
```

默认没有任何异常解析器（上图的`HandlerExceptionResolverComposite`）能处理异常，所以异常会被抛出。

最终底层就会转发`/error`请求，会被底层的`BasicErrorController`处理

```java
@Controller
@RequestMapping("${server.error.path:${error.path:/error}}")
public class BasicErrorController extends AbstractErrorController {

    @RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
    public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
       HttpStatus status = getStatus(request);
       Map<String, Object> model = Collections
             .unmodifiableMap(getErrorAttributes(request, getErrorAttributeOptions(request, MediaType.TEXT_HTML)));
       response.setStatus(status.value());
       ModelAndView modelAndView = resolveErrorView(request, response, status, model);
       //如果/template/error内没有4**.html或5**.html，
       //modelAndView为空，最终还是返回viewName为error的modelAndView
       return (modelAndView != null) ? modelAndView : new ModelAndView("error", model);
    }
    
    ...
}
```

```java
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
    
    ...
    
	protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception {
        ...
     	// Actually invoke the handler.
		mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
		...
        //渲染页面
		processDispatchResult(processedRequest, response, mappedHandler, mv, dispatchException);
        ...
    }
    
    private void processDispatchResult(HttpServletRequest request, HttpServletResponse response,
			@Nullable HandlerExecutionChain mappedHandler, @Nullable ModelAndView mv,
			@Nullable Exception exception) throws Exception {

        boolean errorView = false;
        ...
		// Did the handler return a view to render?
		if (mv != null && !mv.wasCleared()) {
			render(mv, request, response);
			if (errorView) {
				WebUtils.clearErrorRequestAttributes(request);
			}
		}
		...
	}
    
    protected void render(ModelAndView mv, HttpServletRequest request, HttpServletResponse response) throws Exception {
		...

		View view;
		String viewName = mv.getViewName();
		if (viewName != null) {
			// We need to resolve the view name.
            //找出合适error的View，如果/template/error内没有4**.html或5**.html，
            //将会返回默认异常页面ErrorMvcAutoConfiguration.StaticView
            //这里按需深究代码吧！
			view = resolveViewName(viewName, mv.getModelInternal(), locale, request);
			...
		}
		...
		try {
			if (mv.getStatus() != null) {
				response.setStatus(mv.getStatus().value());
			}
            //看下面代码块的StaticView的render块
			view.render(mv.getModelInternal(), request, response);
		}
		catch (Exception ex) {
			...
		}
	}
    
}
```

```java
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass({ Servlet.class, DispatcherServlet.class })
// Load before the main WebMvcAutoConfiguration so that the error View is available
@AutoConfigureBefore(WebMvcAutoConfiguration.class)
@EnableConfigurationProperties({ ServerProperties.class, ResourceProperties.class, WebMvcProperties.class })
public class ErrorMvcAutoConfiguration {
    
    ...
        
   	@Configuration(proxyBeanMethods = false)
	@ConditionalOnProperty(prefix = "server.error.whitelabel", name = "enabled", matchIfMissing = true)
	@Conditional(ErrorTemplateMissingCondition.class)
	protected static class WhitelabelErrorViewConfiguration {

        //将创建一个名为error的系统默认异常页面View的Bean
		private final StaticView defaultErrorView = new StaticView();

		@Bean(name = "error")
		@ConditionalOnMissingBean(name = "error")
		public View defaultErrorView() {
			return this.defaultErrorView;
		}

		// If the user adds @EnableWebMvc then the bean name view resolver from
		// WebMvcAutoConfiguration disappears, so add it back in to avoid disappointment.
		@Bean
		@ConditionalOnMissingBean
		public BeanNameViewResolver beanNameViewResolver() {
			BeanNameViewResolver resolver = new BeanNameViewResolver();
			resolver.setOrder(Ordered.LOWEST_PRECEDENCE - 10);
			return resolver;
		}

	}     
   
    
	private static class StaticView implements View {

		private static final MediaType TEXT_HTML_UTF8 = new MediaType("text", "html", StandardCharsets.UTF_8);

		private static final Log logger = LogFactory.getLog(StaticView.class);

		@Override
		public void render(Map<String, ?> model, HttpServletRequest request, HttpServletResponse response)
				throws Exception {
			if (response.isCommitted()) {
				String message = getMessage(model);
				logger.error(message);
				return;
			}
			response.setContentType(TEXT_HTML_UTF8.toString());
			StringBuilder builder = new StringBuilder();
			Object timestamp = model.get("timestamp");
			Object message = model.get("message");
			Object trace = model.get("trace");
			if (response.getContentType() == null) {
				response.setContentType(getContentType());
			}
            //系统默认异常页面html代码
			builder.append("<html><body><h1>Whitelabel Error Page</h1>").append(
					"<p>This application has no explicit mapping for /error, so you are seeing this as a fallback.</p>")
					.append("<div id='created'>").append(timestamp).append("</div>")
					.append("<div>There was an unexpected error (type=").append(htmlEscape(model.get("error")))
					.append(", status=").append(htmlEscape(model.get("status"))).append(").</div>");
			if (message != null) {
				builder.append("<div>").append(htmlEscape(message)).append("</div>");
			}
			if (trace != null) {
				builder.append("<div style='white-space:pre-wrap;'>").append(htmlEscape(trace)).append("</div>");
			}
			builder.append("</body></html>");
			response.getWriter().append(builder.toString());
		}

		private String htmlEscape(Object input) {
			return (input != null) ? HtmlUtils.htmlEscape(input.toString()) : null;
		}

		private String getMessage(Map<String, ?> model) {
			Object path = model.get("path");
			String message = "Cannot render error page for request [" + path + "]";
			if (model.get("message") != null) {
				message += " and exception [" + model.get("message") + "]";
			}
			message += " as the response has already been committed.";
			message += " As a result, the response may have the wrong status code.";
			return message;
		}

		@Override
		public String getContentType() {
			return "text/html";
		}
	}
}
```

# 48、错误处理-【源码分析】-几种异常处理原理

+ 自定义错误页
  + `error/404.html error/5xx.html`；有精确的错误状态码页面就匹配精确，没有就找4xx.html，如果dou没有就触发白页
+ `@ControllerAdvice+@ExceptionHandler`**处理全局异常**；底层是`ExceptionHandlerExceptionResolver`支持的。

```java
@Slf4j
@ControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler({ArithmeticException.class,NullPointerException.class})  //处理异常
    public String handleArithException(Exception e){

        log.error("异常是：{}",e);
        return "login"; //视图地址
    }
}
```

+ `@ResponseStatus+自定义异常`；底层是`ResponseStatusExceptionHandler`，把@ResponseStatus注解的信息底层调用`response.sendError(statusCode, resolvedReason)`，tomcat发送的`/error`

```java
@ResponseStatus(value= HttpStatus.FORBIDDEN,reason = "用户数量太多")
public class UserTooManyException extends RuntimeException {

    public  UserTooManyException(){

    }
    public  UserTooManyException(String message){
        super(message);
    }
}
```

```java
@Controller
public class TableController {
    
	@GetMapping("/dynamic_table")
    public String dynamic_table(@RequestParam(value="pn",defaultValue = "1") Integer pn,Model model){
        //表格内容的遍历
	     List<User> users = Arrays.asList(new User("zhangsan", "123456"),
                new User("lisi", "123444"),
                new User("haha", "aaaaa"),
                new User("hehe ", "aaddd"));
        model.addAttribute("users",users);

        if(users.size()>3){
            throw new UserTooManyException();//抛出自定义异常
        }
        return "table/dynamic_table";
    }
    
}
```

+ Spring自家异常如` org.springframework.web.bind.MissingServletRequestParameterException，DefaultHandlerExceptionResolver` 处理Spring自家异常。
  + `response.sendError(HttpServletResponse.SC_BAD_REQUEST/*400*/, ex.getMessage());`
+ 自定义实现 `HandlerExceptionResolver `处理异常；可以作为默认的全局异常处理规则

```java
@Order(value= Ordered.HIGHEST_PRECEDENCE)  //优先级，数字越小优先级越高
@Component
public class CustomerHandlerExceptionResolver implements HandlerExceptionResolver {
    @Override
    public ModelAndView resolveException(HttpServletRequest request,
                                         HttpServletResponse response,
                                         Object handler, Exception ex) {

        try {
            response.sendError(511,"我喜欢的错误");
        } catch (IOException e) {
            e.printStackTrace();
        }
        return new ModelAndView();
    }
}
```

+ `ErrorViewResolver `实现自定义处理异常
  + `response.sendError()`，error请求就会转给controller。
  + 你的异常没有任何人能处理，tomcat底层调用response.sendError()，error请求就会转给controller。
  + basicErrorController 要去的页面地址是 ErrorViewResolver 。

```java
@Controller
@RequestMapping("${server.error.path:${error.path:/error}}")
public class BasicErrorController extends AbstractErrorController {

    ...
    
	@RequestMapping(produces = MediaType.TEXT_HTML_VALUE)
	public ModelAndView errorHtml(HttpServletRequest request, HttpServletResponse response) {
		HttpStatus status = getStatus(request);
		Map<String, Object> model = Collections
				.unmodifiableMap(getErrorAttributes(request, getErrorAttributeOptions(request, MediaType.TEXT_HTML)));
		response.setStatus(status.value());
		ModelAndView modelAndView = resolveErrorView(request, response, status, model);
		return (modelAndView != null) ? modelAndView : new ModelAndView("error", model);
	}
    
    protected ModelAndView resolveErrorView(HttpServletRequest request, HttpServletResponse response, HttpStatus status,
			Map<String, Object> model) {
        //这里用到ErrorViewResolver接口
		for (ErrorViewResolver resolver : this.errorViewResolvers) {
			ModelAndView modelAndView = resolver.resolveErrorView(request, status, model);
			if (modelAndView != null) {
				return modelAndView;
			}
		}
		return null;
	}
    
    ...
    
}
```

```java
@FunctionalInterface
public interface ErrorViewResolver {

	ModelAndView resolveErrorView(HttpServletRequest request, HttpStatus status, Map<String, Object> model);

}
```

# 49、原生组件注入-原生注解与Spring方式注入

```
官方文档：https://docs.spring.io/spring-boot/docs/2.4.2/reference/htmlsingle/#howto-add-a-servlet-filter-or-listener
```

## 使用原生注解方式

`@WebServlet、@WebFilter、@WebListener`

```java
@WebServlet(urlPatterns = "/my")
public class MyServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.getWriter().write("66666");
    }
}
```

```java
@Slf4j
@WebFilter(urlPatterns={"/css/*","/images/*"}) //my
public class MyFilter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        log.info("MyFilter初始化完成");
    }

    @Override
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        log.info("MyFilter工作");
        chain.doFilter(request,response);
    }

    @Override
    public void destroy() {
        log.info("MyFilter销毁");
    }
}
```

```java
@Slf4j
@WebListener
public class MyServletContextListener implements ServletContextListener {


    @Override
    public void contextInitialized(ServletContextEvent sce) {
        log.info("MySwervletContextListener监听到项目初始化完成");
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        log.info("MySwervletContextListener监听到项目销毁");
    }
}
```

**最后需要在主启动类加上注解`@ServletComponentScan`**

```java
@ServletComponentScan(basePackages = "com.lun")//
@SpringBootApplication(exclude = RedisAutoConfiguration.class)
public class Boot05WebAdminApplication {

    public static void main(String[] args) {
        SpringApplication.run(Boot05WebAdminApplication.class, args);
    }
}
```

## 使用Spring方式注入

使用`ServletRegistrationBean、FilterRegistrationBean、ServletListenerRegistrationBean`

```java
@Configuration(proxyBeanMethods = true)
public class MyRegistConfig {

    @Bean
    public ServletRegistrationBean myServlet(){
        MyServlet myServlet = new MyServlet();

        return new ServletRegistrationBean(myServlet,"/my","/my02");
    }


    @Bean
    public FilterRegistrationBean myFilter(){

        MyFilter myFilter = new MyFilter();
//        return new FilterRegistrationBean(myFilter,myServlet());
        FilterRegistrationBean filterRegistrationBean = new FilterRegistrationBean(myFilter);
        filterRegistrationBean.setUrlPatterns(Arrays.asList("/my","/css/*"));
        return filterRegistrationBean;
    }

    @Bean
    public ServletListenerRegistrationBean myListener(){
        MySwervletContextListener mySwervletContextListener = new MySwervletContextListener();
        return new ServletListenerRegistrationBean(mySwervletContextListener);
    }
}

```

# 50、原生组件注入-【源码分析】-DispatcherServlet原理

`org.springframework.boot.autoconfigure.web.servlet.DispatcherServletAutoConfiguration`配置类

```java
@AutoConfigureOrder(Ordered.HIGHEST_PRECEDENCE)
@Configuration(proxyBeanMethods = false)
@ConditionalOnWebApplication(type = Type.SERVLET)
@ConditionalOnClass(DispatcherServlet.class)
@AutoConfigureAfter(ServletWebServerFactoryAutoConfiguration.class)
public class DispatcherServletAutoConfiguration {

	/*
	 * The bean name for a DispatcherServlet that will be mapped to the root URL "/"
	 */
	public static final String DEFAULT_DISPATCHER_SERVLET_BEAN_NAME = "dispatcherServlet";

	/*
	 * The bean name for a ServletRegistrationBean for the DispatcherServlet "/"
	 */
	public static final String DEFAULT_DISPATCHER_SERVLET_REGISTRATION_BEAN_NAME = "dispatcherServletRegistration";

	@Configuration(proxyBeanMethods = false)
	@Conditional(DefaultDispatcherServletCondition.class)
	@ConditionalOnClass(ServletRegistration.class)
	@EnableConfigurationProperties(WebMvcProperties.class)
	protected static class DispatcherServletConfiguration {

        //创建DispatcherServlet类的Bean
		@Bean(name = DEFAULT_DISPATCHER_SERVLET_BEAN_NAME)
		public DispatcherServlet dispatcherServlet(WebMvcProperties webMvcProperties) {
			DispatcherServlet dispatcherServlet = new DispatcherServlet();
			dispatcherServlet.setDispatchOptionsRequest(webMvcProperties.isDispatchOptionsRequest());
			dispatcherServlet.setDispatchTraceRequest(webMvcProperties.isDispatchTraceRequest());
			dispatcherServlet.setThrowExceptionIfNoHandlerFound(webMvcProperties.isThrowExceptionIfNoHandlerFound());
			dispatcherServlet.setPublishEvents(webMvcProperties.isPublishRequestHandledEvents());
			dispatcherServlet.setEnableLoggingRequestDetails(webMvcProperties.isLogRequestDetails());
			return dispatcherServlet;
		}

		@Bean
		@ConditionalOnBean(MultipartResolver.class)
		@ConditionalOnMissingBean(name = DispatcherServlet.MULTIPART_RESOLVER_BEAN_NAME)
		public MultipartResolver multipartResolver(MultipartResolver resolver) {
			// Detect if the user has created a MultipartResolver but named it incorrectly
			return resolver;
		}

	}
    
    @Configuration(proxyBeanMethods = false)
	@Conditional(DispatcherServletRegistrationCondition.class)
	@ConditionalOnClass(ServletRegistration.class)
	@EnableConfigurationProperties(WebMvcProperties.class)
	@Import(DispatcherServletConfiguration.class)
	protected static class DispatcherServletRegistrationConfiguration {

        //注册DispatcherServlet类
		@Bean(name = DEFAULT_DISPATCHER_SERVLET_REGISTRATION_BEAN_NAME)
		@ConditionalOnBean(value = DispatcherServlet.class, name = DEFAULT_DISPATCHER_SERVLET_BEAN_NAME)
		public DispatcherServletRegistrationBean dispatcherServletRegistration(DispatcherServlet dispatcherServlet,
				WebMvcProperties webMvcProperties, ObjectProvider<MultipartConfigElement> multipartConfig) {
			DispatcherServletRegistrationBean registration = new DispatcherServletRegistrationBean(dispatcherServlet,
					webMvcProperties.getServlet().getPath());
			registration.setName(DEFAULT_DISPATCHER_SERVLET_BEAN_NAME);
			registration.setLoadOnStartup(webMvcProperties.getServlet().getLoadOnStartup());
			multipartConfig.ifAvailable(registration::setMultipartConfig);
			return registration;
		}

	}
    
    ...
    
}
```

`DispatherServlet`默认映射的是`/`路径，可以通过在配置文件中修改`spring.mvc.servlet.path=/mvc`

# 51、嵌入式Servlet容器-【源码分析】-切换web服务器与定制化

+ 默认支持的WebServer
  + `Tomcat、Jetty 和 Undertow`
  + `ServletWebServerApplicationContext`容器启动会寻找`ServletWebServerFactory`并引导创建服务器。
+ SpringBoot应用启动发现当前是Web应用，web场景-导入tomcat
+ web应用会创建一个web版的IOC容器`ServletWebServerApplicationContext`。
+ `ServletWebServerApplicationContext`容器启动会寻找`ServletWebServerFactory`并引导创建服务器。
+ SpringBoot底层默认有很多的WebServer工厂(ServletWebServerFactoryConfiguration内创建Bean)
  + `TomcatServletWebServerFactory`
  + `JettyServletWebServerFactory`
  + `UndertowServletWebServerFactory`
+ 底层直接会有一个自动配置类`ServletWebServerFactoryAutoConfiguration`
+ `ServletWebServerFactoryAutoConfiguration`导入了`ServletWebServerFactoryConfiguration`（配置类）
+ `ServletWebServerFactoryConfiguration`根据动态判断系统中导入的是哪个web服务器的包。（默认是web-starter导入tomcat包），容器中就有 `TomcatServletWebServerFactory`
+ ``TomcatServletWebServerFactory``创建出Tomcat服务器并启动；`TomcatWebServer`的构造器拥有初始化方法initialize-`this.tomcat,start()`
+ 内嵌服务器，与以前手动启动服务器相比，改成现在使用代码启动（tomcat核心jar包存在）。

**SpringBoot默认使用Tomcat服务器，若需要更改其他服务器，修改pom.xml**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>

<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
</dependency>
```

## 定制化Servlet容器

+ 实现`WebServerFactoryCustomizer<ConfigurableServletWebServerFactory>`
  + 把配置文件的值和`ServletWebServerFactory`进行绑定
+ 修改配置文件`Server.xxx`
+ 直接自定义`ConfigurableServletWebServerFactory`

`xxxxxCustomizer`：定制化器，可以改变xxx的默认规则

```java
import org.springframework.boot.web.server.WebServerFactoryCustomizer;
import org.springframework.boot.web.servlet.server.ConfigurableServletWebServerFactory;
import org.springframework.stereotype.Component;

@Component
public class CustomizationBean implements WebServerFactoryCustomizer<ConfigurableServletWebServerFactory> {

    @Override
    public void customize(ConfigurableServletWebServerFactory server) {
        server.setPort(9000);
    }

}
```

# 52、定制化原理-SpringBoot定制化组件的几种方式（小结）

## 定制化的常见方式

+ **<span style="color:red">修改配置文件</span>**
+ `xxxCustomizer`
+ 编写自定义的配置类`xxxConfiguration + @Bean` 替换、增加容器中默认组件，视图解析器
+ **<span style="color:red">Web应用 编写一个配置类实现`WebMvcConfigurer`即定制化web功能+`@Bean`给容器中再扩展一些组件</span>**

```java
@Configuration
public class AdminWebConfig implements WebMvcConfigurer{
}
```

+ `@EnableWebMvc + WebMvcConfigurer` — @Bean 可以**全面接管SpringMVC**，所有规则全部自己重新配置； 实现定制和扩展功能**（高级功能，初学者退避三舍）。**
  + 原理：
    1. `WebMvcAutoConfiguration`默认的SpringMVC的自动配置功能类，如静态资源、欢迎页等。
    2. 一旦使用 `@EnableWebMvc `，会@Import(DelegatingWebMvcConfiguration.class)。
    3. `DelegatingWebMvcConfiguration`的作用，只保证SpringMVC最基本的使用
       1. 把所有系统中的`WebMvcConfigurer`拿过来，所有功能的定制都是这些`WebMvcConfigurer`合起来一起生效。
       2. 自动配置了一些非常底层的组件，如`RequestMappingHandlerMapping`，这些组件依赖的组件都是从容器中获取如。
       3. `public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport。`
       4. WebMvcAutoConfiguration里面的配置要能生效必须 @ConditionalOnMissingBean(WebMvcConfigurationSupport.class)。
       5. @EnableWebMvc 导致了WebMvcAutoConfiguration 没有生效。

### 原理分析套路

场景starter - `xxxxAutoConfiguration` - 导入xxx组件 - 绑定`xxxProperties` - 绑定配置文件项。

# 53、数据访问-数据库场景的自动配置分析与整合测试

## 导入JDBC场景以及数据库驱动

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jdbc</artifactId>
</dependency>


<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <!--<version>5.1.49</version>-->
</dependency>

```

+ 如果想要修改版本

  + 直接导入有具体版本的依赖（*maven的就近依赖原则*）
  + 在properties中指定版本（*maven的就近依赖原则*）

  ```xml
  <properties>
      <java.version>1.8</java.version>
      <mysql.version>5.1.49</mysql.version>
  </properties>
  ```

## 相关数据源配置

+ `DataSourceAutoConfiguration`：数据源的自动配置
  + 修改数据源相关配置：`spring.datasource`
  + **数据库连接池的配置**：容器中没有指定连接池，那么会自动配置的
  + SpringBoot默认配置好的连接池：`HikariDataSource`
+ `DataSourceTransactionManagerAutoConfiguration`：事务管理器的自动配置
+ `JdbcTemplateAutoConfiguration`：`JdbcTemplate`的自动配置，可以对数据库CRUD
  + 可以通过在配置文件中修改`spring.jdbc.template`来修改`JdbcTemplate`
  + `@Bean @Primary JdbcTemplate`：Spring容器中有这个`JdbcTemplate`组件，使用`@Autowired`。
+ `JndiDataSourceAutoConfiguration`： JNDI的自动配置。
+ `XADataSourceAutoConfiguration`：分布事务相关的

## 修改配置源

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/travel
    username: root
    password: root
    driver-class-name: com.mysql.jdbc.Driver
```

## 单元测试数据源

```java
@SpringBootTest
class Boot05WebAdminApplicationTests {

	@Autowired
	private JdbcTemplate jdbcTemplate;

	@Test
	void contextLoads() {
		Long aLong = jdbcTemplate.queryForObject("select count(*) from tab_category", Long.class);
		System.out.println(aLong);
	}

}
```



# 54、数据访问-自定义方式整合Druid数据源

## Druid是什么

它是数据库连接池，它能够提供强大的监控和扩展功能

**官方文档**

```
https://github.com/alibaba/druid/wiki/Druid%E8%BF%9E%E6%8E%A5%E6%B1%A0%E4%BB%8B%E7%BB%8D
```

**SpringBoot整合第三方技术的两种方式：**

+ 自定义
+ 找starter场景器

## 自定义方式整合Druid

**1、添加依赖**

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.17</version>
</dependency>
```

**2、配置Druid数据源**

```java
@Configuration
public class MyConfig {

    @Bean
    @ConfigurationProperties("spring.datasource")//复用配置文件的数据源配置
    public DataSource dataSource() throws SQLException {
        DruidDataSource druidDataSource = new DruidDataSource();

//        druidDataSource.setUrl();
//        druidDataSource.setUsername();
//        druidDataSource.setPassword();

        return druidDataSource;
    }
}
```

更多的Druid配置教程地址：https://github.com/alibaba/druid/wiki/DruidDataSource%E9%85%8D%E7%BD%AE

**配置Druid监控页功能：**

+ Druid内置提供了一个`StatViewServlet`用于展示Druid的统计信息。这个`StatViewServlet`的用途包括：

  + 提供监控信息展示的html页面

  + 提供监控信息的JSON API 

    ```
    官方文档 - 配置_StatViewServlet配置
    https://github.com/alibaba/druid/wiki/%E9%85%8D%E7%BD%AE_StatViewServlet%E9%85%8D%E7%BD%AE
    ```

+ Druid内置提供了一个`StatFilter`，用于统计监控信息。

```
官方文档 - 配置_StatFilter：
https://github.com/alibaba/druid/wiki/%E9%85%8D%E7%BD%AE_StatFilter
```

+ `WebStatFilter`用于采集web-jdbc关联监控的数据，如SQL监控，URI监控。

```
官方文档 - 配置_配置WebStatFilter:
https://github.com/alibaba/druid/wiki/%E9%85%8D%E7%BD%AE_%E9%85%8D%E7%BD%AEWebStatFilter
```

+ Druid提供了`WallFilter`，它是基于SQL语义分析来实现防御SQL注入攻击的

```
官方文档 - 配置 wallfilter
https://github.com/alibaba/druid/wiki/%E9%85%8D%E7%BD%AE-wallfilter
```

```java
@Configuration
@ConfigurationProperties("spring.datasource")
public class DuridConfig {

    @Bean
    public DruidDataSource dataSource() throws SQLException {
        DruidDataSource druidDataSource = new DruidDataSource();
        // 加入监控和防火墙功能
        druidDataSource.setFilters("stat,wall");
        return druidDataSource;
    }

    /**
     * 配置druid的监控页功能
     * @return
     */
    @Bean
    public ServletRegistrationBean statViewServlet(){
        StatViewServlet statViewServlet = new StatViewServlet();
        ServletRegistrationBean<StatViewServlet> statViewServletServletRegistrationBean = new ServletRegistrationBean<>(statViewServlet);
        statViewServletServletRegistrationBean.addUrlMappings("/druid/*");
        //监控页帐号
        statViewServletServletRegistrationBean.addInitParameter("loginUsername","admin");
        statViewServletServletRegistrationBean.addInitParameter("loginPassword","admin");

        return statViewServletServletRegistrationBean;
    }

    /**
     * WebStatFilter 用于采集web-jdbc关联监控的数据。
     * @return
     */
    @Bean
    public FilterRegistrationBean webStatFilter(){
        WebStatFilter webStatFilter = new WebStatFilter();
        FilterRegistrationBean<Filter> registrationBean = new FilterRegistrationBean<>(webStatFilter);
        registrationBean.setUrlPatterns(Arrays.asList("/*"));
        registrationBean.addInitParameter("exclusions","*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*");
        return registrationBean;
    }
}

```

# 55、数据访问-Druid数据源starter整合方式

```
官方文档：https://github.com/alibaba/druid/tree/master/druid-spring-boot-starter
```

**<span style="color:red">1、引入依赖</span>**

```XML
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.1.17</version>
</dependency>
```

**<span style="color:red">2、分析自动配置</span>**

+ 扩展配置项：`spring.datasource.druid`
+ 自动配置类`DruidDataSourceAutoConfigure`
+ `DruidSpringAopConfiguration.class`，监控SpringBean的，配置项：`spring.datasource.druid.aop-patterns`
+ `DruidStatViewServletConfiguration.class`，监控页的配置。`spring.datasource.druid.stat-view-servlet`默认开启
+ `DruidWebStatFilterConfiguration.class`，web监控配置；`spring.datasource.druid.web-stat-filter`，默认开启
+ `DruidFilterConfiguration.class`所有Druid的filter的配置：

```java
private static final String FILTER_STAT_PREFIX = "spring.datasource.druid.filter.stat";
private static final String FILTER_CONFIG_PREFIX = "spring.datasource.druid.filter.config";
private static final String FILTER_ENCODING_PREFIX = "spring.datasource.druid.filter.encoding";
private static final String FILTER_SLF4J_PREFIX = "spring.datasource.druid.filter.slf4j";
private static final String FILTER_LOG4J_PREFIX = "spring.datasource.druid.filter.log4j";
private static final String FILTER_LOG4J2_PREFIX = "spring.datasource.druid.filter.log4j2";
private static final String FILTER_COMMONS_LOG_PREFIX = "spring.datasource.druid.filter.commons-log";
private static final String FILTER_WALL_PREFIX = "spring.datasource.druid.filter.wall";
```

**配置实例：**

```yaml
spring:
  datasource:
    url: jdbc:mysql://localhost:3306/db_account
    username: root
    password: 123456
    driver-class-name: com.mysql.jdbc.Driver

    druid:
      aop-patterns: com.atguigu.admin.*  #监控SpringBean
      filters: stat,wall     # 底层开启功能，stat（sql监控），wall（防火墙）

      stat-view-servlet:   # 配置监控页功能
        enabled: true
        login-username: admin
        login-password: admin
        resetEnable: false

      web-stat-filter:  # 监控web
        enabled: true
        urlPattern: /*
        exclusions: '*.js,*.gif,*.jpg,*.png,*.css,*.ico,/druid/*'


      filter:
        stat:    # 对上面filters里面的stat的详细配置
          slow-sql-millis: 1000
          logSlowSql: true
          enabled: true
        wall:
          enabled: true
          config:
            drop-table-allow: false

```

# 56、数据访问-整合Mybatis-配置版

```
Mybatis官方网站：https://mybatis.org/mybatis-3/zh/index.html
Mybatis的GitHub地址：https://github.com/mybatis
```

**starter的命名方式：**

+ SpringBoot官方的starter：`spring-boot-starter-*`
+ 第三方：`*-spring-boot-starter`

**引入依赖**

```xml
<dependency>
    <groupId>org.mybatis.spring.boot</groupId>
    <artifactId>mybatis-spring-boot-starter</artifactId>
    <version>2.1.4</version>
</dependency>
```

**Mybatis自动配置分析**

+ `SqlSessionFactory`：自动配置好了
+ `SqlSession`：自动配置了`SqlSessionTemplate`组合了`SqlSession`
+ `@Import(AutoConfiguredMapperScannerRegistrar.class)`
+ `Mapper`： 只要我们写的操作MyBatis的接口标准了`@Mapper`就会被自动扫描进来

```java
@EnableConfigurationProperties(MybatisProperties.class) ： MyBatis配置项绑定类。
@AutoConfigureAfter({ DataSourceAutoConfiguration.class, MybatisLanguageDriverAutoConfiguration.class })
public class MybatisAutoConfiguration{
    ...
}

@ConfigurationProperties(prefix = "mybatis")
public class MybatisProperties{
    ...
}

```

**配置文件**

```yaml
spring:
  datasource:
    username: root
    password: 1234
    url: jdbc:mysql://localhost:3306/my
    driver-class-name: com.mysql.jdbc.Driver

# 配置mybatis规则
mybatis:
  config-location: classpath:mybatis/mybatis-config.xml  #全局配置文件位置
  mapper-locations: classpath:mybatis/*.xml  #sql映射文件位置

```

**mybatis-config.xml：**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
  PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	
    <!-- 由于Spring Boot自动配置缘故，此处不必配置，只用来做做样。-->
</configuration>

```

**Mapper接口**：

```java
import com.lun.boot.bean.User;
import org.apache.ibatis.annotations.Mapper;

@Mapper
public interface UserMapper {
    public User getUser(Integer id);
}

```

**Mapper.xml配置文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lun.boot.mapper.UserMapper">

    <select id="getUser" resultType="com.lun.boot.bean.User">
        select * from user where id=#{id}
    </select>
</mapper>

```

**POJO**

```java
public class User {
    private Integer id;
    private String name;
    
	//getters and setters...
}

```

**DB**：

```sql
CREATE TABLE `user` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8mb4;
```

**Controller and Service**：

```java
@Controller
public class UserController {

    @Autowired
    private UserService userService;

    @ResponseBody
    @GetMapping("/user/{id}")
    public User getUser(@PathVariable("id") Integer id){

        return userService.getUser(id);
    }
}
```

```java
@Service
public class UserService {

    @Autowired
    private UserMapper userMapper;//IDEA下标红线，可忽视这红线

    public User getUser(Integer id){
        return userMapper.getUser(id);
    }

}
```

**<span style="color:red">配置`mybatis.configuration`相关的，就是相当于改mybatis全局配置文件中的值。（也就是说配置了`mybatis.configuration`，就不需配置mybatis全局配置文件了）</span>**

```yaml
# 配置mybatis规则
mybatis:
  mapper-locations: classpath:mybatis/mapper/*.xml
  # 可以不写全局配置文件，所有全局配置文件的配置都放在configuration配置项中了。
  # config-location: classpath:mybatis/mybatis-config.xml
  configuration:
    map-underscore-to-camel-case: true #开启驼峰命名
```

**小结：**

+ 导入Mybatis的starter
+ 编写Mapper接口，需要`@Mapper`注解
+ 编写SQL映射文件并绑定Mapper接口
+ 在`application.yml`中指定Mapper配置文件的所处位置，以及全局配置文件的信息(建议：使用`myabtis.configuration配置`)

# 57、数据访问-整合Mybatis-注解配置混合版

你可以通过Spring Initializr添加MyBatis的Starer。

**Mapper接口：**

```java
@Mapper
public interface UserMapper {
    public User getUser(Integer id);

    @Select("select * from user where id=#{id}")
    public User getUser2(Integer id);

    public void saveUser(User user);

    @Insert("insert into user(`name`) values(#{name})")
    @Options(useGeneratedKeys = true, keyProperty = "id")
    public void saveUser2(User user);

}
```

**Mapper接口对应的sql映射文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.lun.boot.mapper.UserMapper">

    <select id="getUser" resultType="com.lun.boot.bean.User">
        select * from user where id=#{id}
    </select>

    <insert id="saveUser" useGeneratedKeys="true" keyProperty="id">
        insert into user(`name`) values(#{name})
    </insert>

</mapper>

```

+ 简单DAO方法就写在注解上。复杂的就写在配置文件里。
+ **<span style="color:red">使用`@MapperScan("com.lun.boot.mapper")` 简化，Mapper接口就可以不用标注`@Mapper`注解。</span>**

```java
@MapperScan("com.lun.boot.mapper")
@SpringBootApplication
public class MainApplication {

    public static void main(String[] args) {
        SpringApplication.run(MainApplication.class, args);
    }

}
```

# 58、数据访问-整合MybatisPlus操作数据库

```
MybatisPlus官方文档：https://baomidou.com/guide/
```

**MybatisPlus是什么**

[MyBatis-Plus](https://github.com/baomidou/mybatis-plus)（简称 MP）是一个 [MyBatis](http://www.mybatis.org/mybatis-3/)的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

**添加依赖**

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.1</version>
</dependency>
```

+ `MybatisPlusAutoConfiguration`配置类，`MybatisPlusProperties`配置项绑定
+ `SqlSessionFactory`自动配置好了，底层使用默认的数据源
+ `mapperLocations`自动配置好的，有默认值`classpath*：/mapper/**/*.xml`，这表示类路径下的mapper目录下的所有文件都是sql映射文件
+ 容器中也自动配置好了`SqlSessionTemplate`
+ `@Mapper` 标注的接口也会被自动扫描，建议直接 `@MapperScan("com.lun.boot.mapper")`批量扫描。
+ MyBatisPlus**优点**之一：只需要我们的Mapper继承MyBatisPlus的`BaseMapper` 就可以拥有CRUD能力，减轻开发工作。

```java
import com.baomidou.mybatisplus.core.mapper.BaseMapper;
import com.lun.hellomybatisplus.model.User;

public interface UserMapper extends BaseMapper<User> {

}
```

# 59、数据访问-CRUD实验-数据列表展示

```
MP CRUD官方文档：
https://baomidou.com/guide/crud-interface.html#select
```

使用MP提供好的`BaseMapper`、`IService`、`ServiceImpl`，减轻Service层的开发

```java
import com.lun.hellomybatisplus.model.User;
import com.baomidou.mybatisplus.extension.service.IService;

import java.util.List;

/**
 *  Service 的CRUD也不用写了
 */
public interface UserService extends IService<User> {
	//此处故意为空
}

```

```java
import com.lun.hellomybatisplus.model.User;
import com.lun.hellomybatisplus.mapper.UserMapper;
import com.lun.hellomybatisplus.service.UserService;
import com.baomidou.mybatisplus.extension.service.impl.ServiceImpl;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class UserServiceImpl extends ServiceImpl<UserMapper,User> implements UserService {
	//此处故意为空
}
```

与下一节联合在一起

# 60、数据访问-CRUD实验-分页数据展示

与下一节联合在一起

# 61、数据访问-CRUD实验-删除用户完成

**添加分页插件：**

```java
@Configuration
public class MyBatisConfig {


    /**
     * MybatisPlusInterceptor
     * @return
     */
    @Bean
    public MybatisPlusInterceptor paginationInterceptor() {
        MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
        // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
        // paginationInterceptor.setOverflow(false);
        // 设置最大单页限制数量，默认 500 条，-1 不受限制
        // paginationInterceptor.setLimit(500);
        // 开启 count 的 join 优化,只针对部分 left join

        //这是分页拦截器
        PaginationInnerInterceptor paginationInnerInterceptor = new PaginationInnerInterceptor();
        paginationInnerInterceptor.setOverflow(true);
        paginationInnerInterceptor.setMaxLimit(500L);
        mybatisPlusInterceptor.addInnerInterceptor(paginationInnerInterceptor);

        return mybatisPlusInterceptor;
    }
}

```

```html
<table class="display table table-bordered table-striped" id="dynamic-table">
    <thead>
        <tr>
            <th>#</th>
            <th>name</th>
            <th>age</th>
            <th>email</th>
            <th>操作</th>
        </tr>
    </thead>
    <tbody>
        <tr class="gradeX" th:each="user: ${users.records}">
            <td th:text="${user.id}"></td>
            <td>[[${user.name}]]</td>
            <td th:text="${user.age}">Win 95+</td>
            <td th:text="${user.email}">4</td>
            <td>
                <a th:href="@{/user/delete/{id}(id=${user.id},pn=${users.current})}" 
                   class="btn btn-danger btn-sm" type="button">删除</a>
            </td>
        </tr>
    </tfoot>
</table>

<div class="row-fluid">
    <div class="span6">
        <div class="dataTables_info" id="dynamic-table_info">
            当前第[[${users.current}]]页  总计 [[${users.pages}]]页  共[[${users.total}]]条记录
        </div>
    </div>
    <div class="span6">
        <div class="dataTables_paginate paging_bootstrap pagination">
            <ul>
                <li class="prev disabled"><a href="#">← 前一页</a></li>
                <li th:class="${num == users.current?'active':''}" 
                    th:each="num:${#numbers.sequence(1,users.pages)}" >
                    <a th:href="@{/dynamic_table(pn=${num})}">[[${num}]]</a>
                </li>
                <li class="next disabled"><a href="#">下一页 → </a></li>
            </ul>
        </div>
    </div>
</div>
```

`${#numbers.sequence(1,users.pages)}`：是Thymeleaf提供的一个小工具类

地址：https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html#expression-utility-objects

```java
@GetMapping("/user/delete/{id}")
public String deleteUser(@PathVariable("id") Long id,
                         @RequestParam(value = "pn",defaultValue = "1")Integer pn,
                         RedirectAttributes ra){

    userService.removeById(id);

    ra.addAttribute("pn",pn);
    return "redirect:/dynamic_table";
}

@GetMapping("/dynamic_table")
public String dynamic_table(@RequestParam(value="pn",defaultValue = "1") Integer pn,Model model){
    //表格内容的遍历

    //从数据库中查出user表中的用户进行展示

    //构造分页参数
    Page<User> page = new Page<>(pn, 2);
    //调用page进行分页
    Page<User> userPage = userService.page(page, null);

    model.addAttribute("users",userPage);

    return "table/dynamic_table";
}

```

# 62、数据访问-准备阿里云Redis环境

**添加依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<!--导入jedis-->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
</dependency>
```

+ `RedisAutoConfiguration`自动配置类，RedisProperties 属性类 --> spring.redis.xxx是对redis的配置。
+ 连接工厂`LettuceConnectionConfiguration、JedisConnectionConfiguration`是准备好的。
+ 自动注入了`RedisTemplate<Object, Object>，xxxTemplate`。
+ 自动注入了`StringRedisTemplate`，key，value都是String
+ 底层只要我们使用`StringRedisTemplate、RedisTemplate`就可以操作Redis。

**外网Redis环境搭建**：

1. 阿里云按量付费Redis，其中选择**经典网络**。
2. 申请Redis的公网连接地址。
3. 修改白名单，允许`0.0.0.0/0`访问。



# 63、指标监控-SpringBoot Actuator与Endpoint

未来每一个微服务在云上部署以后，我们都需要对其进行监控、追踪、审计、控制等。SpringBoot就抽取了Actuator场景，使得我们在每个微服务快速引用即可获得生产级别的应用监控、审计等功能

```
Actuator官方文档：https://docs.spring.io/spring-boot/docs/2.4.2/reference/htmlsingle/#production-ready
```

+  **SpringBoot Actuator 1.x版本与2.x版本的区别**
  + SpringBoot Actuator 1.x
    + 支持SpringMVC
    + 基于继承方式进行扩展
    + 层级Metrics收集
    + 自定义Metrics收集
    + 默认较少的安全策略
  + SpringBoot Actuator 2.x
    + 支持SpringMVC、JAX-RS和Webflux
    + 注解驱动进行扩展
    + 层级&名称空间Metrics
    + 底层使用MicroMeter、强大、便捷默认丰富的安全策略

## 如何使用Actuator

**1、添加依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

**2、暴露所有web的监控信息(默认没暴露)**

```yaml
management:
  endpoints:
    enabled-by-default: true #暴露所有端点信息
    web:
      exposure:
        include: '*'  #以web方式暴露
```

3、访问`http://localhost:8080/actuator/**`进行查看

+ 测试例子
  + http://localhost:8080/actuator/beans
  + http://localhost:8080/actuator/configprops
  + http://localhost:8080/actuator/metrics
  + http://localhost:8080/actuator/metrics/jvm.gc.pause
  + http://localhost:8080/actuator/metrics/endpointName/detailPath

# 64、指标监控-常用的端点以及开启与禁用

## 常使用的端点

|         ID         | 描述                                                         |
| :----------------: | ------------------------------------------------------------ |
|   `auditevents`    | 暴露当前应用程序的审核事件信息。需要一个`AuditEventRepository组件`。 |
|      `beans`       | 显示应用程序中所有Spring Bean的完整列表。                    |
|      `caches`      | 暴露可用的缓存。                                             |
|    `conditions`    | 显示自动配置的所有条件信息，包括匹配或不匹配的原因。         |
|   `configprops`    | 显示所有`@ConfigurationProperties`。                         |
|       `env`        | 暴露Spring的属性`ConfigurableEnvironment`                    |
|      `flyway`      | 显示已应用的所有Flyway数据库迁移。 需要一个或多个`Flyway`组件。 |
|      `health`      | 显示应用程序运行状况信息。                                   |
|    `httptrace`     | 显示HTTP跟踪信息（默认情况下，最近100个HTTP请求-响应）。需要一个`HttpTraceRepository`组件。 |
|       `info`       | 显示应用程序信息。                                           |
| `integrationgraph` | 显示Spring `integrationgraph` 。需要依赖`spring-integration-core`。 |
|     `loggers`      | 显示和修改应用程序中日志的配置。                             |
|    `liquibase`     | 显示已应用的所有Liquibase数据库迁移。需要一个或多个`Liquibase`组件。 |
|     `metrics`      | 显示当前应用程序的“指标”信息。                               |
|     `mappings`     | 显示所有`@RequestMapping`路径列表。                          |
|  `scheduledtasks`  | 显示应用程序中的计划任务。                                   |
|     `sessions`     | 允许从Spring Session支持的会话存储中检索和删除用户会话。需要使用Spring Session的基于Servlet的Web应用程序。 |
|     `shutdown`     | 使应用程序正常关闭。默认禁用。                               |
|     `startup`      | 显示由`ApplicationStartup`收集的启动步骤数据。需要使用`SpringApplication`进行配置`BufferingApplicationStartup`。 |
|    `threaddump`    | 执行线程转储。                                               |

如果你的应用程序是web应用，则可以使用以下附加端点：

|      ID      | 描述                                                         |
| :----------: | ------------------------------------------------------------ |
|  `heapdump`  | 返回`hprof`堆转储文件。                                      |
|  `jolokia`   | 通过HTTP暴露JMX bean（需要引入Jolokia，不适用于WebFlux）。需要引入依赖`jolokia-core`。 |
|  `logfile`   | 返回日志文件的内容（如果已设置`logging.file.name`或`logging.file.path`属性）。支持使用HTTP`Range`标头来检索部分日志文件的内容。 |
| `prometheus` | 以Prometheus服务器可以抓取的格式公开指标。需要依赖`micrometer-registry-prometheus`。 |

其中最常用的Endpoint：

+ **Health：监控状况**
+ **Metrics：运行时指标**
+ **Loggers：日志记录**

## Health Endpoint

健康检查端点，我们一般使用于云平台，平台会定时检查他们的健康状态，我们就需要Health Endpoint可以为平台返回当前应用的一系列组件健康状况的集合

重要的几点：

- health endpoint返回的结果，应该是一系列健康检查后的一个汇总报告。
- 很多的健康检查默认已经自动配置好了，比如：数据库、redis等。
- 可以很容易的添加自定义的健康检查机制。

## Metrics Endpoint

提供详细的、层级的、空间指标信息，这些信息可以被pull(推送)或者push(被动获取)方式得到：

- 通过Metrics对接多种监控系统。
- 简化核心Metrics开发。
- 添加自定义Metrics或者扩展已有Metrics。

## 开启与禁用Endpoints

+ 默认所有的Endpoint都是开启的。(除了shutdown)
+ 需要开启或者禁用某个Endpoint。配置模式为`management.endpoint.endpoint名称.enabled=true`

```yaml
management:
  endpoint:
    beans:
      enabled: true
```

+ 或者禁用所有的Endpoints ，然后在手动开启指定的Endpoint

```yaml
management:
  endpoints:
    enabled-by-default: false
  endpoint:
    beans:
      enabled: true
    health:
      enabled: true
```

## 暴露Endpoints

+ 支持的暴露方式

  + HTTP：默认只暴露health和info
  + JMX：默认暴露所有Endpoint。
  + 除过health和info，剩下的Endpoint都应该进行保护访问。如果引入Spring Security，则会默认配置安全访问规则。

  |         ID         | JMX  | Web  |
  | :----------------: | ---- | ---- |
  |   `auditevents`    | Yes  | No   |
  |      `beans`       | Yes  | No   |
  |      `caches`      | Yes  | No   |
  |    `conditions`    | Yes  | No   |
  |   `configprops`    | Yes  | No   |
  |       `env`        | Yes  | No   |
  |      `flyway`      | Yes  | No   |
  |      `health`      | Yes  | Yes  |
  |     `heapdump`     | N/A  | No   |
  |    `httptrace`     | Yes  | No   |
  |       `info`       | Yes  | Yes  |
  | `integrationgraph` | Yes  | No   |
  |     `jolokia`      | N/A  | No   |
  |     `logfile`      | N/A  | No   |
  |     `loggers`      | Yes  | No   |
  |    `liquibase`     | Yes  | No   |
  |     `metrics`      | Yes  | No   |
  |     `mappings`     | Yes  | No   |
  |    `prometheus`    | N/A  | No   |
  |  `scheduledtasks`  | Yes  | No   |
  |     `sessions`     | Yes  | No   |
  |     `shutdown`     | Yes  | No   |
  |     `startup`      | Yes  | No   |
  |    `threaddump`    | Yes  | No   |

  若要更改公开的Endpoint，请配置以下的包含和排除属性：

  |                  Property                   | Default        |
  | :-----------------------------------------: | -------------- |
  | `management.endpoints.jmx.exposure.exclude` |                |
  | `management.endpoints.jmx.exposure.include` | `*`            |
  | `management.endpoints.web.exposure.exclude` |                |
  | `management.endpoints.web.exposure.include` | `info, health` |

  ```
  官方文档 - Exposing Endpoints：
  
  https://docs.spring.io/spring-boot/docs/2.4.2/reference/htmlsingle/#production-ready-endpoints-exposing-endpoints
  ```

# 65、指标监控-定制Endpoint

## 定制Health信息

```YAML
management:
    health:
      enabled: true
      show-details: always #总是显示详细信息。可显示每个模块的状态信息
```

通过实现`HealthIndicator`接口，或继承`MyComHealthIndicator`类。

```java
import org.springframework.boot.actuate.health.Health;
import org.springframework.boot.actuate.health.HealthIndicator;
import org.springframework.stereotype.Component;

@Component
public class MyHealthIndicator implements HealthIndicator {

    @Override
    public Health health() {
        int errorCode = check(); // perform some specific health check
        if (errorCode != 0) {
            return Health.down().withDetail("Error Code", errorCode).build();
        }
        return Health.up().build();
    }

}

/*
构建Health
Health build = Health.down()
                .withDetail("msg", "error service")
                .withDetail("code", "500")
                .withException(new RuntimeException())
                .build();
*/

```

```java
@Component
public class MyComHealthIndicator extends AbstractHealthIndicator {

    /**
     * 真实的检查方法
     * @param builder
     * @throws Exception
     */
    @Override
    protected void doHealthCheck(Health.Builder builder) throws Exception {
        //mongodb。  获取连接进行测试
        Map<String,Object> map = new HashMap<>();
        // 检查完成
        if(1 == 2){
//            builder.up(); //健康
            builder.status(Status.UP);
            map.put("count",1);
            map.put("ms",100);
        }else {
//            builder.down();
            builder.status(Status.OUT_OF_SERVICE);
            map.put("err","连接超时");
            map.put("ms",3000);
        }


        builder.withDetail("code",100)
                .withDetails(map);

    }
}

```

### 定制info信息

常用两种方式：

+ 编写配置文件

```YAML
info:
  appName: boot-admin
  version: 2.0.1
  mavenProjectName: @project.artifactId@  #使用@@可以获取maven的pom文件值
  mavenProjectVersion: @project.version@

```

- 编写InfoContributor

```java
import java.util.Collections;

import org.springframework.boot.actuate.info.Info;
import org.springframework.boot.actuate.info.InfoContributor;
import org.springframework.stereotype.Component;

@Component
public class ExampleInfoContributor implements InfoContributor {

    @Override
    public void contribute(Info.Builder builder) {
        builder.withDetail("example",
                Collections.singletonMap("key", "value"));
    }

}

```

http://localhost:8080/actuator/info 会输出以上方式返回的所有info信息

## 定制Metrics信息

```
Spring Boot支持的metrics：
https://docs.spring.io/spring-boot/docs/2.4.2/reference/htmlsingle/#production-ready-metrics-meter
```

增加定制Metrics：

```java
class MyService{
    Counter counter;
    public MyService(MeterRegistry meterRegistry){
         counter = meterRegistry.counter("myservice.method.running.counter");
    }

    public void hello() {
        counter.increment();
    }
}

```

```java
//也可以使用下面的方式
@Bean
MeterBinder queueSize(Queue queue) {
    return (registry) -> Gauge.builder("queueSize", queue::size).register(registry);
}

```

## 定制Endpoint

```JAVA
@Component
@Endpoint(id = "container")
public class DockerEndpoint {

    @ReadOperation
    public Map getDockerInfo(){
        return Collections.singletonMap("info","docker started...");
    }

    @WriteOperation
    private void restartDocker(){
        System.out.println("docker restarted....");
    }

}

```

场景：

- 开发ReadinessEndpoint来管理程序是否就绪。
- 开发LivenessEndpoint来管理程序是否存活。

# 66、指标监控-Boot Admin Server

```
What is Spring Boot Admin?

codecentric’s Spring Boot Admin is a community project to manage and monitor your Spring Boot ® applications. The applications register with our Spring Boot Admin Client (via HTTP) or are discovered using Spring Cloud ® (e.g. Eureka, Consul). The UI is just a Vue.js application on top of the Spring Boot Actuator endpoints.


开始使用方法：https://codecentric.github.io/spring-boot-admin/2.3.1/#getting-started
```

# 67、高级特性-profile环境切换

为了方便多环境适配，SpringBoot简化了profile功能

+ 默认配置文件`application.yaml`任何时候都会加载
+ 指定环境配置文件`application-{env}.yaml`，`env`通常替代为`test`
+ **激活指定配置文件**
  + **配置文件激活：`springboot-active=prod`**
  + **命令行方式激活：`java -jar xxx.jar --spring.profile.active=prod --person.name=haha`(修改配置文件的任意值，命令行优先)**
+ 默认配置文件与环境配置文件同时生效
+ **同名的配置项，profile配置优先。**

## @Profile条件装配功能

```java
@Data
@Component
@ConfigurationProperties("person")//在配置文件中配置
public class Person{
    private String name;
    private Integer age;
}
```

**application.yaml**

```yaml
person: 
  name: lun
  age: 8
```

```java
public interface Person {

   String getName();
   Integer getAge();

}

@Profile("test")//加载application-test.yaml里的
@Component
@ConfigurationProperties("person")
@Data
public class Worker implements Person {

    private String name;
    private Integer age;
}

@Profile(value = {"prod","default"})//加载application-prod.yaml里的
@Component
@ConfigurationProperties("person")
@Data
public class Boss implements Person {

    private String name;
    private Integer age;
}

```

**application-test.yaml**

```yaml
person:
  name: test-张三

server:
  port: 7000
```

**application-prod.yaml**

```yaml
person:
  name: prod-张三

server:
  port: 8000

```

**application.yaml**

```yaml
# 激活prod配置文件
spring:
  profiles:
    active: prod
```

```java
@Autowired
private Person person;

@GetMapping("/")
public String hello(){
    //激活了prod，则返回Boss；激活了test，则返回Worker
    return person.getClass().toString();
}
```

**@Profile还可以修饰在方法上：**

```java
class Color {
}

@Configuration
public class MyConfig {

    @Profile("prod")
    @Bean
    public Color red(){
        return new Color();
    }

    @Profile("test")
    @Bean
    public Color green(){
        return new Color();
    }
}

```

**可以激活一组：**

```properties
spring.profiles.active=production

spring.profiles.group.production[0]=proddb
spring.profiles.group.production[1]=prodmq
```

# 68、高级特性-配置加载优先级

```
官方文档：
https://docs.spring.io/spring-boot/docs/2.4.2/reference/htmlsingle/#boot-features-external-config
```

1. Default properties (specified by setting `SpringApplication.setDefaultProperties`).
2. `@PropertySource `annotations on your `@Configuration` classes. Please note that such property sources are not added to the Environment until the application context is being refreshed. This is too late to configure certain properties such as `logging.* `and `spring.main.*` which are read before refresh begins.
3. Config data (such as `application.properties`files)
4. A `RandomValuePropertySource` that has properties only in random.*.
5. OS environment variables.
6. Java System properties (`System.getProperties()`).
7. JNDI attributes from` java:comp/env.`
8. `ServletContext` init parameters.
9. `ServletConfig `init parameters.
10. Properties from` SPRING_APPLICATION_JSON` (inline JSON embedded in an environment variable or system property).
11. Command line arguments.
12. `properties` attribute on your tests. Available on `@SpringBootTest `and the test annotations for testing a particular slice of your application.
13. `@TestPropertySource` annotations on your tests.
14. Devtools global settings properties in the `$HOME/.config/spring-boot` directory when devtools is active.

```java
import org.springframework.stereotype.*;
import org.springframework.beans.factory.annotation.*;

@Component
public class MyBean {

    @Value("${name}")//以这种方式可以获得配置值
    private String name;

    // ...

}

```

+ 外部配置源
  + Java属性文件。
  + YAML文件。
  + 环境变量。
  + 命令行参数。
  
+ 配置文件查找位置

  + classpath 根路径。
  + classpath 根路径下config目录。
  + jar包当前目录。
  + jar包当前目录的config目录。
  + /config子目录的直接子目录。

+ 配置文件加载顺序：

  + 当前jar包内部的`application.properties`和`application.yml。`
  + 当前jar包内部的`application-{profile}.properties `和 `application-{profile}.yml`
  + 引用的外部jar包的`application.properties`和`application.yml。`
  + 引用的外部jar包的`application-{profile}.properties`和`application-{profile}.yml。`

  - 指定环境优先，外部优先，后面的可以覆盖前面的同名配置项。

  

  

   