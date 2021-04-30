# Swagger引言

```
相信无论是前端还是后端开发，都或多或少的被接口文档折磨过。前端经常抱怨后端给的接口文档与实际情况不一致。后端又觉得编写及维护接口文档会耗费不少精力，经常来不及更新。其实无论是前端调用后端，还是后端调用前端，都期望有一个好的接口文档。但是这个接口文档对于开发来说，就和注释一样，经常还会抱怨别人写的代码没有写注释，然后自己写起代码来，最讨厌的也是写注释。所以仅仅只通过强制来规范大家是不够的，随着时间的推移，版本的迭代，接口文档往往很容易就跟不上代码了。
```

# 1.什么是Swagger

Swagger可以扫描相关的代码，生成该描述文件，进而生成与代码一致的接口文档和客户端代码。**这种通过代码生成接口文档的形式，就是Swagger在做的。**

Swagger官网：https://swagger.io/

![image-20210430110856631](C:\Users\leowlwang\AppData\Roaming\Typora\typora-user-images\image-20210430110856631.png)

# 2 Swagger官网提供的工具

![image-20210430110955181](C:\Users\leowlwang\AppData\Roaming\Typora\typora-user-images\image-20210430110955181.png)

+ **Swagger Codegen**：通过Codegen 可以将描述文件生成html格式和cwiki形式的接口文档，同时也能生成多种语言的服务端和客户端代码。支持通过jar包、docker、node等方式在本地化执行生成。也可以在后面的Swagger Editor中在线生成
+ **Swagger  UI：**提供了一个可视化的UI页面展示描述文件。接口的调用方、测试、项目经理等都可以在该页面中对相关接口进行查阅和做一些简单的接口请求。该项目支持在线导入描述文件和本地部署UI项目。
+ **Swagger  Editor：**类似于MarkDown 编辑器的编辑Swagger描述文件的编辑器，**该编辑器支持实时预览描述文件的更新效果**，也提供了在线编辑器和本地部署器两种方式。
+ **Swagger Inspector：**和Postman差不多，是一个可以对接口进行测试的在线版的postman。比如在 Swagger UI 里面做接口请求，会返回更多的信息，也会保存你请求的实际请求参数等数据。
+ **Swagger Hub：**集成了上面所有项目的各个功能，你可以以项目和版本为单位，将你的描述文件上传到Swagger Hub中。在 Swagger Hub 中可以完成上面项目的所有工作，需要注册账号，分免费版和收费版。
+ **Springfox Swagger：**Spring 基于 Swagger 规范，可以将基于 SpringMVC 和 Spring Boot 项目的项目代码，自动生成 JSON 格式的描述文件。本身不是属于 Swagger 官网提供的，在这里列出来做个说明，方便后面作一个使用的展开。

# 3.SringBoot整合Swagger

## 3.1 引入依赖

```xml
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger2 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- https://mvnrepository.com/artifact/io.springfox/springfox-swagger-ui -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger-ui</artifactId>
    <version>2.9.2</version>
</dependency>
```

## 3.2 编写Swagger 配置类

**<span style="color:red">这个配置类基本上是不变的，就算边拿来直接改改即可。</span>**

```
@EnableSwagger2
@Configuration
public class SwaggerConfig {

    @Bean
    public Docket createRestApi() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(apiInfo())
                .select().apis(RequestHandlerSelectors.basePackage("com.auo.controller"))
                .paths(PathSelectors.any())
                .build();
    }


    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("标题: SpringBoot 整合 Swagger 使用")
                .description("详细信息: SpringBoot 整合 Swagger,详细信息......")
                // 版本信息
                .version("1.1")
                // 开发文档的联系人
                .contact(new Contact("baiyi", "http://www.baidu.com", "1101293873@qq.com"))
                .license("This Baidu License")
                .licenseUrl("http://www.baidu.com")
                .build();
    }
}
```

## 3.3 启动SpringBoot项目

![img](https://img-blog.csdnimg.cn/20201022160315912.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 3.4 访问Swagger 的 UI 界面

```
访问 Swagger 提供的 UI 界面：http://localhost:8080/swagger-ui.html
```

![img](https://img-blog.csdnimg.cn/20201022160448829.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

# 4 测试Swagger整合的结果

## 4.1 开发Controller接口

```java
@RestController
@RequestMapping("/user")
public class HelloController {

    @GetMapping("/findAll")
    public Map<String,Object> findAll(){
        Map<String, Object> map = new HashMap<>();
        map.put("success", "查询所有数据成功");
        map.put("status", true);
        return map;
    }
}
```

## 4.2 重启项目访问接口界面

![img](https://img-blog.csdnimg.cn/20201022160528547.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

# <span style="color:red">5.Swagger注解主要作用(重点)</span>

## 5.1 @Api

```
作用：用来指定接口的描述文字
修饰范围：用在类上
常用属性：
	1.tags，用来修改hello-controller的名称
	2.value：该注解中value属性没有什么意义
```

```java
@RequestMapping("/user")
@Api(tags = "用户服务相关接口描叙")
public class HelloController {

		....
}
```



## 5.2 @ApiOperation

```
作用：用来对接口中具体方法做描述
修饰范围：用在方法上
常用属性：value：用于方法描述
		notes：方法的备注说明
```

```java
@GetMapping("/findAll")
@ApiOperation(value = "查询所有用户接口",
        notes = "<span style='color:red;'>描叙:</span>&nbsp;&nbsp;用来查询所有用户信息的接口")
public Map<String,Object> findAll(){
    Map<String, Object> map = new HashMap<>();
    map.put("success", "查询所有数据成功");
    map.put("status", true);
    return map;
}
```

## 5.3 @ApiImplicitParams

```
作用：用来对方法中参数进行说明
修饰范围：用在方法上
常用属性：
	1.name：参数名
	2.value：参数的解析
	3.required：参数是否必须传
	4.paramType：
		+ query：对应@RequestParam注解的参数
		+ path：对应restful风格的@PathVariable注解的参数
		+ body：对应的是@ReuqestBody注解的参数
		+ header：对应@RequestHeader注解的参数
	5.dateType：参数类型，默认String，其他值dataType="Integer"
	6.defaultValue:参数的默认值
```

### 5.3.1 普通参数使用

```java
@PostMapping("save")
@ApiOperation(value = "保存用户信息接口",
        notes = "<span style='color:red;'>描叙:</span>&nbsp;&nbsp;用来保存用户信息的接口")
@ApiImplicitParams({
        @ApiImplicitParam(name = "id", value = "用户 id", dataType = "String", defaultValue = "21"),
        @ApiImplicitParam(name = "name", value = "用户姓名", dataType = "String", defaultValue = "白衣")
})
public Map<String, Object> save(String id, String name) {
    System.out.println("id = " + id);
    System.out.println("name = " + name);
    Map<String, Object> map = new HashMap<>();
    map.put("id", id);
    map.put("name", name);
    return map;
}
```

![img](https://img-blog.csdnimg.cn/20201022160622895.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 5.3.2 RestFul风格使用(@PostMapping("save/{id}/{name}"))

```
如果使用的是RestFul风格进行传参，必须再添加一个 paramType=“path”
```

```java
@PostMapping("save/{id}/{name}")
@ApiOperation(value = "保存用户信息接口",
        notes = "<span style='color:red;'>描叙:</span>&nbsp;&nbsp;用来保存用户信息的接口")
@ApiImplicitParams({
        @ApiImplicitParam(name = "id", value = "用户 id", dataType = "String", defaultValue = "21", paramType = "path"),
        @ApiImplicitParam(name = "name", value = "用户姓名", dataType = "String", defaultValue = "白衣", paramType = "path")
})
public Map<String, Object> save(@PathVariable("id") String id,@PathVariable("name") String name) {
    System.out.println("id = " + id);
    System.out.println("name = " + name);
    Map<String, Object> map = new HashMap<>();
    map.put("id", id);
    map.put("name", name);
    return map;
}
```

![img](https://img-blog.csdnimg.cn/20201022160716695.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

### 5.3.3 json格式使用

```
如果是 RequestBody 的方式，需要定义一个对象进行接收。
```

**1.定义一个User对象**

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private String id;
    private String name;
}
```

**2.编写Controller**

```java
@PostMapping("save2")
public Map<String, Object> save2(@RequestBody User user) {
    System.out.println("id = " + user.getId());
    System.out.println("name = " + user.getName());
    Map<String, Object> map = new HashMap<>();
    map.put("id", user.getId());
    map.put("name", user.getName());
    return map;
}
```

**3.重启项目，刷新UI界面**

![img](https://img-blog.csdnimg.cn/20201022160752184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

测试：

![img](https://img-blog.csdnimg.cn/20201022160817381.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaXF1ZV9wZXJmZWN0,size_16,color_FFFFFF,t_70#pic_center)

## 5.4@ApiResponses

```
作用：用在请求的方法上，表示一组响应
修饰范围：用在方法上。
常用属性：
	1.code：状态码
	2.message：响应信息
```

```java
@PostMapping("save2")
@ApiResponses({
        @ApiResponse(code = 404, message = "请求路径不对"),
        @ApiResponse(code = 400, message = "程序不对")
})
public Map<String, Object> save2(@RequestBody User user) {
    System.out.println("id = " + user.getId());
    System.out.println("name = " + user.getName());
    Map<String, Object> map = new HashMap<>();
    map.put("id", user.getId());
    map.put("name", user.getName());
    return map;
}
```