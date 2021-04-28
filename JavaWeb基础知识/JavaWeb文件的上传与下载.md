# 文件的上传与下载

文件的上传和下载，是非常常见的功能。在很多的系统中，或者软件中都有文件上传与下载的功能。

比如：QQ头像，就使用了文件的上传

邮箱中也有附件的上传和下载功能

# <span style="color:red">1.文件的上传介绍（重点）</span>

上传的步骤：

1. 要有一个form标签，method是post
2. **<span style="color:red">form标签的encType属性必须是multipart/form-data值</span>**
3. 在form标签中使用input type=file 添加上传的文件
4. 编写服务器代码(Servlet程序)接收，处理上传的数据



encType=multipart/form-data表示提交的数据以多段(每一个表单项一个数据段)的形式进行拼接，然后**<span style="color:red">以二进制流的形式</span>发送给服务器**，**<span style="color:red">服务器也需要以二进制流的方式读取</span>**



## 1.1 文件上传,HTTP协议说明

![image-20210426232802418](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210426232802418.png)



## 1.2 commons-fileupload.jar 常用API介绍说明

+ commons-fileupload.jar 依赖commons-io.jar这个包,所以这两个jar包都需要引入。
+ 第一步：引入两个jar包
  + commons-fileupload.jar
  + commons-io.jar
+ **commons-fileupload.jar和commons-io.jar包中，我们常用的类有哪些？**
  + ServletFileUpload类，用于解析上传的数据
  + FileItem类，表示每一个表单项
  + boolean ServletFileUpload.isMultipartContent(HttpServletRequest req)：判断当前上传的数据格式是否是多段的格式
  + **`public List<FileItem> parseRequest(HttpServletRequest request)`：<span style="color:red">解析上传的数据</span>**
  + boolean FileItem.isFormField()：判断当前这个表单项，是普通的表单项，还是上传的文件类型
    + true，表示是普通类型的表单项
    + false，表示上传的文件类型
  + String FileItem.getFieldName()：获取表单项的name属性值
  + String FileItem.getString()：获取当前表单项的值
  + String FileItem.getName()：获取上传的文件名
  + void FileItem.write(file)：将上传的文件写到参数file所指向的磁盘位置。

## 1.3 文件上传快速入门

1.创建上传文件的表单

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>文件上传</title>
</head>
<body>
<form action="http://localhost:8080/09_el_jstl/fileupload" method="post" enctype="multipart/form-data">
    用户名:<input type="text" name="username"/><br/>
    头像:<input type="file" name="photo"/><br/>
    <input type="submit" value="上传">
</form>
</body>
</html>
```

2.解析上传数据的代码

```java
public class FileUploadServlet extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //System.out.println("文件上传了");

        //1.首先判断当前上传的数据是否是多段（只有是多段的数据，才是文件上传）
        boolean isMultipartContent = ServletFileUpload.isMultipartContent(req);
        if (isMultipartContent) {

            //创建FileItemFactory对象,传入到ServletFileUpload构造器中
            FileItemFactory fileItemFactory = new DiskFileItemFactory();

            //创建用于解析上传数据的工具类ServletFileUpload
            ServletFileUpload servletFileUpload = new ServletFileUpload(fileItemFactory);

            try {
                //解析上传的数据，得到每一个表单项FileItem
                List<FileItem> list = servletFileUpload.parseRequest(req);
                //循环遍历list,获取每一个FileItem
                for (FileItem fileItem : list) {
                    //判断当前的表单项是普通表单项还是上传的文件。
                    if (fileItem.isFormField()) {
                        //普通表单项
                        String fieldName = fileItem.getFieldName();
                        System.out.println("普通表单项name的属性值:" + fieldName);

                        //获取表单项value的属性值, 参数UTF-8 解决乱码问题
                        String fieldValue = fileItem.getString("UTF-8");
                    } else {
                        //文件上传
                        //获取上传文件的名字
                        String name = fileItem.getName();
                        System.out.println("上传的文件名:" + name);

                        //获取文件上传表单项的name的属性值
                        String fieldName = fileItem.getFieldName();
                        System.out.println("文件上传表单项的name的属性值:" + fieldName);

                        fileItem.write(new File("E:\\" + name));
                    }
                }

            } catch (FileUploadException e) {
                e.printStackTrace();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

# 2.文件的下载

## **2.1下载的常用API说明**

+ response.getOutputStream();
+ servletContext.getResourceAsStream();
+ servletContext.getMimeType()；
+ response.setContentType()；

**`response.setHeader("Content-Disposition","attachment;fileName=1.jpg");`**

这个响应头的作用是告诉浏览器，这个是需要下载的。而**`attachment`**表示附件，也就是下载的一个文件。`fileName=文件名.后缀`

## 2.2文件下载的流程

![image-20210427182504062](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210427182504062.png)



**代码示例**

```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //1.获取到要下载的文件名
        String downloadFileName = "iphone.jpg";

        //2.读取要下载的文件内容(通过ServletContext)
        ServletContext servletContext = getServletContext();

        //5.通过ServletContext获取要下载的文件的数据类型
        String mimeType = servletContext.getMimeType("/WEB-INF/file/" + downloadFileName);

        //6.在回传之前告诉浏览器数据的类型
        resp.setContentType(mimeType);

        //7.在回传之前要告诉浏览器这是一个要下载的文件
        resp.setHeader("Content-Disposition","attachment;fileName=" + downloadFileName);

        //通过ServletContext的getResourceAsStream方法读取到文件
        InputStream is = servletContext.getResourceAsStream("/WEB-INF/file/" + downloadFileName);

        //3.获取输出流对象
        ServletOutputStream ops = resp.getOutputStream();
        //4.通过common-io.jar中的IOUtils.copy将内容复制到输出流并回传给客户端。
        IOUtils.copy(is,ops);
        
    }
```

完成上面的两个步骤，下载文件是没问题了。但是如果我们要下载的文件名是中文名的话。你会发现，下载无法正确显示出正确的中文名。

`原因是`在响应头中，不能含有中文字符，只能包含ASCII码。

```
注意：resp.setHeader("Content-Disposition","attachment;fileName=" + downloadFileName)中的fileName可以自定义，不一定要和获取文件的时候名字一致。
```

## 2.3 附件中文乱码问题解决方案

如果我们要下载的文件是中文名的话。你会发现，下载无法显示出正确的中文名。原因是在响应头中，不能包含有中文字符，只能包含ASCII 码。

### 方案一：使用URLEncoder解决IE和谷歌浏览器的文件名中文乱码问题

如果浏览器是IE浏览器或者是谷歌浏览器，我们需要使用URLEncoder类对文件名为中文的文件进行UTF-8的编码操作。

因为IE浏览器和谷歌浏览器收到含有编码后的字符串会以UTF-8字符集进行解码显示。

```java
        //7.在回传之前要告诉浏览器这是一个要下载的文件
        resp.setHeader("Content-Disposition","attachment;fileName= " + URLEncoder.encode("中国.jpg","UTF-8"));
```

### 方案二：使用BASE64编码 解决火狐浏览器的附件中文名问题

如果客户端浏览器是火狐浏览器，那么我们需要对中文名进行BASE64的编码操作。

**<span style="color:red">这时候需要把Content-Disposition:attachment;fileName=中文名.后缀编码成为：Content-Disposition:attachment;fileName==?charset?B?xxxxx?=</span>**

**=?charset?B?xxxxx?=  这段文字说明**

+ = ：代表BASE64编码开始
+ charset：表示字符集
+ B：代表这是BASE64编码
+ xxxx：进行BASE64编码后的内容
+ =：代表BASE64编码的结束
+ 每个之间用?进行连接

```java
        resp.setHeader("Content-Disposition", "attachment;fileName==?UTF-8?B?" + new BASE64Encoder().encode("中国".getBytes("UTF-8")) + "?=");
```

### 方案三：对所有的浏览器都适用

根据User-Agent请求头中是否包含firefox来判断该浏览器是火狐还是其他浏览器。

**代码示例**

```java
@Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //1.获取到要下载的文件名
        String downloadFileName = "iphone.jpg";

        //2.读取要下载的文件内容(通过ServletContext)
        ServletContext servletContext = getServletContext();

        //5.通过ServletContext获取要下载的文件的数据类型
        String mimeType = servletContext.getMimeType("/WEB-INF/file/" + downloadFileName);

        //6.在回传之前告诉浏览器数据的类型
        resp.setContentType(mimeType);

        //7.在回传之前要告诉浏览器这是一个要下载的文件
        if (req.getHeader("User-Agent").contains("Firefox")){
            //火狐浏览器
            resp.setHeader("Content-Disposition", "attachment;fileName==?UTF-8?B?" + new BASE64Encoder().encode("中国".getBytes("UTF-8")) + "?=");
        }else{
            //谷歌浏览器或者IE浏览器
            resp.setHeader("Content-Disposition","attachment;fileName= " + URLEncoder.encode("中国.jpg","UTF-8"));
        }

        //通过ServletContext的getResourceAsStream方法读取到文件
        InputStream is = servletContext.getResourceAsStream("/WEB-INF/file/" + downloadFileName);

        //3.获取输出流对象
        ServletOutputStream ops = resp.getOutputStream();
        //4.通过common-io.jar中的IOUtils.copy将内容复制到输出流并回传给客户端。
        IOUtils.copy(is, ops);

    }
```

注意

```
实测了一下,火狐现在也支持URLEncoder的编码方式了！！也可以识别中文
```



# 3.BASE64编码、解码操作

```java
@Test
    public void test() throws IOException {

        String content= "我爱你塞北的雪";

        //创建一个BASE64编码器
        BASE64Encoder encoder = new BASE64Encoder();

        //进行编码操作
        String encodeStr = encoder.encode(content.getBytes("UTF-8"));

        System.out.println(encoder);

        //创建一个BASE64解码器
        BASE64Decoder decoder = new BASE64Decoder();
        //解码操作
        byte[] buffer = decoder.decodeBuffer(encodeStr);

        System.out.println(new String(buffer));
        
    }
```

