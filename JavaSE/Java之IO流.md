# 一、File类的使用

## 1.File类的理解

+ File类的一个对象，代表一个文件或一个文件目录（俗称：文件夹）

+ File类是声明在java.io包下的：文件和文件路径的抽象表现形式，与平台无关
+ 

## 2.File类的实例化

### 1.**构造方法**

```
// pathname：文件或者文件目录的路径
public File(String pathname) 
// parent：文件目录路径 
// child：文件目录或者文件路径
public File(String parent, String child)
// parent：文件目录的路径
// child：文件目录或者文件的路径
public File(File parent, String child)
```

**代码示例：**

```java
 @Test
    public void test(){
        /*
            public File(String pathname)
            public File(String parent, String child)
            public File(File parent, String child)

       */
        File file1 = new File("E:\\io\\hello.txt");
        File file2 = new File("hi.txt");
        System.out.println(file1);
        System.out.println(file2);

        File file3 = new File("E:\\io","kk.txt");
        System.out.println(file3);

        File file4 = new File("E:\\io");
        File file5 = new File(file4,"abc.txt");
        System.out.println(file5);
    }
```

### 2.路径的分类

+ 绝对路径：相较于某个路径下，指名的路径
+ 相对路径：包含盘符在内的文件或者文件目录的路径
  + IDEA中
    + 如果使用**JUnit**中的单元测试方法测试，**相对路径为当前moudle下**
    + 如果使用**main**方法测试，**相对路径为当前的Project下**
  + Eclipse中
    + **不管是在main方法还是单元测试，相对路径都是当前的project下。**

### 3.路径分隔符说明

+ **windows和DOS系统**默认使用**“\”**来表示
+ **UNIX和URL**使用**“/”**来表示
+ ==**Java支持跨平台，因此路径的分隔符需要慎用。**==
+ 为了解决这个隐患，File类提供了一个常量：**`public static final String separator`**。根据操作系统，Java会动态的提供分隔符

```java
// windows和DOS系统
File file = new File("D:\\io\\hello.txt");
// UNIX和URL环境
File file = new File("C:/io/test.txt");
// Java提供的分隔符常量
File file = new File("E:" + File.serpartor +"io" + File.serparator + "test.txt");
```

## 3.File类的常用方法

### 1.File类的获取功能

+ public  String  getAbsolutePath()：获取文件类对象的绝对路径
+ public  String  getPath()：获取文件类对象的路径
+ public  String  getName()：获取文件的名称
+ public  String  getParent()：获取文件类构造器中传入参数的上层文件目录路径。若无，返回null
+ public  Long  length()：获取文件长度（即：字节数）。不能获取目录的长度
+ public  Long lastModified()：获取最近一次的修改时间；返回毫秒值
+ public String[] list()：获取指定目录下的所有文件或者文件目录的名称数组
+ public File[] listFiles()：获取指定目录下的所有文件或者文件目录的File数组

**代码示例**

```java
 @Test
    public void test2(){
        File file = new File("E:\\io\\hello.txt");
        // 获取绝对路径
        System.out.println(file.getAbsolutePath());
        // 获取文件类构造器中参数的路径
        System.out.println(file.getPath());
        // 获取文件名称
        System.out.println(file.getName());
        // 获取文件构造器中参数的上层目录
        System.out.println(file.getParent());
        // 获取文件的长度
        System.out.println(file.length());
        // 获取文件最近的 修改时间
        System.out.println(new Date(file.lastModified()));

    }
```

### 2.File类的重命名功能

+ public boolean renameTo（File  dest）：把文件重命名到指定的文件路径
+ ==**注意`file1.renameTo(file2)`**==：要想重命名成功，需要两个条件：<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**① file1在硬盘中存在 ② file2在硬盘中不存在。**</span>

**代码示例**:

```java
@Test
    public void test3(){
        File file = new File("E:\\io\\abc.txt");
        File file1 = new File("rename.txt");
        boolean b = file.renameTo(file1);
        if (b){
            System.out.println("改名成功");
        }
    }
```

### 3.File类的判断功能

==**注意**==：这边的判断都是<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**针对文件是否存在于硬盘**</span>，而不是内存中（程序）

+ public boolean exists() ：判断是否存在
+ public boolean isDirectory()：判断是否为文件目录
+ public boolean isFile()：判断是否为文件
+ public boolean isHidden()：判断是否可隐藏
+ public boolean canRead()：判断是否可读
+ public boolean canWrite()：判断是否可写

**代码示例**

```java
@Test
    public void test4(){
        File file = new File("E:\\io\\abc.txt");
        //是否存在
        System.out.println(file.exists());
        // 是否可读
        System.out.println(file.canRead());
        // 是否可写
        System.out.println(file.canWrite());
        // 是否为文件目录
        System.out.println(file.isDirectory());
        // 是否为文件
        System.out.println(file.isFile());
        // 是否隐藏
        System.out.println(file.isHidden());
    }
```

### 4.File类的创建功能

+ public boolean mkdir()：创建文件目录。如果此文件目录存在，就不创建了。==**如果此文件目录的上层目录不存在，也不创建**==
+ public boolean mkdirs()：创建文件目录。如果此文件目录存在，就不创建了。==**如果此文件目录的上层目录不存在，就一并创建**==
+ public boolean createNewFile()：创建文件。若文件存在，则不创建，返回false

**代码示例**

```java
@Test
    public void test5() throws IOException {
        File file1 = new File("E:\\java\\io\\xx.txt");
        File file2 = new File("E:\\java\\io\\world.txt");
        boolean b = file1.mkdir();

        boolean newFile = file1.createNewFile();
        if (newFile){
            System.out.println("创建成功3");
        }
        if (b){
            System.out.println("创建成功1");
        }

        boolean b2 = file2.mkdirs();
        if (b2){
            System.out.println("创建成功2");
        }
    }
```



### 5.File类的删除功能

+ 删除磁盘中的文件或者文件目录
+ public boolean delete()：删除磁盘中的文件或者文件目录
+ ==**删除注意事项：Java中的这个delete删除方法不走回收站。**==

**代码示例**

```java
@Test
    public void test6() throws IOException {
        File file1 = new File("rename.txt");
        if (file1.exists()){
            file1.delete();
            System.out.println("删除成功");
        }else{
            file1.createNewFile();
            System.out.println("创建成功");
        }
    }
```

## 4.内存解析

![image-20200430231445700](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200430231447.png)



## 5.小练习

利用Fie构造器，new一个文件目录file 

1）在其中创建多个文件和目录 2）编写方法，实现删除file中指定文件的操作

```java
@Test
    public void test() throws IOException {
        File file = new File("E:\\io");
        File file2 = new File(file,"abc.txt");
        if (!file2.exists()){
            file2.createNewFile();
        }else{
            file2.delete();
        }
    }
```

2.判断指定目录下是否有后缀名为.jpg的文件，如果有，就输出该文件名称

```java
 @Test
    public void test2(){
        //2.判断指定目录下是否有后缀名为.jpg的文件，如果有，就输出该文件名称
        File file = new File("E:\\io");
        ForSubFile(file);

    }

    private void ForSubFile(File file) {
        //获取指定目录下的文件或者文件目录
        File[] files = file.listFiles();
        // 遍历文件或者文件目录
        for (File f : files){
            // 判断是否为文件目录
            if (f.isDirectory()){
                ForSubFile(f);
//                test2();
                //判断是否为文件
            }else if (f.isFile()){
                if (f.getName().endsWith(".jpg")){
                    System.out.println(f.getName());
                }
            }
        }
    }
```



# 二、IO流

## 1.IO流的简述

+ IO是Input/Output的缩写，I/O技术是非常实用的技术，用于<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**处理设备之间的数据传输**</span>。如读/写文件，网络通信等
+ Java程序中，对于数据的输入输出操作都以“<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**流（stream）**</span>”的形式进行。
+ Java.io包下提供了各种“流”的类和接口，用以获取不同种类的数据，并通过<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**标准的方法**</span>输入或输出数据

## 2.流的分类

==**<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>Java中的流都是站在于程序（内存）的角度划分输入和输出</span>**==

**输入input：**读取外部数据（磁盘、光盘等存储设备的数据） 到程序（内存）中

**输出output：**将程序（内存）中的数据 输出到磁盘、光盘等存储设备中。



### 1.按数据的流向分为：输入流、输出流

+ 对于文本文件（.txt，.java），**使用字符流**处理
+ 对**于图片，视频等非文本文件**，**使用字节流**处理

### 2.按数据的单位分：字节流、字符流

**字节流**：每次读取（写出）**一个字节**，当传输的资源文件**有中文时，会出现乱码**

**字符流**：每次读取（写出）**两个字节**，当传输的资源文件**有中文时，不会出现乱码**

`1字符 = 2字节  1字节=8位(bit)  一个汉字占两个字节`

### 3.按流的角色分：节点流、处理流

节点流：直接从数据源或目的地读写数据

处理流：不直接连接到数据源或目的地，而是在“连接”已存在的流上，通过对数据的处理为程序提供更强大的读写功能

![image-20210323224559519](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210323224559519.png)

## 3.IO流的体系结构

Java IO流有4个**抽象基类（父类）**

| ** **  | **字节流**   | **字符流** |
| ------ | ------------ | ---------- |
| 输入流 | InputStream  | Reader     |
| 输出流 | OutputStream | Writer     |

![image-20210323230514271](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210323230514271.png)



### 1.IO流的总体分类

![image-20200502091615386](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200502091616.png)

**`红框为抽象基类，蓝框为常用IO流`**



# 三、节点流（文件流）

## 1.文件字符流FileReader和FileWriter的使用

### 1.读取文件内容到程序中

从文件中读取内容到程序（内存）中

**方式一：FileReader.read()**

**思路**

	1. 实例化File对象，指名从哪个文件读取内容
	2. 提供文件流对象
	3. 循环读取文件中一个一个char
	4. 关闭流操作

**代码示例**

```java
/*
        从文本中读取内容到程序，并在控制台输出
    */
    @Test
    public void FileReader() {

        // 1.实例化File对象
        File file = new File("hello.txt");
        FileReader fr = null;
        try {
            // 2.提供FileReader对象
            fr = new FileReader(file);

            // 3.进行读取操作
            int length;
            while ((length = fr.read()) != -1) {
                System.out.print((char) length);
            }

        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {

            // 4.关闭字符输入流
            if (fr != null){
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
```

**方式二：FileReader.read(char[] buffer)**

**思路**

 	1. 实例化File对象，指名从哪个文件读取内容
 	2. 提供文件流对象
 	3. 循环读取内容到数组中
 	4. 关闭流操作

**代码示例**

```java
 @Test
    public void FileReader2()   {
        // 1.实例化File对象，指名从哪个文件读取内容
        File file =new File("hello.txt");
        FileReader fr = null;

        try {
            // 2.实例化FileReader对象
            fr = new FileReader(file);

            // 3.进行读取操作
            int len;
            char[] cbuf = new char[5];
            while ((len = fr.read(cbuf)) != -1){
                for (int i = 0; i < len;i++){
                    System.out.print(cbuf[i]);
                }
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(fr != null){
                // 4.关闭资源文件
                try {
                    fr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
```

**说明**

1. read()的理解：返回读入的一个字符。如果达到文件末尾，返回-1
2. 异常的处理：为了保证流资源一定可以执行关闭操作。需要使用try-catch-finally处理
3. **<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>读入的文件一定要存在，否则就会报FileNotFoundException。</span>**



### 2.从内存（程序）写出数据到文件中

将内存（程序）写出数据到文件中

**思路**

 	1. 实例化File对象，指名从哪个文件读取内容
 	2. 提供文件流对象
 	3. 写出操作
 	4. 关闭流操作

**示例代码**

```java
/*
        从内存中写出数据到硬盘的文件里
    */
    @Test
    public void testFileWriter()  {
        // 1.提供File对象，指名写出的目的文件
        File file = new File("hello1.txt");
        FileWriter fw = null;
        try {
            // 2.提供写出字符流对象
            fw = new FileWriter(file);
            // 3.写出操作
            fw.write("i have a dream！");
            fw.write(1);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (fw !=  null){

                // 4.关闭资源文件
                try {
                    fw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }

    }
```

==**说明**==

+ **输出操作，对应的File在硬盘中可以不存在**
  + **File对应的硬盘中的文件如果不存在**，在输出的过程中，**会自动创建此文件**
  + File对应的硬盘中的文件如果存在：
    + **如果流使用的构造器是：<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>FileWriter(file，false) / FileWriter(file) ，对原有文件的覆盖</span>**
    + **如果流使用的构造器是：<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>FileWriter(file，true)</span>：<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>不会</span>对原有文件<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>覆盖</span>，而是在原有文件<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>追加内容</span>**

## 2.字符流的练习

实现文本文件的复制操作

**代码**

```java
@Test
public void testFileReaderFileWriter() {
    FileReader fr = null;
    FileWriter fw = null;
    try {
        //1.创建File类的对象，指明读入和写出的文件
        File srcFile = new File("hello.txt");
        File destFile = new File("hello2.txt");

        //不能使用字符流来处理图片等字节数据
        //            File srcFile = new File("test.jpg");
        //            File destFile = new File("test1.jpg");

        //2.创建输入流和输出流的对象
        fr = new FileReader(srcFile);
        fw = new FileWriter(destFile);

        //3.数据的读入和写出操作
        char[] cbuf = new char[5];
        int len;//记录每次读入到cbuf数组中的字符的个数
        while((len = fr.read(cbuf)) != -1){
            //每次写出len个字符
            fw.write(cbuf,0,len);

        }
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //4.关闭流资源
        try {
            if(fw != null)
                fw.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

        try {
            if(fr != null)
                fr.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

## 3.文件字节流FileInputStream和FileOutputStream的使用

字节流的操作与字符流类似，只是实例化对象操作和数据类型不同。

**代码：**

```java
public static void main(String[] args) {

        // 实例化一个File对象，指明要读取哪个文件
        File file = new File("abc.txt");

        FileInputStream fis = null;
        try {
            // 实例化一个FileInputStream
             fis = new FileInputStream(file);

            // 进行读取
            int len;//记录每次读取的个数
            byte[] buffer = new byte[10];
            while ((len = fis.read(buffer)) != -1){
                String str = new String(buffer,0,len);
                System.out.print(str);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 关闭流资源
            if (fis != null){
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

## 4.文件字节流的练习

实现图片文件的复制操作

**代码：**

```java
/*
    实现图片文件的复制
*/
public class FileInputStreamTest2 {

    public static void main(String[] args) {
        // 1.实例化File对象,指明从哪个图片文件读取
        // 2.实例化File对象,指明写出到哪个文件
        File readFile = new File("logo-paul.png");
        File writeFile = new File("1.png");
        // 3.实例化IO流 FileInputStream对象 读取图片内容
        // 4.实例化IO流 FileOutputStream对象 写出图片内容
        FileInputStream fis = null;
        FileOutputStream fos = null;
        try {
            fis = new FileInputStream(readFile);
            fos = new FileOutputStream(writeFile);

            // 3. 进行读取操作
            int len; //记录读取每个字节的个数
            byte[] buffer = new byte[10];
            while ((len = fis.read(buffer)) != -1) {
                fos.write(buffer, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            // 4. 关闭流资源
            if (fos != null) {
                try {
                    fos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if (fis != null) {
                try {
                    fis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

需要**<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>注意</span>**

+ 字节流读取含有中文内容的文件时，会出现**乱码**
+ 对于**<span style='color:red;'>文本文件</span>**(.txt，.java，.c)：使用**<span style='color:red;'>字符流</span> **操作
+ 对于**<span style='color:red;'>非文本文件<</span>**(.doc，.ppt，图片，视频等)：使用**<span style='color:red;'>字节流</span>**操作
+ **读取文件**时，**必须保证文件存在**，否则会报错
+ **写出文件**时，**文件可以不存在**，并不会报异常。

# 四、缓冲流（是处理流的一种）

## 1.缓冲流涉及到的类

+ BufferedInputStream
+ BufferedOutputStream
+ BufferedReader
+ BufferedWriter

## 2.为什么使用缓冲流

作用：**提高**流的读取、写入的**速度**

**提高读写速度的原因**：**==内部提供了一个缓冲区。==**默认情况下是8kb

![image-20200502110552555](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200502110553.png)

**处理流与节点流的对比图示**

节点流

![image-20200502111050587](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200502111051.png)



处理流

![image-20200502111057666](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200502111058.png)

## 3.使用说明

+ 关闭流的顺序和打开流的顺序相反。**关闭流的操作只需要关闭最外层的流即可**，关闭最外层流也会相应的关闭内层节点流。
+ flush()方法：手动将缓冲区清空
+ 当使用BufferedInputStream读取文件时，BufferedInputStream会一次性从文件中读取8192kb的内容，存入缓冲区，直到缓冲区装满了，才重新从文件中读取下一个8192个字节数组

###  1.使用BufferInputStream和BufferOutputStream实现非文本文件的复制

**代码实现：**

```java
/*
    缓冲流
    实现非文本文件的复制
*/
public class BufferedTest {
    public static void main(String[] args) {
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        // 提供File对象 指明要读取的文件
        // 提供File对象 指明要写出的文件
        File srcFile = new File("logo-paul.png");
        File destFile = new File("2.png");

        try {

             bis = new BufferedInputStream(new FileInputStream(srcFile));
             bos = new BufferedOutputStream(new FileOutputStream(destFile));

            int len;
            byte[] buffer = new byte[10];
            while ((len = bis.read(buffer)) != -1){
                bos.write(buffer,0,len);
            }

        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (bos != null)
                bos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
            try {
                if (bis != null)
                bis.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### 2.使用BufferedReader和BufferedWriter实现非文本文件的复制

**代码实现：**

```java
/*
    BufferedReader和BufferedWriter的使用
*/
public class BufferedTest2 {
    public static void main(String[] args) {
        BufferedReader br = null;
        BufferedWriter bw = null;
        try {
             br = new BufferedReader(new FileReader(new File("abc.txt")));
             bw = new BufferedWriter(new FileWriter(new File("1.txt")));

            //读写操作
            // 方式一：使用char[]数组
            /*int len;
            char[] cbuf = new char[10];
            while ((len = br.read(cbuf)) != -1){
                bw.write(cbuf,0,len);
            }*/
            // 方式二： 使用String
            String data ;
            while ((data = br.readLine()) != null){
                bw.write(data); //默认不换行
                //换行
                bw.newLine();
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (bw != null){
                try {
                    bw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if (br != null){
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

**注意** 使用`readLine()`方法进行读写时，`bw.write(data)`默认是不换行的，**如果要换行**可以通过`bw.newLine()`实现。



## 4.相关练习题

### 练习1：实现图片的加密

**思路**

1. 将图片文件通过字节流读取到程序中
2. 将图片的字节流逐一进行^操作
3. 将处理后的图片字节流输出

**代码实现：**

```java
/*
    实现图片的加密
*/
public class PictiureTest {
    public static void main(String[] args) {
        //1.提供File对象 指明要读取图片的文件
        File srcFile = new File("logo-paul.png");
        //2.提供File对象 指明要写出图片的文件
        File destFile = new File("tmqq.png");
        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;

        try {
            //3.提供节点流 FileInputStream和FileOutputStream
            FileInputStream fis = new FileInputStream(srcFile);
            FileOutputStream fos = new FileOutputStream(destFile);
            //4.提供处理流 BufferedInputStream 和BufferedOutputStream
            bis = new BufferedInputStream(fis);
            bos = new BufferedOutputStream(fos);

            //5. 读写操作
            int len;
            byte[] buffer = new byte[10];

            while ((len = bis.read(buffer)) != -1) {
                for (int i = 0; i < len; i++) {
                    // 对字节进行加密
                    buffer[i] = (byte) (buffer[i] ^ 5);
                }
                bos.write(buffer);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (bis != null) {
                try {
                    bis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if (bos != null) {
                try {
                    bos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### 练习2：实现图片的解密

**思路**

1. 将图片文件通过字节流读取到程序中
2. 将图片的字节流逐一进行^操作（原理：A^B^B = A）
3. 将处理后的图片字节流输出

**代码实现：**

```java
/*
   实现图片的解密
*/
public class PictureTest2 {

    public static void main(String[] args) {

        BufferedInputStream bis = null;
        BufferedOutputStream bos = null;
        try {
             bis = new BufferedInputStream(new FileInputStream(new File("tmqq.png")));
             bos = new BufferedOutputStream(new FileOutputStream(new File("jiemi.png")));

            //解密操作
            int len ;
            byte[] buffer = new byte[1024];
            while ((len = bis.read(buffer)) != -1){
                for (int i = 0; i < len; i++) {
                    buffer[i] = (byte) (buffer[i] ^ 5);
                }
                bos.write(buffer,0,len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (bis != null){
                try {
                    bis.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (bos != null){
                try {
                    bos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

### 练习3： 统计文本字符出现次数

**思路**

1. 读取到文本内容
2. new一个Map,用于记录每个字符中对应的个数 
3. 依次遍历，统计每个字符的个数
4. 将map写出到一个文件中
5. 关闭字符流(注意：自己在操作时由于忘记关闭字符流导致写入文件**失败**)

**代码实现**

```java
public class CountCharTest {

    public static void main(String[] args)  {

        //1.读取文本内容
        File srcFile = new File("abc.txt");
        BufferedReader br = null;
        BufferedWriter bw = null;
        try {
            //1.1 实例化IO流 BufferedReader对象
             br = new BufferedReader(new FileReader(srcFile));

            //2. new一个Map,用于记录每个字符对应的个数
            Map<Character, Integer> map = new HashMap<>();


            //1.2 定义变量
            int len;
            char[] cbuf = new char[1024];
            //1.3 读取内容
            while ((len = br.read(cbuf)) != -1) {
                //String content = new String(cbuf,0,len);
                for (int i = 0; i < len; i++) {
                    //System.out.print(cbuf[i]);
                    if (map.get(cbuf[i]) == null) {
                        map.put(cbuf[i], 1);
                    } else {
                        map.put(cbuf[i], map.get(cbuf[i]) + 1);
                    }
                }
            }

            //遍历map
            //for (Map.Entry<Character, Integer> entry : map.entrySet()){
            //    System.out.println("key:" + entry.getKey() +"  value:" + entry.getValue());
            //}

            //4.将map写出到一个文件中
            //4.1实例化 BufferedWriter对象
             bw = new BufferedWriter(new FileWriter(new File("统计字符个数.txt")));
            //4.3 定义一个String的变量
            String data = "";
            //4.2遍历map 一个一个写出
            for (Map.Entry<Character, Integer> entry : map.entrySet()) {
                data += (entry.getKey() + " = " + entry.getValue() + "\n");
                //System.out.println(entry.getKey() + " = " + entry.getValue());
            }
            //System.out.println(data);
            //4.3 写出
            bw.write(data);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (br != null){
                try {
                    br.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }

            if (bw !=null){
                try {
                    bw.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```

# 五、转换流

## 1.转换流概述

+ 转换流提供了字节流与字符流之间的转换
+ **提到`转换流`想到如下图即可**

![image-20200502114233303](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200502114234.png)



+ Java API 提供了两个转换流
  + `InputStreamReader`：将字节流转换成字符流
  + `OutputStreamWriter`：将字符流转换成字节流
+ 很多时候我们使用**转换流**来处理文件乱码问题。**实现编码和解码的功能。**



## 2.InputStreamReader的介绍与使用

`InputStreamReader`将一个字节的输入流转换为字符的输入流 。一般用于**解码** ： 字节 、字节数组  ---->     字符数组、字符串

**构造器：**

+ `new InputStreamReader(InputStream in)`：该方法没有指明字符集,字符集使用默认值，默认值为IDEA中指定的字符集
+ `new InputStreamReader(InputStream in, String charsetName)`：参数2指明了使用哪个字符集，这取决于保存文件时使用的字符集。

## 3.OutputStreamWriter的介绍与使用

OutputStreamWriter将一个**==字符的输出流转换为字节的输出流==** ，**一般用于编码的功能** ，编码：字符数组、字符串  --->   字节、字节数组

**构造器：**

+ `new OutputStreamWriter(OutputStream out)`：使用默认字符集
+ `new OutputStreamWriter(OutputStream out, Charset cs)`：指定字符集



**代码示例**：综合使用InputStreamReader和OutputStreamWriter

```java
/*
    需求: 将一个字节的文件转换成字符流读到控制，再讲控制台的内容转成gbk字符集下的字节流
*/
public class OutputStreamWriterTest {

    public static void main(String[] args)  {
        FileInputStream fis = null;
        FileOutputStream fos = null;

        InputStreamReader isr = null;
        OutputStreamWriter osr = null;
        try {
             fis = new FileInputStream("统计字符个数.txt");
             fos = new FileOutputStream("ali.txt");


             isr = new InputStreamReader(fis);
             osr = new OutputStreamWriter(fos,"gbk");

            int len;
            char[] cbuf = new char[1024];
            while ((len = isr.read(cbuf)) != -1){
                osr.write(cbuf,0,len);
            }

        } catch (IOException e) {
            e.printStackTrace();
        }finally {
            if (isr != null) {
                try {
                    isr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (osr != null) {
                try {
                    osr.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

## 4.编码集

### 1.常见的编码集

+ ASCII：美国标准信息交换码。用**一个字节的7位**可以表示。
+ ISO8859-1：拉丁码表。欧洲码表用**一个字节的8位**表示。
+ GB2312：**中国的中文编码**表。**最多两个字节**编码所有字符
+ GBK：**中国的中文编码**表升级，融合了更多的中文文字符号。**最多两个字节**编码
+ Unicode：国际标准码，融合了目前人类使用的所字符。为每个字符分配唯一的字符码。**所有的文字都用两个字节来表示。**
+ UTF-8：变长的编码方式，可用1-4个字节来表示一个字符。

![image-20200502120238023](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200502120240.png)

**说明：**

+ 面向传输的众多UTF（UCS Transfer Format）标准出现了，顾名思义，UTF-8就是每次传输8位数据，而UTF-16是每次传输16位数据。
+ Unicode只是定义了一个庞大的、全球通用的字符集，并为每个字符规定了唯确定的编号，具体存储成什么样的字节流，取决于字符编码方案。推荐的Unicode编码是UTF-8和UTF-16。

**UTF-8 变长编码表示**

![image-20200502120042318](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200502120043.png)

### 2.编码应用

+ 编码：字符串 -- > 字节数组
+ 解码：字节数组 --> 字符串

# 六、标准输入输出流

## 1.简介

+ `System.in`：标准的输入流，默认从键盘输入
+ `System.out`：标准的输出流，默认从控制台输出

## 2.主要方法

System类的`setIn(InputStream is) `方式重新指定输入的流

System类的`setOut(PrintStream ps)`方式重新指定输出的流

## 3.使用示例

从键盘输入字符串，要球将读取到的整行字符串转成大写输出。直至当输入“e” 或者“exit”时，退出程序

**设计思路**

方式一：使用Scanner实现，调用next()方法返回一个字符串

方式二：使用System.in实现。System.in ---> 转换流  ---> BufferedReader的readLine()

**方式二代码实现**

```java
/*
    从键盘输入字符串，要球将读取到的整行字符串转成大写输出。
    直至当输入“e” 或者“exit”时，退出程序
*/
public class SystemInOutTest {

    public static void main(String[] args) throws IOException {
        //1.通过System.in从键盘输入字符串
        InputStream in = System.in;
        //2.通过InputStreamReader将字节流转换成字符流
        InputStreamReader isr = new InputStreamReader(in);
        BufferedReader br = null;
        br = new BufferedReader(isr);
        //3.获取整行字符串
        while (true){
            String str = br.readLine();
            //4.判断当输入“e” 或者“exit”时，退出程序
            if ("e".equalsIgnoreCase(str) || "exit".equalsIgnoreCase(str)){
                break;
            }
            String data = str.toUpperCase();
            System.out.println(data);
        }
    }
}
```



# 七、打印流

PrintStream和PrintWriter**说明：**

+ 提供了一系列重载的print()和println()方法，用于多种数据类型的输出
+ System.out返回的是PrintStream的实例

```java
@Test
public void test2() {
    PrintStream ps = null;
    try {
        FileOutputStream fos = new FileOutputStream(new File("D:\\IO\\text.txt"));
        // 创建打印输出流,设置为自动刷新模式(写入换行符或字节 '\n' 时都会刷新输出缓冲区)
        ps = new PrintStream(fos, true);
        if (ps != null) {// 把标准输出流(控制台输出)改成文件
            System.setOut(ps);
        }


        for (int i = 0; i <= 255; i++) { // 输出ASCII字符
            System.out.print((char) i);
            if (i % 50 == 0) { // 每50个数据一行
                System.out.println(); // 换行
            }
        }


    } catch (FileNotFoundException e) {
        e.printStackTrace();
    } finally {
        if (ps != null) {
            ps.close();
        }
    }
}
```

# 八、数据流（了解）

DataInputStream和DataOutputStream

 **作用**：用于读取或写出基本数据类型的变量或字符串

**代码实现：**将内存中的字符串、基本数据类型的变量写出到文件中。

```java
@Test
public void test3(){
    //1.造对象、造流
    DataOutputStream dos = null;
    try {
        dos = new DataOutputStream(new FileOutputStream("data.txt"));
        //数据输出
        dos.writeUTF("Bruce");
        dos.flush();//刷新操作，将内存的数据写入到文件
        dos.writeInt(23);
        dos.flush();
        dos.writeBoolean(true);
        dos.flush();
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //3.关闭流
        if (dos != null){
            try {
                dos.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

将文件中存储的基本数据类型变量和字符串读取到内存中，保存在变量中。

**代码实现**

```java
/*
注意点：读取不同类型的数据的顺序要与当初写入文件时，保存的数据的顺序一致！
 */
@Test
public void test4(){
    DataInputStream dis = null;
    try {
        //1.造对象、造流
        dis = new DataInputStream(new FileInputStream("data.txt"));
        //2.从文件读入数据
        String name = dis.readUTF();
        int age = dis.readInt();
        boolean isMale = dis.readBoolean();

        System.out.println("name:"+name);
        System.out.println("age:"+age);
        System.out.println("isMale:"+isMale);
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        //3.关闭流
        if (dis != null){

            try {
                dis.close();
            } catch (IOException e) {
                e.printStackTrace();
    
            }
        }
    }
}
```

# 九、对象流的使用

+ **`ObjectInputStream`**：反序列化，将对象从磁盘读取到内存（程序）
+ **`ObjectOutputStream`**：序列化，将对象从内存（程序）写入磁盘或进行网络传输
+ 作用：用于存储和读取基本数据类型数据或对象的处理流。==**它的强大之处就是可以把Java对象进行序列化和反序列化。**==

## 1.对象流的序列化操作

序列化：将对象从内存写入磁盘或进行网络传输

通过`ObjectOutputStream`

**代码实现**

```java
 /*
        序列化操作
    */
    @Test
    public void ObjectOutputStream() {
        ObjectOutputStream oos = null;
        try {
            //1.实例化ObjectOutputStream对象
            oos = new ObjectOutputStream(new FileOutputStream("object.dat"));
            //2.序列化操作
            oos.writeObject(new String("我爱北京天安门"));
            //3.手动刷新
            oos.flush();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4.关闭流资源
            if (oos != null) {
                try {
                    oos.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```



## 2.对象流的反序列化操作

反序列化：将对象从磁盘读取到内存（程序）中

**代码实现**

```java
/*
        反序列化操作
    */
    @Test
    public void ObjectInputStream() {
        ObjectInputStream ois = null;
        try {
            //1.实例化ObjectInputStream对象
            ois = new ObjectInputStream(new FileInputStream("object.dat"));
            //2.完成反序列化
            String str = (String) ois.readObject();
            System.out.println(str);
        } catch (IOException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            if (ois != null) {
                //3.关闭流资源
                try {
                    ois.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```

## 3.自定义类的实现序列化和反序列化操作

需要**注意**：

+ 自定义类要实现**`Serializable`**或`Externalizable`

+ 凡是实现**`Serializable`**接口的类都要添加一个表示序列化版本标识符的静态常量**`private static final long serialVersionUID;`** ，如果类没有显示定义这个静态常量，它的值是Java运行时环境根据类的内部细节自动生成的。**若类的实例变量做了修改，serialVersionUID 可能发生变化。**故建议，

  **<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>显式声明。</span>**

+ **除了当前类需要实现可序列化，当前类下的所有属性也必须要序列化**

+ ObjectInputStream和ObjectOutputStream不能序列化**`staic`**和**`transient`**修饰的成员变量



# 十、任意存取文件流RandomAccessFile

## 1.RandomAccessFile的简介

+ RandomAccessFile直接继承于Java.lang.Object类，实现了DataInput和DataOutput接口
+ RandomAccessFile既可以作为输入流、也可以作为输出流
+ RandomAccessFile类支持“随机访问”的方式，程序可以直接跳到文件的任意地方来读、写文件
  + 支持只访问文件的部分内容
  + 可以向已存在的文件后追加内容
+ RandomAccessFile对象包含一个记录指针，用来标记当前读写处的位置
+ RandomaccessFile类对象可以自由移动记录指针：
  + `long getFilePointer`：获取文件记录指针的当前位置
  + `void seek(long pos)`：将文件记录指针定位到pos位置



**构造器**

+ `public RandomAccessFile(File file, String mode)`
+ `public RandomAccessFile(String name,String mode)`

## 2.使用说明

1. 如果RandomAccessFile作为输出流时，写出的文件如果不存在，则在执行过程中自动创建。
2. 如果写出的文件存在，会对原文件内容进行覆盖。（默认情况下，从头覆盖）
3. 创建RandomAccessFile类实例需要指定一个mode参数，该参数指定RandomAccessFile的访问模式；
   + r：以只读方式打开
   + rw：打开便读取和写入
   + rws：打开以便读取和写入；同步文件内容和元数据的更新
   + rwd：打开以便读取和写入；同步文件内容的跟新

## 3.使用案例

文件的读取和写出操作

**代码实现**

```java
@Test
    public void test1() {

        RandomAccessFile rac1 = null;
        RandomAccessFile rac2 = null;
        try {
            //1.实例化RandomAccessFile对象,指定从哪个文件读取内容
            rac1 = new RandomAccessFile(new File("boy.jpg"), "r");
            //2.实例化RandomAccessFile对象,指定
            rac2 = new RandomAccessFile(new File("boy1.jpg"), "rw");

            //3.读写操作
            int len;
            byte[] buffer = new byte[1024];
            while ((len = rac1.read(buffer)) != -1) {
                rac2.write(buffer, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            //4. 关闭流资源
            if (rac1 != null) {
                try {
                    rac1.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
            if (rac2 != null) {
                try {
                    rac2.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
```



练习题：使用RandomAccessFile实现数据的插入效果

**代码实现**

```java
@Test
public void test2(){
    RandomAccessFile raf1 = null;
    try {
        raf1 = new RandomAccessFile(new File("hello.txt"), "rw");

        raf1.seek(3);//将指针调到角标为3的位置
        //            //方式一
        //            //保存指针3后面的所有数据到StringBuilder中
        //            StringBuilder builder = new StringBuilder((int) new File("hello.txt").length());
        //            byte[] buffer = new byte[20];
        //            int len;
        //            while ((len = raf1.read(buffer)) != -1){
        //                builder.append(new String(buffer,0,len));
        //            }

        //方式二
        ByteArrayOutputStream baos = new ByteArrayOutputStream();
        byte[] buffer = new byte[20];
        int len;
        while ((len = raf1.read(buffer)) != -1){
            baos.write(buffer);
        }
        //调回指针，写入“xyz”
        raf1.seek(3);
        raf1.write("xyz".getBytes());
        //将StringBuilder中的数据写入到文件中
        raf1.write(baos.toString().getBytes());
    } catch (IOException e) {
        e.printStackTrace();
    } finally {
        if (raf1 != null){
            try {
                raf1.close();
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

# 十一、流的基本应用总结

+ 流是用来处理数据的。
+ 处理数据时，一定要先明确数据源和数据目的数据源，数据源可以是文件，也可以是键盘。目的数据源可以是文件、显示器或者其他设备
+ 而流只是帮助数据进行传输，并对传输的数据进行处理，比如过滤处理、转换处理等
+ **流操作的整体步骤：**
  + 创建文件对象，创建相应的流对象
  + 读写操作
  + 关闭流资源
  + 用try-catch-finally处理异常



# 十二、NIO

主要是Path、Paths、Files的使用，后期抽时间再详细介绍

## 1.NIO的使用说明

+ Java NIO(New IO ，Non-blocking IO) 是从Java 1.4版本开始引入的一套新的IO API，可以替代标准的Java IO。
+ NIO与原来的IO同样的作用和目的，但是使用的方式完全不同，**NIO支持面向缓冲区**的（IO是面向流的）、基于通道的IO操作。
+ NIO将以**更加高效的方式进行文件的读写操作**。
+ JDK 7.0对NIO进行了极大的扩展，增强了对文件处理和文件系统特性的支持，并被命名为**NIO .2** ；

```
Java API提供了两套NIO,一套是针对标准输入输出NIO，另一套就是网络编程NIO
	|-----java.nio.channels.Channel
      |---- FileChannel：处理本地文件
      |---- SocketChannel：TCP网络编程的客户端的Channel
      |---- ServerSocketChannel：TCP网络编程的服务器端的Channel
      |---- DatagramChannel：UDP网络编程中发送端和接收端的Channel
```

## 2.Path接口

+ 早起的Java只提供了一个File类来访问文件系统，但File类的功能比较有限，所提供的方法性能也不高。而且，大多数方法在出错时仅返回失败，并不会提供异常信息。
+ NIO.2为了弥补这种不足，引入了Path接口，代表一个平台无关的平台路径，描述了目录结构中文件的位置。Path可以看成是File类的升级，实际引用的资源也可以不存在。

### 1.Path的说明

Path替换原有的File类。

+ 以前的写法

```
File file = new File("index.html")
```

+ **JDK 7中的写法**

```
import java.nio.file.Path;
import java.nio.file.Paths;
Path file = Paths.get("index.html")
```

Paths类提供了静态方法`get()`来**获取Path对象**

- `static Path get(String first， String….more)`：用于将多个字符串串连成路径
- `static Path get(URI uri)`：返回指定uri对应的Path路径

**代码示例**

```java
@Test
public void test1(){
    Path path1 = Paths.get("hello.txt");//new File(String filepath)

    Path path2 = Paths.get("E:\\", "test\\test1\\haha.txt");//new File(String parent,String filename);

    Path path3 = Paths.get("E:\\", "test");

    System.out.println(path1);
    System.out.println(path2);
    System.out.println(path3);

}
```

### 2.常用方法

+ String toString() ： 返回调用 Path 对象的字符串表示形式
+ boolean startsWith(String path) : 判断是否以 path 路径开始
+ boolean endsWith(String path) : 判断是否以 path 路径结束
+ boolean isAbsolute() : 判断是否是绝对路径
+ Path getParent() ：返回Path对象包含整个路径，不包含 Path 对象指定的文件路径
+ Path getRoot() ：返回调用 Path 对象的根路径
+ Path getFileName() : 返回与调用 Path 对象关联的文件名
+ int getNameCount() : 返回Path 根目录后面元素的数量
+ Path getName(int idx) : 返回指定索引位置 idx 的路径名称
+ Path toAbsolutePath() : 作为绝对路径返回调用 Path 对象
+ Path resolve(Path p) :合并两个路径，返回合并后的路径对应的Path对象
+ File toFile(): 将Path转化为File类的对象

**代码示例**

```java
@Test
public void test2() {
    Path path1 = Paths.get("d:\\", "nio\\nio1\\nio2\\hello.txt");
    Path path2 = Paths.get("hello.txt");

    //		String toString() ： 返回调用 Path 对象的字符串表示形式
    System.out.println(path1);

    //		boolean startsWith(String path) : 判断是否以 path 路径开始
    System.out.println(path1.startsWith("d:\\nio"));
    //		boolean endsWith(String path) : 判断是否以 path 路径结束
    System.out.println(path1.endsWith("hello.txt"));
    //		boolean isAbsolute() : 判断是否是绝对路径
    System.out.println(path1.isAbsolute() + "~");
    System.out.println(path2.isAbsolute() + "~");
    //		Path getParent() ：返回Path对象包含整个路径，不包含 Path 对象指定的文件路径
    System.out.println(path1.getParent());
    System.out.println(path2.getParent());
    //		Path getRoot() ：返回调用 Path 对象的根路径
    System.out.println(path1.getRoot());
    System.out.println(path2.getRoot());
    //		Path getFileName() : 返回与调用 Path 对象关联的文件名
    System.out.println(path1.getFileName() + "~");
    System.out.println(path2.getFileName() + "~");
    //		int getNameCount() : 返回Path 根目录后面元素的数量
    //		Path getName(int idx) : 返回指定索引位置 idx 的路径名称
    for (int i = 0; i < path1.getNameCount(); i++) {
        System.out.println(path1.getName(i) + "*****");
    }

    //		Path toAbsolutePath() : 作为绝对路径返回调用 Path 对象
    System.out.println(path1.toAbsolutePath());
    System.out.println(path2.toAbsolutePath());
    //		Path resolve(Path p) :合并两个路径，返回合并后的路径对应的Path对象
    Path path3 = Paths.get("d:\\", "nio");
    Path path4 = Paths.get("nioo\\hi.txt");
    path3 = path3.resolve(path4);
    System.out.println(path3);

    //		File toFile(): 将Path转化为File类的对象
    File file = path1.toFile();//Path--->File的转换

    Path newPath = file.toPath();//File--->Path的转换

}
```

## 3.Files类

java.nio.file.Files用于操作文件或目录的**工具类**

### 1.Files类常用方法

- Path copy(Path src, Path dest, CopyOption … how) : 文件的复制

  要想复制成功，要求path1对应的物理上的文件存在。path1对应的文件没有要求。

- Files.copy(path1, path2, StandardCopyOption.REPLACE_EXISTING);

- Path createDirectory(Path path, FileAttribute<?> … attr) : 创建一个目录

  要想执行成功，要求path对应的物理上的文件目录不存在。一旦存在，抛出异常。

- Path createFile(Path path, FileAttribute<?> … arr) : 创建一个文件

- 要想执行成功，要求path对应的物理上的文件不存在。一旦存在，抛出异常。

- void delete(Path path) : 删除一个文件/目录，如果不存在，执行报错

- void deleteIfExists(Path path) : Path对应的文件/目录如果存在，执行删除.如果不存在，正常执行结束

- Path move(Path src, Path dest, CopyOption…how) : 将 src 移动到 dest 位置

  要想执行成功，src对应的物理上的文件需要存在，dest对应的文件没有要求。

- long size(Path path) : 返回 path 指定文件的大小

**代码示例**

```java
@Test
public void test1() throws IOException{
    Path path1 = Paths.get("d:\\nio", "hello.txt");
    Path path2 = Paths.get("atguigu.txt");

    //		Path copy(Path src, Path dest, CopyOption … how) : 文件的复制
    //要想复制成功，要求path1对应的物理上的文件存在。path1对应的文件没有要求。
    //		Files.copy(path1, path2, StandardCopyOption.REPLACE_EXISTING);

    //		Path createDirectory(Path path, FileAttribute<?> … attr) : 创建一个目录
    //要想执行成功，要求path对应的物理上的文件目录不存在。一旦存在，抛出异常。
    Path path3 = Paths.get("d:\\nio\\nio1");
    //		Files.createDirectory(path3);

    //		Path createFile(Path path, FileAttribute<?> … arr) : 创建一个文件
    //要想执行成功，要求path对应的物理上的文件不存在。一旦存在，抛出异常。
    Path path4 = Paths.get("d:\\nio\\hi.txt");
    //		Files.createFile(path4);

    //		void delete(Path path) : 删除一个文件/目录，如果不存在，执行报错
    //		Files.delete(path4);

    //		void deleteIfExists(Path path) : Path对应的文件/目录如果存在，执行删除.如果不存在，正常执行结束
    Files.deleteIfExists(path3);

    //		Path move(Path src, Path dest, CopyOption…how) : 将 src 移动到 dest 位置
    //要想执行成功，src对应的物理上的文件需要存在，dest对应的文件没有要求。
    //		Files.move(path1, path2, StandardCopyOption.ATOMIC_MOVE);

    //		long size(Path path) : 返回 path 指定文件的大小
    long size = Files.size(path2);
    System.out.println(size);

}
```

### 2.Files类常用方法：用于判断

- boolean exists(Path path, LinkOption … opts) : 判断文件是否存在

- boolean isDirectory(Path path, LinkOption … opts) : 判断是否是目录

  不要求此path对应的物理文件存在。

- boolean isRegularFile(Path path, LinkOption … opts) : 判断是否是文件

- boolean isHidden(Path path) : 判断是否是隐藏文件

  要求此path对应的物理上的文件需要存在。才可判断是否隐藏。否则，抛异常。

- boolean isReadable(Path path) : 判断文件是否可读

- boolean isWritable(Path path) : 判断文件是否可写

- boolean notExists(Path path, LinkOption … opts) : 判断文件是否不存在

**代码示例**

```java
@Test
public void test2() throws IOException{
    Path path1 = Paths.get("d:\\nio", "hello.txt");
    Path path2 = Paths.get("atguigu.txt");
    //		boolean exists(Path path, LinkOption … opts) : 判断文件是否存在
    System.out.println(Files.exists(path2, LinkOption.NOFOLLOW_LINKS));

    //		boolean isDirectory(Path path, LinkOption … opts) : 判断是否是目录
    //不要求此path对应的物理文件存在。
    System.out.println(Files.isDirectory(path1, LinkOption.NOFOLLOW_LINKS));

    //		boolean isRegularFile(Path path, LinkOption … opts) : 判断是否是文件

    //		boolean isHidden(Path path) : 判断是否是隐藏文件
    //要求此path对应的物理上的文件需要存在。才可判断是否隐藏。否则，抛异常。
    //		System.out.println(Files.isHidden(path1));

    //		boolean isReadable(Path path) : 判断文件是否可读
    System.out.println(Files.isReadable(path1));
    //		boolean isWritable(Path path) : 判断文件是否可写
    System.out.println(Files.isWritable(path1));
    //		boolean notExists(Path path, LinkOption … opts) : 判断文件是否不存在
    System.out.println(Files.notExists(path1, LinkOption.NOFOLLOW_LINKS));
}
```

**补充**

- StandardOpenOption.READ:表示对应的Channel是可读的。
- StandardOpenOption.WRITE：表示对应的Channel是可写的。
- StandardOpenOption.CREATE：如果要写出的文件不存在，则创建。如果存在，忽略
- StandardOpenOption.CREATE_NEW：如果要写出的文件不存在，则创建。如果存在，抛异常

### 3.Files类常用方法：用于操作内容

- InputStream newInputStream(Path path, OpenOption…how):获取 InputStream 对象
- OutputStream newOutputStream(Path path, OpenOption…how) : 获取 OutputStream 对象
- SeekableByteChannel newByteChannel(Path path, OpenOption…how) : 获取与指定文件的连接，how 指定打开方式。
- DirectoryStream<Path> newDirectoryStream(Path path) : 打开 path 指定的目录

**代码示例**

```java
@Test
public void test3() throws IOException{
    Path path1 = Paths.get("d:\\nio", "hello.txt");

    //		InputStream newInputStream(Path path, OpenOption…how):获取 InputStream 对象
    InputStream inputStream = Files.newInputStream(path1, StandardOpenOption.READ);

    //		OutputStream newOutputStream(Path path, OpenOption…how) : 获取 OutputStream 对象
    OutputStream outputStream = Files.newOutputStream(path1, StandardOpenOption.WRITE,StandardOpenOption.CREATE);


    //		SeekableByteChannel newByteChannel(Path path, OpenOption…how) : 获取与指定文件的连接，how 指定打开方式。
    SeekableByteChannel channel = Files.newByteChannel(path1, StandardOpenOption.READ,StandardOpenOption.WRITE,StandardOpenOption.CREATE);

    //		DirectoryStream<Path>  newDirectoryStream(Path path) : 打开 path 指定的目录
    Path path2 = Paths.get("e:\\teach");
    DirectoryStream<Path> directoryStream = Files.newDirectoryStream(path2);
    Iterator<Path> iterator = directoryStream.iterator();
    while(iterator.hasNext()){
        System.out.println(iterator.next());
    }
}
```



**`Java中有一个commons-io.jar 封装了大量IO操作方法，请知晓`**