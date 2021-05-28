# 1、NoSQL数据库简介

## 1.1技术发展

+ 技术的分类：
  + 1、解决功能性的问题：Java、jsp、Tomcat、HTML、Linux、JDBC、SVN
  + 2、解决扩展性的问题：Spring、SpringMVC、Mybatis、Hibernate
  + 3、解决性能型的问题：NoSQL、Java线程、Hadoop、Nginx、ElasticSearch、MQ

**1、Web1.0时代**

Web1.0的时代，数据访问量很有限，用一夫当关的单点服务器可以解决大部分问题；

![image-20210524115519361](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524115519361.png)

**2、Web2.0时代：**

随着Web2.0时代的到来，用户访问量大幅度提升，同时产生了大量的用户数据。加上后来的智能移动设备的普及，所有互联网都面临着巨大的性能挑战。

![image-20210524115716627](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524115716627.png)

**3、CPU、内存压力的出现以及解决**

![image-20210524115815897](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524115815897.png)

**4、解决IO压力**

![image-20210524115833908](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524115833908.png)

## 1.2 NoSQL数据库

**1、NoSQL数据库概述**

NoSQL(NoSQL = **<span style="color:red">Not Only SQL</span>**) ，意为“不仅仅是SQL”，泛指**<span style="color:red">非关系型的数据库</span>**

NoSQL不依赖业务逻辑方式存储，而以简单的<span style="color:red">key-value</span>模式存储。因此大大增加了数据库的扩展能力。

+ NoSQL的特点：
  + 不遵循SQL标准
  + 不支持ACID。
  + 远超于SQL的性能

+ **<span style="color:red">NoSQL的适用场景</span>**：
  + 对数据高并发的读写
  + 海量数据
  + 对数据高可扩展性的
+ **<span style="color:red">NoSQL不适用的场景</span>**
  + 需要事务支持
  + 基于SQL的结构化查询存储，如处理复杂的关系
  + **<span style="color:red">(用不着SQL的和用了SQL也不行的情况，请考虑使用NoSQL)</span>**

+ **常见的NoSQL：**

  + Memcache
    + 很<span style="color:red">早</span>出现的NoSQL数据库
    + **<span style="color:red">不支持持久化</span>**
    + 支持简单的key-value模式，**支持的类型<span style="color:red">单一</span>**
    + 一般是作为**<span style="color:red">缓存数据库</span>**辅助持久化的数据库

  + Redis
    + 几乎覆盖了Memcache的绝大部分功能
    + **<span style="color:red">支持持久化</span>，主要用作数据备份**
    + <span style="color:red">支持多中类型的数据结构的存储</span>，比如`list、set、hash、zset`
    + 一般是作为**<span style="color:red">缓存数据库</span>**辅助持久化的数据库
  + MongoDB
    + 高性能、开源、模式自由的**<span style="color:red">文档型数据库</span>**
    + 数据都在内存中，如果内存不足，把不常用的数据保存到硬盘中
    + 虽然是key-value模式，但是对value(尤其是**<span style="color:red">json</span>**)提供了丰富的查询
    + 支持二进制数据以及大型对象
    + 可以根据数据的特点**<span style="color:red">替代RDBMS</span>**，成为独立的数据库。或者配置RDBMS，存储特定的数据

## 1.3行式存储数据库(大数据时代)

+ 行式数据库

![image-20210524121252646](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524121252646.png)

+ 列式数据库

![image-20210524121259217](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524121259217.png)

+ 图关系型数据库

主要应用：社会关系、公共交通网络、地图以及网络拓普

![image-20210524121310445](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524121310445.png)

```
数据库排名查看：https://db-engines.com/en/ranking
```



# 2、Redis概述及安装

+ Redis是一个开源的**<span style="color:red">key-value</span>**存储系统。
+ 和Memcache相比，它支持的value类型更多，包括**<span style="color:red">string</span>**(字符串)、**<span style="color:red">list</span>**(链表)、**<span style="color:red">set</span>**(集合)、**<span style="color:red">hash</span>**(哈希类型)、**<span style="color:red">zset</span>**(sorted set --有序集合)。
+ 这些数据类型都支持push/pop、add/remove及取交并集和差集及更丰富的操作，而且这些操作都是**<span style="color:red">原子性</span>**的。
+ 在此基础上，Redis支持各种不同方式的**<span style="color:red">排序</span>**
+ 与Memcache一样，为了保证效率，数据都是**<span style="color:red">缓存在内存</span>**中
+ 区别的是Redis会**<span style="color:red">周期性</span>**的把更新的数据**<span style="color:red">写入磁盘</span>**或者修改操作写入追加的记录文件。
+ 并且在此基础上实现了**<span style="color:red">master-slave(主从)</span>**同步。

## 2.1 应用场景

**1、配合关系型数据库做高速缓存**

+ 高频次、热门访问的数据，降低数据库IO
+ 分布式架构，做session共享

![image-20210524135317261](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524135317261.png)

**2、多样的数据结构存储持久化数据**

![image-20210524135419340](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524135419340.png)

## 2.2 Redis的安装

```
Redis官方网站：http://redis.io
Redis中文官方网站：http://redis.cn/
```

![image-20210524135618758](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524135618758.png)

![image-20210524135659891](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524135659891.png)

### 1. Redis安装版本

+ 6.2.1 for Linux(**<span style="color:red">redis-6.2.1.tar.gz</span>**)
+ 不用考虑在windows环境下对Redis的支持

![image-20210524135837477](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524135837477.png)

### 2. 安装步骤

#### 1.准备工作：下载安装最新版的gcc编译器

+ **安装C语言的编译环境**

  + **第一种方式：`yum install gcc`**

  ![image-20210524153448189](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524153448189.png)

  

  + 第二种方式：

  ```sh
  yum install centos-release-scl scl-utils-build
  yum install -y devtoolset-8-toolchain
  scl enable devtoolset-8 bash 
  ```

+ **测试gcc版本**

**命令：`gcc --version`**

![image-20210524153422794](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524153422794.png)

#### 2.下载redis-6.2.1.tar.gz，上传到/opt目录

#### 3.解压命令：`tar -zxvf redis-6.2.1.tar.gz`

![image-20210524153638368](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524153638368.png)

#### 4.解压完成后进入redis目录：`cd redis-6.2.1`

#### 5.通过`make`命令 进行编译

![image-20210524153735268](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524153735268.png)



#### 6.如果没有准备好C语言编译环境，`make`会报错—Jemalloc/jemalloc.h:没有那个文件

#### 7.解决方案：执行命令`make distclean`

#### 8.在redis-6.2.1目录下再次执行`make`命令

#### 7.编译完成后通过命令 `make install` 进行安装

![image-20210524154014701](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524154014701.png)



## 2.3安装好的redis会默认放在<span style="color:red">/usr/local/bin</span>目录下

![image-20210524154041670](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524154041670.png)

**查看默认的安装目录：**

+ redis-benchmark：性能测试工具，可以在自己的本子上运行，看看自己的性能如何
+ redis-check-aof：修复有问题的AOF文件，rdb和aof后面讲
+ redis-check-dump：修复有问题的dump.rdb文件
+ **<span style="color:red">redis-server</span>：Redis服务器启动程序**
+ **<span style="color:red">redis-cli</span>：redis客户端，操作入口**



## 2.4 前台启动(不推荐)

前台启动，命令行窗口不能关闭，除非服务器停止

## 2.5后台启动(<span style="color:red">推荐</span>)

### 1.备份redis.conf文件

拷贝一份redis.conf到其他目录**：`cp /opt/redis-6.2.1/redis.conf  /etc`**

![image-20210524154228287](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524154228287.png)



### 2.编辑拷贝后的redis.conf文件，设置<span style="color:red">daemonize no </span>改成<span style="color:red">yes</span>

修改redis.conf(128行)文件将里面的daemonize no 改成 yes，让服务在后台启动

### 3.指定etc目录下的redis.conf配置文件 启动Redis

**命令：`redis-server /etc/redis.conf`**

![image-20210524154513230](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524154513230.png)

### 4.启动redis客户端：`redis-cli`

![image-20210524154537969](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524154537969.png)



### 5.多个端口可以：`redis-cli -p6379`

### 6.测试redis客户端是否启动：`ping`

![image-20210524154551783](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524154551783.png)

### 7.Redis关闭

+ **单实例关闭：`redis-cli shutdown`**
+ 也可以进入终端后再关闭
+ **多实例，指定端口关闭：`redis-cli -p 6379 shutdown`**

## 2.6 Redis相关介绍

**Redis端口号6379从何而来**

Alessia  **<span style="color:red">Merz</span>** ：人名，6379对应Merz在手机键盘上的数字

+ **<span style="color:red">Redis相关知识</span>**
  + 默认16个数据库，类似数组下标从0开始，**<span style="color:red">初始默认使用0号库</span>**
  + 使用命令`select dbid`来切换数据库。如：select 8
  + 统一密码管理，所有库使用同样密码
  + **`dbsize`**查看当前数据库的key的数量
  + **`flushdb`：<span style="color:red">清空当前库</span>**
  + **`flushall`：<span style="color:red">清空所有库</span>**

**<span style="color:red">Redis</span>是<span style="color:red">单线程+多路IO复用技术</span>**

多路复用是指使用一个线程来检查多个文件描述符(Socket)的就绪状态，比如调用selecth和poll函数，传入多个文件描述符，如果有一个文件描述符就绪，则返回，否则阻塞直到超时。得到就绪状态后进行真正的操作可以在同一个线程里执行，也可以启动线程执行（比如使用线程池）

**<span style="color:red">Memcache</span>使用的是<span style="color:red">多线程+锁</span>技术**

# 3、Redis常用五大数据类型

获取Redis常见数据类型操作命令：http://www.redis.cn/commands.html

## 3.1 Redis键(key)的简单操作

+ **`keys *`：**查看当前库中所有的key
+ **`exists key`：**判断某个key是否存在
  + 返回1：表示存在
  + 返回0：表示不存在
+ **`type key`：**查看key的数据类型
+ **`del key`：**删除指定的key数据
  + 返回1：表示删除成功
+ **`unlink key` ：<span style="color:red">根据value选择非阻塞删除</span>**
  + 仅将keys从keyspace元数据中删除，真正的删除会在后续异步操作。
+ **`expire key 10`：<span style="color:red">给指定key设置过期时间为10秒钟</span>**

![image-20210524155417177](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524155417177.png)

+ **`ttl key`：<span style="color:red">查看key还有多少秒过期</span>**

  + **-1：永不过期**
  + **-2：表示已过期**

  ![image-20210524155525460](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524155525460.png)

+ `select [dbid]`：切换数据库

+ `dbsize`：查看当前库的key的数量

+ `flushdb`：清空当前库**(<span style="color:red">慎用</span>)**

+ `flushall`：清空所有库**(<span style="color:red">慎用</span>)**

## 3.2 Redis 字符串(string)

### 1.简介

+ String是Redis最基本的类型，你可以理解成与Memcache一模一样的类型，一个key对应一个value
+ String类型是<span style="color:red">**二进制安全的**</span>。意味着Redis中的string可以包含任何数据。比如jpg图片或者序列化的对象。
+ String是Redis最基本的数据类型，**<span style="color:red">一个Redis中字符串value最多可以是512M</span>**

### 2.常用命令

+ **`set <key><value>`：添加键值对**
  + `setnx`：当数据库中**<span style="color:red">key不存在</span>**时，可以将key-value添加到数据库中
  + `setxx`：当数据库中**<span style="color:red">key存在</span>**时，可以将key-value添加数据库，与`setnx`参数互斥。
  + `setex`：添加key-value时候**指定<span style="color:red">超时秒数。</span>**
  + `setpx`：添加key-value时**指定<span style="color:red">超时毫秒数</span>**，与`setex`互斥。
+ **`get <key>`**：获取对应key的value值
+ **`append <key><value>`**：在key的原来值的基础上追加value，返回整个key的整个长度
+ **`strlen <key>`：**获取key对应value的长度
+ **`setnx <key><value>`：**当key不存在时，添加key-value
+ **`incr <key>，是原子操作`**
  + 将key中存储的数字值增加1
  + **<span style="color:red">只能对数字值操作</span>**，如果为空，新增值为1
+ **`decr <key>，是原子操作`**
  + 将key中存储的数字值减少1
  + **<span style="color:red">只能对数字值操作</span>**，如果为空，新增值为-1
+ **`incrby <key> <步长>`：**将key中存储的数字值增加自定义的步长
+ **`decrby <key><步长>`：**将key中存储的数字值减少自定义的步长

![image-20210524165032549](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524165032549.png)

**<span style="color:red">incr和decr都是原子操作；</span>**

所谓**<span style="color:red">原子操作</span>**：是指不会被线程调度机制打断的操作；这种操作一旦开始，就一直运行到结束，中间不会有任何context switch(切换到另一个线程)

(1) 在单线程中，能够在单条指令中完成的操作都可以认为是"原子操作"，因为中断只能发生于指令之间

(2) 在多线程中，不能被其他进程(线程)打断的操作叫原子操作

**<span style="color:red">Redis单命令的原子性主要得益于Redis的单线程。</span>**

```
案例：
	java中的i++是否是原子操作？不是
	i=0;两个线程分别对i进行++100次,值是多少？ 2~
		i=0 		i=0
		i++			
		i=99
 					i++
 					i=1
		i=1
 					i++
					i=100
 
		i++
		i=2
```

+ **`mset <key1><value1><key2><value2>...`：**同时设置一个或多个key-value对
+ **`mget <key1><key2>`：**同时获取一个或多个value
+ **`msetnx <key1><value1><key2><value2><key3><value3>`：**同时设置一个或多个key-value对，但是如果有一个key已经存在数据库中，就设置失败。
+ **`getrange <key> 起始位置 结束位置`：**获取指定范围的值，类似于java的substring，并且[起始位置,结束位置]（前包，后包）
+ **`setrange <key> 起始位置<value>`：**在起始位置添加value
+ **`setex <key><过期时间><value>`：**设置键值的同时，设置过期时间，单位为秒。
+ **`getset<key><value>`：**设置key的值为value，并且返回key的原来的值。

![image-20210524171020607](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524171020607.png)

![image-20210524171033600](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524171033600.png)

### 3.数据结构

Redis中的string数据类型使用的数据结构是简单动态字符串(Simple Dynamic String，简称SDS)。**<span style="color:red">是可以修改的字符串</span>**，内部结构实现上类似于Java的ArrayList，**<span style="color:red">采用预分配冗余空间的方式来减少内存的频繁分配。</span>**

![image-20210524181352589](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524181352589.png)

如图所示，内部为当前字符串实际分配的空间capacity一般要高于实际的字符串长度len，**当字符串长度小于1M时，扩容都是<span style="color:red">加倍现有的空间</span>，如果超过1M，扩容时<span style="color:red">一次只会多扩1M</span>的空间。<span style="color:red">需要注意的是字符串最大长度为512M。</span>**



## 3.3 Redis 列表数据类型(List)

### 1.简介

**<span style="color:red">单键多值</span>**

+ Redis列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部(左边)或者尾部(右边)
+ 它的底层实际是个**<span style="color:red">双向链表</span>**，对两端的操作性能很高，通过索引下标操作中间的节点性能会较差。

![image-20210524174321013](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524174321013.png)

### 2.常用命令

+ **`lpush <key> <value1><value2><value3>`：**从列表左边插入一个或者多个值。
+ **`rpush <key><value1><value2><value3>`：**从列表右边插入一个或者多个值。
+ **`lpop <key>`：从左边吐出一个值。<span style="color:red">值在键在，值光键亡。</span>**
+ **`rpop <key>`：**从右边吐出一个值。**<span style="color:red">值在键在，值光键亡。</span>**
+ **`rpoplpush <key1> <key2>`：**从key1列表中的右边吐出一个值，插入key2列表左边。
+ **`lrange <key> start stop`：**按照索引下标获取元素值(从左到右)
  + **`lrange mylist 0 -1`：获取所有**
+ **`lindex <key> <index>`：**按照索引下标获取元素(从左到右)
+ **`llen <key>`：**获取列表长度
+ **`linsert <key> before/after<value><newValue>`：**在列表key的value值左边/右边插入newValue
+ **`lrem<key> <n><value>`：**从列表左边删除n个value
+ **`lset<key><index><value>`：**将列表指定下标index的值替换成value

**总结**：

+ 增：**`linsert <key> before/after<value><newValue>`**
+ 删：**`lrem<key> <n><value>`**
+ 改：**`lset<key><index><value>`**
+ 查
  + **`llen <key>`**：查长度
  + **`lindex <key> <index>`**：查单个元素
  + **`lrange <key> start stop`：**查所有

![image-20210524181926959](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524181926959.png)

![image-20210524182653354](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524182653354.png)



### 3.数据结构

Redis的list的数据结构为**<span style="color:red">快速链表quickList；</span>**

+ **列表元素较少**的情况：

  + **<span style="color:red">它会使用一块连续的内存存储，这个结构是ziplist，即压缩列表</span>**
  + 它将所有的元素紧挨在一起存储，分配的是一块连续的内存。

+ 当**列表元素较多**的情况才会改用quickList：

  + 因为普通的链表需要的附加指针空间太大，会比较浪费空间。比如这个列表里面存的只是int类型的数据，结构上还需要两个额外的指针prev和next。
  + Redis将链表和ziplist结合起来组成了quicklist。也就是将多个ziplist使用双向指针串起来使用。这样既满足了快速的插入删除性能，又不会出现太大的空间冗余。

  ![image-20210524181140952](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524181140952.png)

## 3.4 Redis集合(Set)

### 1.简介

Redis中set对外提供的功能与list类似，是一个列表的功能。**特殊之处**在于**<span style="color:red">set是可以自动去重的</span>**，**当你需要存储一个列表数据，又不希望出现重复数据时，set是一个很好的选择，并且set提供了判断某个成员是否在一个set集合内的重要接口**，这个也是list所不能提供的。

Redis的set是string类型的**<span style="color:red">无序集合</span>**。**<span style="color:red">它底层其实是一个value为null的hash表</span>**，所以添加、删除、查找的**<span style="color:red">复杂度都是O(1)。</span>**

一个算法，随着数据的增加，执行时间的长短，如果是O(1)，数据增加，查找数据的时间不变。

### 2.常用命令

+ **`sadd<key><value><value>`：**将一个或多个member元素添加到集合key中，已经存在的member将被忽略
+ **`smembers<key>`：**取出该集合中的所有值
+ **`sismeber<key><value>`：**判断集合key中是含有value的值。如果有，返回1，没有返回0
+ **`scard <key>`：**返回该集合的元素个数
+ **`srem <key><value1><value2>`：**删除集合中某个元素
+ **`del <key>`：删除集合**
+ **`spop<key>`：<span style="color:red">随机从key集合中吐出一个值。</span>**
+ **`srandmember<key><n>`：**从集合中随机取出n个值，不会从集合中删除
+ **`smove<source><destination><value>`：**将source集合中的value值移到destination集合中
+ **`sinter<key1><key2>`：**返回两个集合的**<span style="color:red">交集</span>**元素
+ **`sunion<key1><key2>`：**返回两个集合的**<span style="color:red">并集</span>**元素
+ **`sdiff<key1><key2>`：**返回两个集合的**<span style="color:red">差集</span>**元素(ke1中有,key2中没有的)

**<span style="color:red">总结：</span>**

+ 增：
  + **`sadd<key><value><value>`：**添加元素
+ 删
  + **`srem <key><value1><value2>`：**删除元素
  + **`del <key>`**：删除集合
+ 改
  + **`sadd<key><value><value>`：**添加相同key的元素即为修改
+ 查
  + **`smembers<key>`：**查询所有
  + **`scard <key>`：**返回集合中的元素个数
  + **`spop<key>`：<span style="color:red">**：随机吐出一个元素
  + **`srandmember<key><n>`：**随机取出n个元素，不会删除
+ 两个set集合操作：
  + **`sinter<key1><key2>`：**返回两个集合的**<span style="color:red">交集</span>**元素
  + **`sunion<key1><key2>`：**返回两个集合的**<span style="color:red">并集</span>**元素
  + **`sdiff<key1><key2>`：**返回两个集合的**<span style="color:red">差集</span>**元素(ke1中有,key2中没有的)
  + **`smove<source><destination><value>`：**将source集合中的value值移到destination集合中

![image-20210524194709891](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524194709891.png)

![image-20210524194935339](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524194935339.png)



### 3.数据结构

**<span style="color:red">Set数据结构是dict字典，字典是用哈希表实现的。</span>**

Java中的HashSet底层也是用HashMap实现的，只不过所有的value都指向同一个对象。

**<span style="color:red">Redis中的set结构也是一样的，它的内部也使用hash结构，所有的value都指向同一个内部值。</span>**



## 3.5  Redis 哈希(hash)

### 1.简介

+ Redis的hash是一个键值对集合

+ Redis的hash是一个string类型的**<span style="color:red">field和value</span>**的映射表，hash特别适合用于存储对象。

+ 用户ID为查找的key，存储的value为用户对象包含姓名、年龄、生日等信息，如果用普通的key/value结构来存储

+ 主要有以下两种方式：

  + 每次修改用户的某个属性需要，先反序列化改好后再序列化回去。开销较大。

  ![image-20210524205600509](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524205600509.png)

  + 用户ID数据冗余

  ![image-20210524205610993](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524205610993.png)

  + **通过<span style="color:red">key(用户ID) + field(属性标签)</span>就可以操作对应属性数据了，既不需要重复存储数据，也不会带来序列化和并发修改控制的问题**

  ![image-20210524205728007](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524205728007.png)

### 2.常用命令

+ **`hset <key><field><value>`：**给集合中的field键赋值value
+ **`hget <key><field>`：**获取集合key的field的值
+ **`hmset <key1><field1><value1><field2><value2>`：**批量设置hash的值
+ `hexists <key><field>`：查看key中，指定field是否存在
+ **`hkeys <key>`：**列出该hash集合中的所有field
+ **`hvals <key>`：**列出该hash集合中的所有value
+ **`hincrby <key><field><increment>`：**为哈希表key中的域field的值增加increment(步长)
+ **`hsetnx <key><field><value>`：<span style="color:red">当且仅当域field不存在时，</span>**将哈希表key中的域field的值设置为value

![image-20210524210843198](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524210843198.png)

### 3数据结构

**<span style="color:red">Redis的hash类型对应的数据类型有两种：ziplist(压缩列表)、hashtable(哈希表)。当field-value长度较短且个数较少时，使用ziplist，否则使用hashtable。</span>**

## 3.6Redis的有序集合Zset(sorted set)

### 1.简介

+ Redis的有序集合zset与普通集合set非常类似，**zset是一个<span style="color:red">没有重复元素</span>的字符串集合。**
+ 不同之处是zset有序集合的每个成员都关联**<span style="color:red">一个评分(score)，</span>**这个评分(score)被用来按照从最低分到最高分的方式排序集合中的成员。**<span style="color:red">集合的成员是唯一的，但是评分是可以重复的。</span>**
+ 因为元素是有序的，所以你也可以很快的根据评分(score)或者次序(position)来获取一个范围的元素。
+ 访问有序集合的中间元素也是非常快的,因此你能够使用有序集合作为一个没有重复成员的智能列表。

### 2.常用命令

+ **`zadd <key><score1><value1><score2><value2>`：**将一个或者多个member元素及其score值加入到有序集key当中。
+ **`zrange <key><start><stop> [withscores]`：<span style="color:red">返回有序集中，下标在`<start><stop>`之间的元素带`WITHSCORES`，可以让分数一起和值返回到结果集中。</span>**
+ **`zrangebyscore <key> <min> <max> [withscores] [limit offset count]`：**返回有序集key中，所有score值在min和max之间的成员。
+ **`zrevrangebyscore by <key> <min> <max> [withscores] [limit offset count]`：**同上，**改为<span style="color:red">从大到小</span>排序。**
+ **`zincrby <key> <increment><value>`：<span style="color:red">为元素的score加上增量</span>**
+ **`zrem  <key> <value>`：**删除该集合中指定值的元素
+ **`zcount <key> <min> <max>`：<span style="color:red">统计score指定范围内的个数</span>**
+ **`zrank <key> <value>`：<span style="color:red">返回该值在集合key中的排名。</span>**

![image-20210524215542063](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524215542063.png)

### 3.数据结构

SortedSet(zset)是Redis提供的一个非常特别的数据结构，一方面它等价于Java的数据结构`Map<String,Double>`，可以给每一个元素value赋予一个权重score，另一方面它类似于`TreeSet`，内部的元素会按照权重score进行排序，可以得到每个元素的名次，还可以通过score范围来获取元素的列表。

+ zset底层使用两个数据结构：
  + hash，hash的作用就是关联元素的value和权重score，保障元素的唯一性，可以通过元素的value找到对应的score值
  + 跳跃表：跳跃表的目的在于给元素value排序，根据score的范围来获取元素列表。

### 4.跳跃表(跳表)

**1、简介**

​	有序集合在生活中比较常见，例如根据成绩对学生排名，根据得分对玩家排名等。对于有序集合的底层实现，可以用数组、平衡树、链表等。数组不便元素的插入、删除；平衡树或红黑树虽然效率高但结构复杂；链表查询需要遍历所有效率低。Redis采用的是跳跃表。跳跃表效率堪比红黑树，实现远比红黑树简单。

**2、实例**

对比有序链表和跳跃表，从链表中查询出51

（1） 有序链表

![image-20210524215923162](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524215923162.png)

要查找值为51的元素，需要从第一个元素开始依次查找、比较才能找到。共需要6次比较。

（2） 跳跃表

![image-20210524215935905](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210524215935905.png)

从第2层开始，1节点比51节点小，向后比较。

21节点比51节点小，继续向后比较，后面就是NULL了，所以从21节点向下到第1层

在第1层，41节点比51节点小，继续向后，61节点比51节点大，所以从41向下

在第0层，51节点为要查找的节点，节点被找到，共查找4次。

从此可以看出跳跃表比有序链表效率要高



# 4、Redis配置文件介绍

**自定义目录：/etc/redis.conf**

## 4.1###Units单位###

配置大小写单位，配置文件开头定义了一些基本的度量单位，只支持bytes、不支持bit

**大小写不敏感**

![image-20210525090520561](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525090520561.png)

## 4.2 ### Includes 包含###

类似于jsp的include，多实例的情况可以把公用的配置文件提取出来

![image-20210525090644631](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525090644631.png)

## 4.3 ### Network网络相关配置 ###

### 1.bind

默认情况下bind=127.0.0.1，代表只能接收本机的访问请求

不写的情况下，无限制接收任何ip地址的访问(远程访问)

生产环境肯定要写你应用服务器的地址；服务器是需要远程访问的，所以需要将其注释掉

**<span style="color:red">如果开启了protected-mode，那么在没有设定bind ip且没有设密码的情况下，Redis只允许接收本机的响应</span>**

![image-20210525091326371](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525091326371.png)

保存配置，停止服务，重启启动查看进程，不再是本机访问了。

### 2.protected-mode

将本机的protected-mode保护模式设为no

![image-20210525091436259](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525091436259.png)

### 3.port端口号

Redis默认端口号6379

![image-20210525091527855](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525091527855.png)

### 4.tcp-backlog

设置tcp的backlog，backlog其实是一个连接队列，**<span style="color:red">backlog队列总和=未完成三次握手队列+已完成三次握手队列</span>**

在高并发环境下，你需要一个高backlog值来避免慢客户端连接问题

注意Linux内核会将这个值减小到/proc/sys/net/core/somaxconn的值(128)，所以需要确认增大/proc/sys/net/core/somaxconn和/proc/sys/net/ipv4/tcp_max_syn_backlog（128）两个值来达到想要的效果

![image-20210525091941250](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525091941250.png)

### 5.timeout

一个空闲的客户端维持多少秒会关闭，0表示关闭这个功能，即<span style="color:red">永不关闭。</span>

![image-20210525092104120](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525092104120.png)

### 6.tcp-keepalive

对访问客户端的一种心跳检测，每隔n秒检测一次。

单位为秒，如果设置为0，则不会进行keepalive检测，建议设置成60 

![image-20210525092133923](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525092133923.png)



## 4.4 ###GENERAL 通用设置###

### 1.daemonize

是否设置为后台进程，默认设置为no

设置为yes，后台进程

设置为no，前台进程

![image-20210525092446328](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525092446328.png)

### 2.pidfile

存放pid文件的位置，每个实例会产生不同的pid文件

![image-20210525092555848](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525092555848.png)

### 3.loglevel

指定日志记录的级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认**<span style="color:red">notice</span>**

四个级别根据使用阶段来选择，**<span style="color:red">生产环境选择notice或者warning</span>**

![image-20210525092700367](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525092700367.png)

### 4.database 16

设置库的数量为16，默认使用0号库，可以通过`select <dbid>`来切换库。

![image-20210525092900419](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525092900419.png)

### 5.logfile

**指定log文件的输出地址**

![image-20210525093013231](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525093013231.png)

## 4.5 ###SECURITY安全###

### 1.设置密码

![image-20210525093207204](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525093207204.png)

访问密码的查看、设置和取消

在命令中设置密码，只是临时的。重启redis服务器，密码就还原了。

永久设置，需要在配置文件中进行设置

![image-20210525093324753](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525093324753.png)

## 4.6 ### LIMITS限制###

### 1. maxclients

设置redis可以同时与多少个客户端连接，默认10000

如果达到此限制，redis会拒绝新的连接请求，并且向这些连接请求发出“max number of clients reached”以作回应。

![image-20210525093725005](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525093725005.png)

### 2.maxmemory

+ 建议**<span style="color:red">必须设置</span>**，否则，内存满时，会造成服务器宕机。
+ 设置redis可以使用的内存量。一旦达到内存使用上限，redis将会试图移除内部数据，**<span style="color:red">移除规则通过maxmemory-policy来指定。</span>**
+ 如果redis无法根据移除规则来移除内存中的数据，或者设置了“不允许移除”，那么redis则会针对那些需要申请内存的指令返回错误信息，比如SET、LPUSH等。
+ 但是对于无内存申请的指令，仍然会正常响应，比如GET等。如果你的redis是主redis（说明你的redis有从redis），那么在设置内存使用上限时，需要在系统中留出一些内存空间给同步队列缓存，只有在你设置的是“不移除”的情况下，才不用考虑这个因素。

![image-20210525094037640](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525094037640.png)

### 3.memory-policy

+ volatile-lru：使用LRU算法移除key，只对设置了**<span style="color:red">过期时间</span>**的键（最近最少使用）
+ allkeys-lru：在所有集合key中，使用LRU算法移除key
+ volatile-random：在过期集合中移除随机的key，只对设置了过期时间的键
+ allkeys-random：在所有集合key中，移除随机的key
+ volatile-ttl：移除那些ttl值最小的key，即那些最近要过期的key
+ noeviction：不进行移除。针对写操作，只是返回错误信息



![image-20210525094144515](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525094144515.png)

### 4.maxmemory-sample

设置样本数量，LRU算法和最小TTL算法都并非是精确的算法，而是估算值，所以你可以设置样本的大小，redis默认会检查这么多个key并选择其中的LRU的那个

**一般设置3到7的数字，数值越小样本越不准确，但性能消耗越小。**

![image-20210525094554012](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525094554012.png)



# 5、Redis的发布和订阅

## 5.1什么是发布和订阅

Redis的发布订阅(pub/sub)是一种消息通信模式，发送者(pub)发布消息，订阅者(sub)接收消息。

Redis客户端可以订阅任意数量的频道。

## 5.2 Redis的发布和订阅

1、客户端可以订阅频道

![image-20210525095804465](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525095804465.png)

2、当给这个频道发送消息时，消息就会发送给订阅的客户端

![image-20210525095828090](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525095828090.png)

## 5.3 发布订阅命令行实现

1、打开一个客户端，订阅channel1 频道

`subscribe channel1`

![image-20210525100133688](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525100133688.png)

2、打开另一个客户端，给channel1频道发送消息

`publish channel1 helloRedis`

![image-20210525100201305](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525100201305.png)

**返回的1是订阅者的数量**

3、打开订阅channel1频道的客户端，可以接收到消息

![image-20210525100255171](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525100255171.png)

**<span style="color:red">注意：发布的消息没有持久化，订阅者只能收到订阅之后发布的消息。订阅之前发布的消息接收不到。</span>**



# 6、Redis新数据类型

## 6.1Bitmaps新数据类型

### 1.简介

现代计算机用二进制(位)作为信息的基础单位，1个字节等于8位，例如“abc”字符串是由3个字节组成，但实际在计算机存储时将其用二进制表示，“abc”分别对应的ASCII码分是97、98、99，对应的二进制分别是01100001、 01100010和01100011，如下图

![image-20210525102753978](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525102753978.png)

合理地使用操作 位能够有效地提高内存使用率和开发效率

Redis提供了Bitmaps这个“数据类型”可以实现对 位的操作：

（1）Bitmaps本身不是一种数据类型，实际上它就是字符串(key-value)，但是它可以对字符串的位进行操作

（2）Bitmaps单独提供了一套命令，所以在Redis中使用Bitmaps和使用字符串的方法不太相同。**可以把Bitmaps想象成一个以位为单位的数组**，数组的每个单元只能存储0和1，**数组的下标在Bitmaps中叫做<span style="color:red">偏移量</span>**

![image-20210525103131608](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525103131608.png)

### 2.命令

+ **`setbit <key><offset><value>`：**设置Bitmaps中某个偏移量的值(0或者1)

  + offset：偏移量从0开始

  + **命令演示：**

    + 每个独立用户是否访问过网站存放在Bitmaps中， 将访问的用户记做1， 没有访问的用户记做0， 用偏移量作为用户的id。设置键的第offset个位的值（从0算起） ， 假设现在有20个用户，userid=1， 6， 11， 15， 19的用户对网站进行了访问，

    ![image-20210525104004635](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525104004635.png)

    ![image-20210525104240236](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525104240236.png)

+ **`getbit <key><offset>`：**获取Bitmaps中某个偏移量的值

  + **命令演示：**

    + 获取userid=1， 6， 11， 15， 19的值

    ![image-20210525104439310](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525104439310.png)

+ **`bitcount <key>[start end]`：**统计Bitmaps中**从<span style="color:red">start字节</span>到<span style="color:red">end字节</span>value=1**的数量

  + **命令演示**：

    + 统计`users:20210101`的Bitmaps中value=1的数量

    ![image-20210525104545791](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525104545791.png)

+ **`bitop and(or/not/xor) <destkey> key1 key2`：**bitop是一个复合操作，它可以做多个Bitmaps的and(交集)、or(并集)、not(非)、xor(异或)操作，并把结果保存在destkey中

  + 命令演示：

    + 2020-11-04 日访问网站的userid=1,2,5,9。

      setbit unique:users:20201104 1 1

      setbit unique:users:20201104 2 1

      setbit unique:users:20201104 5 1

      setbit unique:users:20201104 9 1

    + 2020-11-03 日访问网站的userid=0,1,4,9。

      setbit unique:users:20201103 0 1

      setbit unique:users:20201103 1 1

      setbit unique:users:20201103 4 1

      setbit unique:users:20201103 9 1

    + 计算出两天都访问过网站的用户数量(and交集)

    ![image-20210525104844638](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525104844638.png)

    ![image-20210525104901099](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525104901099.png)

    + 计算出任意一天都访问过网站的用户数量（例如月活跃就是类似这种） ， 可以使用or求并集

    ![image-20210525104918508](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525104918508.png)



### 3Bitmaps与set对比

假设网站有1亿用户， 每天独立访问的用户有5千万， 如果每天用集合类型和Bitmaps分别存储活跃用户可以得到表

​											**<font size="5">set和Bitmaps存储一天活跃用户对比</font>**

| 数据类型 | 每个用户id占用空间 | 需要存储的用户量 | 全部内存量             |
| :------: | ------------------ | ---------------- | ---------------------- |
| 集合类型 | 64位               | 50000000         | 64位*50000000 = 400MB  |
| Bitmaps  | 1位                | 100000000        | 1位*100000000 = 12.5MB |

很明显， 这种情况下使用Bitmaps能节省很多的内存空间， 尤其是随着时间推移节省的内存还是非常可观的

| set和Bitmaps存储独立用户空间对比 |        |        |       |
| -------------------------------- | ------ | ------ | ----- |
| 数据类型                         | 一天   | 一个月 | 一年  |
| 集合类型                         | 400MB  | 12GB   | 144GB |
| Bitmaps                          | 12.5MB | 375MB  | 4.5GB |

但Bitmaps并不是万金油， 假如该网站每天的独立访问用户很少， 例如只有10万（大量的僵尸用户） ， 那么两者的对比如下表所示， 很显然， 这时候使用Bitmaps就不太合适了， 因为基本上大部分位都是0。

| set和Bitmaps存储一天活跃用户对比（独立用户比较少） |                    |                  |                        |
| -------------------------------------------------- | ------------------ | ---------------- | ---------------------- |
| 数据类型                                           | 每个userid占用空间 | 需要存储的用户量 | 全部内存量             |
| 集合类型                                           | 64位               | 100000           | 64位*100000 = 800KB    |
| Bitmaps                                            | 1位                | 100000000        | 1位*100000000 = 12.5MB |

## 6.2HyperLogLog新数据类型

### 1.简介

在工作中，我们经常会遇到与统计相关的功能需求，比如统计网站PV（PageView页面访问量），可以使用Redis的incr、incrby轻松实现。

但像UV（UniqueVisitor，独立访客）、独立IP数、搜索记录数等需要去重和计数的问题如何解决？**<span style="color:red">这种求集合中不重复元素个数的问题称为基数问题</span>**

**<span style="color:red">解决基数问题有很多种方案：</span>**

（1）数据存储在MySQL表中，使用distinct count计算不重复个数

（2）使用Redis提供的hash、set、bitmaps等数据结构来处理

以上的方案结果精确，但随着数据不断增加，导致占用空间越来越大，对于非常大的数据集是不切实际的。

能否能够降低一定的精度来平衡存储空间？Redis推出了HyperLogLog

Redis HyperLogLog 是用来做基数统计的算法，HyperLogLog 的优点是，在输入元素的数量或者体积非常非常大时，计算基数所需的空间总是固定的、并且是很小的。

在 Redis 里面，每个 HyperLogLog 键只需要花费 12 KB 内存，就可以计算接近 2^64 个不同元素的基数。这和计算基数时，元素越多耗费内存就越多的集合形成鲜明对比。

但是，因为 HyperLogLog 只会根据输入元素来计算基数，而不会储存输入元素本身，所以 HyperLogLog 不能像集合那样，返回输入的各个元素。

**<span style="color:red">什么是基数？</span>**

比如数据集 {1, 3, 5, 7, 5, 7, 8}， 那么这个数据集的基数集为 {1, 3, 5 ,7, 8}, 基数(**不重复元素的个数即基数集的个数**)为5。 基数估计就是在误差可接受的范围内，快速计算基u数。

### 2.命令

+ **`pfadd <key> <element> [element..]`：**添加指定元素到HyperLogLog中

  + **命令示例**

  ![image-20210525112300190](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525112300190.png)

  

+ **`pfcount <key>`：**统计HyperLogLog中的基数个数

  + **命令示例：**

![image-20210525112308218](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525112308218.png)



+ **`pfmerge <destkey> <key1><key2>`：**将一个或者多个HyperLogLog合并后的结果存到destkey中。比如每月活跃用户可以使用每天的活跃用户来合并计算可得

  + 命令示例：

  ![image-20210525112422747](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525112422747.png)

## 6.3 Geospatial新数据类型

### 1.简介

Redis3.2中增加了对GEO类型的支持。GEO，Geographic，地理信息的缩写。该类型，就是元素的2维坐标，在地图上就是经纬度。redis基于该类型，提供了经纬度设置，查询，范围查询，距离查询，经纬度Hash等操作

### 2.命令

+ **`geoadd <key> <longitude> <latitude> <member>`：**添加地址位置(经度、纬度、名称)

  + **命令演示：**

  ![image-20210525114239640](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525114239640.png)

+ **`geopos <key> <member>`：**获取指定地区的经度、纬度

  + **命令演示：**

  ![image-20210525114322428](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525114322428.png)

+ **`geodist <key> <member1> <member2> [m|km|mi|ft]`：**获取两个位置之间的直线距离

  + 单位：
    + m：表示米，默认值
    + km：千米
    + mi：英里
    + ft：英尺
    + 如果用户没有显式地指定单位参数， 那么 GEODIST 默认使用米作为单位
  + **命令演示：**

  ![image-20210525114444229](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525114444229.png)

  

+ **`georadius <key> <longitude><latitude> <radius> [m|km|mi|ft]`：**以给定的经度、纬度为中心、radius为半径，找出这个范围内的元素

  + **命令演示：**

  ![image-20210525114539200](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525114539200.png)



# 7、Jedis操作Redis6

## 7.1 Jedis测试

**<span style="color:red">1、引入jedis所需要的jar包</span>**

```XML
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
            <version>3.2.0</version>
        </dependency>
```

**<span style="color:red">2、Jedis连接Redis注意事项</span>**

1. 禁用Linux的防火墙：Linux(CentOS7)里执行命令

   ```
   systemctl stop firewalld
   ```

2. redis.conf里**<span style="color:red">注释掉bind=127.0.0.1</span>**，并且**<span style="color:red">protected-mode改为no</span>**

**<span style="color:red">3、Jedis常用操作</span>**

1. **测试Jedis是否连接上redis**

```java
public class JedisDemo {

    public static void main(String[] args) {

        //1.创建jedis对象
        Jedis jedis = new Jedis("192.168.158.137",6379);
        String value = jedis.ping();
        System.out.println(value); //返回pong,代表连接成功
    }
}
```

2. **Jedis-API：string**

```java
    //测试string
    @Test
    public void demo1(){
        Jedis jedis = new Jedis("192.168.158.137",6379);
        jedis.set("k1","v1");
        jedis.set("k2","v2");
        jedis.set("k3","v3");

        // keys *
        Set<String> keys = jedis.keys("*");
        for (String key : keys) {
            System.out.println(key);
        }
        Boolean exists = jedis.exists("k1");
        System.out.println(exists);

        Long k3 = jedis.ttl("k3");
        System.out.println(k3); //-1:永不过期

    }
```

3. **Jedis-API：list**

```java
    //测试list
    @Test
    public void demo2(){
        Jedis jedis = new Jedis("192.168.158.137",6379);
        jedis.lpush("q1","b1","b2","b3");
        List<String> lrange = jedis.lrange("q1", 0, -1);
        for (String s : lrange) {
            System.out.println(s);
        }
    }
```

4. **Jedis-API：set**

```java
    //测试set
    @Test
    public void demo3(){
        Jedis jedis = new Jedis("192.168.158.137",6379);
        jedis.sadd("bb","lucy");
        jedis.sadd("bb","tom");
        jedis.sadd("bb","bobby");
        Set<String> names = jedis.smembers("bb");
        for (String name : names) {
            System.out.println(name);
        }

    }
```

5. **Jedis-API：hash**

```java
 //测试hash
    @Test
    public void demo4(){
        Jedis jedis = new Jedis("192.168.158.137",6379);
        jedis.hset("users","name","tom");
        jedis.hset("users","age","20");
        jedis.hset("users","gender","1");

        Set<String> keys = jedis.hkeys("users");
        List<String> values = jedis.hvals("users");
        for (String key : keys) {
            System.out.println(key);
        }

        for (String value : values) {

            System.out.println(value);
        }
    }
```

6. **Jedis-API：zset**

```JAVA
//测试zset
    @Test
    public void demo5(){
        Jedis jedis = new Jedis("192.168.158.137",6379);
        jedis.zadd("lessons",100,"java");
        jedis.zadd("lessons",200,"python");
        jedis.zadd("lessons",300,"mysql");
        jedis.zadd("lessons",400,"redis");

        Set<String> lessons = jedis.zrange("lessons", 0, -1);
        for (String lesson : lessons) {
            System.out.println(lesson);
        }

        Set<String> zrevrange = jedis.zrevrange("lessons", 0, -1);
        for (String s : zrevrange) {
            System.out.println(s);
        }
    }
```

## 7.2 Jedis实例

**1完成一个手机验证码功能**

**要求：**

​	1、输入手机号，点击发送后随机生成6位数字码，2分钟有效

​	2、输入验证码，点击验证，返回成功或失败

​	3、每个手机号每天只能输入3次

**分析：**

![image-20210525151610716](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525151610716.png)

**代码实现：**

```java
public class CheckCode {
    public static void main(String[] args) {
        //获取验证码
        String code = generateCode();
        //设置验证码过期时间
        expireCodeCount(code,"17751228120");
//        //比对验证码
        verifyCode("12312","17751228120");

    }


    //1.生成6位随机数字
    public static String generateCode(){

        String code = "";
        Random random = new Random();
        for (int i = 0; i < 6; i++) {
            int num = random.nextInt(10);
            code += num;
        }
        return code;
    }

    //2.设置两分钟过期时间、并且每天只能输入三次
    public static void expireCodeCount(String code,String phone){

        //1.连接redis
        Jedis jedis = new Jedis("192.168.158.137",6379);
        //2.通过手机号获取redis中次数
        String count = jedis.get(phone+":count");
        if (count == null){
            //第一次
            jedis.setex(phone+":count",24*60*60,"1");
        }else if(Integer.parseInt(count) <= 2){
            jedis.incr(phone+":count");
        }else if (Integer.parseInt(count) > 2){
            System.out.println("每天只能发送三次验证码");
            //关闭redis
            jedis.close();
        }

        //设置验证码的过期时间为两分钟
        jedis.setex(phone+":code",120,code);
        jedis.close();

    }

    //比对输入的验证码与redis的验证码是否一致
    public static void verifyCode(String inputCode,String phone){
        //1.连接redis
        Jedis jedis = new Jedis("192.168.158.137",6379);
        String redisCode = jedis.get(phone + ":code");
        if (redisCode.equals(inputCode)){
            System.out.println("验证码正确");
        }else{
            System.out.println("验证码不正确");
        }

    }
}
```

# 8、Redis与SpringBoot整合

**<span style="color:red">1、在pom.xml文件中引入redis相关依赖</span>**

```xml
		<!-- redis -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-data-redis</artifactId>
		</dependency>

		<!-- spring2.X集成redis所需common-pool2-->
		<dependency>
			<groupId>org.apache.commons</groupId>
			<artifactId>commons-pool2</artifactId>
			<version>2.6.0</version>
		</dependency>
		<!-- web场景 -->
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
```

**<span style="color:red">2、在application.properties配置文件中配置redis相关配置</span>**

```properties
#Redis服务器ip地址
spring.redis.host=192.168.158.137
#Redis服务器连接端口
spring.redis.port=6379
#Redis数据库索引（默认为0）
spring.redis.database= 0
#连接超时时间（毫秒）
spring.redis.timeout=1800s
#连接池最大连接数（使用负值表示没有限制）
spring.redis.lettuce.pool.max-active=20
#最大阻塞等待时间(负数表示没限制)
spring.redis.lettuce.pool.max-wait=-1s
#连接池中的最大空闲连接
spring.redis.lettuce.pool.max-idle=5
#连接池中的最小空闲连接
spring.redis.lettuce.pool.min-idle=0
```

**<span style="color:red">3、添加Redis配置类</span>**

```java
@EnableCaching
@Configuration
public class RedisConfig extends CachingConfigurerSupport {

    @Bean
    public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory factory) {
        RedisTemplate<String, Object> template = new RedisTemplate<>();
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        template.setConnectionFactory(factory);
        //key序列化方式
        template.setKeySerializer(redisSerializer);
        //value序列化
        template.setValueSerializer(jackson2JsonRedisSerializer);
        //value hashmap序列化
        template.setHashValueSerializer(jackson2JsonRedisSerializer);
        return template;
    }

    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory) {
        RedisSerializer<String> redisSerializer = new StringRedisSerializer();
        Jackson2JsonRedisSerializer jackson2JsonRedisSerializer = new Jackson2JsonRedisSerializer(Object.class);
        //解决查询缓存转换异常的问题
        ObjectMapper om = new ObjectMapper();
        om.setVisibility(PropertyAccessor.ALL, JsonAutoDetect.Visibility.ANY);
        om.enableDefaultTyping(ObjectMapper.DefaultTyping.NON_FINAL);
        jackson2JsonRedisSerializer.setObjectMapper(om);
        // 配置序列化（解决乱码的问题）,过期时间600秒
        RedisCacheConfiguration config = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(600))
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(redisSerializer))
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(jackson2JsonRedisSerializer))
                .disableCachingNullValues();
        RedisCacheManager cacheManager = RedisCacheManager.builder(factory)
                .cacheDefaults(config)
                .build();
        return cacheManager;
    }
}
```

**4、测试：在RedisTestController中添加测试方法**

```java
@RestController
@RequestMapping("/redisTest")
public class RedisTestController {

    @Autowired
    private RedisTemplate redisTemplate;

    @GetMapping
    public String redisTest(){
        redisTemplate.opsForValue().set("weight","500kg");

        String weight = (String) redisTemplate.opsForValue().get("weight");

        return weight;

    }
}
```



# 9、Redis-事务-锁机制-秒杀

## 9.1Redis事务的定义

![image-20210525171144460](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525171144460.png)

Redis事务是**一个单独的隔离操作**：事务中的所有命令都会序列化、有顺序的执行。事务在执行过程中，不会被其他客户端发来的命令请求锁打断。

**Redis的事务的作用**：**<span style="color:red">串联多个命令</span>**防止别的命令插队。

## 9.2 Multi、Exec、Discard

从输入**`Multi`**开始，输入的命令都会进入命令队列中，但不会被执行，直到输入**`Exec`**命令后，Redis才会将命令队列中的命令依次执行。

组队阶段可以通过**`discard`**命令来放弃组队。

![image-20210525171751156](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525171751156.png)

**事务示例：**

![image-20210525172046665](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525172046665.png)

## 9.3 事务的错误处理

+ 如果组队过程中出现命令错误，那么在执行时整个命令队列的所有命令都会被取消

![image-20210525172238709](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525172238709.png)

+ **错误示例**

![image-20210525172506471](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525172506471.png)



+ 如果组队过程中没有出现错误，但在执行过程中某个命令出错，那么这个命令不会被执行，但是其他命令会被正常执行

![image-20210525172246535](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525172246535.png)

+ **错误示例**

![image-20210525172730716](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525172730716.png)

## 9.4事务冲突的问题以及解决方案

### 1.例子

一个请求想给金额减8000

一个请求想给金额减5000

一个请求想给金额减1000

![image-20210525174857502](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525174857502.png)

### 2.事务冲突解决方案1—悲观锁

![image-20210525174951028](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525174951028.png)

**<span style="color:red">悲观锁（Pessimistic Lock）</span>**，顾名思义，就是很悲观，**每次去拿数据的时候都认为别人会修改，所以<span style="color:red">每次在拿数据的时候先上锁</span>，**这样别人就拿不到数据了。等操作完成之后释放锁。**==<span style="color:red">传统的关系型数据库里边就有很多用到了这种锁机制</span>==**，比如**<span style="color:red">行锁，表锁</span>**等，**<span style="color:red">读锁，写锁</span>**等，都是在操作之前先上锁。

### 3.事务冲突解决方案2—乐观锁

![image-20210525175412288](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525175412288.png)

**<span style="color:red">乐观锁（Optimistic Lock），</span>**顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改数据，**所以不会上锁**，但是在更新的时候会判断一下在此期间有没有人更新这个数据，可以使用**==<span style="color:red">版本号机制</span>==**（**<span style="color:red">在修改数据之前，先判断自己拿到的版本号与数据的版本号是否一致，不一致先更新版本号再进行修改,一致就直接修改数据</span>**）。**<span style="color:red">==乐观锁适合于多读的应用类型，这样可以提高吞吐量==。Redis就是利用这种check-and-set机制实现事务的。</span>**

### 4.watch key [key...]

在执行`multi`之前，执行`watch key [key..]` 可以监视一个或者多个key。如果在执行事务之前，这个key被其他命令所改动，那么事务将会被打断。

![image-20210525182116713](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525182116713.png)

![image-20210525182305667](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210525182305667.png)

### 5.unwatch

取消**`WATCH`**命令**对所有key的监视**

如果在执行`watch`命令之后，`exec`命令或`discard`命令先被执行了的话，那么就不需要再执行`unwatch`命令了

## 9.5 Redis事务三大特性

+ **<span style="color:red">单独的隔离操作</span>**
  + 事务中的所有命令都会被序列化、按顺序地执行。事务在执行过程中不会被其他客户端发送来的命令请求锁打断。
+ **<span style="color:red">没有隔离级别的概念</span>**
  + 命令队列中的命令没有`exec`之前都不会被实际执行，因为事务提交之前任何指令都不会被实际执行
+ **<span style="color:red">不保证原子性</span>**
  + 如果**组队阶段没有报错**，在执行阶段出现错误，只有错误的命令不会被执行，其他正确的命令依然被执行
  + 如果是在**组队阶段报了错**，那么执行的时候所有命令**都不会执行**

# 10、Redis-事务-秒杀案例

## 10.1 解决计数器和人员记录的事务操作

![image-20210526010718823](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526010718823.png)

## 10.2 无并发情况下的秒杀案例

**<span style="color:red">1、前端页面-单击秒杀按钮</span>**

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
         pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
  <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
  <title>Insert title here</title>
</head>
<body>
<h1>iPhone 13 Pro !!!  1元秒杀！！！
</h1>


<form id="msform" action="${pageContext.request.contextPath}/doseckill" enctype="application/x-www-form-urlencoded" method="post">
  <input type="hidden" id="prodid" name="prodid" value="0101">
  <input type="button"  id="miaosha_btn" name="seckill_btn" value="秒杀点我"/>
</form>

</body>
<script  type="text/javascript" src="${pageContext.request.contextPath}/script/jquery/jquery-3.1.0.js"></script>
<script  type="text/javascript">
    $(function(){
        $("#miaosha_btn").click(function(){
            var url=$("#msform").attr("action");
            $.post(url,$("#msform").serialize(),function(data){
                if(data=="false"){
                    alert("抢光了" );
                    $("#miaosha_btn").attr("disabled",true);
                }
            } );
        })
    })
</script>
</html>
```

**<span style="color:red">2、form表单提交到Servlet</span>**

```java

/**
 * 秒杀案例
 */
public class SecKillServlet extends HttpServlet {
	private static final long serialVersionUID = 1L;

    public SecKillServlet() {
        super();
    }

	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

		String userid = new Random().nextInt(50000) +"" ;
		String prodid =request.getParameter("prodid");
		
		boolean isSuccess=SecKill_redis.doSecKill(userid,prodid);
//		boolean isSuccess= SecKill_redisByScript.doSecKill(userid,prodid);
		response.getWriter().print(isSuccess);
	}

}
```

**<span style="color:red">3、SecKill_redis.doSecKill(userid,prodid);方法</span>**

```java
//秒杀过程
	public static boolean doSecKill(String uid,String prodid) throws IOException {
		//1.对传过来的uid、proid进行校验
		if (uid ==null || prodid == null){
			return false;
		}
		//2.连接redis
		Jedis jedis = new Jedis("192.168.158.137",6379);
		//3.拼接key
		//3.1拼接userkey
		String userKey = "sk:" + prodid + ":user";
		//3.2拼接propkey
		String kcKey = "sk:" + prodid +":qt";  //sk:0101:qt

		//4.判断库存数量是否为空
		String kc = jedis.get(kcKey);
		if (kc == null){
			System.out.println("秒杀活动还未开始..");
			jedis.close();
			return false;
		}

		//5.判断库存数量是否0
		if (Integer.parseInt(kc) < 1){
			System.out.println("秒杀活动已结束..");
			jedis.close();
			return false;
		}

		//6.判断用户是否已经抢购过了
		if (jedis.sismember(userKey,uid)){
			//如果用户存在redis中,表示用户已经抢过商品了
			System.out.println("不能重复抢购商品");
			jedis.close();
			return false;
		}

		//7.秒杀
		//7.1库存-1
		jedis.decr(kcKey);
		//7.2把秒杀成功用户添加清单里面
		jedis.sadd(userKey,uid);
		System.out.println("秒杀成功了..");
		jedis.close();
		return true;
	}
```



## 10.3 使用Linux中的ab工具模拟并发秒杀

使用工具ab模拟测试

+ CentOS6：默认安装了
+ CentOS7：需要手动安装

### 1.联网情况下：通过`yum install httpd-tools`命令安装

![image-20210526152011262](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526152011262.png)

### 2.无网络情况下的安装

（1） 进入cd  /run/media/root/CentOS 7 x86_64/Packages（路径跟centos6不同）

（2） 顺序安装

apr-1.4.8-3.el7.x86_64.rpm

apr-util-1.5.2-6.el7.x86_64.rpm

httpd-tools-2.4.6-67.el7.centos.x86_64.rpm  

### 3.通过ab工具进行测试

![image-20210526152719940](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526152719940.png)

+ 语法：`ab [options] [http[s]://]hostname[:port]/path`
+ 常用参数：
  + `-n`：表示总共多少请求
  + `-c`：表示并发数量
  + `-p`：表示参数文件的路径
  + `-T`：需要携带application/x-www-form-urlencoded参数

vim postfile 模拟表单提交参数,以&符号结尾;存放当前目录。

![image-20210526153253270](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526153253270.png)

**并发环境下出现的问题：**

![image-20210526154504307](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526154504307.png)

![image-20210526154530133](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526154530133.png)

这种情况的问题叫做**<span style="color:red">超卖问题</span>**

**还有一种问题是：多并发环境下，<span style="color:red">连接redis超时！</span>**



## 10.4 超卖问题

**<span style="color:red">什么是超卖问题？</span>**

库存会出现-N情况；

![image-20210526154755000](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526154755000.png)

## 10.5 利用Redis连接池解决连接超时问题

![image-20210526160402420](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526160402420.png)

+ **节省每次连接redis服务带来的消耗，把连接好的实例反复利用。**

+ **通过<span style="color:red">参数</span>管理连接的行为**
+ **<span style="color:red">Jedis连接池工具类：解决连接超时问题</span>**

```java
public class JedisPoolUtil {
	private static volatile JedisPool jedisPool = null;

	private JedisPoolUtil() {
	}

	public static JedisPool getJedisPoolInstance() {
		if (null == jedisPool) {
			synchronized (JedisPoolUtil.class) {
				if (null == jedisPool) {
					JedisPoolConfig poolConfig = new JedisPoolConfig();
					// 控制一个Pool可以分配多少个jedis实例
					poolConfig.setMaxTotal(200);
					// 设置一个Pool最多有多少个空闲的Jedis实例
					poolConfig.setMaxIdle(32);
					// 表示连接的最大等待毫秒数
					poolConfig.setMaxWaitMillis(100*1000);
					poolConfig.setBlockWhenExhausted(true);
					// 获得一个Jedis实例的时候是否检查连接可用性(ping())
					poolConfig.setTestOnBorrow(true);  // ping  PONG
				 
					jedisPool = new JedisPool(poolConfig, "192.168.158.137", 6379, 60000 );
				}
			}
		}
		return jedisPool;
	}

	public static void release(JedisPool jedisPool, Jedis jedis) {
		if (null != jedis) {
			jedisPool.returnResource(jedis);
		}
	}

}
```

+ 连接池参数：
  +  MaxTotal：控制一个pool可分配多少个jedis实例，通过pool.getResource()来获取；如果赋值为-1，则表示不限制；如果pool已经分配了MaxTotal个jedis实例，则此时pool的状态为exhausted。
  + maxIdle：控制一个pool最多有多少个状态为idle(空闲)的jedis实例；
  + MaxWaitMillis：表示当borrow一个jedis实例时，最大的等待毫秒数，如果超过等待时间，则直接抛JedisConnectionException；
  + testOnBorrow：获得一个jedis实例的时候是否检查连接可用性（ping()）；如果为true，则得到的jedis实例均是可用的；

## 10.6 利用乐观锁解决超卖问题

+ **<span style="color:red">分析：利用==乐观锁的版本号机制==，在每次操作库存时候都先判断一下本号</span>**

![image-20210526160215283](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526160215283.png)

+ **<span style="color:red">思路：</span>`通过multi`开启事务，将库存减1的命令和添加用户清单的命令添加到组队队列中，再通过`exec`进行执行命令。**
+ **代码实现：**

![image-20210526161853861](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526161853861.png)

```java
public class SecKill_redis {

	public static void main(String[] args) {
		Jedis jedis =new Jedis("192.168.158.137",6379);
		System.out.println(jedis.ping());
		jedis.close();
	}

	//秒杀过程
	public static boolean doSecKill(String uid,String prodid) throws IOException {
		//1.对传过来的uid、proid进行校验
		if (uid ==null || prodid == null){
			return false;
		}
		//2.连接redis
		//Jedis jedis = new Jedis("192.168.158.137",6379);
		//通过连接池获取Jedis实例：解决连接超时问题
		JedisPool jedisPoolInstance = JedisPoolUtil.getJedisPoolInstance();
		Jedis jedis = jedisPoolInstance.getResource();

		//3.拼接key
		//3.1拼接userkey
		String userKey = "sk:" + prodid + ":user";
		//3.2拼接propkey
		String kcKey = "sk:" + prodid +":qt";  //sk:0101:qt

		//###对库存进行监视
		jedis.watch(kcKey);

		//4.判断库存数量是否为空
		String kc = jedis.get(kcKey);
		if (kc == null){
			System.out.println("秒杀活动还未开始..");
			jedis.close();
			return false;
		}

		//5.判断库存数量是否0
		if (Integer.parseInt(kc) < 1){
			System.out.println("秒杀活动已结束..");
			jedis.close();
			return false;
		}

		//6.判断用户是否已经抢购过了
		if (jedis.sismember(userKey,uid)){
			//如果用户存在redis中,表示用户已经抢过商品了
			System.out.println("不能重复抢购商品");
			jedis.close();
			return false;
		}


		//###利用Redis事务的版本号机制解决超卖问题
		Transaction multi = jedis.multi();
		//### 将命令加入到队列中
		multi.decr(kcKey);
		multi.sadd(userKey,uid);
		//### 执行队列中的命令
		List<Object> results = multi.exec();

		if (results == null || results.size() == 0){
			System.out.println("秒杀失败...");
			jedis.close();
			return false;
		}

		//7.秒杀
		//7.1库存-1
		//jedis.decr(kcKey);
		//7.2把秒杀成功用户添加清单里面
		//jedis.sadd(userKey,uid);
		System.out.println("秒杀成功了..");
		jedis.close();
		return true;
	}
}
```

+ **redis中最终的库存数量：**

![image-20210526161946890](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526161946890.png)

![image-20210526162040447](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526162040447.png)



## 10.7 解决库存遗留问题

### 1.LUA脚本

![image-20210526171227196](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526171227196.png)

Lua是一个小巧的脚本语言，Lua脚本可以很容易的被C/C++ 代码调用，也可以反过来调用C/C++的函数，Lua并没有提供强大的库，一个完整的Lua解释器不过200k，所以Lua不适合作为开发独立应用程序的语言，而是作为**<span style="color:red">嵌入式脚本语言。</span>**

很多应用程序、游戏使用LUA作为自己的嵌入式脚本语言，以此来实现可配置性、可扩展性。

这其中包括魔兽争霸地图、魔兽世界、博德之门、愤怒的小鸟等众多游戏插件或外挂。

https://www.w3cschool.cn/lua/

### 2.LUA脚本在Redis中的优势

将复杂的或者多步的redis操作，写为一个脚本，一次提交给redis执行，**减少反复连接redis的次数。提升性能**

LUA脚本是类似redis事务，有一定的原子性，不会被其他命令插队，可以完成一些redis事务性的操作。

但是注意redis的lua脚本功能，只有在Redis 2.6以上的版本才可以使用。

利用lua脚本淘汰用户，解决超卖问题。

**redis 2.6版本以后，**通过lua脚本解决**<span style="color:red">争抢问题</span>**，实际上是**<span style="color:red">redis 利用其单线程的特性，用任务队列的方式解决多任务并发问题。</span>**

![image-20210526171631581](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526171631581.png)

## 10.8Redis-事务-秒杀案例-代码

### 1.项目结构

![image-20210526171725046](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526171725046.png)

### 2.第一版：简单版

老师点10次，正常秒杀

同学一起点试一试，秒杀也是正常的。这是因为还达不到并发的效果。

**使用工具ab模拟并发测试，会出现<span style="color:red">超卖</span>情况。查看库存会出现<span style="color:red">负数。</span>**

### 3.第二版：加事务-乐观锁(解决超卖问题)，但出现库存遗留问题和连接超时问题

### 4.第三版：连接池解决超时问题

### 5.第四版：解决库存依赖问题，LUA脚本

```lua
local userid=KEYS[1]; 
local prodid=KEYS[2];
local qtkey="sk:"..prodid..":qt";
local usersKey="sk:"..prodid.":usr'; 
local userExists=redis.call("sismember",usersKey,userid);
if tonumber(userExists)==1 then 
  return 2;
end
local num= redis.call("get" ,qtkey);
if tonumber(num)<=0 then 
  return 0; 
else 
  redis.call("decr",qtkey);
  redis.call("sadd",usersKey,userid);
end
return 1;
```

# 11、Redis持久化—RDB

![image-20210526182742573](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526182742573.png)



+ Redis提供两种持久化方式
  + RDB（Redis DataBase）
  + AOF（Append Only File）

## 11.1 什么是RDB

在**<span style="color:red">指定的时间间隔内</span>**将内存中的数据集**<span style="color:red">快照</span>**写入磁盘中。也就是行话讲的**`snapshop`快照**，它恢复时是将快照文件直接读到内存中。

## 11.2 RDB是如何持久化的(备份)

Redis会单独创建fork子进程来进行持久化操作，**首先会将数据写入到一个临时文件中，等持久化过程都结束了，再将临时文件把上次那个已经持久化好的rdb文件替换掉。**整个过程中主进程是不进行IO操作的，这就确保了极高的性能 。**<span style="color:red">如果要进行大规模数据的恢复且对于数据恢复的完整性不是非常敏感，那RDB方式要比AOF方式更加高效。</span>==RDB的缺点==**是<span style="color:red">最后一次持久化后的数据可能会丢失。</span>

## 11.3 Fork子进程

+ Fork的作用是复制一个与当前进程一样**<span style="color:red">进程。</span>**新进程的所有数据(变量、环境变量、程序计数器)数值都和原进程一致，并且**<span style="color:red">是原进程的子进程。</span>**
+ 在Linux程序中，fork()会产生一个和父进程完全相同的子进程。但子进程在此后多会`exec`系统调用，出于效率考虑，Linux中引入了**<span style="color:red">“写时复制技术”</span>**
+ **一般情况父进程与子进程都会共用同一段物理内存，**只有进程空间的各段的内容要发生变化时，才会将父进程的内容复制一份给子进程。

## 11.4 RDB持久化流程

![image-20210526184358987](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526184358987.png)

## 11.5 RDB的配置文件

**<span style="color:red">在redis.conf中配置文件名称</span>**，默认是**`dump.rdb`**

![image-20210526184622917](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526184622917.png)

## 11.6 RDB配置文件的默认位置

rdb配置文件的保存位置，也可以进行修改，**<span style="color:red">默认为Redis启动时命令所在的目录下。</span>**

![image-20210526184813803](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526184813803.png)

## 11.7 如何触发RDB快照：保存策略（重点）

### 1.配置文件中默认的快照间隔和key

![image-20210526185145964](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526185145964.png)

+ After 3600 seconds (an hour) if at least 1 key changed：一个小时内至少1个key发生改变时触发
+ After 300 seconds (5 minutes) if at least 100 keys changed：5分钟内至少100个key发生改变时触发
+ After 60 seconds if at least 10000 keys changed：1分钟内至少1W个key发生改变时触发
+ **<span style="color:red">发生改变的key越多，时间间隔就越短。</span>**

### 2.命令`save`和`bgsave`开启手动保存还是自动保存

+ **`save`：<span style="color:red">手动保存</span>**，save只管保存，其他不管，全部阻塞。**不建议**
+ **`bgsave`：<span style="color:red">redis会在后台异步进行快照操作，快照同时还可以响应客户端请求</span>**
+ 可以通过`lastsave`命令获取最后一次成功执行快照(持久化)的时间

### 3.flushall命令

执行flushall命令，也会产生dump.rdb，但里面是空的，无意义

### 4.stop-writes-on-bgsave-error yes

![image-20210526190112085](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526190112085.png)

**<span style="color:red">当Redis无法写入磁盘的时候，直接关掉Redis的写操作。推荐yes</span>**

### 5.rdbcompression压缩文件

![image-20210526190221893](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526190221893.png)

对于存储到磁盘中的快照，可以设置是否进行压缩存储。如果是的话，redis会采用**<span style="color:red">LZF算法</span>**进行压缩。

如果你不想消耗CPU来进行压缩的话，可以设置为关闭此功能。推荐yes.

### 6.rdbchecksum检查完整性

![image-20210526190433202](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526190433202.png)

在存储快照后，还可以让redis使用CRC64算法来进行数据校验，

但是这样做会增加大约10%的性能消耗，如果希望获取到最大的性能提升，可以关闭此功能

推荐yes.

### 7.rdb的备份（重点掌握）

先通过`config get dir` 查询rdb的文件目录

将*.rdb文件拷贝一份，到其他目录（方便以后的恢复）

+ **<span style="color:red">rdb的恢复</span>**
  + **<span style="color:red">关闭redis</span>**
  + **<span style="color:red">将拷贝的rdb文件复制到redis-server命令所在目录</span>**
  + **<span style="color:red">启动redis，备份数据会直接加载</span>**

## 11.8 RDB优势

+ 适合大规模的数据恢复
+ 对数据的完整性不是非常敏感的推荐使用
+ 恢复速度快

![image-20210526190905164](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526190905164.png)

## 11.9 RDB劣势

+ fork的时候，内存中的数据会被克隆一份，大致2倍的膨胀性需要考虑
+ 比较消耗性能
+ 在备份周期一定时间间隔做一次备份，所以如果redis意外宕机，就会丢失最后一次快照后的所有修改

## 11.10 如何停止RDB

**动态停止RDB：`redis-cli config set save “”` <span style="color:red">#给save后给空值，表示禁用保存策略</span>**

## 11.11 小结

![image-20210526191248355](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526191248355.png)



## 11.12 测试

1. redis.conf配置保存策略
2. 往redis中在30秒内存入11个key会触发rdb

![image-20210526191807432](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526191807432.png)

3. 进行快照恢复；删除dump.rdb，将dump.rdb.bak修改为dump.rdb，重新启动redis-server自动加载快照

![image-20210526192145987](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526192145987.png)

恢复成之前的数据了 。



# 12、Redis持久化之AOF

## 12.1什么是AOF（Append only File）

**<span style="color:red">以日志的形式记录每个写操作</span>**，将Redis执行过的所有**<span style="color:red">写指令</span>**记录下来（读操作不记录），**<span style="color:red">只许追加文件但不可以改写文件。</span>**Redis启动的时候会读取该文件并且重新加载数据。

## 12.2 AOF持久化流程

1. 每次客户端的请求**<span style="color:red">写命令</span>**都会被追加到AOF缓冲区内；
2. AOF根据AOF持久化策略【always，everysec，no】同步策略将操作sync同步到磁盘的AOF文件中；
3. AOF文件大小超过**重写**策略或手动重写时，会对AOF文件rewrite重写，压缩AOF文件容量
4. Redis重启，会重新加载AOF文件达到数据恢复的目的

![image-20210526213738036](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526213738036.png)



## 12.3 AOF默认不开启

可以在redis.conf配置文件中，开启AOF **`appendonly yes`**

**<span style="color:red">AOF保存的路径与rdb一样，会保存到redis-server所在目录</span>**

![image-20210526211730373](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526211730373.png)



## 12.4 AOF和RDB同时开启，redis听谁的？

**AOF和RDB同时开启，<span style="color:red">redis默认取AOF的数据（数据不会存在丢失）</span>**

## 12.5 AOF启动/修复/恢复

+ AOF的备份机制和性能虽然和RDB不同，**<span style="color:red">但是备份和恢复的操作同RDB一样，都是复制备份文件，需要恢复时再将备份文件复制到redis-server所在目录，启动redis重新加载即可。</span>**
+ 正常修复
  + 修改默认的`appendonly no` 改为yes
  + 将有数据的aof文件复制一份到对应目录
  + 重启redis会默认重新加载
+ `appendonly.aof`配置文件损坏修复
  + 修改默认的`appendonly no` 改为yes
  + 如果遇到AOF文件损坏，通过`/usr/local/bin/redis-check-aof --fix appendonly.aof`进行修复
  +  备份被写坏的AOF文件
  +  恢复：重启redis，然后重新加载

## 12.6 AOF同步频率设置

+ **`appendfsync always`：始终同步，每次Redis的写入都会立刻记入日志；**性能较差但数据完整性较好
+ **`appendfsync everysec`：每秒同步，每秒记入日志一次，如果有宕机，本秒的数据可能丢失**
+ **`appendfsync no`：**redis不主动同步，**<span style="color:red">把同步时机交给操作系统</span>**

## 12.7 Rewrite 压缩

### **1、rewrite是什么**

AOF文件采用追加的方式，文件会越来越大，为了避免出现这个情况，新增了重写机制，当AOF文件的大小超过所设定的阀值时，Redis就会启动AOF的文件内容压缩， 只保留可以恢复数据的最小指令集.可以使用命令bgrewriteaof

### **2、重写原理、如何实现重写**

AOF文件持续增长而过大时，会fork出一条新进程来将文件重写(也是先写临时文件最后再rename)，**<span style="color:red">redis4.0版本后的重写，是指就是把rdb 的快照，以二级制的形式附在新的aof头部，作为已有的历史数据，替换掉原来的流水账操作。</span>**

no-appendfsync-on-rewrite：

如果 no-appendfsync-on-rewrite=yes ,不写入aof文件只写入缓存，用户请求不会阻塞，但是在这段时间如果宕机会丢失这段时间的缓存数据。（降低数据安全性，提高性能）

​	如果 no-appendfsync-on-rewrite=no,  还是会把数据往磁盘里刷，但是遇到重写操作，可能会发生阻塞。（数据安全，但是性能降低）

触发机制，何时重写

Redis会记录上次重写时的AOF大小，默认配置是当AOF文件大小是上次rewrite后大小的一倍且文件大于64M时触发

**<span style="color:red">重写虽然可以节约大量磁盘空间，减少恢复时间。但是每次重写还是有一定的负担的，因此设定Redis要满足一定条件才会进行重写。</span>** 

auto-aof-rewrite-percentage：设置重写的基准值，文件达到100%时开始重写（文件是原来重写后文件的2倍时触发）

auto-aof-rewrite-min-size：设置重写的基准值，最小文件64MB。达到这个值开始重写。

例如：文件达到70MB开始重写，降到50MB，下次什么时候开始重写？100MB

系统载入时或者上次重写完毕时，Redis会记录此时AOF大小，设为base_size,

**如果<span style="color:red">Redis的AOF当前大小>= base_size +base_size*100% (默认)且当前大小>=64mb(默认)</span>的情况下，Redis会对<span style="color:red">AOF进行重写。</span>** 

### 3、重写流程

（1）bgrewriteaof触发重写，判断是否当前有bgsave或bgrewriteaof在运行，如果有，则等待该命令结束后再继续执行。

（2）主进程fork出子进程执行重写操作，保证主进程不会阻塞。

（3）子进程遍历redis内存中数据到临时文件，客户端的写请求同时写入aof_buf缓冲区和aof_rewrite_buf重写缓冲区保证原AOF文件完整以及新AOF文件生成期间的新的数据修改动作不会丢失。

（4）1).子进程写完新的AOF文件后，向主进程发信号，父进程更新统计信息。2).主进程把aof_rewrite_buf中的数据写入到新的AOF文件。

（5）使用新的AOF文件覆盖旧的AOF文件，完成AOF重写。

![image-20210526214310095](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526214310095.png)



## 12.8 AOF 优势

![image-20210526213108018](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526213108018.png)

+ 备份机制更稳健，数据丢失的概率更低
+ 可读的日志文件，可以处理文件损坏。

## 12.9 AOF劣势

+ 比起RDB占用更多的磁盘空间
+ 恢复备份速度要慢
+ 每次读写都同步的话，有一定的性能压力
+ 存在个别Bug，造成恢复不能。

## 12.10 小总结

![image-20210526213249235](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526213249235.png)

## 12.11 AOF和RDB用哪个好

1. 官方推荐两个都启用。
2. 数据量大，且对数据不敏感，使用RDB
3. 不建议单独使用AOF，因为可能出现bug
4. 如果只是做内存缓存，可以都不用

**官网建议：**

![image-20210526213410928](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210526213410928.png)

l RDB持久化方式能够在指定的时间间隔能对你的数据进行快照存储

l AOF持久化方式记录每次对服务器写的操作,当服务器重启的时候会重新执行这些命令来恢复原始的数据,AOF命令以redis协议追加保存每次写的操作到文件末尾. 

l Redis还能对AOF文件进行后台重写,使得AOF文件的体积不至于过大

l 只做缓存：如果你只希望你的数据在服务器运行的时候存在,你也可以不使用任何持久化方式.

l 同时开启两种持久化方式

l 在这种情况下,当redis重启的时候会优先载入AOF文件来恢复原始的数据, 因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整.

l RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件。那要不要只使用AOF呢？ 

l 建议不要，因为RDB更适合用于备份数据库(AOF在不断变化不好备份)， 快速重启，而且不会有AOF可能潜在的bug，留着作为一个万一的手段。

```
性能建议：
因为RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留save 900 1这条规则。
 
如果使用AOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了。
代价,一是带来了持续的IO，二是AOF rewrite的最后将rewrite过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。
只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上。
默认超过原大小100%大小时重写可以改到适当的数值。
```



# 13、Redis 主从复制(重点)

## 13.1 什么是Redis的主从复制

主机数据更新后根据配置和策略，自动同步到备份的<span style="color:red">master/slaver机制</span>，**<span style="color:red">master以写为主，slaver以从为主</span>**

## 13.2 Redis的主从复制能干啥

+ 读写分离，性能扩展
+ 容灾快速恢复

![image-20210528081402910](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528081402910.png)

## 13.3 主从复制的步骤

**<span style="color:red">1、关掉redis.conf中的appendonly</span>，因为后面需要复制这个配置文件** 

![image-20210528084058567](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528084058567.png)

**<span style="color:red">2、新建myredis文件夹用来做主从复制</span>**

![image-20210528084742971](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528084742971.png)

**<span style="color:red">3、创建三个配置文件 `redis6379.conf，redis6380.conf，redis6381.conf`</span>** 内容如下

**redis6379.conf**

```CONF
include /root/myredis/redis.conf
pidfile /var/run/redis_6379.pid
port 6379
dbfilename dump6379.rdb
```

redis6380.conf

```
include /root/myredis/redis.conf
pidfile /var/run/redis_6380.pid
port 6380
dbfilename dump6380.rdb
```



**redis6381.conf**

```
include /root/myredis/redis.conf
pidfile /var/run/redis_6381.pid
port 6381
dbfilename dump6381.conf
```

**<span style="color:red">4、启动三台redis服务器</span>**

![image-20210528085940451](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528085940451.png)



**<span style="color:red">5、可以通过命令`info replication`命令查看主从的相关信息</span>**

![image-20210528090214444](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528090214444.png)

![image-20210528090247399](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528090247399.png)

![image-20210528090329862](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528090329862.png)

**<span style="color:red">6、通过`slaveof <ip><port>` 在从库配置主库</span>**

**`slaveof <ip><port>`：成为某个实例的从服务器**

1.在6380和6381上执行:` slaveof 127.0.0.1 6379`

![image-20210528090726433](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528090726433.png)



![image-20210528090751756](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528090751756.png)



**<span style="color:red">2.在从机上写数据 报错！</span>**

![image-20210528090837348](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528090837348.png)

**<span style="color:red">3、主机挂掉，重启就行，一切如初</span>**

**<span style="color:red">4、从机重启需重设：slaveof 127.0.0.1 6379</span>**

可以将配置增加到文件中。永久生效。

## 13.4 主从复制常用3招

### 1.一主二仆

+ 主要有两个部分：

  + 主服务器挂掉，会怎么样

    + 主服务器挂掉，还是主服务器，还是那个大哥，只需要重启即可

    ![image-20210528093145946](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528093145946.png)

  + 从服务器挂掉，会怎么样

    + 从服务器挂掉：重启后需要重新配置主服务器
    + **<span style="color:red">在从服务器挂掉的时候，如果主服务器在进行写操作，从服务器重新连接之后会获取到主服务器中的所有数据</span>**

    ![image-20210528093459506](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528093459506.png)

  

  

  ![image-20210528093635015](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528093635015.png)

### 2.薪火相传

薪火相传，就类似于项目经理手下200人，但是原来的做法是一个人管理200个人，薪火相传的意思是现在项目经理有2个左膀右臂，2个左膀右臂下面管理很多小组，每个小组有小组长进行人员管理。在这里就是：

**上一个slave(从服务器)可以成为下一个slave(从服务器)的master(主服务器)，从服务器同样可以接收其他slaves的连接和同步请求，那么该slave作为了链条中下一个的master, 可以有效减轻master的写压力,去中心化降低风险。**

缺点：一旦某个slave宕机，后面的slave都没法备份

用法：`slaveof  <ip><port>`

主机挂了，从机还是从机，无法写数据了

![image-20210528095909851](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528095909851.png)

![image-20210528100423752](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528100423752.png)



### 3.反客为主

当一个(master)主机宕机后，后面的slave(从服务器)可以立刻升为master，其后面的slave不用做任何修改。

**<span style="color:red">通过`slaveof no one`可以将从机变成主机</span>**

![image-20210528100859943](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528100859943.png)

## 13.5 主从复制原理（掌握）

+ **<span style="color:red">当从服务器连接上主服务器之后，会主动向主服务器发送同步数据的消息</span>**

+ **<span style="color:red">当主服务器接收到从服务器发送来的同步消息后，首先会将数据备份到rdb文件中，然后将rdb文件发送给从服务器，从服务器读取里面的内容</span>**

+ **<span style="color:red">在主服务器进行写操作的时候，会主动向从服务器进行数据同步</span>**

+ l 全量复制：（复制全部的数据）而slave服务在接收到数据库文件数据后，将其存盘并加载到内存中。

  l 增量复制：（复制修改的部分）Master继续将新的所有收集到的修改命令依次传给slave,完成同步

## 13.6 哨兵模式（sentinel）

### 1.什么是哨兵模式

**<span style="color:red">反客为主的自动版</span>**，能够后台监控主机是否故障，如果故障了根据投票数自动将从服务器转换为主服务器。

![image-20210528121457064](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528121457064.png)

### 2.哨兵模式的实现

**<span style="color:red">1.首先调成一主二从模式，6379带着6380和6381。</span>**

![image-20210528121917375](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528121917375.png)

**<span style="color:red">2.在myredis目录中创建`sentinel.conf`配置文件，文件名必须叫这个。</span>**

![image-20210528122234785](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528122234785.png)

**<span style="color:red">3、在`sentinel.conf`中添加内容</span>**

```
sentinel monitor mymaster 127.0.0.1 6379 1
其中 mymaster为监控对象的服务器名称，1为至少有多少个哨兵同意迁移的数量
```

**<span style="color:red">4、通过执行`redis-sentinel /root/myredis/sentinel.conf`启动哨兵</span>**

![image-20210528122703302](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528122703302.png)



### 3.当master宕掉，slave会根据选举产生新的master

(大概10秒左右可以看到哨兵窗口日志，切换了新的主机)

**<span style="color:red">哪个从机会被选举为主机呢？根据优先级别：slave-priority</span>** 

原主机重启后会变为从机。

![image-20210528122949554](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528122949554.png)

### 4.复制延时

由于所有的写操作都是先在Master上操作，然后同步数据到slave上，所以从Master同步到Slave机器有一定的延迟，当系统繁忙的时候，延迟问题会更加重要，Slave机器数量的增加也会使这个问题更加严重

### 5.故障恢复

![image-20210528123151910](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528123151910.png)

优先级在redis.conf中默认：slave-priority 100，值越小优先级越高

偏移量是指获得原主机数据最全的

每个redis实例启动后都会随机生成一个40位的runid

### 6、主从复制

```java
private static JedisSentinelPool jedisSentinelPool=null;

public static  Jedis getJedisFromSentinel(){
if(jedisSentinelPool==null){
            Set<String> sentinelSet=new HashSet<>();
            sentinelSet.add("192.168.11.103:26379");

            JedisPoolConfig jedisPoolConfig =new JedisPoolConfig();
            jedisPoolConfig.setMaxTotal(10); //最大可用连接数
jedisPoolConfig.setMaxIdle(5); //最大闲置连接数
jedisPoolConfig.setMinIdle(5); //最小闲置连接数
jedisPoolConfig.setBlockWhenExhausted(true); //连接耗尽是否等待
jedisPoolConfig.setMaxWaitMillis(2000); //等待时间
jedisPoolConfig.setTestOnBorrow(true); //取连接的时候进行一下测试 ping pong

jedisSentinelPool=new JedisSentinelPool("mymaster",sentinelSet,jedisPoolConfig);
return jedisSentinelPool.getResource();
        }else{
return jedisSentinelPool.getResource();
        }
}
```

# 14、Redis集群

## 14.1 问题

容量不够，Redis如何进行扩容？

并发写操作，Redis如何进行分摊？

**<span style="color:red">另外，主从复制，薪火相传模式、主机宕机，导致ip地址发生变化，应用程序中配置需要修改对应的主机地址、端口信息等。</span>**

之前通过代理主机来解决，但是Redis3.0后，提供的解决方案是**<span style="color:red">无中心化集群配置</span>**

## 14.2 什么是集群

Redis集群实现了对Redis的水平扩容，即启动N个redis节点，将整个数据分布存储在这N个节点中，每个节点存储的数据为总数据1/N。

Redis集群通过分区（partition）来提供一定程度的可用性（availability）：即使集群中有一部分节点失效或者无法通讯，集群也可以继续处理命令请求。

## 14.3 集群的搭建

**<span style="color:red">1、将rdb、aof文件都删除</span>**

![image-20210528150057792](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528150057792.png)

**通过命令`rm -rf dump*`**

**<span style="color:red">2、创建6个Redis实例，分别是6379，6380，6381，6389，6390，6391</span>**

![image-20210528150626720](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528150626720.png)

cluster-enabled **yes**  打开集群模式

cluster-config-file **nodes-6379.conf** 设定节点配置文件名

cluster-node-timeout **15000**  设定节点失联时间，超过该时间（毫秒），集群自动进行主从切换。

**redis6379.conf配置文件内容**

```sh
include /home/bigdata/redis.conf
port 6379
pidfile "/var/run/redis_6379.pid"
dbfilename "dump6379.rdb"
dir "/home/bigdata/redis_cluster"
logfile "/home/bigdata/redis_cluster/redis_err_6379.log"
#开启集群模式
cluster-enabled yes
#设置节点配置文件内容
cluster-config-file nodes-6379.conf
#设定节点失联时间，超过该时间（毫秒），集群自动进行主从切换
cluster-node-timeout 15000
```

`redis6380.conf、redis6381.conf、redis6389.conf、redis6390.conf、redis6391.conf`配置文件中的内容与上面一致，只是将6379部分换成对应的端口号

**<span style="color:red">可以通过使用`%s/6379/6380`：将配置文件中6379的地方替换成6380</span>**

**<span style="color:red">3、启动6个redis服务器</span>**

![image-20210528151618146](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528151618146.png)

**<span style="color:red">4、将6个redis服务器合并成一个集群</span>**

在进行合并之前，请确保redis实例都启动了，nodes-xxx.conf也正常运行。

然后先进入到**`cd  /opt/redis-6.2.1/src`**，**<span style="color:red">在合并成集群时需要rb(ruby)环境，但是最新redis的版本已经帮我们下载好了ruby环境，我们需要进入到redis的src下进行集群合并</span>**

**通过命令`redis-cli --cluster create --cluster-replicas 1 192.168.11.101:6379 192.168.11.101:6380 192.168.11.101:6381 192.168.11.101:6389 192.168.11.101:6390 192.168.11.101:6391`**  进行集群合并。

**<span style="color:red">此处不要用127.0.0.1,请用真实ip地址</span>**

**`--replicas`说明：<span style="color:red">采用最简单的方式配置集群，一台主机，一台从机，正好三组</span>**

![image-20210528152417540](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528152417540.png)

![image-20210528152450162](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528152450162.png)

**<span style="color:red">5、以集群方式登录客户端</span>**

**通过命令`redis-cli -c -p 6379`方式进行登录；这里端口号可以使用任意一个，不局限6379**

**-c采用集群策略连接，设置数据会自动切换到相应的写主机上**

![image-20210528152726083](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528152726083.png)

**<span style="color:red">6、通过`cluster nodes`命令查看集群信息</span>**

![image-20210528152814404](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528152814404.png)

## 14.4 redis cluster如何分配这六个节点

+ **一个集群<span style="color:red">至少要有三个主节点</span>。**
+ 选项 --cluster-replicas 1 表示我们希望为集群中的每个主节点创建一个从节点。
+ **分配原则尽量保证每个主数据库运行在不同的IP地址，每个从库和主库不在一个IP地址上。**

## 14.5 什么是slots

![image-20210528160756344](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528160756344.png)

**一个Redis集群中包含<span style="color:red">16384个插槽(hashslot)。</span>**数据库中的每个键都属于这16384个插槽的其中一个。

集群使用公式CRC16(key) % 16384 来计算每个key属于哪个插槽，其中 CRC16(key) 语句用于计算键 key 的 CRC16 校验和 。

**<span style="color:red">集群中的每个节点负责一部分插槽。</span>**举个例子，如果一个集群可以有主节点，其中：

节点A负责处理0号至5460号插槽。

节点 B 负责处理 5461 号至 10922 号插槽。

节点 C 负责处理 10923 号至 16383 号插槽。

![image-20210528161219286](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528161219286.png)

## 14.6 在集群中录入值

在redis-cli每次写入、查询键值 ，redis都会计算出该key应该送往的插槽，如果不是该客户端对应服务器的插槽，redis会报错，并告知应前往的redis实例地址和端口。

redis-cli 客户端提供了-c 参数实现自动重定向。

如redis-cli -c -p 6379 登入后，再录入、查询键值对可以自动重定向。

**<span style="color:red">不在一个slot下的键值，是不能使用mset、mget等多键操作。</span>**

![image-20210528161604483](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528161604483.png)

可以通过{}来定义组的概念，从而使得key中{}内相同内容的键值对放到一个slot中去。

![image-20210528161643966](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528161643966.png)

## 14.7 查询集群中的值

+ **`cluster keyslot k1`：查询键k1所对应的集群插槽值**
+ **`cluster countkeysinslot 4847`：统计4847插槽中的key的个数：这个需要在对应的服务器下查询才可以查询到**
+ **`cluster getkeysinslot 4847 10`：返回 count 个 slot 槽中的键。**

![image-20210528162137593](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528162137593.png)

## 14.8 故障恢复

如过主节点下线？从节点是否自动升级为主节点？

注意：会的，15秒超时。

![image-20210528162326049](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528162326049.png)

如果主节点恢复后，主从关系如何？主节点恢复后变成从节点

![image-20210528162329463](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528162329463.png)

如果某一段的**<span style="color:red">主从节点都挂掉</span>**，redis服务是否还能继续？

如果某一段插槽的主从都挂掉，而cluster-require-full-coverage 为**<span style="color:red">yes</span>** ，那么 ，**<span style="color:red">整个集群都挂掉</span>**

如果某一段插槽的主从都挂掉，而cluster-require-full-coverage 为**<span style="color:red">no</span>** ，那么，**<span style="color:red">该插槽数据全都不能使用</span>**，也无法存储。

redis.conf中的参数  cluster-require-full-coverage



## 14.9 集群的Jedis开发

即使连接的不是主机，集群会自动切换主机存储。主机写、从机读。

无中心化配置。无论从哪台主机写的数据，其他主机上都能读到数据。

```java
public class JedisClusterTest {
  public static void main(String[] args) { 
     Set<HostAndPort>set =new HashSet<HostAndPort>();
     set.add(new HostAndPort("192.168.31.211",6379));
     JedisCluster jedisCluster=new JedisCluster(set);
     jedisCluster.set("k1", "v1");
     System.out.println(jedisCluster.get("k1"));
  }
}
```

## 14.10 Redis集群的好处

+ 实现扩容
+ 分摊压力
+ 无中心化配置相对简单

## 14.11 Redis集群的不足

+ 多键操作是不被支持的。
+ 由于集群方案出现较晚，很多公司已经采用了其他的集群方案，而代理或者客户端分片的方案想要迁移至redis cluster，需要整体迁移而不是逐步过渡，复杂度较大。
+ 多键的Redis事务是不被支持的。lua脚本不被支持

# 15、Redis应用问题解决

## 15.1 缓存穿透

key对应的数据在redis中不存在，每次针对key的请求从缓存获取不到，就会去数据库获取，从而可能导致数据库崩掉。比如用一个不存在的用户id获取用户信息，不论缓存还是数据库都没有，若黑客利用此漏洞进行攻击可能会压垮数据库。

![05-缓存穿透](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/05-%E7%BC%93%E5%AD%98%E7%A9%BF%E9%80%8F.png)

**解决方案：**

1. **<span style="color:red">对空值缓存</span>**

   如果一个查询返回的数据为空（不管数据是否存在），我们仍然把这个空结果（null）进行缓存，设置空结果的过期时间会很短，最长不超高5分钟。

2. **<span style="color:red">设置可访问的名单（白名单）</span>**

   使用bitmaps类型定义一个可以访问的名单，名单id作为bitmaps的偏移量，每次访问和bitmaps里面的id进行比较，如果访问id不在bitmaps中，进行拦截，不允许访问

3. **<span style="color:red">采用布隆过滤器</span>**

   它实际上是一个很长的二进制向量(位图)和一系列随机映射函数（哈希函数）。

   布隆过滤器可以用于检索一个元素是否在一个集合中。它的优点是空间效率和查询时间都远远超过一般的算法，缺点是有一定的误识别率和删除困难。)

4. **<span style="color:red">进行实时监控</span>**

   当发现Redis的命中率极具下降，需要排查访问对象和访问的数据，和运维人员配合，可以设置黑名单限制服务。

## 15.2 缓存击穿

key对应的数据在redis中存在，但是key有过期时间，此时有大量的并发请求过来，这些请求发现用到的key过期了，就会去DB中查询数据，并设置到缓存中，这个时候大并发的请求可能会瞬间把DB压垮

![06-缓存击穿](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/06-%E7%BC%93%E5%AD%98%E5%87%BB%E7%A9%BF.png)

**解决方案：**

key可能会在某个时间点被超高并发地访问，是一种非常“热点”的数据。这个时候，需要考虑一个问题，缓存被“击穿”。

1. **<span style="color:red">预先设置热门数据：</span>**在redi高峰访问之前，把这些热门数据提前存入到redis里面，加大这些热门数据key的时长。

2. **<span style="color:red">实时调整：</span>**现场监控哪些热门数据，实时调整key的过期时间

3. **<span style="color:red">使用锁</span>**

   1. 就是在缓存失效的时候（判断拿出来的值为空），不是立即去load db；
   2. 先使用缓存工具的某些带成功操作返回值的操作（比如Redis的SETNX）去set一个mutex key
   3. 当操作返回成功时，再进行load db的操作，并回设缓存,最后删除mutex key；
   4. 当操作返回失败，证明有线程在load db，当前线程睡眠一段时间再重试整个get缓存的方法。

   ![image-20210528174607119](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528174607119.png)

## 15.3 缓存雪崩

key对应的数据存在，但在redis中过期，此时若有大量并发请求过来，这些请求发现缓存过期，一般会从DB查询数据并回设到缓存中，这个时候大并发的请求可能会瞬间把后端DB压垮。

缓存雪崩和缓存击穿的区别在于 这里针对很多key缓存，后者只是某一个key。
![07-缓存雪崩](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/07-%E7%BC%93%E5%AD%98%E9%9B%AA%E5%B4%A9.png)

**解决方案：**

缓存失效时的雪崩效应对底层系统的冲击非常可怕！

**<span style="color:red">1.构建多级缓存架构：</span>**nginx缓存 + redis缓存 + 其他缓存(ehcache等)

**<span style="color:red">2.使用锁或队列</span>**：

用加锁或者队列的方式保证不会有大量的线程对数据库一次性进行读写，从而避免失效时大量的并发请求落到底层存储系统上。**不适合高并发情况。**

**<span style="color:red">3.设置过期标志更新缓存</span>**

记录缓存数据是否过期（设置提前量）；如果过期触发通知另外的线程去更新实际key的缓存

**<span style="color:red">4.将缓存失效时间分散开：</span>**

比如我们可以在原有的失效时间基础上增加一个随机值，比如1-5分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效的事件。



## 15.4 分布式锁

### 1.什么是分布式锁

随着业务发展的需要，原单体机部署的系统被演化成分布式集群系统后，由于分布式系统多线程、单进程并且分布在不同机器上，这将使原单击部署情况下的并发控制锁策略失效，单纯的JavaAPI并不能提供分布式锁的能力。为了解决这个问题就需要一种跨JVM的互斥机制来控制共享资源的访问，这就是分布式锁要解决的问题。

分布式锁主流的实现方案：

1. 基于数据库实现分布式锁
2. 基于缓存
3. 基于zookeeper
4. 每一种分布式锁解决方案都有各自的特点：
5. 性能：redis最高
6. 可靠性：zookeeper最高
7. 这里，我们就基于redis实现分布式锁。

### 2.使用Redis实现分布式锁

通过命令：`set user zhangsan nx ex [second]`：上锁+设置过期时间（目的释放锁）

EX second ：设置键的过期时间为 second 秒。 SET key value EX second 效果等同于 SETEX key second value

PX millisecond ：设置键的过期时间为 millisecond 毫秒。 SET key value PX millisecond 效果等同于 PSETEX key millisecond value 。

NX ：只在键不存在时，才对键进行设置操作。 SET key value NX 效果等同于 SETNX key value 。

![image-20210528184331737](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528184331737.png)

1. 多个客户端同时获取锁（setnx）

2. 获取成功，执行业务逻辑{从db获取数据，放入缓存}，执行完成释放锁（del）

3. 其他客户端等待重试

### 3.编写代码

```java
@GetMapping("testLock")
public void testLock(){
    //1获取锁，setne
    Boolean lock = redisTemplate.opsForValue().setIfAbsent("lock", "111");
    //2获取锁成功、查询num的值
    if(lock){
        Object value = redisTemplate.opsForValue().get("num");
        //2.1判断num为空return
        if(StringUtils.isEmpty(value)){
            return;
        }
        //2.2有值就转成成int
        int num = Integer.parseInt(value+"");
        //2.3把redis的num加1
        redisTemplate.opsForValue().set("num", ++num);
        //2.4释放锁，del
        redisTemplate.delete("lock");

    }else{
        //3获取锁失败、每隔0.1秒再获取
        try {
            Thread.sleep(100);
            testLock();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

重启，服务集群，通过网关压力测试：

ab -n 1000 -c 100 http://192.168.140.1:8080/test/testLock

![image-20210528190401125](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528190401125.png)

查看redis中num的值：

![image-20210528190408061](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528190408061.png)

基本实现。

问题：setnx刚好获取到锁，业务逻辑出现异常，导致锁无法释放

解决：设置过期时间，自动释放锁。



### 4.优化之设置锁的过期时间

设置过期时间有两种方式：

1. 首先想到通过expire设置过期时间（缺乏原子性：如果在setnx和expire之间出现异常，锁也无法释放）

2. 在set时指定过期时间（推荐）

![image-20210528190517689](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528190517689.png)

设置过期时间：

![image-20210528190522680](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528190522680.png)

压力测试肯定也没有问题。自行测试

问题：可能会释放其他服务器的锁。

 

场景：如果业务逻辑的执行时间是7s。执行流程如下

1. index1业务逻辑没执行完，3秒后锁被自动释放。

2. index2获取到锁，执行业务逻辑，3秒后锁被自动释放。

3. index3获取到锁，执行业务逻辑

4. index1业务逻辑执行完成，开始调用del释放锁，这时释放的是index3的锁，导致index3的业务只执行1s就被别人释放。

最终等于没锁的情况。

解决：setnx获取锁时，设置一个指定的唯一值（例如：uuid）；释放前获取这个值，判断是否自己的锁

### 5.优化UUID防误删

![image-20210528190606774](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528190606774.png)

![image-20210528190610123](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528190610123.png)

问题：删除操作缺乏原子性。

场景：

1. index1执行删除时，查询到的lock值确实和uuid相等

uuid=v1

set(lock,uuid)；

![img](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/wps1.jpg) 

2. index1执行删除前，lock刚好过期时间已到，被redis自动释放

在redis中没有了lock，没有了锁。

![img](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/wps2.jpg) 

3. index2获取了lock

index2线程获取到了cpu的资源，开始执行方法

uuid=v2

set(lock,uuid)；

4. index1执行删除，此时会把index2的lock删除

index1 因为已经在方法中了，所以不需要重新上锁。index1有执行的权限。index1已经比较完成了，这个时候，开始执行

![img](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/wps3.jpg) 

删除的index2的锁！



### 6.优化之LUA脚本保证删除的原子性

```JAVA
@GetMapping("testLockLua")
public void testLockLua() {
    //1 声明一个uuid ,将做为一个value 放入我们的key所对应的值中
    String uuid = UUID.randomUUID().toString();
    //2 定义一个锁：lua 脚本可以使用同一把锁，来实现删除！
    String skuId = "25"; // 访问skuId 为25号的商品 100008348542
    String locKey = "lock:" + skuId; // 锁住的是每个商品的数据

    // 3 获取锁
    Boolean lock = redisTemplate.opsForValue().setIfAbsent(locKey, uuid, 3, TimeUnit.SECONDS);

    // 第一种： lock 与过期时间中间不写任何的代码。
    // redisTemplate.expire("lock",10, TimeUnit.SECONDS);//设置过期时间
    // 如果true
    if (lock) {
        // 执行的业务逻辑开始
        // 获取缓存中的num 数据
        Object value = redisTemplate.opsForValue().get("num");
        // 如果是空直接返回
        if (StringUtils.isEmpty(value)) {
            return;
        }
        // 不是空 如果说在这出现了异常！ 那么delete 就删除失败！ 也就是说锁永远存在！
        int num = Integer.parseInt(value + "");
        // 使num 每次+1 放入缓存
        redisTemplate.opsForValue().set("num", String.valueOf(++num));
        /*使用lua脚本来锁*/
        // 定义lua 脚本
        String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
        // 使用redis执行lua执行
        DefaultRedisScript<Long> redisScript = new DefaultRedisScript<>();
        redisScript.setScriptText(script);
        // 设置一下返回值类型 为Long
        // 因为删除判断的时候，返回的0,给其封装为数据类型。如果不封装那么默认返回String 类型，
        // 那么返回字符串与0 会有发生错误。
        redisScript.setResultType(Long.class);
        // 第一个要是script 脚本 ，第二个需要判断的key，第三个就是key所对应的值。
        redisTemplate.execute(redisScript, Arrays.asList(locKey), uuid);
    } else {
        // 其他线程等待
        try {
            // 睡眠
            Thread.sleep(1000);
            // 睡醒了之后，调用方法。
            testLockLua();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

**Lua 脚本详解：**

![image-20210528193840880](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528193840880.png)

项目中正确使用：

1. 定义key，key应该是为每个sku定义的，也就是每个sku有一把锁。

```java
String locKey ="lock:"+skuId; // 锁住的是每个商品的数据
Boolean lock = redisTemplate.opsForValue().setIfAbsent(locKey, uuid,3,TimeUnit.*SECONDS*);
```

![image-20210528193929347](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528193929347.png)



### 7.总结

1、加锁

```java
// 1. 从redis中获取锁,set k1 v1 px 20000 nx
String uuid = UUID.randomUUID().toString();
Boolean lock = this.redisTemplate.opsForValue()
      .setIfAbsent("lock", uuid, 2, TimeUnit.SECONDS);
```

2、使用LUA释放锁

```java
// 2. 释放锁 del
String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
// 设置lua脚本返回的数据类型
DefaultRedisScript<Long> redisScript = new DefaultRedisScript<>();
// 设置lua脚本返回类型为Long
redisScript.setResultType(Long.class);
redisScript.setScriptText(script);
redisTemplate.execute(redisScript, Arrays.asList("lock"),uuid);
```

3、重试

```java
Thread.sleep(500);
testLock();
```

为了确保分布式锁可用，我们至少要确保锁的实现同时**满足以下四个条件**：

\- 互斥性。在任意时刻，只有一个客户端能持有锁。

\- 不会发生死锁。即使有一个客户端在持有锁的期间崩溃而没有主动解锁，也能保证后续其他客户端能加锁。

\- 解铃还须系铃人。加锁和解锁必须是同一个客户端，客户端自己不能把别人加的锁给解了。

\- 加锁和解锁必须具有原子性。

# 16、Redis6.0的新功能

## 16.1 ACL

Redis ACL是Access Control List（访问控制列表）的缩写，该功能允许根据可以执行的命令和可以访问的键来限制某些连接。

在Redis 5版本之前，Redis 安全规则只有密码控制 还有通过rename 来调整高危命令比如 flushdb ， KEYS* ， shutdown 等。Redis 6 则提供ACL的功能对用户进行更细粒度的权限控制 ：

（1）接入权限:用户名和密码 

（2）可以执行的命令 

（3）可以操作的 KEY

参考官网：https://redis.io/topics/acl

**<span style="color:red">常用命令：</span>**

+ `acl list`：展现用户权限列表

![image-20210528194354331](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528194354331.png)

+ `acl act`：查看添加权限指令类别

![image-20210528194410961](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528194410961.png)

​       加参数类型名可以查看类型下具体命令

![image-20210528194430368](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528194430368.png)



+ `acl  whoami`：查看当前用户

![image-20210528194435751](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528194435751.png)



+ `acl setuser`：创建和编辑用户ACL

![image-20210528194640317](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528194640317.png)

在上面的示例中，我根本没有指定任何规则。如果用户不存在，这将使用just created的默认属性来创建用户。如果用户已经存在，则上面的命令将不执行任何操作。

**示例：**

（1）设置有用户名、密码、ACL权限、并启用的用户

acl setuser user2 on >password ~cached:* +get

![image-20210528194716285](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528194716285.png)

（2)切换用户，验证权限

![image-20210528194728391](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528194728391.png)

## 16.2 IO多线程

Redis6终于支撑多线程了，告别单线程了吗？

IO多线程其实指**客户端交互部分**的**网络IO**交互处理模块**多线程**，而非执行**命令多线程。**Redis6执行命令依然是单线程。

**原理架构：**

Redis 6 加入多线程,但跟 Memcached 这种从 IO处理到数据访问多线程的实现模式有些差异。Redis 的多线程部分只是用来处理网络数据的读写和协议解析，执行命令仍然是单线程。之所以这么设计是不想因为多线程而变得复杂，需要去控制 key、lua、事务，LPUSH/LPOP 等等的并发问题。整体的设计大体如下:

![image-20210528194900153](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528194900153.png)

另外，多线程IO默认也是不开启的，需要再配置文件中配置

**io-threads-do-reads  yes** 

**io-threads 4**

## 16.3 工具支持cluster

之前老版Redis想要搭集群需要单独安装ruby环境，Redis 5 将 redis-trib.rb 的功能集成到 redis-cli 。另外官方 redis-benchmark 工具开始支持 cluster 模式了，通过多线程的方式对多个分片进行压测。

![image-20210528194929273](Redis6%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210528194929273.png)


