# 1.JavaScript介绍

+ JavaScript语言诞生主要是完成页面的数据验证。因此它运行在客户端，需要运行浏览器来解析执行JavaScript代码。
+ JS是Netscape网景公司的产品，最早取名为LivScript；为了吸引更多java程序员。更名为JavaScript。
+ **<span style='color:red;'>JS是弱类型，Java是强类型</span>**
+ 特点：
  + 交互性(它可以做的就是信息的动态交互)
  + 安全性(不允许直接访问本地磁盘)
  + 跨平台性(只要是可以解释JS的浏览器都能执行，和平台无关)

# 2.JavaScript与html代码的结合方式

## 2.1 第一种方式

只需要在`head标签`或者`body标签`中 使用`script标签`来书写JavaScript代码

**示例代码**

```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript">
        // alert 是 JavaScript 语言提供的一个警告框函数。 
        // 它可以接收任意类型的参数，这个参数就是警告框的提示信息
        alert("hello JavaScript");
    </script>
</head>
```

## 2.2 第二种方式

使用script标签的src属性引入 单独的JavaScript代码文件

**文件结构：**

![image-20210412184542106](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210412184542106.png)

**html代码内容**

```html
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <script type="text/javascript" src="1.js"></script>
</head>
```

**<span style='color:red;'>需要注意</span>**：script标签可以用来定义js代码,也可以用来引入js文件

​					但是，两个功能二选一使用。不能同时使用这两个功能。

# 3.变量

+ **变量的定义**：变量是可以存放某些值的内存的命名。
+ **JavaScript的变量类型：**
  + 数值类型：number
  + 字符串类型：String
  + 布尔类型：boolean
  + 对象类型：object
  + 函数类型：function
+ **JavaScript里特殊的值：**
  + undefined：只声明，未初始化值时。
  + null：当变量赋给一个null时。
  + **NAN：Not a Number ，非数字、非数值。**
+ JS中定义变量格式：

`var 变量名;`

`var 变量名 = 值 ;`

**示例代码**

```javascript
 <script type="text/javascript">
        var i ;
        //typeof是JS提供的一个函数，主要判断变量的类型
        //alert(typeof(i));//undefined

        //i = 12;
        //alert(typeof(i))//number

        //i = "abc";
        //alert(typeof(i))//string

        var a = 12;
        var b = "abc";
        alert(a * b)//NAN
        
    </script>
```

# 4.关系(比较)运算符

+ **<span style='color:red;'>==：只做字面值的比较</span>** 
+ **<span style='color:red;'>===：除了字面值的比较外，还比较两个变量的数据类型</span>**

**示例代码**

```js
    <script type="text/javascript">
        var a = 12;
        var b = "12";
        alert(a == b);

        alert(a === b);
    </script>
```

# 5.逻辑运算符

+ 逻辑且：&&
  + **有两种情况：**
  + **<span style='color:red;'>第一种：当表达式全为真时，返回最后一个表达式的值</span>**
  + **<span style='color:red;'>第二种：当表达式中有一个为假时，返回第一个为假的表达式的值</span>**
+ 逻辑或：||
  + **有两种情况：**
  + **<span style='color:red;'>第一种：当表达式全为假时，返回最后一个表达式的值</span>**
  + **<span style='color:red;'>第二种：当表达式中有一个为真时，就会返回第一个为真的表达式的值。</span>**
+ 取反运算：！
+ 在JavaScript语言中,所有的变量，都可以作为一个boolean类型的变量去使用。
+ **<span style='color:red;'>0、null、undefined、“”(空串) 都认为是false；</span>**

**代码示例**

```js
<script type="text/javascript">
        var i = 0;
        // if (i){
        //     alert("零为真")
        // }else{
        //     alert("零为假")
        // }

        i = null;
        // if (i){
        //     alert("null为真")
        // }else{
        //     alert("null为假")
        // }

        i = undefined;
        // if (i){
        //     alert("undefined为真")
        // }else{
        //     alert("undefined为假")
        // }

        i = "";
        // if (i){
        //     alert("空串为真")
        // }else{
        //     alert("空串为假")
        // }


        var a = "abc";
        var b = true;
        var d = false;
        var c = null;
        /*
        && 且运算。 有两种情况：
                    第一种：当表达式全为真的时候。返回最后一个表达式的值。
                    第二种：当表达式中，有一个为假的时候。返回第一个为假的表达式的值

        */
        // alert(a && b);
        // alert(d && c);//都为假时,返回false

        // alert(a && c);// null
        // alert(c && a);// null

        /*
        || 或运算
            第一种情况：当表达式全为假时，返回最后一个表达式的值
            第二种情况：只要有一个表达式为真。就会把回第一个为真的表达式的值

        */
        // alert(d || c);//null，都为假时，返回最后一个为假的表达式值
        // alert(d || a);//abc

        alert(a || b);//abc，都为真时,返回第一个为真的表达式值

    </script>
```

# <span style='color:red;'>6.数组（重点）</span>

## 6.1 数组的定义

**在JS中数组定义的格式：**

```js
var 数组名 = []; // 空数组
var 数组名 = [1,true,'abc'];//定义数组的同时初始化
```

**<span style='color:red;'>注意：JS语言中的数组，只要我们通过数组下标赋值，那么最大的下标值,就会自动的给数组做扩容操作；只有赋值的时候可以,读数组的时候不会扩容。</span>**

**代码示例**

```js
<script type="text/javascript">

        //数组定义
        var arr = [];//空数组
        // alert(arr.length)//0

        arr[0] = 1;
        // alert(arr[0]) //1

        arr[2] = "abc";
        // alert(arr.length)//3

        //数组的遍历
        for (var i = 0 ;i <arr.length;i++){
            alert(arr[i])
        }
    </script>
```

# <span style='color:red;'>7.函数（重点）</span>

## 7.1 函数的两种定义方式

### 7.1.1 函数定义的第一种方式

可以使用function关键字来定义函数

**格式如下：**

```js
function 函数名(形参列表){
    函数体
}
```

**示例代码：**

```html
 <script type="text/javascript">

        function fun() {
            alert("无参函数被调用了..")
        }

        // fun();

        function fun1(a,b) {
            alert("有参函数被调用了.. a = " + a +",b = " + b);
        }

        // fun1(1,2);

        function fun2(num1,num2){
            return num1 + num2;
        }
        alert(fun2(100,200));
    </script>
```

### 7.1.2 函数定义的第二种方式

**格式：**

```js
var 函数名 = function(形参列表){
    函数体
}
```

**代码示例**

```js
<script type="text/javascript">

        var fun = function(){
            alert("无参函数被调用!!");
        }


        var fun2 = function (num1, num2) {
            alert(num1 + num2);
        }

        fun();

        fun2(1,2);

    </script>
```

```
注意：在Java中方法是可以重载的，但是在JS中函数不能重载。
```

## 7.2 函数的arguments隐形参数(只在function函数内)

在function函数的形参列表中不需要定义，但却可以直接用来获取所有参数的变量。我们管它叫隐形参数。隐形参数特别像java基础的可变数组。

**代码示例**

```JS
//需求：要求 编写 一个函数。用于计算所有参数相加的和并返回	
<script type="text/javascript">
    
        function fun(num1,num2){
            var result = 0;
            //遍历隐形参数数组
            for(var i = 0;i<arguments.length;i++){
                if (typeof(arguments[i]) =="number"){
                      result += arguments[i];
                }
            }
            return result;
        }


        alert(fun(1,2,3,4,5,6,7,8,"abc",9,10));
    </script>
```

# 8.JS中的自定义对象(扩展内容)

## 8.1 Object形式的自定义对象

**对象的定义：**

```js
var 变量名 = new Object();
变量名.属性名 = 值; 
变量名.函数名 = function(形参列表){函数体}
```

**对象的访问：**

```js
变量名.属性/函数名();
```

**代码示例**

```js
 <script type="text/javascript">

        // 对象的定义
        var obj = new Object();
        obj.name = "华仔";
        obj.age = 18;
        obj.show = function(){
            alert("name = " + this.name + ",age = " + this.age);
        }
        // 对象的访问
        alert(obj.name);
        alert(obj.age);

        //obj.show()


    </script>
```



## 8.2 {}花括号形式的自定义对象

**对象的定义：**

```js
var 变量名 = {
    属性名1：值1,
    属性名2：值2,
    函数名：function(形参列表){
        函数体
    }
}
```

**对象的访问：**

```
变量名.属性名/函数名();
```

**示例代码**

```JS
<script type="text/javascript">

        //对象的定义
        var obj = {
            name:"文博",
            age:25,
            eat:function(){
                alert("吃饭很积极");
            }
        }

        //对象的访问
        alert(obj.name);
        alert(obj.age);
        obj.eat()
</script>
```

# 9.JS中的事件

什么是是事件？事件是电脑输入设备与页面交互的响应，这就叫做事件。

**常用的事件**

+ onload加载完成事件：页面加载完成之后,常用于做页面JS代码初始化操作
+ onclick：常用于按钮的点击事件
+ onblur：常用于输入框失去焦点后验证其输入内容是否合法。
+ onchange：常用于下拉列表或者输入框内容改变后的操作
+ onsubmit：常用于**表单提交前**，验证所有表单项是否合法。

**事件注册又分为静态注册和动态注册两种**

+ 静态注册：直接在html标签的事件属性赋予事件响应后的代码。
+ 动态注册：先通过JS代码获取到标签的dom对象,在通过dom对象.事件名 = function(){}，这种形式赋予事件响应后的代码，叫动态注册。
  + 动态注册的基本步骤：
    1. 获取标签对象
    2. 标签对象.事件名 = function(){}

## 9.1 onload事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>onload事件</title>
    <script type="text/javascript">

        //onload动态注册方式
        window.onload = function () {
            alert("onLoad动态注册方式")
        }

        function onloadStaticMethod() {
            alert("onload静态~~")
        }
    </script>
</head>
<!--
    onload 静态注册：
    <body onload="onloadStaticMethod()">

-->
<body>
    <h1>hello</h1>
</body>
</html>
```



## 9.2 onClick事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>onclick事件</title>
    <script type="text/javascript">
        // function onclickFun(){
        //     alert("onclick事件的静态注册~")
        // }

        window.onload = function(){
            //获取按钮标签对象
            /*
            document:是js提供的一个对象,是整个页面的对象
            getElementById():通过id获取元素对象
            */
            //1.获取按钮标签对象
            var btnObj = document.getElementById("btn01");
            //2.通过标签对象.事件名
            btnObj.onclick = function(){
                alert("onclick事件的动态注册~")
            }
        }
    </script>
</head>
<body>
<!--onclick事件的静态注册-->
<button onclick="onclickFun()">按钮1</button>
<!--onclick事件的动态注册-->
<button id="btn01">按钮2</button>
</body>
</html>
```



## 9.3 onBlur失去焦点事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>onblur事件</title>
    <script type="text/javascript">

        function onblurFun(){
            console.log("onblur的静态注册！");
        }


        window.onload = function () {
            //1.获取password文本框对象
            var inputObj = document.getElementById("password");
            inputObj.onblur = function(){
                console.log("onblur的动态注册~~")
            }
        }
    </script>
</head>
<body>
<!--onblur事件的静态注册-->
用户名:<input type="text" onblur="onblurFun()"/><br/>
<!--onblur事件的动态注册-->
密码：<input id="password" type="text"/>
</body>
</html>
```

## 9.4 onchange内容发生改变事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>onchange事件</title>
    <script type="text/javascript">
        function onchangeFun(){
            alert("女神已经发生改变了")
        }


        window.onload = function(){

            //1.获取select标签对象
            var selObj = document.getElementById("sel01");
            // alert(selObj)
            selObj.onchange = function(){
                alert("男神已经发生改变了");
            }
        }
    </script>
</head>
<body>

<!--onchange事件的静态注册-->
<select onchange="onchangeFun()">
    <option>--请选择女神--</option>
    <option>白冰</option>
    <option>高圆圆</option>
    <option>白百何</option>
</select>

<!--onchange事件的动态注册-->
<select id="sel01">
    <option>--请选择男神--</option>
    <option>梁朝伟</option>
    <option>欧豪</option>
    <option>蔡徐坤</option>
</select>

</body>
</html>
```

## 9.5 onsubmit事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>onsubmit事件</title>
    <script type="text/javascript">
        function onsubmitFun(){
            alert("onsubmit事件的静态注册...")


            return false;
        }

        window.onload = function(){

            //1.获取表单标签的对象
            var formObj = document.getElementById("form01");
            // alert(formObj)
            //2.对象.onsubmit事件
            formObj.onsubmit = function () {

                alert("onsubmit事件的动态注册.. 不合法表单。禁止提交")

                return false;
            }
        }
    </script>
</head>
<body>

<!--onsubmit事件的静态注册方法-->
<form action="http://localhost:8080" onsubmit="return onsubmitFun()">
    <input type="submit" value="静态注册">
</form>

<!--onsubmit事件的动态注册方法-->
<form action="http://localhost:8080" id="form01">
    <input type="submit" value="动态注册">
</form>
</body>
</html>
```

# <span style='color:red;'>10.DOM 模型(重点)</span>

## <span style='color:red;'>10.1Document对象(重点)</span>

![image-20210413183929822](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210413183929822.png)

**<span style='color:red;'>Document对象的理解：</span>**

+ 第一点：Document管理了所有的HTML文档内容
+ 第二点：document它是一种树结构的文档。有层级关系。
+ 第三点：它让我们把所有的标签都对象化
+ 第四点：我们可以通过document访问所有的标签对象。

**什么是对象化：**

举例：有一个人有年龄、性别、名字

我们要把这个人的信息对象化就需要这样做：

```
Class Person{
	String name;
	int age;
	String sex;
}
```

那么html标签要**对象化**怎么办？

```html
<body> 
    <div id="div01">div01</div> 
</body>
```

```java
class Dom{
    private String id;//id属性
    private String tagName;//标签名
    private Dom parentNode;//父类结点
    private List<Dom> childNode;//孩子结点
    private String innerHTML;//起始标签和结束标签中间的文本
}
```

## 10.2 正则表达式对象

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>正则表达式</title>
    <script type="text/javascript">


        // var patt = new RegExp('e');
        // 表示 要求字符串中至少包含字母e
        // var patt = /e/;

        // 表示 要求字符串中包含a或b或c的字母
        // var patt = /[abc]/;

        // 表示 要求字符串中不包含a或b或c的字母
        // var patt = /[^abc]/;

        // 表示 要求字符串中包含小写字母即可。
        // var patt = /[a-z]/;

        // 表示 要求字符串中包含大写字母即可。
        // var patt = /[A-Z]/;

        // 表示 要求字符串由 数字、字母、下划线组成
        // var patt = /\w/;

        // 表示 要求字符串中 至少有一个a
        // var patt = /a+/;

        // 表示 要求字符串中 包含零个 或者 多个a
        // var patt = /a*/;

        // 表示 要求字符串中 包含零个 或者 1个a
        // var patt = /a?/;

        // 表示 要求字符串中至少包含3个连续a
        // var patt = /a{3}/;

        // 表示 要求字符串中包含3个a或者 5个a时为true
        var patt = /^a{3,5}$/
        var str = "aaaaa";

        alert(patt.test(str))
    </script>
</head>
<body>

</body>
</html>
```



## 10.3 Document对象中的方法介绍

+ **document.getElementById(elementId)**：通过标签的id属性查找标签dom对象,elementId是标签的id属性值
+ **document.getElementsByName(elementName)**：通过标签的name属性查找标签dom对象,elementName标签的name属性值
+ **document.getElementsByTagName(tagName)：**通过标签名查找标签dom对象。tagName是标签名
+ **document.createElement( tagName)**：通过给定的标签名，创建一个标签对象。tagName是要创建的标签名

**<span style='color:red;'>注意</span>**：

```
document 对象的三个查询方法，如果有 id 属性，优先使用 getElementById 方法来进行查询 

如果没有 id 属性，则优先使用 getElementsByName 方法来进行查询 

如果 id 属性和 name 属性都没有最后再按标签名查 getElementsByTagName 

以上三个方法，一定要在页面加载完成之后执行，才能查询到标签对象
```





### 10.3.1 getElementById方法示例代码

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>getElementById</title>
    <script type="text/javascript">
        //1.给校验按钮绑定点击事件
        function onclickFun() {
            //2.获取输入框的对象
            var inputObj = document.getElementById("username");
            //3.获取输入框中输入的内容
            var inputVal = inputObj.value;
            //4.进行校验
            var patt = /^\w{5,12}$/;

            //5.获取span标签对象
            var spanObj = document.getElementById("usernameSpan");

            if (patt.test(inputVal)) {
                // alert("输入的内容合规")
                // spanObj.innerHTML = "输入的内容合规";
                spanObj.innerHTML = "<img src=\"right.png\" width=\"18\" height=\"18\">"
            } else {
                // alert("输入的内容不合规");
                // spanObj.innerHTML = "输入的内容不合规";
                spanObj.innerHTML = "<img src=\"wrong.png\" width=\"18\" height=\"18\">";
            }
        }

    </script>
</head>
<body>
<!--需求：对输入框的内容进行校验，要求：数字、字母、下划线组成,并且长度5 ~ 12-->
用户名：<input id="username" type="text"/>
<span id="usernameSpan" style="color:red">
    <img src="right.png" width="18" height="18">
</span><br/>
<button onclick="onclickFun()">校验</button>
</body>
</html>
```

### 10.3.2 getElementsByName方法示例代码

```html
<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/html">
<head>
    <meta charset="UTF-8">
    <title>getElementsByName</title>
    <script type="text/javascript">
        function checkAll() {
            //1.获取hobby的标签对象
            var hobbys = document.getElementsByName("hobby");
            // alert(hobbys)//NodeList
            // alert(hobbys.length)

            //2.遍历
            for (var i = 0;i < hobbys.length;i++){
                //3.设置每个checkbox的属性checked="checked"
                hobbys[i].checked = true;
            }
        }

        function checkNo() {
            //1.获取name="hobby"的标签对象
            var hobbys = document.getElementsByName("hobby");
            //2.遍历hobbys
            for (var i = 0;i<hobbys.length;i++){
                hobbys[i].checked = false;
            }
        }

        function checkReverse() {
            //1.获取name="hobby"的标签对象
            var hobbys = document.getElementsByName("hobby");
            //2.遍历hobbys
            for (var i = 0; i < hobbys.length; i++) {

                hobbys[i].checked = !hobbys[i].checked;
            }
        }
    </script>
</head>
<body>
<input type="checkbox" name="hobby" value="java">java</input>
<input type="checkbox" name="hobby" value="js">js</input>
<input type="checkbox" name="hobby" value="vue">vue</input>

<button onclick="checkAll()">全选</button>
<button onclick="checkNo()">全不选</button>
<button onclick="checkReverse()">反选</button>
</body>
</html>
```

### 10.3.4 getElementsByTagName 方法示例代码

```html
<!DOCTYPE html>
<html lang="en" xmlns="http://www.w3.org/1999/html">
<head>
    <meta charset="UTF-8">
    <title>getElementsByName</title>
    <script type="text/javascript">
        function checkAll() {

            var inputs = document.getElementsByTagName("input");

            for (var i = 0; i < inputs.length; i++) {
                inputs[i].checked = true;
            }
        }
    </script>
</head>
<body>
<input type="checkbox" value="java">java</input>
<input type="checkbox" value="js">js</input>
<input type="checkbox" value="vue">vue</input>

<button onclick="checkAll()">全选</button>
</body>
</html>
```

## 10.4 节点的常用属性和方法

节点就是标签对象

### 10.4.1 常用方法

+ **getElementByTagName(tagName)：**当前节点(标签对象)调用该方法获取指定标签名的孩子节点
+ **appendChild(oChildNode)：**可以添加一个子节点，oChildNode就是要添加的孩子节点。

### 10.4.2 常用属性

+ childNodes属性：获取当前节点的所有孩子节点
+ firstChild属性：获取当前节点的第一个子节点
+ lastChild属性：获取当前节点的最后一个子节点
+ parentNode属性：获取当前节点的父节点
+ nextSibling属性：获取当前节点的下一个兄弟节点
+ previousSibling属性：获取当前接的上一个兄弟节点
+ className：获取或设置标签的class属性值
+ innerHTML：获取/设置起始标签和结束标签中的内容
+ innerText：获取/设置起始标签和结束标签中的文本

**练习**

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>dom查询</title>
    <link rel="stylesheet" type="text/css" href="style/css.css"/>
    <script type="text/javascript">
        window.onload = function () {
            //1.查找#bj节点
            document.getElementById("btn01").onclick = function () {
                var bjObj = document.getElementById("bj");
                alert(bjObj.innerHTML)
            }

            //2.查找所有li节点
            var btn02Ele = document.getElementById("btn02");
            btn02Ele.onclick = function () {
                var lis = document.getElementsByTagName("li");
                alert(lis.length)
            };
            //3.查找name=gender的所有节点
            var btn03Ele = document.getElementById("btn03");
            btn03Ele.onclick = function () {
                var genders = document.getElementsByName("gender");
                alert(genders.length)
            };
            //4.查找#city下所有li节点
            var btn04Ele = document.getElementById("btn04");
            btn04Ele.onclick = function () {
                //1.获取id=city的标签对象
                var cityLiObj = document.getElementById("city");
                //2.获取city下的所有li节点
                var lis = cityLiObj.getElementsByTagName("li");
                alert(lis.length)
            };
            //5.返回#city的所有子节点
            var btn05Ele = document.getElementById("btn05");
            btn05Ele.onclick = function () {
                //1.获取id=city的标签对象
                //2.通过标签对象.childNode
                var childNodes = document.getElementById("city").childNodes;
                alert(childNodes.length)
            };
            //6.返回#phone的第一个子节点
            var btn06Ele = document.getElementById("btn06");
            btn06Ele.onclick = function () {
                alert(document.getElementById("phone").firstChild.innerHTML);
            };
            //7.返回#bj的父节点
            var btn07Ele = document.getElementById("btn07");
            btn07Ele.onclick = function () {
                alert(document.getElementById("bj").parentNode.innerHTML);
            };
            //8.返回#android的前一个兄弟节点
            var btn08Ele = document.getElementById("btn08");
            btn08Ele.onclick = function () {
                alert(document.getElementById("android").previousSibling.innerHTML);
            };
            //9.读取#username的value属性值
            var btn09Ele = document.getElementById("btn09");
            btn09Ele.onclick = function () {
                alert(document.getElementById("username").value);
            };
            //10.设置#username的value属性值
            var btn10Ele = document.getElementById("btn10");
            btn10Ele.onclick = function () {
                document.getElementById("username").value = "qq";
                alert(document.getElementById("username").value)
            };
            //11.返回#bj的文本值
            var btn11Ele = document.getElementById("btn11");
            btn11Ele.onclick = function () {
                alert(document.getElementById("bj").innerText);
            };
        };
    </script>
</head>
<body>
<div id="total">
    <div class="inner">
        <p>
            你喜欢哪个城市?
        </p>

        <ul id="city">
            <li id="bj">北京</li>
            <li>上海</li>
            <li>东京</li>
            <li>首尔</li>
        </ul>

        <br>
        <br>

        <p>
            你喜欢哪款单机游戏?
        </p>

        <ul id="game">
            <li id="rl">红警</li>
            <li>实况</li>
            <li>极品飞车</li>
            <li>魔兽</li>
        </ul>

        <br/>
        <br/>

        <p>
            你手机的操作系统是?
        </p>

        <ul id="phone">
            <li>IOS</li>
            <li id="android">Android</li>
            <li>Windows Phone</li>
        </ul>
    </div>

    <div class="inner">
        gender:
        <input type="radio" name="gender" value="male"/>
        Male
        <input type="radio" name="gender" value="female"/>
        Female
        <br>
        <br>
        name:
        <input type="text" name="name" id="username" value="abcde"/>
    </div>
</div>
<div id="btnList">
    <div>
        <button id="btn01">查找#bj节点</button>
    </div>
    <div>
        <button id="btn02">查找所有li节点</button>
    </div>
    <div>
        <button id="btn03">查找name=gender的所有节点</button>
    </div>
    <div>
        <button id="btn04">查找#city下所有li节点</button>
    </div>
    <div>
        <button id="btn05">返回#city的所有子节点</button>
    </div>
    <div>
        <button id="btn06">返回#phone的第一个子节点</button>
    </div>
    <div>
        <button id="btn07">返回#bj的父节点</button>
    </div>
    <div>
        <button id="btn08">返回#android的前一个兄弟节点</button>
    </div>
    <div>
        <button id="btn09">返回#username的value属性值</button>
    </div>
    <div>
        <button id="btn10">设置#username的value属性值</button>
    </div>
    <div>
        <button id="btn11">返回#bj的文本值</button>
    </div>
</div>
</body>
</html>
```

