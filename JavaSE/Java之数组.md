# 一、数组的概述

## 1.数组的理解

理解：数组(Array)，是一组<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>相同数据类型</span>的按照<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>一定顺序排列</span>的集合，并使用一个名字命名，并通过<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>编号</span>来对这些数据进行统一的管理。

## 2.数组的相关概念

	+ 数组名
	+ 元素
	+ 角标、下标、索引
	+ 数组的长度：元素的个数

## 3.数组的特点

	1. 数组是有序的
	2. 数组属于<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>引用数据类型</span>。数组的元素 既可以是基本数据类型，也可以是引用数据类型（String）
	3. 创建数组对象需要开辟一堆连续的空间
	4. 数组的长度一旦确定，不能更改

## 4.数组的分类

数组按照维数分类：一维数组、多维数组

数组按照元素的数据类型分类：基本数据类型元素的数组、引用数据类型元素的数组

# 二、一维数组

一维数组主要包含的内容：

		1. 一维数组的声明和初始化
		2. 如何调用数组指定位置的元素
		3. 如何获取数组的长度
		4. 如何遍历数组
		5. 数组元素的默认初始化值
		6. 数组的内存解析

## 1.一维数组的声明和初始化

数组的初始化分为:静态初始化 + 动态初始化

**静态初始化语法**:数据类型[]  数组名 = new 数据类型 []｛元素1，元素2，元素3，....｝

**动态初始化语法**: 数据类型[]  数组名 = new 数据类型 [数组的长度]

```java
public class ArrayTest {

    public static void main(String[] args) {

        // 数组的声明
        int[] arr ;
        // 数组的初始化
        arr  = new int[]{1001,1002,1003,1004};

        //数组的声明 + 初始化
        int[] arr2 = new int[]{1001,1002,1003,1004};

        //数组的初始化分为：静态初始化 + 动态初始化

        // 静态初始化: 数组的声明 和数组元素的赋值 同时操作
        int[] staticArr = new int[]{1,2,3,4,5,6};

        //动态初始化: 数组的声明和数组元素的赋值 分开操作
        String[] strArr = new String[5];
    }
}
```



## 2.一维数组的引用

通过数组的索引来引用。

Java中一般索引都是从<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>0</span>开始的

```java
//动态初始化: 数组的声明和数组元素的赋值 分开操作
        String[] names = new String[5];
        
        //2.一维数组的引用
        names[0] = "张学良";
        names[1] = "杨虎城";
        names[0] = "曹操";
```

## 3.一维数组的长度

通过数组的属性：length

```java
System.out.println(names.length);
```

## 4.一维数组的遍历

通过循环去遍历一维数组

```java
  for (int i = 0; i < names.length; i++) {
            System.out.println(names[i]);
        }
```

## 5.一维数组的默认初始化值

+ 数组元素是整型：0
+ 数组元素是浮点型：0.0
+ 数组元素是char型：0或'\u000' ，而不是 '0'
+ 数组元素是boolean型：false
+ 数组元素是<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>引用类型</span>：null  而不是"null"

## 6.一维数组的内存解析

首先看一下**内存的简化图**

![image-20210306133431434](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210306133431434.png)



![image-20210306140327353](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210306140327353.png)



一维数组练习题：

```java
public class ArrayExer {

    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        System.out.print("请输入学生个数:");

        // 1.使用Scanner读取学生个数
        int stuCount = scan.nextInt();
        System.out.print("请输入" + stuCount + "个成绩:");

        //1.1定义数组,用于存放学生的成绩
        int[] scoreArr = new int[stuCount];

        // 2. 给数组中每个元素赋上成绩
        for (int i = 0; i < scoreArr.length; i++) {
            scoreArr[i] = scan.nextInt();
        }

        // 3.获取组高分
       int maxScore = 0;
        for (int i = 0; i < scoreArr.length; i++) {
            if (maxScore < scoreArr[i]){
                maxScore = scoreArr[i];
            }
        }

        // 4.输出每个学生的成绩等级
        char scoreGrade;
        for (int i = 0; i < scoreArr.length; i++) {
            if (scoreArr[i] >= maxScore - 10){
                scoreGrade = 'A';
            }else if(scoreArr[i] >= maxScore - 20){
                scoreGrade = 'B';
            }else if(scoreArr[i] >= maxScore - 30){
                scoreGrade = 'C';
            }else{
                scoreGrade = 'D';
            }

            System.out.println("student " + i + " score is " + scoreArr[i] + " grade is " + scoreGrade);

        }

    }
}
```



# 三、多维数组

多维数组的学习主要包含以下：

	1. 多维数组的声明和初始化
	2. 如何调用数组指定位置的元素
	3. 如何获取数组的长度
	4. 如何遍历数组
	5. 数组元素的默认初始化值
	6. 数组的内存解析

## 1.二维数组的理解

数组属于引用类型，数组的元素也可以是引用类型，一个数组A的元素是另一个数组B，那么这就是二维数组。

## 2.二维数组的声明和初始化

二维数组**静态初始化语法**：数组类型[ ] [ ]  数组名 = new 数据类型[ ] [ ]{{1,2,3},{4,5},{7,8,9}}

二维数组**动态初始化语法**：数组类型[ ] [ ]   数组名 =  new 数组类型[ 行数] [列数 【可省略】]

```java
    //二维数组的声明 + 初始化
        //二维数组静态初始化
        int[][] ids = new int[][]{{1,2,3},{4,5},{6,7,8}};

        //二维数组的动态初始化
        String[][] names  = new String[2][3];
```

## 3.二维数组的引用

```java
 // 数组的声明 + 初始化
        // 静态初始化
        int[][] arr = new int[][]{{1,2,3},{4,5},{6,7,8}};

        //动态初始化
        int[][] arr2 = new int[2][3];
        
        //数组的引用
        System.out.println(arr[1][1]);
```

## 4.二维数组的长度

通过属性length获取

```java
        // 获取数组的长度
        int[][] arr = new int[][]{{1,2,3},{4,5},{6,7,8}};
        System.out.println(arr.length);//3
        System.out.println(arr[0].length);//3
        System.out.println(arr[1].length);//2
```

## 5.二维数组的遍历

二维数组的遍历需要两层for循环

```java
// 二维数组的遍历
        for (int i = 0; i < arr.length; i++) {

            for (int j = 0; j < arr[i].length; j++) {
                System.out.print(arr[i][j] +" ");
            }
            System.out.println();
        }
```

## 6.二维数组的初始化默认值

**规定**：二维数组分为外层数组的元素，内层数组的元素

​			比如： int [ ] [ ] arr3 = new int[ 2 ] [ 3 ];

​			外层元素：arr[ 0 ]，arr[ 1 ]等

​			内层元素：arr[ 1 ] [ 1 ], arr[ 0 ] [ 2 ]



<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**①：数组元素的默认初始化值：**</span>

​		**针对于初始化方式一：  int [ ] [ ] arr3 = new int[ 2 ] [ 3 ];**

​			外层元素的初始化值为（int【0】）： 地址值

​			内层元素的初始化值为（int【0】【1】）：与一维数组初始化情况相同



​		**针对于初始化条件二： int [ ] [ ] arr3 = new int[ 2 ] [  ];**

​			外层元素的初始化值为（int【0】）： null

​			内层元素的初始化值为（int【0】【1】）：会报错，空指针异常

​	

## 7 .二维数组的内存解析

![image-20210306195044506](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210306195044506.png)





# 四、数组的常见算法

	1. 数组的赋值（杨辉三角、回形数）
	2. 求数值型数组中元素的最大值、最小值、平均值、求和等等
	3. 数组的复制、反转、查找（线性查找、二分查找）
	4. 数组元素的排序算法



## 1.数组的赋值（杨辉三角）

```java
/*
    使用二维数组打印一个 10 行杨辉三角。

        【提示】
    1. 第一行有 1 个元素, 第 n 行有 n 个元素
    2. 每一行的第一个元素和最后一个元素都是 1
    3. 从第三行开始, 对于非第一个元素和最后一个元
    素的元素。即：
    yanghui[i][j] = yanghui[i-1][j-1] + yanghui[i-1][j];

*/
public class YangHuiExer {

    public static void main(String[] args) {

        // 1.声明初始化二维数组
        int[][] arr = new int[10][];

        // 2. 给数组赋值
        // 2.1 搭建初始模型
        for (int i = 0; i < arr.length; i++) {
            arr[i] = new int[i + 1];

            //2.2 给首列赋值1
            arr[i][0] = 1;
            //2.3 给每行的最后一个元素赋值1
            arr[i][i] = 1;
            //从第三行开始，因为下标是从0开始的，所以第三行对应的下标是2
            // 优化一： if条件判断可以省略，因为下面arr[i].length-1就代表从第三行开始
            //if (i > 1) {
                // 内层循环的变量j应该从第二列开始，所以是1，到当前行的倒数第二个数
                for (int j = 1; j < arr[i].length - 1; j++) {
                    arr[i][j] = arr[i-1][j] + arr[i-1][j-1];
                }
            //}
        }

        // 3. 遍历数组
        for (int i = 0; i < arr.length; i++) {
            for (int j = 0; j < arr[i].length; j++) {
                System.out.print(arr[i][j] + " ");
            }
            System.out.println();
        }

    }
}
```



## 2.针对于数值型数组——求最大值、最小值、平均值、总和

```java
/*
    定义一个int型的一维数组，包含10个元素，分别赋一些随机整数，
    然后求出所有元素的最大值，最小值，和值，平均值，并输出出来。
    要求：所有随机数都是两位数。
*/
public class ArrayExer2 {

    public static void main(String[] args) {
        // 1.声明初始化数组
        int[] arr = new int[10];

        // 2.赋值：要求是两位数的随机数
        // 3.求最大值，最小值，和值，平均值
        int min = 0;
        int sum = 0;

        for (int i = 0; i < arr.length; i++) {
            arr[i] = (int) (Math.random() * 90 + 10);
            // 求最大值
            sum += arr[i];
        }
        System.out.println("总和:" + sum);
        // 求平均值
        int avg = sum / arr.length;
        System.out.println("平均值:" + avg);

        int maxValue = arr[0];
        for (int i = 0; i < arr.length; i++) {
            // 求最大值
            if (maxValue < arr[i]) {
                maxValue = arr[i];
            }
        }
        System.out.println("最大值：" + maxValue);

        int minValue = arr[0];
        for (int i = 0; i < arr.length; i++) {
            // 求最大值
            if (minValue > arr[i]) {
                minValue = arr[i];
            }
        }
        System.out.println("最小值：" + minValue);
    }
}
```

## 3.数组的复制、反转、查找

**数组的复制**

```java
/*
   算法的考察：实现数组的复制
*/
public class ArrayExer3 {

    public static void main(String[] args) {

        // 已有的数组
        int[] arr1 = new int[]{1, 2, 3, 4, 5, 7};

        // 声明一个长度和arr1相等的数组
        int[] arr2 = new int[arr1.length];

        // 遍历数组，进行复制

        for (int i = 0; i < arr1.length; i++) {
            arr2[i] = arr1[i];
        }

        // 打印输出
        for (int i = 0; i < arr2.length; i++) {
            System.out.print(arr2[i]);
        }

    }
}
```



**数组的反转**

数组的反转是实现数组中元素的交换，而不是逆序遍历输出。

数组的反转需要使用**临时变量**。

```java
public class ArrayExer4 {

    public static void main(String[] args) {


        String[] arr = new String[]{"AA", "HH", "EE", "GG", "PP", "QQ", "JD"};

        //方式一： 使用一个临时变量temp
        String temp;
        for (int i = 0; i < arr.length / 2; i++) {
            temp = arr[i];
            arr[i] = arr[arr.length - 1 - i];
            arr[arr.length - 1 - i] = temp;
        }

        //方式二：使用两个变量
        /*for(int i = 0,j = arr.length-1;i<j;i++,j--){
            temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }*/

        //打印输出
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

**数组查找（线性查找）**

线性查找就是一个一个去匹配，如果找到就退出循环。

```java
/*
     算法的考察：数组的查找（线性查找）

*/
public class ArrayExer5 {

    public static void main(String[] args) {

        String[] arr = new String[]{"AA", "HH", "EE", "GG", "PP", "QQ", "JD"};

        String str = "cb";

        boolean flag = true;
        for (int i = 0; i < arr.length; i++) {

            if (str.equals(arr[i])){
                System.out.println("找到了~~ 位置为:" + i);
                flag = false;
                break;
            }

        }

        if (flag){
            System.out.println("很抱歉，没有找到喔");
        }
    }
}
```

**数组的查找（二分查找）**

二分查找的前提是 数据必须是<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**有顺序**</span>的！！

```java
/*
    数组的查找 ：二分查找
    二分查找的前提： 数据必须是有序的
*/
public class ArrayExer6 {

    public static void main(String[] args) {

        int[] arr = new int[]{-98,-5,34,58,66,69,78,88,95,100};
        // 初始化首的索引
        int head = 0;
        // 初始化尾的索引
        int tail =  arr.length - 1;

        // 要找的数据
        int num = 100;

        //定义一个标记，用于标记是否找到
        boolean isExistFlag = true;
        // 循环 :条件是head <= tail
        while (head <= tail){

            // 找到中间的索引
            int middle = (head + tail) / 2 ;
            if (num == arr[middle]){
                System.out.println("数据找到了，位置是:" + middle);
                isExistFlag = false;
                break;
            }else if(num < arr[middle]){
                // 在middle左半部分
                tail = middle - 1;
            }else{// 在middle的右半部分
                head = middle + 1;
            }
        }

        // 判断
        if (isExistFlag) {
            System.out.println("很抱歉，并没有找到~");
        }
    }
}
```

## <span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>4.数组中涉及到的排序算法</span>

十大内部排序算法

<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**红色标记**</span>的是<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>**需要手写**</span>的、**<span style='color:blue;background:背景颜色;font-size:文字大小;font-family:字体;'>蓝色标记</span>**的是需要<span style='color:blue;background:背景颜色;font-size:文字大小;font-family:字体;'>**理解思路**</span>。

 + 选择排序 
   	+ 直接选择排序、<span style='color:blue;background:背景颜色;font-size:文字大小;font-family:字体;'>**堆排序**</span>
 + 交换排序
    + **<span style='color:red;background:背景颜色;font-size:文字大小;font-family:字体;'>冒泡排序、快速排序</span>**
 + 插入排序
   	+ 直接插入排序、折半插入排序、Shell排序
	+ <span style='color:blue;background:背景颜色;font-size:文字大小;font-family:字体;'>**归并排序**</span>
	+ 桶式排序
	+ 基数排序



衡量一个排序算法的优劣：

​	时间复杂度、空间复杂度、稳定性

排序的分类： 内部排序、外部排序

**不同排序算法的时间复杂度对比**

![image-20200430124517742](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200430124520.png)





**冒泡排序的实现**

冒泡的排序的思路：相邻元素之间进行比较，如果前者比后者大，就交换

```java
/*
    实现数组的冒泡排序

*/
public class BubbleSortTest {

    public static void main(String[] args) {

        int[] arr = new int[]{-6, 58, 24, 30, 0, 81, -12, 3, 40};

        //冒泡排序
        for (int i = 0; i < arr.length - 1; i++) {

            for (int j = 0; j < arr.length - 1 - i; j++) {
                if (arr[j] > arr[j + 1]) {
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }

        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }

    }
}
```



# 五、Arrays工具类的使用

## 1.理解

​	包的路径：Java.util.Arrays

​	是操作数组的工具类

## 2.使用

```java
    int[] arr1 = new int[]{1,6,7,9,10};
        int[] arr2 = new int[]{1,2,7,9,58};

        // 1. boolean equals(int[] a,int[] b) 判断两个数组是否相等。
        boolean equals = Arrays.equals(arr1, arr2);
        System.out.println(equals);

        // 2. String toString(int[] a) 输出数组信息。
        System.out.println(Arrays.toString(arr1));

        // 3. void fill(int[] a,int val) 将指定值填充到数组之中。
        Arrays.fill(arr1,7);
        System.out.println(Arrays.toString(arr1));

        // 4. void sort(int[] a) 对数组进行排序
        Arrays.sort(arr2);
        System.out.println(arr2);

        // 5.int binarySearch(int[] a,int key) 对排序后的数组进行二分法检索指定的值。
        int num = Arrays.binarySearch(arr1, 7);
        System.out.println(num);
```



# 六、数组中的常见异常

## 1.数组角标越界异常：ArrayIndexOutOfBoundsException

```java
int[] arr = new int[]{1,2,3,4,5};

for(int i = 0;i <= arr.length;i++){
    System.out.println(arr[i]);
}

System.out.println(arr[-2]);

System.out.println("hello");
```



## 2.空指针异常：NullPointerException

```java
//情况一：
int[] arr1 = new int[]{1,2,3};
arr1 = null;
System.out.println(arr1[0]);

//情况二：
int[][] arr2 = new int[4][];
System.out.println(arr2[0][0]);

//情况：
String[] arr3 = new String[]{"AA","BB","CC"};
arr3[0] = null;
System.out.println(arr3[0].toString());
```

