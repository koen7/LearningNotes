# 六、Vi和Vim编辑器

## 6.1 vi和vim的基本介绍

Linux系统内置vi**文本编辑器**，相当于windows下的记事本

Vim具有程序编辑的能力，**可以看做是Vi的增强版**，可以主动的以字体颜色辨别语法的正确性。

## 6.2 vi和vim常用的三种模式

vim具有三种模式，分别是 **正常模式(一般模式)、编辑模式、命令行模式**

### 6.2.1 正常模式

正常一打开vim文件，就是处在该模式下，在这个模式下，可以使用【上下左右】来移动光标，你可以使用一些快捷键来操作相关内容，例如【删除当前行】 ，【复制、粘贴】等

### 6.2.2 编辑模式

在正常模式中，按下i、I、o、O、a、A、r、R等任何一个字母之后就会进入该编辑模式，一般来说按i即可

### 6.2.3 命令行模式

在编辑模式中，先按ESC键，再按`：`或者`/`就可进入到该模式下。

## 6.3 三种模式之间的相互切换

![image-20210823225012574](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210823225012574.png)

## 6.4  vi和vim快捷键

### 6.4.1 快捷键使用练习

+ `yy`：复制当前行
+ `5yy`：复制当前行以及向下的5行
+ `p`：粘贴
+ `u`：撤销，相当于windows下的`ctrl + z`
+ `dd`：删除当前行
+ `5dd`：删除当前行及当前行以下的5行
+ 在文件中查找某个单词，使用`/关键字`，回车 查找，输入n就查找下一个
+ 设置文件的行号，`set nu 和 set nonu`
+ 编辑/etc/profile文件，使用快捷键`G`到该文档的最末行，使用快捷键`gg`到该文档的最首行，相当于IDEA中的 `ctrl + home / ctrl + end`
+ ` 20  shift + g` ：将光标移动到行号为20的行 ，相当于IDEA中的`ctrl + g`

## 6.5 vi/vim快捷键——键盘图

![image-20210823230430544](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210823230430544.png)

# 七、开机、重启和用户登录注销

## 7.1 关机&重启命令

### 7.1.1 基本介绍

+ shutown  -h now ：立刻关机
+ shutdown -h  1：一分钟后关机
+ **shutdown  等价于  shutdown -h 1：一分钟后关机**
+ halt：关机
+ reboot：重启
+ **sysc：将内存数据同步到磁盘上，建议在关机或者重启之前执行这个指令**

### 7.1.2 使用细节

1. 不管是重启还是关机，首先要运行sync命令，把内存的数据写到磁盘上
2. 目前的shutdown/reboot/halt 等命令均已经在关机前进行了sync。

## 7.2 登录和注销

### 7.2.1 基本介绍

1. 尽量少使用root登录，可以先利用普通用户进行登录，然后通过`su - 用户名` 来切换系统管理员
2. 在提示符下输入logout即可注销用户

### 7.2.2 使用细节

1. 当用户先登录了tom ，然后通过su - root来切换到root用户，这时执行logout退出root用户，返回到tom用户
2. logout注销指令在图形运行级别无效，在运行级别3下有效

# 八、用户管理

## 8.1 基本介绍

Linux是一个多用户多任务的操作系统，任何一个要使用系统资源的用户，都必须首先向系统管理员申请一个账号，然后以这个账号的身份进入系统。

![image-20210826213717944](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210826213717944.png)

## 8.2 添加用户

### 8.2.1 基本语法

useradd 用户名

### 8.2.2 应用案例

案例1：添加一个用户milan，默认该用户的家目录在/home/milan

### 8.2.3 细节说明

1. 当用户创建成功之后，会自动的创建和用户同名的家目录 (/home/milan)
2. 也可以通过`useradd -d 家目录 用户名`创建指定家目录的用户

## 8.3 指定/修改密码

### 8.3.1 基本语法

passwd 用户名

### 8.3.2 应用案例

给milan指定密码：`passwd milan`

## 8.4 删除用户

### 8.4.1 基本语法

userdel 用户名

### 8.4.2 应用案例

1. 删除用户milan，但要保留家目录，`userdel milan`
2. 删除用户milan以及家目录，比如tom，`userdel -r milan`

### 8.4.3 讨论

删除用户的时候，是否需要保留其家目录？？？

答：一般情况下，**建议保留家目录**

## 8.5 查询用户信息

### 8.5.1 基本语法

id 用户名

### 8.5.2 应用案例

案例1：查询root信息：`id root`

![image-20210826215118895](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210826215118895.png)



### 8.5.3 细节说明

当用户不存在时，返回无此用户

![image-20210826215144403](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210826215144403.png)

## 8.6 切换用户

### 8.6.1 基本语法

su - 切换用户名

### 8.6.2 应用案例

创建一个用户jack，指定密码，然后切换到jack用户。

![image-20210826215356813](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210826215356813.png)

### 8.6.3 使用细节

1. **当权限高的用户切换到权限低的用户，不需要密码，反之需要**
2. 当需要返回到原来用户时，使用`exit/logout`指令

## 8.7 查看当前登录的用户

### 8.7.1 基本语法

who am i/whoami

### 8.7.2 使用细节

**whoami：查看当前登录的用户**

**who am i：如果当前进行了用户切换操作，那么查看的是切换前的用户信息。**

这两者还是有一点点区别的

## 8.8 用户组

### 8.8.1 介绍

用户组，类似于角色，系统可以对一群用户进行统一的管理

![image-20210826215924012](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210826215924012.png)

### 8.8.2 新增组

基本语法：`groupadd 组名`

应用案例：

	1. 创建一个组，名叫 dev2

![image-20210826220114514](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210826220114514.png)

2. 创建用户的同时，给该用户上组，

![image-20210826220341978](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210826220341978.png)

### 8.8.3 删除组

基本语法：`groupdel 组名`

应用案例： 删除用户组 dev2  ，`groupdel dev2`

当组中有用户时，不可以删除用户

### 8.8.4 修改组

基本语法：`usermod -g 组名 用户名`

## 8.9 用户组和相关文件

### 8.9.1 /etc/passwd 文件

**用户(user)的配置文件**，记录了用户的各种信息

**每行的含义：`用户名：密码：用户id：组id::家目录: shell`**

### 8.9.2 /ect/shadow 文件

**口令**的配置文件

**每行的含义：`组名：口令：组标识号：组内用户列表`**

# 九、实用指令

## 9.1 指定运行级别

### 9.1.1 基本介绍

Linux中运行级别一共有7级，分别如下：

0：关机

1：单用户

2：多用户无网络状态

3：多用户有网络状态

4：系统未使用，保留给用户

5：图形化界面

6：重启

常用的运行级别是**3**和**5**，也可以指定默认的运行级别。

### 9.1.2 应用案例

基本语法：init 运行级别

应用案例：通过init来切换不同的运行级别，比如5-3 ，然后关机

### 9.1.3 CentOS7后运行级别说明

在centos7以前，/etc/inittab文件中，进行了简化，如下：

**multi-user.target**：analogous to runlevel 3

**graphical.target**：analogous to runlevel 5  

**查看当前默认的级别：**`systemctl get-default`

**设置默认的级别：**`systemctl set-default xxxx.target`

## 9.2 如何找回root密码

步骤如下：

1. 设置运行级别为1(单用户)，重启系统，进入类似于windows的bios界面 ；

## 9.3 帮助指令

### 9.3.1 man 获得帮助信息

基本语法： man [命令或配置文件]

应用案例：查看ls指令的帮助信息，`man ls`

![image-20210826222113651](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210826222113651.png)

在Linux文件中，**以.开头的为隐藏文件**，

CentOS7中，文件夹是**蓝色**的，文件是**白色**的

### 9.3.2 help 指令

基本语法：help 命令 【获取shell内置指令的帮助信息】

### 9.3.3 应用案例

查看cd的帮助信息，通过help指令实现

## 9.4 文件目录类

### 9.4.1 pwd指令

基本语法：pwd （功能描述：显示当前工作目录的**绝对路径**）

应用案例：显示当前工作目录的绝对路径

### 9.4.2 ls指令

基本语法：ls [选项] [目录或是文件]

常用选项：

-a：显示当前目录所有的文件和目录，包括隐藏的。

-l：以列表的方式显示信息

### 9.4.3 cd指令

基本语法： cd 【参数】 （功能描述：切换到指定目录）

理解：**绝对路径和相对路径**

![image-20210826234334356](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210826234334356.png)

`cd ~` 或者`cd`：回到自己的家目录，比如，当前登录用户是root，就到/root，当前登录是tom，就到/home/tom

`cd..`：回到当前目录的上一级目录

应用案例：

1. 案例1：使用绝对路径切换到root目录，cd/root
2. 案例2：使用相对路径到/root目录，比如在/home/tom，cd../../root
3. 案例3：表示回到当前目录的上一级目录，cd..
4. 案例4：回到家目录，cd ~

### 9.4.4 mkdir指令

mkdir用于创建目录

基本语法： mkdir [选项]  要创建的目录

常用选项：-p 创建多级目录

应用案例：

​	案例1：创建**一个**目录/home/dog，`mkdir /dog`

​	案例2：创建**多级**目录/home/animal/tiger 

​				`mkdir -p /animal/tiger`

### 9.4.5 rmdir 指令

基本语法：rmdir [选项] 要删除的目录

常用选项：-p 删除多级目录 (空目录)

应用案例：

​	案例1：删除多级目录/animal/tiger，`rmdir -p /animal/tiger`

​	如果要删除非空目录，需要使用` rm -rf 要删除的目录`

​	比如：`rm -rf /home/animal`

​					-r 多级删除

​					-f  强制删除 (force的缩写)

### 9.4.6 touch指令

基本语法：touch 文件名称

应用案例：在/home目录下，创建一个空文件 hello.txt

### 9.4.7 cp指令

cp指令用来复制文件到指定目录

基本语法：cp 源文件  目标目录

常用选项：-r 递归复制

应用案例：

​	案例1：将/home/hello.txt 拷贝到 /home/bbb目录下  

​	`cp /home/hello.txt /home/bbb/`

​	案例2：递归复制整个文件夹，比如将/home/bbb 整个目录，拷贝到/opt  

 	`cp -r /home/bbb  /opt` 

使用细节：**强制覆盖不提示** ：`\cp` ， `\cp -r hello.txt /home/bbb`

### 9.4.8 rm指令

rm指令是用来删除文件或目录。

基本语法： rm 文件/目录

**常用选项： -f  强制删除**

​					**-r 递归删除整个文件夹**

应用案例：

​				案例1：将/home/hello.txt 删除， `rm /home/hello.txt`

​				案例2：递归删除整个文件夹 /home/bbb，`rm -rf /home/bbb`
**使用细节：强制删除不提示，使用-f 参数即可**

### 9.4.9 mv指令

mv指令是用来重命名或者移动文件的。

基本语法：mv 源文件 目标文件（功能描述：重命名）
				    mv 源文件  目标文件夹 （功能描述：移动文件）

应用案例：

​				案例1： 将/home/cat.txt文件重命名为pig.txt ， `mv cat.txt pig.txt`

​				案例2： 将/home/pig.txt 文件 移动到/root目录下     `mv /home/pig.txt /root`

​				案例3：移动整个目录，比如将/opt/bbb 移动到 /home下  `mv /opt/bbb /home`

### 9.4.10 cat指令

cat指令是用来查看文件内容的。

基本语法：cat 文件

常用选项：-n 显示行号

应用案例：

​				案例1：查看/etc/profile文件内容，并显示行号   	 `cat -n /etc/profile`

使用细节：cat只能浏览文件，不能编辑文件，为了浏览方便，一般会带上 管道命令 | more

​					cat -n /etc/profile | more [进行交互]

### 9.4.11 more指令

more指令是一个基于VI编辑器的文本过滤器，它以全屏幕的方式**按页显示**文件内容。

基本语法：more 文件

**操作说明：如图**

![image-20210829204917299](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210829204917299.png)

应用案例：

​				案例：采用more查看文件/etc/profile

### 9.4.12 less指令

less指令动态的加载文件内容，你看到哪加载哪，而不会一下子全部加载文件内容。**一般用于看大型的文件。**

基本语法： less 要查看的文件

**操作说明：如图**

![image-20210829205147637](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210829205147637.png)

应用案例：采用less指令查看**大型文件** /opt/杂文.txt 		`less /opt/杂文.txt`

### 9.4.13 echo指令

echo将内容输出到控制台

基本语法：echo  [选项]  [输出内容]

应用案例：使用echo输出环境变量，比如输出 $PATH  $HOSTNAME,		`echo $PATH`

### 9.4.14 head指令

用于显示文件的开头部分内容，默认显示前10行。

基本语法：head 文件

常用选项：-n 10  显示任意行数

应用案例：

​				案例：查看/etc/profile文件前20行内容  `head -n 20 /etc/profile` 

### 9.4.15 tail指令

用于输出文件中尾部的内容，默认情况下tail指令显示文件的前10行内容。

基本语法：tail 文件名（功能描述：显示文件末尾10行内容）

​					tail -n 5 文件名（功能描述：显示文件末尾5行内容）

​					tail  -f 文件名（功能描述：实时监控某个文件）

应用案例：

​				案例1：查看/etc/profile 最后5行的内容   `tail -n 5 /etc/profile`

​				案例2：实时监控mydata.txt，看看文件有没有变化 。		`tail -f /etc/profile`

### 9.4.16 >指令和>>指令

 `>指令` 重定向，`>>指令`追加 

基本语法：

​				ls -l > 文件1  （功能描述：列表的内容**写入**文件1中）

​				ls -al >>文件  （功能描述：列表的内容**追加**文件1中）

​				cat 文件1 > 文件2

​				echo "内容" >> 文件(追加)

应用案例：将/home目录的文件列表 写入到/home/info.txt 中，覆盖写入 ls -l  > /home/info.txt

​					将当前日历信息 追加到 /home/mycal 文件中    cal >> /home/mycal

### 9.4.17 ln指令

软链接也称为符号链接， 类似于windows里的快捷方式，主要存放了链接其他文件的路径

基本语法：ln  -s【源文件】 链接名（功能描述：给源文件创建一个软连接）

常用选项：-s 软连接

应用案例：案例1：在/home目录下创建一个软链接myroot，连接到/root目录   `ln -s /root myroot`

​					案例2：删除软连接 	`rm /home/myroot`\

### 9.4.18 history

查看已经执行过的历史指令。

基本语法： histroy

常用选项：history 10 查看最近执行的10条指令

## 9.5 时间日期类

### 9.5.1 date指令

显示当前日期

基本语法：

​				1.date：显示当前日期

​				2.date "+%Y"：显示当前年

​				3.date "+%Y-%m-%d"：显示当前年月日

### 9.5.2 date指令-设置日期

基本语法：date -s  字符串日期

应用案例：设置系统当前当前，比如设置成2021-11-11 	`date -s "%+Y-%m-%d"`

### 9.5.3 cal指令

基本语法： cal  【年份】 （年份不填，默认是当前月的日历）

## 9.6 搜索查找类

### 9.6.1 find指令

find指令是查找文件或者目录的路径，将满足的文件或者目录显示在终端。

**基本语法：find 搜素范围 [选项]**  

常用选项：

![image-20210829230647891](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210829230647891.png)

应用案例：

​				案例1：按文件名，根据名称查找/home目录下的hello.txt文件 		`find /home -name hello.txt`

​				案例2：按拥有者，查找系统中文件拥有者是nobody的文件			`find / -user nobody`

​				案例3：按文件大小查找，查找系统中文件大小超过200M的文件		`find / -size +200M`**（+n 大于，-n小于，n等于）**

### 9.6.2 locate指令

**locate指令可以快速定位文件路径。**locate指令利用事先建立的系统中所有的文件名称及其路径的locate数据库实现快速定位给定的文件。locate指令无需从头到尾遍历整个文件系统，查询速度较快。**为了保证查询结果的准确性，管理员需要定期更新locate时刻。**

基本语法：locate 文件

**特别说明：由于locate指令基于数据库进行查询，所以在第一次运行前，必须使用updatedb指令创建locate数据库**

应用案例：请使用locate指令快速定位hello.txt文件所在的路径			`updatedb    locate hello.txt`

### 9.6.3 which指令

which指令，可以查看指令在哪个目录下，比如ls指令			`which  ls`

### 9.6.4 grep指令和管道符 |

grep过滤查找，**管道符，“|”，表示将前一个命令的处理结果输出传递给后面的指令处理**

**基本语法：grep [选项]  查找内容  源文件**

**常用选项：**

![image-20210829231534930](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210829231534930.png)

应用案例：

​				案例1：请在hello.txt文件中，查找“yes”所在行，并且显示行号

​				写法1：`grep -n "yes" hello.txt`

​				写法2：`cat hello.txt | grep -n "yes"`

## 9.7 压缩和解压缩

### 9.7.1 gzip/gunzip指令

gzip用于压缩文件，gunzip用于解压。

基本语法：

​				gzip 文件（功能描述：压缩文件，只能将文件压缩为*.gz）

​				gunzip 文件（功能描述：解压文件命令）

应用实例：

​				案例1：gzip压缩，将/home下hello.txt文件进行压缩		

​							`gzip hello.txt`

​				案例2：gunzip压缩，将/home下的hello.txt.gz文件进行解压缩

​							`gunzip hello.txt.gz`

### 9.7.2 zip/unzip指令

zip用于压缩文件，unzip用于解压文件的，这个在项目打包发布中很有用的。

基本语法：

​				zip  [选项] xxx  （功能描述：压缩文件和目录）

​				unzip [选项]  xxx.zip （功能描述：解压缩文件）

常用选项：

​				-r ：递归压缩

​				-d  <目录>：指定解压后文件的存放目录

应用案例：

​				案例1：将/home下的所有文件及文件夹进行压缩成 myhome.zip

​							`zip -r /home myhome.zip`

​				案例2：将myhome.zip解压到/opt/tmp目录下

​							`unzip -d /opt/tmp  /home/myhome.zip`

### 9.7.3 tar指令

tar 指令是打包指令，最后打包后的文件是*.tar.gz

基本语法：tar [选项]  xxx.tar.gz 要打包的内容 

常用选项：

![image-20210904102016532](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904102016532.png)

​				-C ：指定解压后的目录

应用案例：

​				案例1：压缩多个文件，将/home/pig.txt 和/home/cat.txt 压缩成pc.tar.gz

​							`tar -zcvf  pc.tar.gz /home/pig.txt /home/cat.txt`

​				案例2：将/home的文件夹 压缩成 myhome.tar.gz

​							`tar -zcvf myhome.tar.gz /home`

​				案例3：将pc.tar.gz解压到当前目录

​							`tar -zxvf pc.tar.gz`

​				案例4：将myhome.tar.gz 解压到 /opt/tmp2目录下

​							`tar -zxvf /home/myhome.tar.gz -C /opt/tmp2/`

# 十、组管理和权限管理

## 10.1 组基本介绍

在Linux中的每个用户都属于一个组，不能独立于组外。在Linux中的**每个文件有所有者、所在组、其他组**的概念。

## 10.2 文件/目录 所有者

一般为文件的创建者，谁创建了该文件，就自然的成为了该文件的所有者

### 10.2.1 查看文件的所有者

基本语法：ls -alh

特别说明： -h (human)：将文件的大小转为人眼可看的大小

![image-20210904104644118](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904104644118.png)

### 10.2.2 修改文件的所有者

基本语法：chown 用户名 文件名

案例：使用root用户创建一个文件apple.txt，然后将其所有者修改成tom

![image-20210904104843951](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904104843951.png)

## 10.3 组的创建

基本语法：groupadd 组名

应用案例：

​				案例1：创建一个组，monster

​						`groupadd monster`

​				案例2：创建一个用户fox ，并放入monster组中

​						`useradd -g monster fox`

## 10.4 文件/目录 所在组

当某个用户创建一个文件后，该文件的所在组，就是用户的所在组

### 10.4.1 查看文件/目录 所在组

基本语法：ls -l 

### 10.4.2 修改文件/目录 所在组

基本语法：chgrp 组名 文件名

应用实例：修改apple.txt文件所在组为monster

![image-20210904105613667](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904105613667.png)	

## 10.5 其他组

除了文件的所有者和所在组的用户外，系统的其他用户都是文件的其他组

## 10.6 修改用户所在组

基本语法：usermod -g 组名 用户名

​					usermode -d 目录名 用户名 （改变该用户登录的初始目录） **特别说明：用户需要进入到新目录的权限**

## 10.7 权限的基本介绍

ls -l 中显示的内容如下：

`-rw-r--r--. 1 tom  monster    0 9月   4 10:47 apple.txt`

0-9位说明：

第0位确定文件的类型（d，-，l，c，b）

**l：链接，相当于windows下的快捷方式**

**d：目录**

**-：文件**

**c：字符设备文件，鼠标、键盘等**

**b：块设备，比如硬盘**

第1-3位：是文件拥有者拥有该文件的权限。

第4-6位：是文件所在组的用户拥有该文件的权限

第7-9位：是其他组的用户所拥有的权限

## 10.8 rwx权限详解

rwx，分别对应read、write、执行的权限

也可以使用数字表示，4 = r ， 2 = w， 1 = x，因此rwx = 4+2+1 = 7

### 10.8.1 rwx作用到文件

+ r：代表可读(read)，可以读取文件
+ w：代表可写（write），可以修改文件，**但是不代表可以删除文件，删除文件的前提是该文件所在目录具有可写的权限。**
+ x：代表可执行(execute)，文件可以被执行的权限

### 10.8.2 rwx作用到目录

+ r：代表可读(read)，可以查看目录，ls
+ w：代表可写（write），可以修改目录，**对目录进行创建+删除+重命名目录**
+ x：代表可执行(execute)，可以进入该目录

## 10.9 文件及目录权限实际案例

`-rw-r--r--. 1 tom  monster    0 9月   4 10:47 apple.txt`

第一组：rw- ：代表文件拥有者拥有读+写的权限，但是没有执行的权限

第二组：r--：代表文件所在组拥有读的权限，没有写+执行的权限

第三组：r--：代表文件所在组拥有读的权限，没有写+执行的权限

## 10.10 修改权限 chmod

### 10.10.1 第一种方式： + 、-、 = 变更权限

u：拥有者，g：所在组，o：其他人，a：所有人（u + g + o = a）

+ chmod u = rwx，g=rx，o=x  文件/目录
+ chmod o + w 文件/目录名
+ chmod a - x  文件/目录名

应用案例：

​				案例1：给abc文件的拥有者读写执行的权限，给所在组读执行权限，给其他组读执行权限

​							`chmod u=rwx,g=rx,o=x`

​				案例2：给abc文件的拥有者除去执行的权限，增加组写的权限

​							`chmod u-x,g+w`

​				案例3：给abc文件的所有用户 添加读的权限

​							`chmod a+r`

### 10.10.2 第二种方式：通过数字变更权限

r=4，w=2，x=1     rwx =4+2+1 = 7

chmod u=rwx，g=rx，o=x 文件目录名  相等于 chmod 751 文件/目录名

# 十一、定时任务调度

## 11.1 crond 任务调度

crontab 进行 定时任务的设置

### 11.1.1 概述

任务调度：指系统在某个时间执行的特定的命令或程序

任务调度分类：

1. 系统工作：有些重要的工作必须周而复始地执行。如病毒扫描等
2. 个人工作：用户希望执行某些程序或者指令，比如对数据库的备份等

![image-20210904142622721](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904142622721.png)

### 11.1.2 基本语法

crontab [选项]

### 11.1.2常用选项

-l：查询contab定时任务

-e：编辑crontab定时任务

-r：删除当前用户所有的crontab任务

### 11.1.4 快速入门

设置任务调度文件：/etc/crontab

设置个人任务调度：执行 crontab -e命令

接着输入任务到调度文件

如：*/1 * * * * ls-l /home > /tmp/to.txt

意思是说 每分钟执行 ls-l /home > /tmp/to.txt命令

**参数细节说明**

5个占位符说明，分别代表分、时、日、月、周

![image-20210904143842884](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904143842884.png)

**特殊符号说明**

![image-20210904143910893](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904143910893.png)

**特殊时间执行案例**

![image-20210904144055420](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904144055420.png)

### 11.1.5 应用案例

案例1：每隔1分钟，就将当前的日期信息，追加到/tmp/mydate文件中

​		`*/1 * * * * date >> /tmp/mydate`

案例2：每隔1分钟，将当前日期和日历都追加到/home/mycal文件中

​		方式1：写两个命令追加到/home/mycal

​		方式2：写一个sh脚本，定时调度sh脚本

案例3：每天凌晨2点将mysql数据库testdb，备份到文件中。提示：指令为mysqldump -u root -p 密码 数据库 > /home/db.bak

`0 2 * * *  mysqldump -u root -proot testdb > /home/db.bak`

### 11.1.6  crond 相关指令

service crond restart：重启任务调度

## 11.2  at定时任务

### 11.2.1 基本介绍

1. at命令是**一次性定时计划任务**，at的守护进程atd会以后台模式运行，检查作业队列来运行。

2. 默认情况下，**atd守护进程每60秒检查作业队列**，有作业时，会检查作业运行时间，如果时间与当前时间匹配，则运行此作业。

3. at命令是一次性定时任务计划，执行完一个任务后不再执行此任务了。

4. **在使用at命令时，一定要保证atd进程的启动，**可以使用相关指令来查看

   ​	`ps -ef | grep atd`

5. 示意图

![image-20210904155652596](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904155652596.png)

### 11.2.2 at命令格式

基本语法：at [选项]  [时间]，输入完成之后 ，输入两次CTRL+D

常用选项：

![image-20210904155803638](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904155803638.png)

**时间定义：**

1. **接受在当天的hh:mm**(小时：分钟) 式的时间指定。假如时间已过去，那么就放在第二天执行。例如：04：00
2. 使用midnight（深夜），noon（中午），teatime（饮茶时间，一般是下午4点）等比较模糊的词语来指定时间。
3. 采用12小时计时制，即在**时间后面加上AM（上午）或PM（下午）**来说明是上午还是下午。例如：12pm
4. 指定命令执行的具体日期，指定格式为month day（月 日）或者mm/dd/yy（月/日/年） 或者dd.mm.yy（日.月.年），指定的日期必须跟在指定时间的后面。例如：04：00 2021-03-1
5. 使用相对计时法。指定的格式为：**now+count time-units**，就是当前时间，time-units是时间单位，这里能够是minutes（分钟）、hours（小时）、days（天）、weeks（星期）。count是时间的数量，几天，几小时。例如：now + 5 minutes
6. 直接使用**today**（今天），**tomorrow**（明天）来指定完成命令的时间

11.2.3 应用实例

+ 案例1：2天后的下午5点执行 ls /home

![image-20210904161421815](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904161421815.png)

+ 案例2：atq命令查看系统中没有被执行的工作任务

![image-20210904161431898](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904161431898.png)

+ 案例3：明天17点，输出时间到指定文件内  比如/home/date100.log

![image-20210904161554773](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904161554773.png)

+ 案例4：2分钟后，输出时间到指定文件内 比如/home/date200.log

![image-20210904161624790](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904161624790.png)

+ 案例5：**删除已经设置的任务**  		`atrm 编号`

![image-20210904161704480](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210904161704480.png)



# 十二、磁盘分区和挂载 

## 12.1 分区

### 12.1.1 原理介绍

1. Linux来说无论有几个分区，分给哪一个目录使用，归根结底只有一个根目录，一个独立且唯一的文件结构，Linux中每个分区都是用来组成整个文件系统的一部分。
2. Linux采用一个叫**“载入”**的处理方法，它的整个文件系统中包含了一整套的文件和目录，且**将一个分区和一个目录联系起来**。这时要载入的一个分区将使它的存储空间在一个目录下获得。
3. 示意图

![image-20210905082016085](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905082016085.png)

### 12.1.2 硬盘说明

1. Linux的硬盘分为IDE硬盘和SCSI硬盘，目前主要使用的是SCSI硬盘
2. 对于IDE硬盘，驱动器标识符为“**hdx~**”，其中“hd”表明分区所在设备的类型，这里指的是IDE硬盘了。“x"为盘号，（a为基本盘，b为基本从属盘，c为辅助主盘，d为辅助从属盘），”~“代表分区，前四个分区用数字1到数字4表示，它们是主分区或者扩展分区，从5开始就是逻辑分区。例如hda3，表示第一个IDE硬盘上的第三个主分区或扩展分区，hdb2表示为第二个IDE硬盘上的第二个主分区或扩展分区
3. 对于SCSI硬盘，驱动器标识符为”**sdx~**"，sd表示分区所在设备的类型，这里指的是SCSI硬盘。其余则和IDE硬盘的表示方法一样。

### 12.1.3 查看所有设备挂载情况

基本语法：**lsblk -f 或者 lsblk**

![image-20210905082923064](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905082923064.png)

## 12.2 挂载的经典案例

### 12.2.1 说明

下面我们以增加一个硬盘为例来熟悉磁盘的相关指令和深入理解磁盘分区、挂载、卸载的概念

### 12.2.2 增加磁盘并挂载的步骤

1. 增加一个磁盘
2. 分区
3. 格式化
4. 挂载
5. 设置自动挂载

### 12.2.3 详细步骤

**1.增加一个磁盘**

![image-20210905083317112](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905083317112.png)

![image-20210905083336258](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905083336258.png)



创建完磁盘之后，需要重新启动虚拟机。

**2.通过 `fdisk /dev/sdb` 开始分区**

+ m：显示命令列表
+ n：添加一个新的分区
+ p：显示磁盘分区 同 fdisk -l
+ d：删除一个分区
+ w：写入并退出
+ 说明：开始分区后先输入n，然后输入p，分区类型为**主分区**，两次回车默认剩余全部空间。最后输入w写入分区并退出，若不保存退出按q

![image-20210905083918612](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905083918612.png)

**3.格式化磁盘**

格式化磁盘命令：`mkfs -t ext4 /dev/sdb1`

其中ext4是分区的类型

**4.在根路径下创建一个新目录，newdisk，然后将分区与该目录进行挂载**

基本语法：`mount  设备名称  文件目录` 挂载

​					`umount 设备名称  文件目录` 取消挂载

![image-20210905084849849](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905084849849.png)

**注意：用命令进行挂载，重启后会失效。**

### 12.2.4 永久挂载

永久挂载：通过修改/etc/fstab 实现永久挂载

添加完成之后 执行mount -a 即刻生效

![image-20210905085245545](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905085245545.png)

![image-20210905085303919](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905085303919.png)

## 12.3 磁盘情况查询

### 12.3.1 查询系统整体磁盘使用情况

基本语法：df -h

应用实例：查询系统中磁盘的整体使用情况

![image-20210905085629081](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905085629081.png)

一般磁盘已用超过**80%** 就需要想办法清理磁盘了。

### 12.3.2 查询指定目录的磁盘占用情况

基本语法：du -h

查询指定目录的磁盘占用情况，默认为当前目录

-s：指定目录占用大小汇总

-h：带计量单位

-a：含文件

--max-length=1 子目录深度

-c：列出明细的同时，增加汇总值 

应用实例：

查询/opt目录的磁盘占用情况，深度为1

![image-20210905090137321](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905090137321.png)

## 12.4 磁盘情况-工作实用指令

1. 统计/opt文件夹下文件的个数

   ​	`ls -l /home | grep '^-' | wc -l`

2. 统计/opt文件夹下目录的个数

   ​	`ls -l /home | grep '^d' | wc -l`

3. 统计/opt文件夹下文件的个数，包括子文件夹里的

   ​	`ls -Rl /home | grep '^-' | wc -l`

4. 统计/opt文件夹下目录的个数，包括子文件夹里的

   ​	`ls -Rl /home | grep '^d' | wc -l`

# 十三、网络配置

## 13.1 Linux网络配置原理图

![image-20210905104227688](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905104227688.png)

## 13.2 查看网络IP和网关

### 13.2.1 查看虚拟网络编辑器和修改IP地址

![image-20210905104337656](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905104337656.png)

### 13.2.2 查看网关

![image-20210905104426455](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905104426455.png)



### 13.3 查看windows环境中的VMnet8网络配置(ipconfig指令)

![image-20210905104535834](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905104535834.png)

## 13.4 查看Linux的网络配置

![image-20210905104627396](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905104627396.png)

## 13.5 ping测试主机之间网络连通性

基本语法：ping  目的主机ip（功能描述：测试当前服务器是否可以连接目的主机）

## 13.6 Linux网络环境配置

### 13.6.1 第一种方式(自动获取)

登录Linux之后，通过界面的方式来设置自动获取ip。

特点：每次自动获取IP地址，可以减少IP冲突

![image-20210905105143366](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905105143366.png)

### 13.6.2 第二种方式(指定ip地址)

直接通过修改网络配置文件的方式，指定IP地址。

**特点：一般的服务器，都需要一个指定的IP地址。**

编辑： `vi /etc/sysconfig/network-scripts/ifcfg-ens33 `

主要改如下几个部分

```
#由原来的hpdc改成static
BOOTPROTO="static"
#IP地址
IPADDR=192.168.200.2
#网关
GATEWAY=192.168.200.2
#域名解析器
DNS1=192.168.200.2
```

**修改NAT网络的网关和ip地址**

![image-20210905110032035](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905110032035.png)



**重启Linux网络服务或者重启**：`service network restart `  or   `reboot`

## 13.7 设置主机名和hosts映射

### 13.7.1 设置主机名

1. 为了方便记忆，可以给**Linux系统设置主机名**，也可以根据需要修改主机名
2. **查看主机名：`hostname`**
3. 修改主机名在 **`/etc/hostname `**指定
4. 修改后，**重启**生效

### 13.7.2 设置hosts映射

思考：如何通过主机名找到（比如ping）Linux系统？

**windows：**

在**C:\\Windows\System32\drivers\etc\hosts** 文件指定即可

比如：196.168.36.134   mylinuxos

**Linux：**

在**/etc/hosts** 文件指定

比如： 192.168.36.1  dell200

## 13.8 主机域名解析过程分析(Hosts、DNS)

### 13.8.1 Hosts是什么

一个文本文件，**记录了IP和Hostname（主机名）的映射关系**

### 13.8.2 DNS

DNS，全程 domain Name System的缩写，域名系统

是互联网上作为**域名和IP地址相互映射的一个分布式数据库**

### 13.8.3 应用实例：用户在浏览器输入了www.baidu.com后都做了什么





![image-20210905112117916](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905112117916.png)

# 十四、进程管理

## 14.1 基本介绍

1. 在Linux中，每个执行的程序都被称为一个进程。每一个进程都分配一个ID号（pid，进程号）。
2. 每个进程都有可能**以两种方式存在，前台与后台**。所谓前台进程就是用户目前的屏幕上可以进行操作的。后台进程则是实际在操作，但由于屏幕上无法看到的进程，通常使用后台方式执行。
3. 一般系统的服务都是以后台进程的方式存在，而且都会常驻在系统中。直到关机才结束。

## 14.2 显示执行的进程

ps命令用来查看目前系统中，有哪些正在执行，以及它们执行的状况。可以不加任何参数。

基本语法：ps [选项] 

**常用选项：**

​				-a：显示当前终端的所有进程信息

​				-u：以用户的格式显示进程信息

​				-x：显示后台进程运行的参数

**注意：ps  -ef 的效果 等同于 ps -aux ，只是风格不一样。**

![image-20210905122959312](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905122959312.png)

### 14.2.1 ps详解

+ 指令：ps -aux | grep xxx
+ 指令说明：
  + USER：进程的执行用户
  + PID：进程编号
  + %CPU：进程占用cpu的百分比
  + %MEM：进程占用物理内存的百分比
  + VSZ：进程占用的虚拟内存大小（单位：KB）
  + RSS：进程占用的物理内存大小（单位：KB）
  + TTY：终端名称，缩写
  + STAT：进程的状态，**其中S-睡眠**，s-表示该进程是会话的先导进程，N-表示进程拥有比普通优先级更低的优先级，**R-正在运行**，D-短期等待，**Z-僵死进程**，T-被跟踪或者被停止
  + START：进程的启动时间
  + TIME：进程使用CPU的时间
  + COMMAND：启动进程使用的命令和参数，**如果过长会被截断**

### 14.2.2 应用实例

要求：以全格式显示当前所有的进程，查看进程的父进程。查看sshd的**父进程**信息

ps -ef 是**以全格式**显示当前所有的进程，**可以显示父进程的pid**

-e 显示所有进程， -f 全格式

## 14.3 终止进程 kill 和 killall

### 14.3.1 介绍

若是进程执行了一半需要停止时，或是已消耗了很大的系统资源时，此时可以考虑停止该进程。使用kill命令来完成此项任务。

### 14.3.2 基本语法

kill [选项] 进程编号 （功能描述：通过进程号来杀死进程）

killall 进程名（功能描述：通过进程名称来杀死进程，**支持通配符**，这在系统因负载过大而变得很慢时很有用）

### 14.3.3 常用选项

`- 9` ：表示强制停止进程

## 14.4 查看进程树pstree

基本语法：pstree  [选项]

常用选项：

				- p ：显示进程的PID
				- u：显示进程的所属用户

应用案例：

​			案例1：请以树状的形式显示进程的pid

​				`pstree -p `

​			案例2：请以树状的形式显示进程的用户

​				`pstree -u`

## 14.5 服务(service)管理

### 14.5.1 介绍

服务(service)本质就是进程，但是是运行在后台的，通常监听某个端口，等待其他程序的请求，比如(mysqld，sshd，防火墙等)，因此我们又称为守护进程，是Linux中非常重要的知识点。

### 14.5.2 service管理指令

1. **service 服务名  [start|stop|restart|reload|status]**
2. **在CentOS7.0后，很多服务不再使用service了，而是使用systemctl；**
3. service指令管理的服务在/etc/init.d查看

![image-20210905155355685](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905155355685.png)

### 14.5.3 service管理指令案例

请使用service指令，查看，关闭，启动 network [注意：在虚拟系统演示，因为网络连接会关闭]

指令：

service network stop

service network start

service network restart

### 14.5.4 查看服务名

方式1：使用setup 指令，就可以看到全部的系统服务

![image-20210905163018164](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905163018164.png)

方式2：/etc/init.d 看到 service指令管理的服务

​			`ls -l /etc/init.d`

### 14.5.5 服务的运行级别

Linux有七种运行级别（runlevel）：常用的级别是3和5

运行级别0：关机

运行级别1：单用户状态

运行级别2：多用户无网络状态

运行级别3：多用户有网络状态

运行级别4：系统未使用，保留

运行级别5：图形化界面

运行级别6：重启状态

开机的流程说明：

![image-20210905163317697](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905163317697.png)

### 14.5.6 CentOS7 后运行级别说明

在/etc/initab

进行了简化，如下：

systemctl  get-default：查看默认的运行级别

systemctl set-default 运行级别.TARGET：设置运行级别

### 14.5.7 chkconfig 指令

**通过chkconfig可以设置服务在各个运行级别下的 启动/关闭状态**

chkconfig服务可以在/etc/init.d查看

chkconfig基本用法：

查看服务：chkconfig --list

设置服务启动/关闭状态：chkconfig  --level  5  服务名 on/off

### 14.5.8 systemctl管理指令

基本语法：systemctl [start|stop|restart|status] 服务名

**systemctl 指令**管理的服务在 **/usr/lib/systemd/system** 查看

### 14.5.9 systemctl设置服务的自启动状态

systemctl list-unit-files [ | grep  服务名]：查看服务开机启动状态

systemctl enable 服务名：设置服务开机启动

systemctl  disable 服务名：关闭服务开机启动

systemctl is-enabled 服务名：查询某个服务是否自启动的

### 14.5.10 应用案例

查看当前防火墙的状态，关闭防火墙和重启防火墙

`systemctl status firewalld   systemctl stop firewalld 	systemctl restart firewalld`

**细节讨论：**

+ 关闭或者启用防火墙后，立即生效。[talnet 测试 某个端口号]
+ 这种方式只是临时生效，当重启系统后，还是回归以前对服务的设置
+ **如果希望设置某个服务自启动或关闭永久生效，要使用systemctl [enable | disable] 服务名。**

### 14.5.11 打开或者关闭指定端口

在真实的环境中，我们往往需要将防火墙打开，这样，外部请求数据包就不能跟服务器监听端口通讯，这时，**需要打开指定的端口**。比如80、22、8080等。

![image-20210905174146201](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905174146201.png)

### 14.5.12 firewall指令

1. 打开端口：firewall -cmd --permanent --add-port= 端口号/协议
2. 关闭端口：firewall-cmd--permanent--remove-port=端口号/协议
3. 重新载入，才能生效：firewall-cmd--reload
4. 查询端口号是否开放：firewall-cmd--query-port=端口号/协议

## 14.6 动态监听进程

### 14.6.1 介绍

top命令与ps命令类似，它们都是用来显示正在执行的进程。Top与ps最大的不同之处是**Top是实时更新的，默认是3秒一次更新。**

### 14.6.2 基本语法

基本语法：top [选项]

**常用选项：**

![image-20210905180555136](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905180555136.png)

### 14.6.3 交互操作说明

![image-20210905180622604](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905180622604.png)

### 14.6.4 应用案例

案例1：监视特定用户，比如tom

首先输入**top**指令，然后输入**u**，最后输入**tom**

案例2：终止指定的进程，比如我们要结束tom登录

先输入**top**指令，然后输入**k**，再输入tom的**PID**

## 14.7 监控网络状态

查看系统网络情况 netstat

基本语法：netstat  [选项]

常用选项：

​				-an：按一定顺序排列输出

​				-p：显示哪个进程在调用

应用案例：请查看服务名为sshd的服务网络信息

​		`netstat -anp  | grep sshd`

![image-20210905182713101](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905182713101.png)

![image-20210905182733938](D:\learnnotes\Linux基础\Linux基础学习.assets\image-20210905182733938.png)

## 14.8 检测主机连接命令ping

主要是检测远程主机是否连接正常

**基本语法：ping 目的主机ip**

# 十五章、RPM与YUM

## 15.1 rpm包的管理

### 15.1.1 介绍

rpm用于互联网下载包的打包以及安装工具，它包含在某些Linux分发版中。它生成具有.RPM扩展名的文件。RPM是RedHat  Package   Manager的缩写。

### 15.1.2 rpm包的简单查询指令

基本语法：rpm -qa （功能描述：查询已安装的rpm列表）

应用案例：查看当前系统中是否安装了firefox

​		`rmp -qa | grep firefox `

### 15.1.3 rpm包名基本格式

一个rpm包：fire-fox-60.2.2-1.el7.centos.x86_64

名称：firefox

版本号：60.2.2.1

使用操作系统：centos.x86_64

如果是i686、i386表示32位系统，noarch表示通用

### 15.1.4 rpm包的其他查询指令

+ rpm -qa：查询所有已经安装的rpm包
+ rpm -q  软件包名：查询软件包是否安装

​		`rpm -q firfox`

+ rpm -qi  软件包名：查询软件包信息
+ rpm -ql  软件包名：查询软件包中的文件
  + 比如：rpm -ql  firfox
+ **rpm -qf  文件全路径名 ：查询文件所属的软件包**
  + rpm -qf  /etc/passwd
  + rpm -qf /root/install.log

### 15.1.5 卸载rpm包

基本语法：rpm -e rpm包名

应用案例：删除firefox的软件包

​	`rpm -e firefox`
**细节讨论：**

1. 如果其他软件包依赖于你要删除的软件包，卸载时则会产生错误信息

   ​	`rpm -e foo`

`removing these packages would break dependencies：foo is needed by bar-1.0.1`

2. 如果我们就是要删除foo这个rpm包，可以增加参数 **--nodeps**，就可以强制删除，一般不推荐这样做

   ​		`rpm -e --nodeps foo `

### 15.1.6 安装rpm包

基本语法：rpm [选项]  软件包名

常用选项：

​				 i =install：安装

​				v=verbose：提示

​				h=hash：进度条

## 15.2 yum

### 15.2.1 介绍

Yum是一个Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理包之间的依赖关系。

### 15.2.2 yum指令

基本语法：

​			**yum list**：查询YUM服务器是否有需要安装的软件

​			**yum install  xxx**：安装某个rpm包

​			**rpm -e xxx** ：删除某个rpm包





