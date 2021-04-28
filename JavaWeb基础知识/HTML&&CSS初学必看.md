# 1.B/S软件的结构

在之前学的 JavaSE   ------>       C/S      ------>   Client/Server

B/S ------> 	Browser/Server

![image-20210408211541962](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210408211541962.png)

# 2.前端的开发流程

![image-20210408211744952](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210408211744952.png)

# 3.网页的组成部分

页面主要由三部分组成，分别是内容（结构）、表现、行为。

+ 内容(结构)：是我们可以在页面看到的数据。我们称之为内容。一般内容我们使用html技术来展示
+ 表现：指的是这些内容在页面上的展示形式。比如说：布局，颜色、样式；一般用CSS技术实现
+ 行为：指页面中元素与输入设备交互的响应。一般使用JavaScript技术实现

# 4.HTML简介

+ HTML：Hyper  Text   Markup  Language（超文本标记语言）
+ HTML通过**标签**来标记要显示的网页中的各个部分。网页文件本身是一种文本文件，通过在文本文件中添加标记符，可以告诉浏览器如何显示其中的内容（如：文字如何处理，画面如何安排，图片如何显示等）

# 5.创建HTML文件

## 1.创建一个web工程（静态的工程）

File	----->	NEW	----->	Project

![image-20210408212421660](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210408212421660.png)



![image-20210408212647403](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210408212647403.png)



## 2.在工程下创建html页面

![image-20210408212728406](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210408212728406.png)



选择浏览器执行页面

![image-20210408212831068](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210408212831068.png)



第一个html示例：

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>标题</title>
    </head>
    <body>
        hello!
    </body>
</html>
```



# 6.HTML文件的书写规范

```html
<html>
    <head>
        <title>标题</title>
    </head>
    <body>
        页面主体内容
    </body>
</html>

HTML的代码注释  <!-- 注释的内容 -->
```



# 7.HTML标签介绍

1. 标签的格式
2. **标签名大小写不敏感**
3. 标签拥有自己的属性
   1. 分为基本属性：`bgcolor="red"`
   2. 事件属性：`onclick="alert('你好！');"`
4. 标签又分为：单标签和双标签
   1. 单标签格式：`<标签名/>`
   2. 双标签格式：<标签名> 封装的数据 </标签名>

![image-20210408215356718](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210408215356718.png)

**标签的语法**

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>0-标签语法.html</title>
</head>
<body>

	<!-- ①标签不能交叉嵌套 -->
	正确：<div><span>早安，尚硅谷</span></div>
	错误：<div><span>晚安，尚硅谷</div></span>
	<hr />

	<!-- ②标签必须正确关闭 -->
	<!-- i.有文本内容的标签： -->
	正确：<div>早安，尚硅谷</div>
	错误：<div>晚安，上柜
	<hr />
	
	<!-- ii.没有文本内容的标签： -->
	正确：<br />
	错误：<hr>
	<hr />
	
	<!-- ③属性必须有值，属性值必须加引号 -->
	正确：<font color="blue">早安，尚硅谷</font>
	错误：<font color=aqua>晚安，哒哒哒</font>
	错误：<font color=>滴滴滴</font>
	<hr />
		
	<!-- ④注释不能嵌套 -->
	正确：<!-- 注释内容 --> <br/>
	错误：<!-- <!---->-->
	<hr />
</body>
</html>
```

# 8.常用标签的介绍

## 8.1 font字体标签

**需求1：在网页显示  我是字体标签，并修改字体为 黑体， 颜色为红色。**

```html
<body>

		<!-- 字体标签

		需求1：在网页上显示我是字体标签，并修改字体为宋体，颜色为红色。
		font标签：是字体标签，它可以修改文本的字体、颜色、大小（尺寸）
			color:颜色
			face:字体
			size:大小 1-7

		 -->
		<font color="red" face="宋体" size="7">我是字体标签</font>
	</body>
```

## 8.2 特殊字符

**常用特殊字符表：**

![image-20210408222027477](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210408222027477.png)

**其他特殊字符表**：

![image-20210408222049109](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210408222049109.png)

**需求1：把`<br>` 换行标签变成文本转换成字符显示在页面上**

```html
<body>
	
	<!-- 特殊字符

	需求1：把<br> 换行标签变成文本转换成字符显示在页面上
	<   -----》   &lt;
	>   -----》	  &gt;
	空格 -----》   &nbsp;
	-->

	我是&lt;br&gt;标签


	
</body>
```

## 8.3 标题标签

+ **标题标签 是 h1 到 h6**
+ h1最大，h6最小
+ 标题标签中的属性：align属性是**对齐属性**
  + left：左对齐（默认）
  + right：右对齐
  + center：居中

**需求1：演示 标题1 到标题6**

```html
<body>
	
	<!-- 标题标签
	需求1：演示 标题1 到标题6
	h1 - h6
	h1最大,h6最小
	align:
			left	左对齐（默认）
			right	右对齐
			center	居中
	-->
	<h1>标题1</h1>
	<h2>标题2</h2>
	<h3>标题3</h3>
	<h4>标题4</h4>
	<h5>标题5</h5>
	<h6>标题6</h6>
	<h7>标题7</h7>
</body>
```

## 8.4 超链接（重点）

在网页中所有点击之后可以跳转的内容都是超链接

```html
<body>

   <!-- a标签就是 超链接
      href属性：设置连接的地址
      target属性：设置跳转的方式
         _self: 直接在当前窗口跳转(默认)
         _blank: 打开一个新窗口，进行跳转
    -->

   <a href="http://www.baidu.com">百度</a>
   <a href="http://www.baidu.com" target="_self">百度_self</a>
   <a href="http://www.baidu.com" target="_blank">百度_blank</a>
</body>
```

## 8.5 列表标签

列表标签主要分为 有序列表和无序列表

**需求 1：使用无序，列表方式，把东北F4，赵四，刘能，小沈阳，宋小宝，展示出来**

```html
<body>

<!--
    需求 1：使用无序，列表方式，把东北F4，赵四，刘能，小沈阳，宋小宝，展示出来
    分为 ul(unorder list) 和ol(order list)
    li(list item)

-->
<!--无序列表 unorder list-->
<ul>
    <li>赵四</li>
    <li>刘能</li>
    <li>小沈阳</li>
    <li>宋小宝</li>
</ul>

<!--有序列表 order list-->
<ol>
    <li>赵四</li>
    <li>刘能</li>
    <li>小沈阳</li>
    <li>宋小宝</li>
</ol>
</body>
```

```
注意：只要跟浏览器有关的,或多或少都有一些兼容问题。
```

## 8.6 img标签

+ img标签是用来显示图片的
+ 主要的属性有src、width、height、alt
  + **src属性：表示图片的路径**
    + JavaSE阶段路径分为相对路径和绝对路径
      + **<span style='color:red;'>相对路径：相对于工程名下</span>**
      + **绝对路径：盘符：/目录/资源文件/文件**
    + JavaWeb阶段路径分为相对路径和绝对路径
      + 相对路径：
        + **<span style='color:red;'>`.`：表示当前文件所在的目录</span>**
        + **<span style='color:red;'>`..`：表示当前文件所在的目录的上一层目录</span>**
        + **<span style='color:red;'>`文件名`：表示当前文件所在目录的文件，相当于`./文件名` ，`./可以省略`</span>**
      + **<span style='color:red;'>绝对路径：`http:/ip:port/工程名/资源路径`</SPAN>**
  + width：表示图片的宽度
  + height：表示图片的高度
  + **<span style='color:red;'>alt：当图片找不到对应的路径时，显示的内容</span>**



**小练习：需求1，使用img标签显示一张美女的图片，并修改宽高和边框属性**

```html
<body> 
    <!--需求 1：使用 img 标签显示一张美女的照片。并修改宽高，和边框属性 
        img 标签是图片标签,用来显示图片
        src 属性可以设置图片的路径 
		width 属性设置图片的宽度 
		height 属性设置图片的高度 
		border 属性设置图片边框大小 
		alt 属性设置当指定路径找不到图片时,用来代替显示的文本内容
		在 JavaSE 中路径也分为相对路径和绝对路径.
			相对路径:从工程名开始算 
			绝对路径:盘符:/目录/文件名 
		在 web 中路径分为相对路径和绝对路径两种 
			相对路径: 
					. 表示当前文件所在的目录
					.. 表示当前文件所在的上一级目录 
					文件名 表示当前文件所在目录的文件,相当于 ./文件名 ./ 可以省略
			绝对路径: 正确格式是: http://ip:port/工程名/资源路径 
			错误格式是: 盘符:/目录/文件名 
		-->
    <img src="../imgs/0.jpg" alt="图片加载失败" border="1" />
    <img src="2.jpg" alt="图片加载失败"  />
    <img src="../1.jpg" alt="图片加载失败"  />
</body>
```

## 8.7 table标签（重点,必须掌握）

+ table标签是单元格标签
+ **table标签常用的属性：**
  + width：设置表格宽度
  + height：设置表格高度
  + border：设置边框宽度
  + align：设置表格相对于页面的对齐方式
  + cellspacing：设置单元格间距
+ **<span style='color:red;'>table标签所涉及的标签：</span>**
  + tr：行标签
  + td：单元格标签
  + th：表头标签

**小练习：**需求 1：做一个 带表头的 ，三行，三列的表格，并显示边框 

​			   需求 2：修改表格的宽度，高度，表格的对齐方式，单元格间距。

```html
<body>

<!--
    需求 1：做一个 带表头的 ，三行，三列的表格，并显示边框
    需求 2：修改表格的宽度，高度，表格的对齐方式，单元格间距。
-->
<table border="1" cellspacing="0" width="500" height="500" align="center">
    <tr>
        <th>1.1</th>
        <td>1.2</td>
        <td>1.3</td>
    </tr>
    <tr>
        <td>2.1</td>
        <td>2.2</td>
        <td>2.3</td>
    </tr>
    <tr>
        <td>3.1</td>
        <td>3.2</td>
        <td>3.3</td>
    </tr>
</table>

</body>
```

## 8.8 跨行跨列表格（次重点,必须掌握）

![image-20210410222217127](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210410222217127.png)

+ 跨行：rowspan属性
+ 跨列：colspan属性



**练习：新建一个五行，五列的表格，第一行，第一列的单元格要跨两列，第二行第一列的单元格跨两行，第四行第四 列的单元格跨两行两列**

```html
<body>
<!--

新建一个五行，五列的表格，
第一行，第一列的单元格要跨两列，
第二行第一列的单元格跨两行，
第四行第四 列的单元格跨两行两列
-->
<table width="500" height="500" cellspacing="0" border="1">
    <tr>
        <td colspan="2">1.1</td>
        <td>1.3</td>
        <td>1.4</td>
        <td>1.5</td>
    </tr>
    <tr>
        <td rowspan="2">2.1</td>
        <td>2.2</td>
        <td>2.3</td>
        <td>2.4</td>
        <td>2.5</td>
    </tr>
    <tr>
        <td>3.2</td>
        <td>3.3</td>
        <td>3.4</td>
        <td>3.5</td>
    </tr>
    <tr>
        <td>4.1</td>
        <td>4.2</td>
        <td>4.3</td>
        <td colspan="2" rowspan="2">4.4</td>
    </tr>

    <tr>
        <td>5.1</td>
        <td>5.2</td>
        <td>5.3</td>
    </tr>

</table>
</body>
```

## 8.9 了解iframe框架标签(内嵌窗口)

iframe标签它可以在一个html页面上，打开一个小窗口，去加载一个单独的页面

**现在有一个需求：当点击超链接时，会在iframe标签中显示对应超链接的内容。**

**<span style='color:red;'>iframe和a标签的组合使用步骤</span>**：

​	1.在iframe标签中使用name属性定义一个名称

​    2.在a标签的target属性中设置iframe的name属性值

```html
<body>
	我是html页面<br/><br/>
    <iframe  width="500" height="500" name="myiframe" ></iframe>
    <ul>
        <li><a href="0-标签语法.html" target="myiframe">0-标签语法.html</a></li>
        <li><a href="2.特殊字符.html" target="myiframe">2.特殊字符.html</a></li>
        <li><a href="9.列表标签.html"   target="myiframe">9.列表标签.html</a></li>
    </ul>
</body>
```

## <span style='color:red;'>8.10、表单标签（重点，必须掌握）</span>

表单就是html页面中用来收集用户信息的所有元素集合。然后把这些数据发送给服务器。

![image-20210411122828876](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210411122828876.png)

### 8.10.1 表单的显示

form标签就是表单

+ `input type=text `  是文本输入框  `value`设置默认显示的内容
+ `input type=password` 是密码输入框 `value属性`设置默认显示内容
+ `input type=radio`  是单选框  `name属性`可以对其进行分组  `checked="checked"`表示默认选中
+ `input type=checkbox` 是复选框 `name属性`可以对其进行分组 `checked="checked"`表示默认选中
+ `select` 是下拉列表框 `option`是是下拉列表框的选项  `selected="selected"`表示默认选中
+ `input type=textarea` 多行文本输入框(起始标签和结束标签中的内容是默认值)
  + rows属性：设置多行文本框的行数
  + cols属性：设置多行文本框的列数宽度
+ `input type=reset` 重置按钮 `value属性`修改按钮上的文本
+ `input type=submit` 提交按钮 `value属性`修改按钮上的文本
+ `input type=button` 按钮  `value属性`修改按钮上的文本
+ **`input type=hidden` <span style='color:red;'>隐藏文本 当我们发送某些信息，并且不希望用户参与进来，就可以使用隐藏域(提交表单的同时会发送给服务器)</span>**
+ **`input type=file`<span style='color:red;'> 文件上传域</span>**

```html
<body>
<form action="http://localhost:8080" method="get">
    <input type="hidden" name="hidden" value="abc">
    <table>
        <tr>
            <td>用户名称:</td>
            <td><input type="text" name="name" value="默认值"/></td>
        </tr>
        <tr>
            <td>用户密码:</td>
            <td><input name="pwd" type="password"/></td>
        </tr>

        <tr>
            <td>确认密码:</td>
            <td><input name="confirmPwd" type="password"/></td>
        </tr>

        <tr>
            <td>选择性别:</td>
            <td>
                <input type="radio" name="sex" value="男"/>男<input type="radio" name="sex" value="女" />女
            </td>
        </tr>

        <tr>
            <td>选择爱好:</td>
            <td>
                <input type="checkbox" name="hobby" value="java"/>Java
                <input type="checkbox" name="hobby" value="js"/>JavaScript
                <input type="checkbox" name="hobby" value="cpp"/>C++
            </td>
        </tr>
        <tr>
            <td>选择国家:</td>
            <td>
                <select name="country">
                    <option value="none">--请选择国家--</option>
                    <option value="CN">中国</option>
                    <option value="usa">美国</option>
                </select>
            </td>
        </tr>

        <tr>
            <td>自我评价:</td>
            <td>
                <textarea>aaa</textarea>
            </td>
        </tr>

        <tr>
            <td><input type="reset"/></td>
            <td><input type="submit"/></td>
        </tr>
    </table>
    <input type="file">
</form>
</body>
```

### 8.10.2 表单提交细节

+ form标签是表单标签
  + **<span style='color:red;'>action：设置提交的服务器地址</span>**
  + method：设置提交的方式，GET(默认)和POST
+ **<span style='color:red;'>表单提交数据时,数据没有发送给服务器的三种情况：</span>**
  1. 表单中标签没有提供`name属性`
  2. 单选、复选(下拉列表中的option标签) 都需要添加`value属性`,以便发送给服务器
  3. 标签没有写在表单标签中
+ **<span style='color:red;'>GET请求的特点：</span>**
  1. 浏览器的地址栏中的地址是： action属性 + ? + 参数列表
  2. GET请求不安全 
  3. 它有数据长度的限制
+ **<span style='color:red;'>POST请求的特点：</span>**
  1. 浏览器的地址栏中的地址是：action属性
  2. 相对于GET请求 安全
  3. 理论上没有数据长度的限制



## 8.11 其他标签

+ div：默认会占1行
+ span：span标签的长度就是封装的数据的长度
+ p：默认会在段落的上方或下方各空出一行(如果已有空行就不再空行)

**练习：需求1，div、span、p标签的演示**

```html
<body>
<div>div标签1</div>
<div>div标签2</div>

<span>span标签1</span>
<span>span标签2</span>

<p>p段落标签1</p>
<p>p段落标签2</p>
</body>
```



# 9.CSS技术

## 9.1 CSS技术介绍

CSS是层叠样式表单。是用于(增强)控制网页样式并允许将样式信息与网页内容分离的一种标记语言。

## 9.2 CSS语法规则

![image-20210411183035806](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210411183035806.png)

+ **选择器：**浏览器根据“选择器”来决定受CSS样式影响的HTML元素（标签）。
+ **属性：**是你要改变的样式名的属性，并且每个属性都有一个值。属性和值用冒号隔开，并由花括号括起来，这样就组成了一个完整的样式声明(declaration)，例如：p{color :  red}
+ **多个声明：**如果要定义的不止一个声明，则需要用分号将每个声明隔开。尽量在每个声明的末尾加上分号
+ **例如：**

```css
div{
	color:red;
}
```

CSS注释： /* 要注释的内容 */



## 9.3 CSS的三种引入方式

### 9.3.1 第一种

在标签的style属性设置“key:value”，修改标签样式

**需求1：分别定义两个div、span标签，分别修改每个div的标签样式为：边框1个像素，实线、红色。**

```html
<body>
<!--
    需求1：分别定义两个div、span标签，分别修改每个div的标签样式为：边框1个像素，实线、红色。

-->
<div style="border: 1px solid red">div标签1</div>
<div style="border: 1px solid green">div标签2</div>

<span style="border: 1px solid yellow">span标签1</span>
<span>span标签2</span>
</body>
```

问题：这种方式的缺点。

1.如果标签多了。样式多了。代码量非常庞大。 

2.可读性非常差。 

3.Css 代码没什么复用性可方言。

### 9.3.2 第二种

**<span style='color:red;'>在head标签中，使用style标签来定义各种自己需要的css样式</span>**

格式如下：

```css
xxx｛
		key：value value;
｝
```

**需求1：分别定义两个div、span标签，分别修改每个div的标签样式为：边框1个像素，实线、红色。**

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title> <!--style 标签专门用来定义 css 样式代码-->
    <style type="text/css"> /* 需求 1：分别定义两个 div、span 标签，分别修改每个 div 标签的样式为：边框 1 个像素，实线，红色。*/
    div {
        border: 1px solid red;
    }

    span {
        border: 1px solid red;
    } </style>
</head>
<body>
<!--
    需求1：分别定义两个div、span标签，分别修改每个div的标签样式为：边框1个像素，实线、红色。

-->
<div>div标签1</div>
<div>div标签2</div>

<span>span标签1</span>
<span>span标签2</span>
</body>
</html>
```

问题：这种方式的缺点。

	1. 只能在同一个页面中复用代码，不能在多个页面中复用css代码。
	2. 维护起来不方便，实际的项目中会有成千上万的页面，要到每个页面中去修改。工作量太大了。

### 9.3.3 第三种

**<span style='color:red;'>把css样式写成1个单独的css文件，再通过link表现引入</span>**

使用html的`<link rel="stylesheet" type="text/css" href=./divspan.css`标签，导入css样式文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title> <!--style 标签专门用来定义 css 样式代码-->
    <link href="divspan.css" rel="stylesheet" type="text/css">
</head>
<body>
<!--
    需求1：分别定义两个div、span标签，分别修改每个div的标签样式为：边框1个像素，实线、红色。

-->
<div>div标签1</div>
<div>div标签2</div>

<span>span标签1</span>
<span>span标签2</span>
</body>
</html>
```



## 9.4 CSS选择器

### 9.4.1 标签名选择器

**标签名选择器的格式：**

```css
标签名{
    属性：值;
}
```

标签名选择器，可以决定哪些标签被动的使用这个样式。

**相关练习：**

+ 需求 1：在所有 div 标签上修改字体颜色为蓝色，字体大小 30 个像素。边框为 1 像素黄色实线。 

  并且修改所有 span 标签的字体颜色为黄色，字体大小 20 个像素。边框为 5 像素蓝色虚线。

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>CSS选择器</title>
	<style type="text/css">
		div{
			border: 1px solid yellow;
			font-size: 30px;
			color:blue;
		}
		span{
			border: 5px dashed  blue;
			font-size: 20px;
			color: yellow;
		}
	</style>
</head>
<body>		
	
	
	<!-- 
	需求1：在所有div标签上修改字体颜色为蓝色，字体大小30个像素。边框为1像素黄色实线。
	并且修改所有span 标签的字体颜色为黄色，字体大小20个像素。边框为5像素蓝色虚线。
	 -->
	<div>div标签1</div>
	<div>div标签2</div>
	<span>span标签1</span>
	<span>span标签2</span>
	
</body>
</html>
```



### 9.4.2 id选择器

id选择器，可以让我们通过id属性选择性的使用样式。

**格式：**

```css
#id{
    属性：值
}
```

**小练习：**

需求 1：分别定义两个 div 标签， 

第一个 div 标签定义 id 为 id001 ，然后根据 id 属性定义 css 样式修改字体颜色为蓝色， 

字体大小 30 个像素。边框为 1 像素黄色实线。 

第二个 div 标签定义 id 为 id002 ，然后根据 id 属性定义 css 样式 修改的字体颜色为红色，字体大小 20 个像 

素。边框为 5 像素蓝色点线

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="UTF-8">
	<title>ID选择器</title>
	<style type="text/css">
		#id001{
			color:blue;
			font-size: 30px;
			border: 1px solid yellow;
		}
		#id002{
			color: red;
			font-size: 20px;
			border: 5px dotted blue;
		}
	</style>
</head>
<body>		
	
	<!-- 
	需求1：分别定义两个 div 标签，
	第一个div 标签定义 id 为 id001 ，然后根据id 属性定义css样式修改字体颜色为蓝色，
	字体大小30个像素。边框为1像素黄色实线。
	
	第二个div 标签定义 id 为 id002 ，然后根据id 属性定义css样式 修改的字体颜色为红色，字体大小20个像素。边框为5像素蓝色点线。
	 -->
	
	<div id="id001">div标签1</div>
	<div id="id002">div标签2</div>
	
</body>
</html>
```



### 9.4.3 class选择器

class类型选择器，可以通过class属性有选择的使用某个样式。

**格式：**

```css
.class属性值{
    属性：值
}
```

**小练习：**

需求 1：修改 class 属性值为 class01 的 span 或 div 标签，字体颜色为蓝色，字体大小 30 个像素。边框为 1 像素黄色实线。 

需求 2：修改 class 属性值为 class02 的 div 标签，字体颜色为灰色，字体大小 26 个像素。边框为 1 像素红色实线。

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>class类型选择器</title>
    <style type="text/css">
        .class01 {
            color: blue;
            font-size: 30px;
            border: 1px yellow solid;
        }

        .class02 {
            color: grey;
            font-size: 26px;
            border: 1px solid red;
        }
    </style>
</head>
<body>

<!-- 
    需求1：修改 class 属性值为 class01的 span 或 div 标签，字体颜色为蓝色，字体大小30个像素。边框为1像素黄色实线。
    需求2：修改 class 属性值为 class02的 div 标签，字体颜色为灰色，字体大小26个像素。边框为1像素红色实线。
 -->

<div class="class01">div标签class01</div>
<div class="class02">div标签</div>
<span class="class01">span标签class01</span>
<span>span标签2</span>

</body>
</html>
```



### 9.4.4 组合选择器

组合选择器可以让多个选择器共用一个css样式

**格式：**

```css
选择器1,选择器2,选择器3{
    属性：值
}
```

**练习：**需求 1：修改 class=*"class01"* *的* div 标签 和 id=*"id01"* 所有的 span 标签，字体颜色为蓝色，字体大小 20 个像素。 边框为 1 像素黄色实线。

```HTML
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>class类型选择器</title>
    <style type="text/css">
        .class01, #id01 {
            font-size: 20px;
            border: 1px solid yellow;
            color: blue;
        }
    </style>
</head>
<body>

<!-- 
需求1：修改 class="class01" 的div 标签 和 id="id01" 所有的span标签，
字体颜色为蓝色，字体大小20个像素。边框为1像素黄色实线。
 -->

<div class="class01">div标签class01</div>
<br/>
<span id="id01">span 标签</span> <br/>
<div>div标签</div>
<br/>
<div>div标签id01</div>
<br/>

</body>
</html>
```

## 9.5 CSS常用样式

1、字体颜色 

color：red； 

颜色可以写颜色名如：black, blue, red, green 等 

颜色也可以写 rgb 值和十六进制表示值：如 rgb(255,0,0)，#00F6DE，如果写十六进制值必 

须加#2、宽度

width:19px; 

宽度可以写像素值：19px； 

也可以写百分比值：20%； 

3、高度

height:20px; 

高度可以写像素值：19px； 

也可以写百分比值：20%； 

4、背景颜色 

background-color:#0F2D4C 

4、字体样式： 

color：#FF0000；字体颜色红色 

font-size：20px; 字体大小 

5、红色 1 像素实线边框 

border：1px solid red; 

7、DIV 居中 

margin-left: auto; 

margin-right: auto; 

8、文本居中： 

text-align: center;9、超连接去下划线 

text-decoration: none; 

10、表格细线 

table { 

border: 1px solid black; /*设置边框*/ 

border-collapse: collapse; /*将边框合并*/ 

}

td,th {

border: 1px solid black; /*设置边框*/ 

} 

11、列表去除修饰 

ul { 

list-style: none; 

}