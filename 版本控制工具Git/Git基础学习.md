# 1、Git概述

Git是一个免费的、开源的**<span style="color:red">分布式版本控制系统</span>**，可以快速高效的处理从小型到大型的各种项目。

Git易于学习、占地面积小、性能极快。它具有廉价的本地库，方便得暂存区域和多个工作流分支等特性。其性能优于Subversion、CVS、Perforce和ClearCase等版本控制工具。

## 1.1 何为版本控制

版本控制是一种记录文件内容变化，以便将来查阅特定版本修订情况的系统。

版本控制其实最重要的是可以记录文件修改历史记录，从而让用户查看历史版本，方便版本切换。

![image-20210531174042938](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531174042938.png)

## 1.2 为什么需要版本控制

个人开发过渡到团队协作

![image-20210531174151279](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531174151279.png)

## 1.3 版本控制工具

+ **<span style="color:red">集中式</span>**版本控制工具

  + **<span style="color:red">SVN</span>**、CVS、VSS

  集中化的版本控制系统诸如SVN、CVS等等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连接到这台服务器，取出最新的文件或者提交更新。多年以来，这已成为版本控制系统的标准做法。

  这种做法带来了**许多好处**，每个人都可以在一定程度上看到项目中的其他人正在做些什么。而**管理员也可以轻松掌握每个开发者的权限**，并且管理一个集中化的版本控制系统，要远比在各个客户端维护本地数据库来的轻松容易。

  事分两面，有好有坏。**这么做显而易见的缺点**是中央服务器的单点故障。如果服务器宕机一小时，那么在这一小时内，谁都无法提交更新，也就无法协同工作。

  ![image-20210531180027522](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531180027522.png)

+ **<span style="color:red">分布式</span>**版本控制工具
  
  + **<span style="color:red">git</span>**、Bazaar、Darcs..

像git这种分布式版本控制系统，客户端提取的不是最新版本的文件快照，而是把代码仓库完整地镜像下来（本地库）。这样任何一处协同工作用的文件发生故障，事后都可以用其他客户端的本地仓库进行恢复。因为每个客户端的每一次文件提取操作，实际上都是一次对整个文件仓库的完整备份。

分布式版本控制系统的出现，解决了集中式版本控制系统的缺陷：

1. 服务器断网的情况下也可以进行开发（因为版本控制是在本地进行的）
2. 每个客户端保存的也都是整个完整的项目（包含历史记录、更加安全）

![image-20210531180439763](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531180439763.png)



## 1.4 Git简史

![image-20210531181354723](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531181354723.png)

## 1.5 Git工作机制

![image-20210531183214734](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531183214734.png)

## 1.6 Git和代码托管中心

代码托管中心是基于网络服务器的远程代码仓库，一般我们简单称为**<span style="color:red">远程库</span>**

+ 局域网
  + GitLab
+ 互联网
  + GitHub（外网）
  + Gitee码云（国内）

# 2、Git安装

官网地址：https://git-scm.com/

查看GNU 协议，可以直接点击下一步。

![image-20210531185152868](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531185152868.png)

选择 Git安装位置，要求是**<span style="color:red">非中文并且没有空格的目录</span>**然后下一步。

![image-20210531185236971](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531185236971.png)

Git选项配置，**推荐默认设置**，然后下一步。

![image-20210531185308387](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531185308387.png)

Git安装目录名，不用修改，直接点击下一步。

![image-20210531185350550](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531185350550.png)

Git的 默认编辑器，建议使用的默认的vim编辑器，然后下一步。

![image-20210531185425628](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531185425628.png)

默认分支名设置，选择让 Git决定，分支名默认为`master`，下一步。

![image-20210531185507883](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531185507883.png)

修改 Git的环境变量，选第一个，不修改环境变量，只在Git Bash里面使用Git。

![image-20210531185556411](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531185556411.png)

选择后台客户端连接协议，选默认值 **`OpenSSL`**，然后下一步。

![image-20210531185642012](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531185642012.png)

配置 Git文件的行末换符，Windows使用CRLF，Linux使用LF，选择第一个自动转换，然后继续下一步。

![image-20210531185930192](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531185930192.png)

选择Git终端类型，选择默认的Git Bash终端，然后继续下一步。

![image-20210531190025989](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531190025989.png)

选择`Git pull`合并的模式，选择默认，然后下一步。

![image-20210531190058110](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531190058110.png)

选择 Git的凭据管理器，选择默认的跨平台凭据管理器，然后下一步

![image-20210531190124549](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531190124549.png)

其他配置，选择默认设置，然后下一步。 

![image-20210531190145381](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531190145381.png)

实验室功能， 技术还不成熟， 有已知的的 bug，不要勾选，然后点击右下角的 Install按钮，开始安装Git。

![image-20210531190238347](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531190238347.png)

点击 Finsh按钮， Git安装成功！ 

![image-20210531190254452](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531190254452.png)

右键任意位置，在菜单里选择`Git Bash Here`即可打开`Git Bash`命令行终端。

![image-20210531190341946](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531190341946.png)

在 Git Bash终端里输入` git --version`查看 git版本，如图所示说明 Git安装成功。

![image-20210531190449876](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531190449876.png)



# 3 Git常用命令

| **命令名称**                                                 | **作用**         |
| ------------------------------------------------------------ | ---------------- |
| **git config --global user.name 用户名**                     | **用户签名**     |
| **git config --global user.email 邮箱**                      | **用户签名**     |
| **<span style="color:red">git init</span>**                  | 初始化本地库     |
| **<span style="color:red">git status</span>**                | 查看本地库状态   |
| **<span style="color:red">git add 文件名</span>**            | 添加文件到暂存区 |
| **<span style="color:red">git commit -m “日志文件” 文件名</span>** | 提交文件到本地库 |
| **<span style="color:red">git reflog</span>**                | 查看历史记录     |
| **<span style="color:red">git reset -hard 版本号</span>**    | 版本穿梭         |

## 3.1 设置用户签名

**1) 基本语法**

git config --global user.name 用户名

git config --global user.email 邮箱

**2) 案例实操**

![image-20210531193332483](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531193332483.png)

说明：

​	签名的作用是区分不同操作者身份。用户的签名信息在每一个版本的提交信息中都能够看到，以此来确认本次提交是谁做的。**<span style="color:red">Git首次安装必须设置用户签名，否则无法提交代码。</span>**

**<span style="color:red">注意：</span>**这里设置的用户签名与将来登录GitHub（或其他代码托管中心）的账号没有任何关系。

## <span style="color:red">3.2 初始化本地库</span>

**1)基本语法**

git init

**2)初始化本地库**

![image-20210531194254927](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531194254927.png)

**3)结果查看**

![image-20210531194320672](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531194320672.png)

## 3.3 查看本地库状态

**1)语法**

git status

**2)案例实操**

### 3.3.1 首次查看（工作区没有任何文件）

![image-20210531211239557](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531211239557.png)

### 3.3.2 新增文件

![image-20210531211407835](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531211407835.png)

### 3.3.3 再次查看状态（检测到未追踪的文件）

![image-20210531211437388](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531211437388.png)

## 3.4 添加文件到暂存区

### 3.4.1 将工作区的文件添加到暂存区

**1)语法：**

**git add 文件名**

**2)案例实操**

![image-20210531211608853](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531211608853.png)

### 3.4.2 查看状态（检测到暂存区有新文件）

![image-20210531211642445](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531211642445.png)

## 3.5 提交文件到本地库

### 3.5.1 将暂存区的文件提交到本地库

**1)基本语法**

git commit -m “日志信息” 文件名

**2)案例实操**

![image-20210531211831932](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531211831932.png)

### 3.5.2 查看状态（没有文件需要提交）

![image-20210531211855195](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531211855195.png)

## 3.6 修改文件（hello.txt）

![image-20210531213153515](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531213153515.png)

### 3.6.1 查看状态（检测到工作区有文件被修改）

![image-20210531213218981](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531213218981.png)

### 3.6.2 将修改的文件添加到暂存区

![image-20210531213244011](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531213244011.png)

### 3.6.3 查看状态（工作区的修改添加到暂存区）

![image-20210531213306833](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531213306833.png)

## <span style="color:red">3.7 历史版本</span>

### 3.7.1 查看历史版本

**1)基本语法**

git reflog 查看版本信息——简洁版

git log 查看版本信息—详细版

**2)案例实操**

![image-20210531213430968](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531213430968.png)

### 3.7.2 版本穿梭

**1)基本语法**

**<span style="color:red">git reset --hard  版本号</span>**

**2)案例实操**

![image-20210531213608735](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531213608735.png)

**<span style="color:red">Git切换版本，底层其实是移动的HEAD指针，具体原理如下图所示</span>**

![image-20210531213700060](Git%E5%AD%A6%E4%B9%A0.assets/image-20210531213700060.png)



# 4、Git分支操作

![image-20210601081300717](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601081300717.png)

## 4.1 什么是分支

在版本控制过程中，我们会同时推进多个任务，为每个任务，我们就可以创建每个任务的单独分支。使用分支意味着程序员可以把自己的工作从开发主线分离出来，开发自己分支的时候，不会影响主线分支的运行。对于初学者，分支可以简单理解为副本，一个分支就是单独的副本。（分支底层其实也是指针的引用）

![image-20210601081729738](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601081729738.png)

## 4.2 分支的好处

可以同时并行推进多个功能开发，提高开发效率。

各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。

## 4.3 分支的操作

| 命令名称            | 作用     |
| ------------------- | -------- |
| git branch 分支名   | 创建分支 |
| git branch -v       | 查看分支 |
| git checkout 分支名 | 切换分支 |
| git merge 分支名    | 合并分支 |

### 4.3.1 查看分支

**1) 基本语法**

**git branch -v** 

**2) 案例实操**

![image-20210601083248033](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601083248033.png)

### 4.3.2 创建分支

**1) 基本语法**

**git branch 分支名**

**2) 案例实操**

![image-20210601083357261](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601083357261.png)

### <span style="color:red">4.3.3 切换分支</span>

**1) 基本语法**

**git checkout 分支名**

**2) 案例实操**

![image-20210601083614895](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601083614895.png)

### <span style="color:red">4.3.4 合并分支</span>

**1) 基本语法**

**git merge 分支名**

**2) 案例实操	在master分支上合并hot-fix分支**  

![image-20210601084358827](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601084358827.png)

### 4.3.5 合并分支时产生冲突

冲突产生的表现：后面的状态为**<span style="color:red">MERGING</span>**

**冲突产生的原因：**

![image-20210601091202694](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601091202694.png)

![image-20210601091226146](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601091226146.png)



​		合并分支时，两个分支在**<span style="color:red">同一个文件的同一个位置</span>**有两套完全不同的修改。Git无法替我们决定使用哪一个。必须**<span style="color:red">人为决定</span>**新代码内容。

**查看状态：（检测到有文件有两处修改）**

![image-20210601091309878](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601091309878.png)

###  4.3.6  解决冲突

1） 编辑有冲突的文件，删除特殊符号，保留要使用的内容

特殊符号：`<<<<<<<HEAD` 当前分支的代码 `=======` 合并过来的代码 `>>>>>>>hot-fix`

![image-20210601092655214](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601092655214.png)

2）将修改的文件添加到暂存区

![image-20210601092735859](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601092735859.png)

3）执行提交（注意：此时使用**`git commit`**命令时**<span style="color:red">不能带文件名</span>**）

![image-20210601092941653](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601092941653.png)

master、hot-fix 其实都是指向具体版本记录的指针。当前所在的分支，其实是由HEAD
决定的。所以创建分支的本质就是多创建一个指针。
HEAD 如果指向master，那么我们现在就在master 分支上。
HEAD 如果执行hotfix，那么我们现在就在hotfix 分支上。

**<span style="color:red">所以切换分支的本质就是移动HEAD 指针。</span>**

# 5、Git团队协作机制

## 5.1团队内协作

![image-20210601094438985](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601094438985.png)

## 5.2跨团队协作

![image-20210601094547129](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601094547129.png)

# 6、GitHub操作

Github网址：https://github.com/

## 6.1 创建远程仓库

![image-20210601100919596](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601100919596.png)

![image-20210601101007178](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601101007178.png)

## 6.2 远程仓库操作

| 命令名称                                                     | 作用                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **git remote -v**                                            | **查看当前所有远程地址别名**                                 |
| **git remote add 别名 远程地址**                             | **起别名**                                                   |
| **<span style="color:red">git push 别名 分支</span>**        | **<span style="color:red">推送本地分支上的内容到远程仓库</span>** |
| **<span style="color:red">git pull 远程地址别名 远程分支名</span>** | **<span style="color:red">将远程仓库的远程分支上的最新内容拉下来后与当前本地分支直接合并</span>** |
| **<span style="color:red">git clone 远程地址</span>**        | **<span style="color:red">将远程仓库的内容克隆到本地</span>** |

### 6.2.1 创建远程仓库别名

**1）基本语法**

**git remote -v 查看当前所有远程仓库别名**

**git remote add 别名 远程仓库**

**2）案例实操**

![image-20210601101655623](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601101655623.png)

### 6.2.2 推送本地分支到远程库

**1）基本语法**

**<span style="color:red">git push 远程地址/远程地址别名 本地分支</span>**

**2）案例实操**

![image-20210601103105988](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601103105988.png)

此时发现我们已经将master分支的内容推送到远程库中

![image-20210601103152675](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601103152675.png)

### 6.2.3 拉取远程库最新内容到本地

**1）基本语法**

**git pull 远程地址/远程地址别名 分支**

**2）案例实操**

![image-20210601103352410](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601103352410.png)

![image-20210601103412730](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601103412730.png)

### 6.2.4 克隆远程仓库到本地

**1）基本语法**

**<span style="color:red">git clone 远程仓库地址</span>**

**2）案例实操**

![image-20210601104552581](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601104552581.png)

![image-20210601104617977](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601104617977.png)

![image-20210601104655775](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601104655775.png)

**小结：clone操作会做三件事：(1) 拉取代码。(2) 初始化本地仓库。(3) 创建别名**

### 6.2.5 邀请加入团队(团队内协作机制)

**1）选择邀请合作者**

![image-20210601110005664](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601110005664.png)

**2）填入想要邀请的人**

![image-20210601110030747](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601110030747.png)

**3）复制地址并通过钉钉或者微信等方式发送给该用户，复制内容如下：**

https://github.com/atguiguyueyue/git-shTest/invitations

![image-20210601110125577](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601110125577.png)

**4）在atguigulinghuchong这个账号中的地址栏复制收到邀请的链接，点击接收邀请。**

![image-20210601110223125](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601110223125.png)

**5）成功之后可以在atguigulinghuchong这个账号上看到git-test的远程仓库**

![image-20210601110253269](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601110253269.png)

**6）令狐冲可以修改内容并push到远程仓库**

![image-20210601110347680](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601110347680.png)

![image-20210601110401184](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601110401184.png)

**7）回到atguiguyueyue的GitHub远程仓库中可以看到，最后一次是lhc提交的。**

![image-20210601110437088](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601110437088.png)

![image-20210601110440925](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601110440925.png)

## 6.3 跨团队协作

**1）将远程仓库地址复制发给邀请跨团队协作的人，比如东方不败**

![image-20210601112227623](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601112227623.png)

**2）在东方不败的GitHub账号里的地址栏复制收到的连接，然后点击fork将项目叉到自己的本地仓库**

![image-20210601112330193](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601112330193.png)

叉入成功可以看到当前仓库信息

![image-20210601114045203](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114045203.png)

**3）东方不败就可以在线编辑叉过来的文件**

![image-20210601114113769](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114113769.png)

![image-20210601114123432](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114123432.png)

**4）编辑完成后，填写描述信息并点击左下角绿色按钮提交**

![image-20210601114209785](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114209785.png)

**5）接下来点击上方的pull请求，并创建一个新的请求。**

![image-20210601114235171](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114235171.png)

![image-20210601114242498](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114242498.png)

![image-20210601114248850](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114248850.png)

**6）回到岳岳GitHub帐号可以看到一个Pull request请求**

![image-20210601114323109](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114323109.png)

![image-20210601114325736](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114325736.png)

进入聊天室，可以讨论相关内容；

![image-20210601114403842](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114403842.png)

![image-20210601114411080](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114411080.png)

**7）如果代码没有问题，点击Merge pull request合并代码**

![image-20210601114448642](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114448642.png)

![image-20210601114457333](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114457333.png)

## 6.4 SSH 免密登录

**我们可以看到远程仓库有一个SSH的地址 ，因此可以通过SSH进行访问。**

![image-20210601114550031](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114550031.png)

**具体操作如下：**

1. 进入当前用户的家目录下

![image-20210601114657178](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601114657178.png)

2. 如果有`.ssh`目录，删除目录

**rm -rvf .ssh**

3. 运行命令生成.ssh秘钥目录

**`ssh -keygen -t ras -C leo@163.com`** **[注意 这里的-C参数 是大写的]**

4. 进入`.ssh`目录，查看 `id_rsa.pub`文件

  `cd .ssh  `

`cat id_rsa.pub`

5. 复制id_rsa.pub文件内容，登录GitHub，点击用户头像-settings-SSH and GPG keys

![image-20210601115137319](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601115137319.png)

![image-20210601115141089](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601115141089.png)

![image-20210601115147090](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601115147090.png)



接下来再往远程仓库push东西的时候使用SSH就不需要再登录了。

# 7、IDEA集成Git

## 7.1 配置Git忽略文件

1）Eclipse特定忽略文件

![image-20210601132524792](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601132524792.png)

2）IDEA特定忽略文件

![image-20210601132537270](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601132537270.png)

3）Maven工程的target目录

![image-20210601132551379](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601132551379.png)

**问题1：为什么要忽略他们？**

与项目的实际功能无关，不参与服务器上部署运行。把它们忽略掉能屏蔽IDE工具之间的差异。

**问题2：怎么忽略？**

1）创建忽略规则文件**<span style="color:red">xxx.ignore（前缀名随便起，建议是git.ignore）</span>**

这个文件的位置原则上在哪里都可以，为了便于~/.gitconfig文件，**<span style="color:red">建议也放在家目录下。</span>**

![image-20210601133057134](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601133057134.png)

2）git.ignore文件模板内容如下：

```xml
# Compiled class file
*.class
# Log file
*.log
# BlueJ files
*.ctxt
# Mobile Tools for Java (J2ME)
.mtj.tmp/
# Package Files #
*.jar
*.war
*.nar
*.ear
*.zip
*.tar.gz
*.rar
# virtual machine crash logs, see 
http://www.java.com/en/download/help/error_hotspot.xml
hs_err_pid*
.classpath
.project
.settings
target
.idea
*.iml
```

3）在.gitconfig中引用忽略配置文件（此文件在Windows的家目录下）

```
[user]
name = Layne
email = Layne@atguigu.com
[core]
excludesfile = C:/Users/Administrator/git.ignore
注意：这里要使用“正斜线（/）”，不要使用“反斜线（\）”
```

## 7.2 定位Git程序

![image-20210601133648246](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601133648246.png)

## 7.3 初始化本地库

![image-20210601134727941](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601134727941.png)

选择要创建git本地仓库的工程

![image-20210601134902622](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601134902622.png)

## 7.4 添加到暂存区

右键项目点击选择Git  -> Add 将项目添加到暂存区

![image-20210601135013432](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601135013432.png)

**<span style="color:red">添加到暂存区后，代码颜色会变成绿色</span>**

## 7.5 提交到本地库

![image-20210601135902496](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601135902496.png)

![image-20210601140609825](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601140609825.png)

## 7.6 切换版本

![image-20210601140759110](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601140759110.png)



**右键要选择切换的版本，然后在菜单里点击Checkout Revision**
![image-20210601140850573](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601140850573.png)

## 7.7 创建分支

**（1）第一种方式**

右击项目 选择Git  -> Repository -> Branches

![image-20210601142445365](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601142445365.png)



**（2）第二种方式**

右下角选择Git -> New Branches

![image-20210601142542411](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601142542411.png)

填写分支名称，创建 hot-fix分支。

![image-20210601142615903](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601142615903.png)

然后在右下角可以看到hot-fix，说明分支创建成功，并且已经切换到hot-fix分支了。

![image-20210601142817746](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601142817746.png)



## 7.8 切换分支

在IDEA的右下角，切换到mster分支

![image-20210601142905820](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601142905820.png)

## 7.9 合并分支

在IDEA的右下角，将hot-fix分支合并到当前master分支

![image-20210601143131153](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601143131153.png)

如果代码没有冲突，分支直接合并成功，**<span style="color:red">分支合并成功后，代码自动提交，无需手动提交到本地库。</span>**

![image-20210601143308885](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601143308885.png)

## 7.10 解决冲突

如图所示，如果master分支和hot-fix分支都修改了代码，在合并分支的时候就会发生冲突。

![image-20210601144606953](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601144606953.png)

![image-20210601144613370](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601144613370.png)

我们现在站在master分支上合并hot-fix分支，就会发生冲突

![image-20210601145033739](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145033739.png)

点击Conficts框里的Merge按钮进行手动合并

![image-20210601145011787](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145011787.png)

手动合并完代码以后，点击右下角的 Apply按钮。

![image-20210601145050677](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145050677.png)

代码冲突解决，自动提交本地库。

![image-20210601145106036](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145106036.png)

# 8、IDEA集成GitHub帐号

## 8.1 设置GitHub帐号

![image-20210601145151888](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145151888.png)

如果出现401等情况连接不上的，是因为网络原因，可以使用以下方式进行连接：

![image-20210601145231711](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145231711.png)![image-20210601145237720](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145237720.png)

然后去 然后去 GitHub账户 上设置 上设置 token。

![image-20210601145249176](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145249176.png)![image-20210601145256035](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145256035.png)

![image-20210601145302847](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145302847.png)

![image-20210601145306055](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145306055.png)

![image-20210601145311001](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145311001.png)

 点击生成 token。

![image-20210601145324176](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145324176.png)

复制红框中的字符串到IDEA

![image-20210601145348840](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145348840.png)

点击登录。

![image-20210601145358048](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601145358048.png)



## 8.2 分享工程到GitHub

![image-20210601151137990](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601151137990.png)

![image-20210601151236997](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601151236997.png)

![image-20210601151308857](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601151308857.png)

来到GitHub中发现已经帮我们创建好了git-demo的远程仓库

![image-20210601151353774](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601151353774.png)

## 8.3 推送到远程仓库

右键 点击项目，可以将当前分支的内容push到GitHub的远程仓库中。

![image-20210601151528366](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601151528366.png)

![image-20210601151538551](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601151538551.png)

![image-20210601151706306](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601151706306.png)

![image-20210601151807955](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601151807955.png)

![image-20210601152042717](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601152042717.png)



注意：push是将本地库代码推送到远程库，如果本地库的代码跟远程库的代码版本不一致，push的操作是会被拒绝的。也就是说，**要想push成功，一定要保证本地库的版本要比远程库的版本高！**<span style="color:red">因此一个成熟的程序员在动手改本地代码之前，一定会检查下远程库跟本地库代码的区别！如果本地的代码版本已经落后了，切记要先pull拉取一下远程库的代码，将本地代码更新到最新以后，然后再修改，提交，推送！</span>

## 8.4 pull拉取远程库到本地库

右键点击项目，可以将远程库中的最新代码pull到本地库

![image-20210601153511793](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601153511793.png)

![image-20210601153518019](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601153518019.png)

注意： pull是拉取远端仓库代码到本地，如果远程库代码和本地库代码不一致，会自动合并，如果自动合并失败，还会涉及到手动解决冲突的问题。

## 8.5 clone远程库代码到本地

![image-20210601154507193](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601154507193.png)

或者

![image-20210601154559207](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601154559207.png)

![image-20210601154605242](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601154605242.png)

# 9、国内代码托管中心-码云

## 9.1简介

众所周知，github的服务器在国外，使用Github作为项目托管网站，如果网速不好的话，严重影响使用体验，甚至会出现登录不上的问题。针对这个情况，大家也可以使用国内的项目托管中心-码云。

码云是开源中国推出的基于git的代码托管中心，网址是https://gitee.com/，使用方式跟GitHub一样，而且它还是个中文网站。

## 9.2 码云注册和登录

官网：https://gitee.com/

![image-20210601160057132](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601160057132.png)

输入个人信息，进行注册即可。

![image-20210601160100664](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601160100664.png)

帐号注册成功后，可以进行登录。

![image-20210601160137159](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601160137159.png)

登录以后，就可以看到码云的首页了。

![image-20210601160207967](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601160207967.png)

## 9.3 码云创建远程库

点击首页右上角的加号，选择下面的新建仓库

![image-20210601160250862](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601160250862.png)

填写仓库名称，路径和选择仓库类型

![image-20210601160356727](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601160356727.png)

最后点击创建按钮

![image-20210601160424694](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601160424694.png)

远程仓库创建好了之后，我们就可以看到Http和SSH链接了

![image-20210601160516786](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601160516786.png)

## 9.4 IDEA 集成码云

### 9.4.1 IDEA安装Gitee插件

IDEA默认不带码云插件，我们第一步需要安装Gitee插件

![image-20210601161512105](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601161512105.png)

安装完成Gitee之后，重新打开IDEA。

![image-20210601161539357](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601161539357.png)

IDEA重启以后在 Version Control设置里面看到Gitee，说明码云插件安装成功。

![image-20210601161642495](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601161642495.png)

然后在 Gitee插件里面添加帐号和密码，我们就可以使用IDEA来连接Gitee了。

![image-20210601161854744](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601161854744.png)

### 9.4.2 IDEA连接码云

IDEA连接码云几乎和GitHub一样，首先在IDEA里面创建一个工程，初始化git工程，然后将代码添加到暂存区，提交到本地库，推送代码到远程库，从远程库拉取代码等等。

**将本地代码推送到远程库**

![image-20210601162310654](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601162310654.png)

自定义远程连接

![image-20210601162346078](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601162346078.png)

码云的话使用Https即可，因为服务器在国内，速度还是相当可观的。点击push进行推送

![image-20210601162454700](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601162454700.png)

去码云远程库查看代码。

![image-20210601162524219](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601162524219.png)

## 9.5 码云复制GitHub项目

码云提供了直接复制GitHub项目的功能，方便我们做项目的迁移和下载。

具体操作如下：

创建仓库 -- > 导入已有仓库

![image-20210601163608004](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601163608004.png)

**将GitHub的远程库Https链接复制过来，点击导入按钮即可**

![image-20210601163543439](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601163543439.png)

![image-20210601163718441](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601163718441.png)



如果Github上的项目更新了，在码云可以手动强制同步，进行更新

![image-20210601163737337](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601163737337.png)

# 10、自建代码托管平台-GitLab

## 10.1 GitLab简介

GitLab 是由 GitLabInc.开发，使用 MIT 许可证的基于网络的 Git 仓库管理工具，且具有

wiki 和 issue 跟踪功能。使用 Git 作为代码管理工具，并在此基础上搭建起来的 web 服务。

GitLab 由乌克兰程序员 DmitriyZaporozhets 和 ValerySizov 开发，它使用 Ruby 语言写

成。后来，一些部分用 Go 语言重写。截止 2018 年 5 月，该公司约有 290 名团队成员，以

及 2000 多名开源贡献者。GitLab 被 IBM，Sony，JülichResearchCenter，NASA，Alibaba，

Invincea，O’ReillyMedia，Leibniz-Rechenzentrum(LRZ)，CERN，SpaceX 等组织使用。

## 10.2 GitLab官网

官网地址：https://about.gitlab.com/

安装说明：https://about.gitlab.com/installation/

## 10.3 GitLab安装

### 10.3.1 服务器准备

准备一个系统为 CentOS7 以上版本的服务器，要求内存 4G，磁盘 50G。

关闭防火墙，并且配置好主机名和 IP，保证服务器可以上网。

此教程使用虚拟机：主机名：gitlab-server IP 地址：192.168.6.200

### 10.3.2 安装包准备

Yum 在线安装 gitlab- ce 时，需要下载几百 M 的安装文件，非常耗时，所以最好提前把

所需 RPM 包下载到本地，然后使用离线 rpm 的方式安装。

下载地址：

```
https://packages.gitlab.com/gitlab/gitlab-ce/packages/el/7/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm
```

注：资料里提供了此 rpm 包，直接将此包上传到服务器/opt/module 目录下即可。

### 10.3.3 编写安装脚本

安装 gitlab 步骤比较繁琐，因此我们可以参考官网编写 gitlab 的安装脚本。

```sh
[root@gitlab-server module]# vim gitlab-install.sh
sudo rpm -ivh /opt/module/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm
sudo yum install -y curl policycoreutils-python openssh-server cronie
sudo lokkit -s http -s ssh
sudo yum install -y postfix
sudo service postfix start
sudo chkconfig postfix on
curl https://packages.gitlab.com/install/repositories/gitlab/gitlabce/script.rpm.sh | sudo bash
sudo EXTERNAL_URL="http://gitlab.example.com" yum -y install gitlabce
```

给脚本增加执行权限

```sh
[root@gitlab-server module]# chmod +x gitlab-install.sh
[root@gitlab-server module]# ll
总用量 403104
-rw-r--r--. 1 root root 412774002 4 月 7 15:47 gitlab-ce-13.10.2-
ce.0.el7.x86_64.rpm
-rwxr-xr-x. 1 root root 416 4 月 7 15:49 gitlab-install.sh
```

然后执行该脚本，开始安装 gitlab-ce。注意一定要保证服务器可以上网。

```sh
[root@gitlab-server module]# ./gitlab-install.sh 
警告：/opt/module/gitlab-ce-13.10.2-ce.0.el7.x86_64.rpm: 头 V4 
RSA/SHA1 Signature, 密钥 ID f27eab47: NOKEY
准备中... ################################# 
[100%]
正在升级/安装...
 1:gitlab-ce-13.10.2-ce.0.el7 
################################# [100%]
。 。 。 。 。 。
```

### 10.3.4 初始化GitLab服务

执行以下命令初始化 GitLab 服务，过程大概需要几分钟，耐心等待…

```sh
[root@gitlab-server module]# gitlab-ctl reconfigure
。 。 。 。 。 。
Running handlers:
Running handlers complete
Chef Client finished, 425/608 resources updated in 03 minutes 08 
seconds
gitlab Reconfigured!
```

### 10.3.5 启动GitLab服务

执行以下命令启动 GitLab 服务，如需停止，执行 gitlab-ctl stop

```SH
[root@gitlab-server module]# gitlab-ctl start
ok: run: alertmanager: (pid 6812) 134s
ok: run: gitaly: (pid 6740) 135s
ok: run: gitlab-monitor: (pid 6765) 135s
ok: run: gitlab-workhorse: (pid 6722) 136s
ok: run: logrotate: (pid 5994) 197s
ok: run: nginx: (pid 5930) 203s
ok: run: node-exporter: (pid 6234) 185s
ok: run: postgres-exporter: (pid 6834) 133s
ok: run: postgresql: (pid 5456) 257s
ok: run: prometheus: (pid 6777) 134s
ok: run: redis: (pid 5327) 263s
ok: run: redis-exporter: (pid 6391) 173s
ok: run: sidekiq: (pid 5797) 215s
ok: run: unicorn: (pid 5728) 221s
```

### 10.3.6 使用浏览器访问 GitLab

使用主机名或者 IP 地址即可访问 GitLab 服务。需要提前配一下 windows 的 hosts 文件

![image-20210601172458508](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601172458508.png)

![image-20210601172502094](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601172502094.png)

![image-20210601172505063](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601172505063.png)



首次登陆之前，需要修改下 GitLab 提供的 root 账户的密码，要求 8 位以上，包含大小

写子母和特殊符号。因此我们修改密码为 **Atguigu.123456**

然后使用修改后的密码登录 GitLab。

![image-20210601172534673](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601172534673.png)



GitLab 登录成功。

![image-20210601172550151](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601172550151.png)

### 10.3.7 GitLab 创建远程库

![image-20210601172615307](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601172615307.png)

![image-20210601172618539](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601172618539.png)

![image-20210601172621729](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601172621729.png)

### 10.3.8 IDEA集成GitLab

1）安装 GitLab 插件

![image-20210601172750056](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601172750056.png)

2）设置 GitLab 插件

![image-20210601173011396](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601173011396.png)

![image-20210601173025363](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601173025363.png)

![image-20210601173031644](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601173031644.png)

3）push 本地代码到 GitLab 远程库

![image-20210601173055632](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601173055632.png)

自定义远程连接

![image-20210601173211226](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601173211226.png)

![image-20210601173213974](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601173213974.png)

注意：gitlab 网页上复制过来的连接是：http://gitlab.example.com/root/git-test.git，

需要手动修改为：http://gitlab-server/root/git-test.git

选择 gitlab 远程连接，进行 push。

![image-20210601173232172](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601173232172.png)

首次向连接 gitlab，需要登录帐号和密码，用 root 帐号和我们修改的密码登录即可。

![image-20210601173245588](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601173245588.png)

代码 Push 成功。

![image-20210601173544296](Git%E5%AD%A6%E4%B9%A0.assets/image-20210601173544296.png)

只要 GitLab 的远程库连接定义好以后，对 GitLab 远程库进行 pull 和 clone 的操作和

Github 和码云一致，此处不再赘述