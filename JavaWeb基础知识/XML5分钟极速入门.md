# 1.XML简介

## 什么是XML？

xml是可扩展的标记性语言。

## xml的作用？

1. 用来保存数据，而且这些数据具有自我描述性
2. 它还可以作为项目或者模块的配置文件
3. 还可以作为网络传输数据的格式(现在以JSON为主)



# 2.xml语法

1. XML文件的注释格式与HTML相同：<!--注释内容-->
   每个xml文件第一行必须是：`<?xml version="1.0" encoding="UTF-8"?>`
   注意：
   ① <?xml中不能有空格，否则报错
   ② version代表版本号
   ③ encoding是XML的文件编码
   XML的标签、属性等用法与HTML一致
   XML中的大段文字可以使用文本区域(CDATA区)，此区域中的内容不会被解析，只当作纯文本
   语法：`<![CDATA[输入的内容]]>`

**代码示例**

```xml
<?xml version="1.0" encoding="utf-8" ?>
<books> <!--表示有多个book-->
    <book id="SN123"> <!--book标签描述一本书，id属性描述编号-->
        <!--以下标签描述图书的信息-->
        <name>时间简史</name>		
        <author>霍金</author>
        <price>23.6</price>
    </book>
    <book> <!--book标签用来描述另一本书-->
        <name>毛泽东语录</name>
        <author>毛泽东</author>
        <price>10.5</price>
    </book>
<![CDATA[<book>我不会被解析</book>]]>
</books>
```

# 3.DOM4J解析技术

XML文件和HTML文件都可以使用DOM技术来解析，DOM将XML文档作为一个树形结构，使 用DOM4J时需要到官网下载jar包`dom4j-1.6.1.jar`
DOM4J编程的使用
(1) 在当前的Moudle下新建libs文件夹，将上述jar包导入，右键Add as Library
(2) 创建一个需要解析的XML文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<books>
    <book sn="SN123">
        <name>白马啸西风</name>
        <price>33.5</price>
        <author>金庸</author>
    </book>
    <book sn="SN456">
        <name>汇编语言</name>
        <price>25.9</price>
        <author>王爽</author>
    </book>
</books>
```

(3)创建XML文件对应的Book类

```java
public class Book {
    private String name;
    private String sn;
    private Double price;
    private String author;
    //无参、全参构造器
    public Book(String name, String sn, Double price, String author) {
        this.name = name;
        this.sn = sn;
        this.price = price;
        this.author = author;
    }
    public Book() {
    }
    //各属性的getter/setter方法
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public String getSn() {
        return sn;
    }
    public void setSn(String sn) {
        this.sn = sn;
    }
    public Double getPrice() {
        return price;
    }
    public void setPrice(Double price) {
        this.price = price;
    }
    public String getAuthor() {
        return author;
    }
    public void setAuthor(String author) {
        this.author = author;
    }
    //toString方法
    @Override
    public String toString() {
        return "Book{" +
                "name='" + name + '\'' +
                ", sn='" + sn + '\'' +
                ", price=" + price +
                ", author='" + author + '\'' +
                '}';
    }
}
```

(4)遍历XML文件中所有标签的内容

```java
public class Test {
    @org.junit.Test
    public void readXMl() throws DocumentException {
        //1.创建SAXReader对象通过read方法读取XML文件
        SAXReader saxReader = new SAXReader();
        Document document = saxReader.read("src/books.xml");
        //2.通过Document对象的getRootElement方法得到XML文件的根元素对象
        Element rootElement = document.getRootElement();
        //3.通过Element中的elements(标签名)或element(标签名)获取当前元素下
        //  的指定子元素集合(加s)或子元素(不加s)
        List<Element> books = rootElement.elements("book");
        //4.遍历每个book标签对象，获取到book对象内的每一个子元素
        for (Element book: books) {
            /***********************************************************/
            //获取到book下的name元素
            Element name = book.element("name");
            //Element的getText方法获取标签中的文本内容
            String nameText = name.getText();
            /***********************************************************/
            //获取到book下的sn属性，Element的attributeValue方法可以获取属性
            String snValue = book.attributeValue("sn");
            //获取到book下的price元素，elementText方法直接获取指定标签中的文本内容
            String priceText = book.elementText("price");
            //获取到book下的author元素的内容
            String authorText = book.elementText("author");
            //使用得到的标签内容创建Book对象，并输出
            System.out.println(new Book(nameText,snValue,
                    Double.parseDouble(priceText),authorText));
        }
    }
}
```

运行结果：

![image-20210428230336370](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210428230336370.png)

