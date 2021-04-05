# 一、程序、进程、线程基本概念

## 1.程序

概念：是为了完成特定的任务、用某种语言编写的一组指令的集合。即指<span style='color:red;'>一段静态的代码</span>

## 2.进程

概念：程序的一次执行过程，或是<span style='color:red;'>正在执行的程序。</span>

说明：进程作为资源分配的单位，系统在运行时会为每个进程分配不同的内存区域。

## 3.线程

概念：进程可进一步细化为线程，是一个程序内部的一条执行路径。

说明：线程作为调度和执行的单位，每个线程拥有独立的运行栈和程序计数器（pc），线程切换的开销小

![image-20200411195447115](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200411195449.png)

**内存结构**

![image-20200411195509394](https://gitee.com/realbruce/blogImage/raw/master/imgs/20200411195510.png)

```
进程可以细化多个线程。每个线程，拥有自己独立的 虚拟机栈 和 程序计数器(pc)。
多个线程共享同一个进程内的资源(方法区 和 堆)
```



# 二、并行与并发

## 1.单核CPU和多核CPU的概念

+ **单核CPU**：其实是一种假的多线程，因为在一个时间单元内，也只能执行一个线程的任务。涉及到CPU处理线程的方式，CPU在单位时间（也就是一个时间片内）只能处理一个线程，于是就将其他线程设置为阻塞状态，加入到阻塞队列中，等到处理完当前线程后就从就绪队列中取出新的线程进行处理，由于切换和处理时间很快，用户感知不到，于是用户便认为CPU在同一时间内处理多个线程。  单核类比：一个人只有一个大脑
+ **多核CPU**：可以更好的发挥多线程的效率。（现在的服务器都是多核的）
+ **一个Java应用程序 java.exe，包含3个线程，分别是 <span style='color:red;'>main线程，gc线程，异常处理线程。</span>**

## 2.并行与并发的概念

+ **并行**：多个CPU同时执行多个任务。比如：篮球场上的人各打各的球。
+ **并发**：一个CPU同时执行多个任务。比如：篮球场上的人都在打**一个球**

## 3.多线程的优点

	1. 提高应用程序的响应。对图形化界面更有意义，可增强用户体验。
	2. 提高计算机系统CPU的利用率。
	3. 改善程序结构。将既复杂又长的进程分为多个线程，独立运行，利于理解和修改。

## 4.多线程应用的场景

+ 程序需要同时执行两个或多个任务。
+ 程序需要实现一些需要等待的任务时，如用户输入、文件读写操作、网络操作、搜索等
+ 需要一些后台运行的程序时。

# 三、<span style='color:red;'>线程的创建和使用</span>(重点)

## 1.Thread类实现多线程

JVM 允许程序运行多个线程，它通过Java.lang.Thread类来体现

### 1.Thread类的特性

每个线程都是通过`Thread类的对象.start()`启动线程并执行`Thread类中的run()`方法执行线程体。

### 2.Thread类的构造器

### 3.Thread类创建线程的方式

1. 创建一个类继承于Thread类
2. 重写Thread类的run()  ---> 将线程要执行的内容写在`run()`中
3. 创建子类的对象
4. 通过`子类对象.start()`启动并执行线程

**注意**：

	1. 我们启动一个线程,是调用`start()`，而不能直接调用`run() `(如果直接调用调用run()，视为普通调用);
	2. **如果需要多个线程**，必须重新创建一个Thread类的子类对象，在通过`start()`方法启动并执行线程。



**代码示例**

```java
/*
    创建线程的步骤：
        1.创建一个类 继承于Thread类
        2.重写Thread类中的run()
        3.创建一个子类的对象
        4.调用start()

    举例:输出100以内的偶数
*/
public class ThreadTest {
    public static void main(String[] args) {
        //3.创建一个子类的对象
        MyThread t1 = new MyThread();
        //4.调用start();
        t1.start();
        // 问题一：直接调用run()方法不会启动新的线程
        //t1.run();

        // 问题二：启动多个线程，我们需要重新创建一个线程的对象
        MyThread t2 = new MyThread();
        t2.start();

        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ":" + i + "*******main()********");
            }
        }
    }
}


//1.创建一个类 继承于Thread类
class MyThread extends Thread {
    //2.重写Thread类中的run()

    @Override
    public void run() {

        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + ":" + i);
            }
        }
    }
}
```



### **4.Thread类方式的多线程练习**

创建两个线程，其中一个线程遍历100以内的偶数，另一个线程遍历100以内的奇数。

```java
/*
    练习：创建两个线程，其中一个线程遍历100以内的偶数，另一个线程遍历100以内的奇数。
*/
public class ThreadDemo {

    public static void main(String[] args) {
        //方式一: 通过 创建子类，并调用start()方法
        MyThread1 t1 = new MyThread1();
        MyThread2 t2 = new MyThread2();
        t1.start();
        t2.start();

        //方式二:匿名子类对象.start()
        new Thread() {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    if (i % 2 == 0) {
                        System.out.println(Thread.currentThread().getName() + " :" + i);
                    }
                }
            }
        }.start();
        new Thread() {
            @Override
            public void run() {
                for (int i = 0; i < 100; i++) {
                    if (i % 2 != 0) {
                        System.out.println(Thread.currentThread().getName() + " :" + i);
                    }
                }
            }
        }.start();

    }
}

class MyThread1 extends Thread {


    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0) {
                System.out.println(Thread.currentThread().getName() + " :" + i);
            }
        }
    }
}

class MyThread2 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 != 0) {
                System.out.println(Thread.currentThread().getName() + " :" + i);
            }
        }
    }
}
```



### 5.Thread类的常用方法

+ start()：启动当前线程，调用当前线程的run()，只有Thread类和它的子类才能调用start()方法
+ run()：将创建线程要执行的操作声明在该方法中
+ getCurrentThread()：静态方法，返回执行当前代码的线程
+ getName()：获取当前线程的名字
+ setName()：设置当前线程的名字
+ <span style='color:red;'>**yield()：释放当前CPU的执行权**</span>
+ <span style='color:red;'>**join()：在线程A中调用线程B的join()方法，线程A会被阻塞，直到线程B完全执行完成以后，线程A才结束阻塞状态**</span>
+ stop()：强制结束当前线程，该方法已过时，**不建议使用**
+ <span style='color:red;'>**sleep()：让当前线程“睡眠”指定毫秒。在指定的毫秒时间内，当前线程是阻塞状态**</span>
+ isAlive()：判断当前线程是否存活

### 6.线程的优先级

+ MAX_PRIORITY： 默认值10
+ MIN_PRIORITY：默认值1
+ NORM_PRIORITY：默认值5 

**获取和设置当前线程的优先级**

+ setPriority(int p)：设置当前线程的优先级， p 在MIN_PRIORITY和MAX_PRIORITY之间
+ getPriority()：获取当前线程的优先级

```
说明:高优先级的线程会抢占低优先级的线程CPU的执行权。但只是从概率上讲，高优先级的线程会高概率的先被执行。并不意味着当高优先级的线程执行完毕之后，低优先级的线程才执行。
```

### 7.线程的分类

+ 守护线程：gc垃圾回收线程，依赖于main线程而存在
+ 用户线程：main方法的线程

## 2.Runnable接口实现多线程

**实现步骤**

	1. 定义一个类实现Runnable接口
	2. 实现Runnable接口中的run()
	3. 创建实现类对象
	4. 创建Thread类，把实现类对象作为参数传递给Thread类的构造器
	5. 调用start()启动线程

**代码实现**

```java
public class RunnableTest {
    public static void main(String[] args) {
        MThread mt = new MThread();
        Thread t1 = new Thread(mt);
        t1.start();

    }
}

class MThread implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0){
                System.out.println(i);
            }

        }
    }
}
```



## 3.创建线程的两种方式比较

**开发中优先选择**：实现Runnable接口的方式

**原因**：

			1.  实现的方式没有类的单继承性的局限
			2.  实现的方式更适合处理多个线程有共享数据的情况

**联系**：`public class Thread implements Runnable`

**相同点**：两种方式都需要重写`run()`，将线程要执行的逻辑声明在`run()`中



# 四、线程的生命周期

## 1.线程的五种状态

+ 新建（NEW）：当一个Thread类或者其子类New完之后，就是处于这个状态
+ 就绪：当Thread类或者其子类的对象调用`start()`时
+ 运行（RUNNABLE）：当就绪的线程被调度并获得CPU资源时,便进入运行状态
+ 阻塞（BLOCKED）：在某种情况下，被人挂起或执行输入输出操作时，让出CPU或临时中止自己的执行
+ 死亡（TERMINATED）：线程完成了他的全部工作或者线程被强制终止或者线程出现异常并没有处理时。

![image-20210327193029708](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210327193029708.png)



## 2.说明

+ 生命周期的关注点：**哪些状态、以及状态之间的切换**
+ 阻塞只是临时状态，不是最终的状态
+ **最终的状态是死亡**
+ 生命周期需要关注两个概念：状态、方法
+ 关注：状态a  ---> 状态b:哪些方法执行了(回调方法)

# 五、线程的同步

## 1.问题的引出

例子：创建多个窗口卖票，票的总数为100张，使用实现Runnbale接口的方式

+ **问题**：会出现重票、错票的情况
+ 出现的**原因**：当某个线程在操作票的时候，尚未操作完成时，其他线程参与进来，也操作车票。
+ 如何**解决**：当一个线程A在操作票的时候，其他线程不能参与进来。直到线程A操作完ticket时，其他线程才可以开始操作ticket。这种情况即使线程A出现阻塞情况,也不会出现其他线程操作票。

## 2.Java解决线程安全问题方案：同步机制

在Java中我们使用同步机制来解决线程不安全的问题。主要有三种方式解决。

+ 同步代码块
+ 同步方法
+ Lock锁

## 3.<span style='color:red;'>同步代码块</span>处理实现Runnable的线程安全问题

### 1.**<span style='color:red;'>语法</span>**

```java
synchronized(同步监视器){
    // 需要被同步的代码（操作共享数据的代码）
}
```

### 2.**说明**

+ **<span style='color:red;'>操作共享数据的代码</span>，即为需要<span style='color:red;'>被同步的代码</span>**
+ 共享数据：多个线程共同操作的变量。比如：ticket就是共享数据
+ 同步监视器：俗称**锁**，==**<span style='color:red;'>任何一个类的对象，即可充当锁</span>**==
  + **<span style='color:red;'>要求：多个线程必须使用同一把锁</span>**
+ 在实现Runnable接口创建多线程的方式时，我们可以考虑使用`this`来充当同步监视器（锁）
+ 在继承Thread类创建多线程的方式时,我们慎用this，可以考虑使用`类.class`来充当同步监视器（锁）

### 3.代码实现

```java
public class RunnableTicketSafeTest {
    public static void main(String[] args) {
        //3.创建Ticket对象
        Ticket ticket = new Ticket();
        //4.创建Thread类对象
        Thread t1 = new Thread(ticket);
        Thread t2 = new Thread(ticket);
        Thread t3 = new Thread(ticket);
        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");
        //5.调用start()
        t1.start();
        t2.start();
        t3.start();

    }
}

// 1.创建一个类实现Runnable接口
class Ticket implements Runnable {

    private int ticket = 1000;
    Object obj = new Object();

    //2. 实现run方法
    @Override
    public void run() {

        while (true) {
            //同步监视器：俗称锁，即任何类的对象，但是要求：多个线程共用一把锁
            synchronized (obj) {
                //需要被同步的代码
                if (ticket > 0) {
                    System.out.println(Thread.currentThread().getName() + ":卖票, 票号:" + ticket);
                    ticket--;
                } else {
                    break;
                }
            }
        }
    }
}
```

## 4.同步代码块处理继承Thread的线程安全问题

### 1.说明

在继承Thread类创建多线程的方式时,我们慎用this，可以考虑使用`类.class`来充当同步监视器（锁）

### 2.**代码实现**

```java
public class ThreadTicketSafeTest {
    public static void main(String[] args) {
        //3.创建MThread对象
        MThread t1 = new MThread();
        MThread t2 = new MThread();
        MThread t3 = new MThread();

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");
        //5.调用start();
        t1.start();
        t2.start();
        t3.start();

    }
}

//1. 定义一个类继承Thread
class MThread extends Thread {

    private static int ticket = 100;

    //重写run()
    @Override
    public void run() {

        while (true) {
            // 同步代码块 使用类对象
            synchronized (MThread.class) {// Class clazz = MThread.class;反射
                if (ticket > 0) {
                    System.out.println(Thread.currentThread().getName() + "，卖票，票号:" + ticket);
                    ticket--;
                } else {
                    break;
                }
            }
        }
    }
}
```



## 5.同步方法处理实现Runable接口的线程安全问题

**代码实现**

```java
public class SynchronizedRunnableMethodTest {
    public static void main(String[] args) {
        //3.创建Thread类对象，将Ticket4对象作为参数传入
        Ticket4 ticket = new Ticket4();
        Thread t1 = new Thread(ticket);
        Thread t2 = new Thread(ticket);
        Thread t3 = new Thread(ticket);
        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");
        t1.start();
        t2.start();
        t3.start();
    }
}

//1.创建类实现Runnable接口
class Ticket4 implements Runnable {

    private int ticket = 100;

    //2.重写run()
    @Override
    public void run() {
        while (true) {
            sale();
            if (ticket <= 0){
                break;
            }
        }
    }

    private synchronized void sale(){//同步监视器：this
        if (ticket > 0) {
            System.out.println(Thread.currentThread().getName() + "，卖票，票号为:" + ticket);
            ticket--;
        }
    }
}
```



## 6.同步方法处理实现Thread类的线程安全问题

**代码实现**

```java
/*
    例子：三个窗口进行卖票
*/
public class SynchronizedThreadMethodTest {

    public static void main(String[] args) {
        //3.创建Ticket3对象
        Ticket3 t1 = new Ticket3();
        Ticket3 t2 = new Ticket3();
        Ticket3 t3 = new Ticket3();

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");
        t1.start();
        t2.start();
        t3.start();

    }

}

//1.创建类继承Thread
class Ticket3 extends Thread {

    private static int ticket = 100;

    //2.重写方法
    @Override
    public void run() {
        while (true) {
            saleTicket();
            if (ticket <= 0) {
                break;
            }
        }
    }

    private static synchronized void saleTicket() {
        if (ticket > 0) {
            System.out.println(Thread.currentThread().getName() + "，卖票，票号为:" + ticket);
            ticket--;
        }
    }
}
```

## 7.Lock锁方式解决线程安全问题

+ 从JDK5.0开始,Java提供了更强大的线程同步机制——通过显式的定义同步锁对象实现同步。同步锁使用Lock对象充当。
+ `java.util.concurrent.locks.Lock`接口是控制多个线程对共享资源进行访问的工具。锁提供了对共享资源的独占式访问，每次只能有一个线程对Lock对象加锁，线程开始访问资源之前应先获得Lock对象。
+ `ReentrantLock`类实现了`Lock接口`，它拥有与synchronized相同的并发性和内存定义，在实现线程安全的控制中，比较常用的是`ReentrantLock`，可以显示加锁`lock()`、解锁`unlock()`

### 1.实现的步骤

	1. 创建`ReentrantLock对象`   
	2. 通过`ReentrantLock对象.lock()`加锁
	3. 通过`ReentrantLock对象.unlock()`解锁

### 2.**代码实现**

```java
public class LockTest {
    public static void main(String[] args) {
        Window w = new Window();
        Thread t1 = new Thread(w);
        Thread t2 = new Thread(w);
        Thread t3 = new Thread(w);

        t1.setName("窗口1");
        t2.setName("窗口2");
        t3.setName("窗口3");

        t1.start();
        t2.start();
        t3.start();
    }
}


class Window implements Runnable{

    private int ticket = 100;

    //定义ReentrantLock对象
    private Lock lock = new ReentrantLock();


    @Override
    public void run() {
        while (true) {
            lock.lock();
            if (ticket > 0) {
                System.out.println(Thread.currentThread().getName() + "，售票，票号为:" + ticket);
                ticket--;
            } else {
                break;
            }
            lock.unlock();
        }
    }
}
```

**注意**：ReentrantLock构造器有分别有两种

+ `public ReentrantLock()`：抢占式锁
+ `public ReentrantLock(boolean fair)`：true,公平锁，你拿一个我拿一个，false为抢占式锁

### 3.面试题：synchronized 和 lock的异同

+ 相同点：都是用来处理线程安全问题的
+ **不同点**：
  + **synchronized在执行完相应的同步代码之后<span style='color:red;'>自动释放锁</span>**
  + **lock**需要**<span style='color:red;'>手动加锁和手动解锁</span>**



## 8.线程安全的单例模式之懒汉式

**代码实现**

```java
// 线程安全的单例模式之懒汉式
class Bank {

    //1.构造器私有化
    private Bank() {
    }

    //2.定义类对象
    private static Bank instance = null;

    //3.定义一个方法返回对象
    public static Bank getInstance() {

        if (instance == null) {

            synchronized (Bank.class) {
                if (instance == null) {
                    instance = new Bank();
                }
            }
        }
        return instance;
    }
}
```

## 9.死锁的问题

### 1.死锁的理解

不同的线程分别占用对方需要的同步资源不放弃，都在等待对方放弃自己需要的资源，而造成的死锁现象。

### 2.死锁的说明

+ 出现死锁后，不会出现异常，也不会出现提示，只是所有线程都处于死锁状态，无法继续
+ 我们使用同步时，要避免出现死锁。

### 3.死锁的举例

```java
public static void main(String[] args) {

    StringBuffer s1 = new StringBuffer();
    StringBuffer s2 = new StringBuffer();


    new Thread(){
        @Override
        public void run() {

            synchronized (s1){

                s1.append("a");
                s2.append("1");

                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }


                synchronized (s2){
                    s1.append("b");
                    s2.append("2");

                    System.out.println(s1);
                    System.out.println(s2);
                }


            }

        }
    }.start();


    new Thread(new Runnable() {
        @Override
        public void run() {
            synchronized (s2){

                s1.append("c");
                s2.append("3");

                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }

                synchronized (s1){
                    s1.append("d");
                    s2.append("4");

                    System.out.println(s1);
                    System.out.println(s2);
                }
            }
        }
    }).start();
}
```

## 10.同步机制的课后习题

银行有一个账户。

有两个储户分别向同一个账户存3000元，每次存1000，存3次。每次存完打印账户余额。

### 1.分析

```
     是否有多线程？          两个储户
     是否有共享资源          账户
     是否存在线程安全问题     有,操作共同资源
```

### 2.代码实现

```java
/*
银行有一个账户。
    有两个储户分别向同一个账户存3000元，每次存1000，存3次。每次存完打
    印账户余额。

    分析：是否有多线程？          两个储户
         是否有共享资源          账户
         是否存在线程安全问题     有,操作共同资源
*/
public class AccountTest {
    public static void main(String[] args) {

        Account acct = new Account(0);
        Customer c1 = new Customer(acct);
        Customer c2 = new Customer(acct);
        Thread t1 = new Thread(c1);
        Thread t2 = new Thread(c2);

        t1.setName("甲方");
        t2.setName("乙方");

        t1.start();
        t2.start();

    }
}


// 共享资源 账户
class Account {

    private double balance;

    public Account(double balance) {
        this.balance = balance;
    }

    // 存钱操作
    public synchronized void deposite(double amt) {
        if (amt > 0) {
            balance += amt;

            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName() + ":存钱成功，最新余额:" + balance);
        }
    }
}

// 储户线程
class Customer implements Runnable {

    private Account acct;

    public Customer(Account acct) {
        this.acct = acct;
    }

    @Override
    public void run() {
        //每次存1000，存3次
        for (int i = 0; i < 3; i++) {
            acct.deposite(1000);
        }
    }
}
```

# 六、线程的通信

实现线程之间的交互,比如你一下，我一下

## 1.线程通信涉及到的三个方法

+ `wait()`：一旦执行此方法，**当前线程进入阻塞状态**，并且**<span style='color:red;'>释放锁</span>**
+ `notify()`：一旦执行此方法，就会唤醒被wait()的一个线程，如果有多个线程，优先级高的线程会优先被唤醒
+ `notifyAll()`：一旦执行此方法，就会唤醒所有被wait()的线程

## 2.三个方法的说明

+ `wait()、notify()、notifyAll()`都是**Object类中的方法**，不是Thread类的方法
+ `wait()、notify()、notifyAll()`==**<span style='color:red;'>只能在同步方法或者同步代码块中使用</span>**==
+ `wait()、notify()、notifyAll()`的**调用者**必须和**同步方法或者同步代码块中**的同步监视器**<span style='color:red;'>一致</span>**

### 3.代码示例：使用两个线程打印 1-100，线程1, 线程2 交替打印。

使用两个线程打印 1-100，线程1, 线程2 交替打印完成线程通信。

```java
class Number implements Runnable {

    private int number = 1;

    @Override
    public void run() {


        while (true) {

            synchronized (this) {

                // 线程B 唤醒 线程A
                notify();

                if (number <= 100) {
                    System.out.println(Thread.currentThread().getName() + ":,打印了" + number);
                    number++;
                }

                //让线程A阻塞
                try {
                    wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

/*
使用两个线程打印 1-100，线程1, 线程2 交替打印完成线程通信。
*/
public class CommunicationTest {
    public static void main(String[] args) {
        Number number = new Number();

        Thread t1 = new Thread(number);
        Thread t2 = new Thread(number);

        t1.setName("线程A");
        t2.setName("线程B");

        t1.start();
        t2.start();

    }
}
```

## 3.面试题：sleep和wait()的异同

+ **相同点**：一旦执行此方法，都可以让当前线程进入阻塞状态
+ **不同点**：
  + 两个方法声明的**位置不同**，Thread类中声明sleep()，Object类中声明wait()
  + 两个方法调用的**范围不同**，sleep()可以在任何地方调用**,wait()**只能调用在**同步方法或同步代码块中**
  + 关于**<span style='color:red;'>是否释放锁</span>**：**<span style='color:red;'>wait()会</span>释放锁**，**<span style='color:red;'>sleep()不会</span>释放锁**



## 4.生产者和消费者例题

生产者(Productor)将产品交给店员(Clerk)，而消费者(Customer)从店员处取走产品，店员一次只能持有固定数量的产品(比如:20），如果生产者试图生产更多的产品，店员会叫生产者停一下，如果店中有空位放产品了再通知生产者继续生产；如果店中没有产品了，店员会告诉消费者等一下，如果店中有产品了再通知消费者来取走产品。

### 1.**分析**

```
分析：
    是否存在多个线程？   有，生产者，消费者
    是否操作共享数据？   产品（店员）
    是否线程安全？
```

### 2.代码实现

```java
public class ProduceConsumerTest {
    public static void main(String[] args) {
        Clerk clerk = new Clerk();
        Producer p1 = new Producer(clerk);
        p1.setName("生产者1");
        Consumer c1 = new Consumer(clerk);
        c1.setName("消费者1");

        p1.start();
        c1.start();
    }
}

//1.生产者
class Producer extends Thread {

    private Clerk clerk;

    public Producer(Clerk clerk) {
        this.clerk = clerk;
    }

    @Override
    public void run() {
        //生产产品
        clerk.produce();

    }


}

//2.消费者
class Consumer extends Thread {

    private Clerk clerk;

    public Consumer(Clerk clerk) {
        this.clerk = clerk;
    }

    //消费产品
    @Override
    public void run() {
        clerk.consume();
    }
}

//3.共享数据 店员（产品）
class Clerk {

    private int productCount = 0;


    public synchronized void produce() {
        while (true) {
            if (productCount < 20) {
                productCount++;
                System.out.println(Thread.currentThread().getName() + ":生产了" + productCount + "个产品");
                // 生产一个,就要唤醒消费者过来消费一次
                notify();
            } else {
                // 等待,唤醒消费者消费
                try {
                    // wait(),notify(),notifyAll()需要在同步代码快或者同步方法中使用
                    wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    public synchronized void consume() {
        while (true) {
            if (productCount > 0) {
                System.out.println(Thread.currentThread().getName() + ":消费了" + productCount + "个产品");
                productCount--;
                //消费一次,就要唤醒生产者生产一次
                notify();
            } else {
                //唤醒生产者 生产产品
                try {
                    wait();
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```



# 七、JDK5.0新增创建多线程的方式（两种）

## 1.使用实现Callable接口的方式创建多线程

### 1.**实现方式**

	1. 创建一个类实现`Callable`接口
 	2. 实现`call()`，将此线程需要执行的操作声明在`call()内`
 	3. 创建实现`Callable`接口的实现类的对象。
 	4. 创建`FutureTask`类的对象，将实现`Callable`接口的实现类的对象作为参数传递给`FutureTask`的构造器中
 	5. 创建`Thread类`对象，将`FutureTask`对象作为参数传递给`Thread类`的构造器。
 	6. 通过`FutureTask`类的对象调用`get()`，返回call()方法的返回值。

### 2.代码实现

```java
//1.创建一个实现Callable的实现类
class NumThread implements Callable{
    //2.实现call方法，将此线程需要执行的操作声明在call()中
    @Override
    public Object call() throws Exception {
        int sum = 0;
        for (int i = 1; i <= 100; i++) {
            if(i % 2 == 0){
                System.out.println(i);
                sum += i;
            }
        }
        return sum;
    }
}


public class ThreadNew {
    public static void main(String[] args) {
        //3.创建Callable接口实现类的对象
        NumThread numThread = new NumThread();
        //4.将此Callable接口实现类的对象作为传递到FutureTask构造器中，创建FutureTask的对象
        FutureTask futureTask = new FutureTask(numThread);
        //5.将FutureTask的对象作为参数传递到Thread类的构造器中，创建Thread对象，并调用start()
        new Thread(futureTask).start();

        try {
            //6.获取Callable中call方法的返回值
            //get()返回值即为FutureTask构造器参数Callable实现类重写的call()的返回值。
            Object sum = futureTask.get();
            System.out.println("总和为：" + sum);
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }

}
```

### 3.如何理解实现Callable接口要比实现Runnable接口强大？

+ `call()`有返回值。
+ `call()`可以抛出异常
+ `call()`可以指定泛型。



## 2.使用线程池创建多线程

背景：经常创建和销毁、使用量特别大的资源时，并发情况下的线程对性能影响很大

### 1.解决方案

提前创建好多个线程，放入线程池中，使用线程时，从线程池中取出线程，使用完毕之后放回线程池中。可以避免频繁创建和销毁线程，从而达到重复利用。

### 2.实现方式

1. 提供指定数量的线程池
2. 执行指定的线程的操作。需要提供实现Runnable接口或者Callable接口实现类的对象
3. 关闭线程池

### 3.相关API

```
JDK 5.0后提供了线程池相关API： ExecutorService 和 Executors

ExecutorServcie：真正的线程池接口。 常见的实现类：ThreadPoolExcutor
void execute(Runnable command):执行任务/命令，没有返回值，一般用来执行Runnable接口的run()方法
<T> Future<T> submit（Callable<T> task）:执行任务，有返回值，一般执行Callable接口的call()方法
void shutdown():关闭线程池

Executors：工具类、线程池的工厂类，用于创建并返回不同类型的线程池
Executors.newCachedThreadPool():创建一个可根据需要 创建新线程的线程池
Executors.newFixedThreadPool(n)：创建一个固定数量为n线程的线程池
Executors.newSingleThreadExcutor():创建一个只有一个线程的线程池
```



### 4.代码示例

```java
public class ThreadPoolTest {
    public static void main(String[] args) {

        //创建线程池
        ExecutorService service = Executors.newFixedThreadPool(10);

        // 设置线程池的相关属性
        ThreadPoolExecutor threadPoolExecutor = (ThreadPoolExecutor) service;

        threadPoolExecutor.setCorePoolSize(5);

        service.execute(new NumThread());

        service.shutdown();
    }
}

class NumThread implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            if (i % 2 == 0){
                System.out.println(i);
            }

        }
    }
}
```

