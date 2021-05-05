# 一、安装Maven以及Maven环境搭建

## 1.1 下载安装Maven

官网地址：https://maven.apache.org/download.cgi

下载对应操作系统的Maven

![image-20210430195026723](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210430195026723.png)

## 1.2 IDEA中进行Maven配置

1.在IDEA中打开如下位置：File | Settings | Build, Execution, Deployment | Build Tools | Maven，并进行如下配置

![image-20210430195444875](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210430195444875.png)

**<span style="color:red">2.配置Maven阿里云镜像</span>**

找到下载的maven的conf目录下的settings.xml中`<mirrors></mirrors>`

**加上如下配置**

```xml
     <mirror>
        <id>nexus-aliyun</id>
        <mirrorOf>central</mirrorOf>
        <name>Nexus aliyun</name>
        <url>http://maven.aliyun.com/nexus/content/groups/public</url>
      </mirror>
```

![image-20210430195831816](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210430195831816.png)

# 二、Mybatis引言

## 2.1什么Mybatis

+ 定义：Mybatis是一款优秀的持久层框架，**<span style="color:red">Mybatis是用来完成数据库操作的半ORM框架</span>，用来操作数据库等，解决原始JDBC编程技术中代码冗余，方便访问数据。**
+ Hibernate是全ORM框架。
+ ORM框架：Object  Relationship  Mapping  对象关系映射
+ 半ORM框架：典型代表Mybatis，也就是Mybatis自己在mapper配置文件中书写字段和对象属性映射关系。

## 2.2 Mybatis入门案例

**<span style="color:blue">1.创建maven项目</span>**

![image-20210430202019567](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210430202019567.png)

**<span style="color:blue">2.在项目中引入Mybatis依赖</span>**

```xml
<!--Mybatis-->
		<dependency>
			<groupId>org.mybatis</groupId>
			<artifactId>mybatis</artifactId>
			<version>3.4.6</version>
		</dependency>
```

**<span style="color:blue">3.新建Mybatis主配置文件</span>**

主配置文件：是核心配置文件。作用：用来创建SqlSessionFactory对象

```xml
<configuration>
    <environments default="dev">
        <!--environment用来指定当前操作哪个环境下的数据库
            常见的有 dev ，test ，prod
        -->
        <environment id="dev">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="$root"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
</configuration>
```

**<span style="color:blue">4.获取SqlSessionFactory对象</span>**

```java
        //通过Resource获取mybatis-config.xml文件
        InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
        //获取SqlSessionFactory对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        //获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession();
```

**<span style="color:blue">5.建库建表</span>**

```sql
CREATE TABLE `t_user` (
  `id` int(6) NOT NULL AUTO_INCREMENT,
  `name` varchar(40) DEFAULT NULL,
  `age` int(3) DEFAULT NULL,
  `bir` timestamp NULL DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

**<span style="color:blue">6.建立表对应的实体对象</span>**

```java
public class User {

    private Integer id;
    private String name;
    private Integer age;
    //对应数据库中 timestamp类型的字段
    private Date birth;
	// constructor ,getter/setter 省略...
}
```

**<span style="color:blue">7.新建UserMapper接口</span>**

注意：在mybatis中一个Mapper接口对应一个Mapper配置文件，**<span style="color:red">IDEA中建立多级配置文件目录要使用斜杠/</span>**

```java
public interface UserMapper {

    int save(User user);
}
```

**<span style="color:blue">8.创建UserMapper接口对应的mapper配置文件 UserMapper.xml</span>**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace:命名空间，指定当前mapper配置文件对应哪个Mapper接口-->
<mapper namespace="com.baizhi.mybatis.mapper.UserMapper">
    <!--
    insert：对应插入操作
        id：对应Mapper接口中的方法
        parameterType:参数类型， 包.类
        insert标签内部 写sql语句
        #{对象中的属性名}：进行传参


    -->
    <insert id="save" parameterType="com.baizhi.mybatis.entity.User">
        insert into t_user values(#{id},#{name},#{age},#{birth})
    </insert>
</mapper>
```

**<span style="color:blue">9.将mapper配置文件注册到Mybatis的主配置文件中</span>**

```xml
    <mappers>
        <mapper resource="mapper/UserMapper.xml"/>
    </mappers>
```

**<span style="color:blue">10.测试UserMapper</span>**

```java
@Test
    public void testSave() throws IOException {
        //通过Resource获取mybatis-config.xml文件
        InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
        //获取SqlSessionFactory对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

        //获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession();

        //通过SqlSession获取UserMapper代理接口对象
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);

        //通过userMapper.save()方法进行插入
        User user = new User(1, "tom", 18, new Date());
        //count:代表受影响的行数
        int count = userMapper.save(user);
        System.out.println("受影响的行数 :" + count);
        //增删改操作，都需要提交事务。
        sqlSession.commit();

        //最后关闭SqlSession
        sqlSession.close();
    }
```

**<span style="color:blue">运行结果：</span>**

![image-20210430212558064](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210430212558064.png)

![image-20210430212613110](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210430212613110.png)

# 三、在Mybatis执行过程中 数据库可能存在乱码问题

## 3.1 数据库中的数据为什么会出现乱码

Java中的编码在通过不同操作系统底层传递过程中由于和操作系统的编码不一致，导致出现乱码

## 3.2 解决方案

在进行jdbc-url配置时，添加数据库编码

```xml
<property name="url" value="jdbc:mysql://localhost:3306/mybatis?characterEncoding=utf-8"/>
```

# 四、Mybatis中在插入数据后如何返回数据库自动生成的id

在UserMapper.xml文件中 添加属性**` keyProperty="id" 和useGeneratedKeys="true"`**

+ **keyProperty="id"：表示将数据库自增的id赋给JavaBean中的id属性**
+ **useGeneratedKeys：设置为true**

![image-20210430214029112](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210430214029112.png)

**在进行新增数据之后,获取当前id**

![image-20210430214117553](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210430214117553.png)



# 五、Mybatis中CRUD操作

## 5.1 记录更新操作

**<span style="color:blue">1.UserMapper接口</span>**

```java
public interface UserDAO {
	//更新方法
	int update(User user);
}
```

**<span style="color:blue">2.UserMapper.xml配置文件</span>**

+ **<span style="color:red">动态sql</span>**
  + **`<set></set>`的作用：去掉赋值语句前后多余的逗号"，"**
  + **`<if></if>`<span style="color:RED">标签中 test属性中的name 是对象的属性名,而非数据库字段名</span>**

```xml
 <update id="update">

        update t_user
        <!-- set标签的作用：去掉赋值语句前后的逗号 ","-->
        <set>
        <!-- test中的name属性 是对象的属性名，不是数据库中的 -->
        <if test="name != null and name != ''">
            name=#{name},
        </if>
        <if test="age != null">
            age=#{age},
        </if>
        <if test="birth != null">
            birth=#{birth}
        </if>
        </set>

        where id=#{id}
    </update>
```

**<span style="color:blue">3.更新操作</span>**

```java
@Test
    public void updateTest() throws IOException {

        //获取流对象
        InputStream is = Resources.getResourceAsStream("mybatis-config.xml");

        //获取SqlSessionFactory对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

        //通过SqlSessionFactory获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession();

        try {
            //通过sqlSession获取UserMapper对象
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);

            User user = new User();
            user.setId(5);
            user.setName("小王");
            int count = userMapper.update(user);

            System.out.println("修改操作受影响的行数：" + count);

            //修改操作 ，需要手动提交事务
            sqlSession.commit();
        } catch (Exception e) {
            e.printStackTrace();
            sqlSession.rollback();
        } finally {
            sqlSession.close();
        }
    }
```

## 5.2 删除操作

**<span style="color:blue">1.UserMapper接口</span>**

```java
public interface UserMapper {
    // 删除操作
    int delete(Integer id);
}
```

**<span style="color:blue">2.UserMapper.xml配置文件</span>**

```XML
<delete id="delete" parameterType="Integer">
    delete from t_user where id = #{id}
</delete>
```

**<span style="color:blue">3.删除操作</span>**

```java
 @Test
    public void deleteTest() throws IOException {
        //获取流对象
        InputStream is = Resources.getResourceAsStream("mybatis-config.xml");

        //获取SqlSessionFactory对象
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

        //通过SqlSessionFactory获取SqlSession对象
        SqlSession sqlSession = sqlSessionFactory.openSession();

        try {
            //通过sqlSession获取UserMapper对象
            UserMapper userMapper = sqlSession.getMapper(UserMapper.class);

            int count = userMapper.delete(5);

            System.out.println("删除操作受影响的行数：" + count);

            //删除操作 ，需要手动提交事务
            sqlSession.commit();
        } catch (Exception e) {
            e.printStackTrace();
            sqlSession.rollback();
        } finally {
            sqlSession.close();
        }
    }
```

## 5.3 封装工具类：GetAndCloseSqlSessionUtil

**作用：获取SqlSession实例和关闭SqlSession**

```java
public class GetAndCloseSqlSessionUtil {

    private static SqlSessionFactory sqlSessionFactory;

    static {

        InputStream is = null;
        try {
            is = Resources.getResourceAsStream("mybatis-config.xml");
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if (is != null) {
                try {
                    is.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    /**
     * 获取SqlSession实例对象
     *
     * @return
     */
    public static SqlSession getSqlSession() {
        return sqlSessionFactory.openSession();
    }

    /**
     * 关闭SqlSession
     *
     * @param sqlSession
     */
    public static void closeSqlSession(SqlSession sqlSession) {

        sqlSession.close();
    }
}
```

## 5.4 Mybatis查询操作

###  5.4.1 查询所有

**<span style="color:blue">1.UserMapper接口</span>**

```java
public interface UserMapper {
    //查询所有的User
    List<User> queryAll();
}
```

**<span style="color:blue">2.UserMapper.xml配置文件</span>**

**<span style="color:red">如果查询结果返回的是集合, resultType写的是集合中的泛型类型，而不是List</span>**

```xml

    <!--查询所有的用户-->
    <!--如果返回的是集合, resultType写的是集合中的泛型类型-->
    <select id="queryAll" resultType="com.baizhi.mybatis.entity.User">
        select id,name,age,birth from t_user
    </select>
```

**<span style="color:blue">3.查询全部User操作</span>**

```java
   /**
     * 查询所有的User
     */
    @Test
    public void queryAllTest(){
        //1.通过封装的工具类 获取SqlSession对象
        SqlSession sqlSession = GetAndCloseSqlSessionUtil.getSqlSession();
        //2.通过SqlSession获取UserMapper的实例
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        //3.调用查询全部的方法
        List<User> users = userMapper.queryAll();
        //4.遍历输出
        users.forEach(System.out::println);

        sqlSession.close();
    }
```

**<span style="color:blue">4.运行结果</span>**

![image-20210501140625959](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210501140625959.png)

### 5.4.2根据id查询

**<span style="color:blue">1.UserMapper接口</span>**

```java
public interface UserMapper {
    //通过id查询User
    User queryById(Integer id);
}
```

**<span style="color:blue">2.UserMapper.xml配置文件</span>**

```xml
    <select id="queryById" parameterType="integer" resultType="com.baizhi.mybatis.entity.User">
        select id, name, age , birth
        from t_user
        where id = #{id}
    </select>
```



**<span style="color:blue">3.查询单个User操作</span>**

```java
    @Test
    public void queryOneTest(){
        //1.通过封装的工具类 获取SqlSession对象
        SqlSession sqlSession = GetAndCloseSqlSessionUtil.getSqlSession();
        //2.通过SqlSession获取UserMapper的实例
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        //3.调用查询全部的方法
        User user = userMapper.queryById(4);
        System.out.println(user);

        sqlSession.close();
    }
```

**<span style="color:blue">4.运行结果</span>**

![image-20210501141412263](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210501141412263.png)



### 5.4.3 模糊查询

+ **注意：mysql在进行字符串拼接时只能使用concat()函数**
+ **oracle在进行字符串拼接时既可以使用concat()函数，也可以使用`'%' || #{name} || '%'`**
+ **`<sql></sql>`的作用：sql片段，可以在其他标签中进行引用**
+ **`<include></include>标签`：用来引入`<sql></sql>`标签片段的**

**<span style="color:blue">1.UserMapper接口</span>**

```java
public interface UserMapper {
    //根据用户名进行模糊查询
    List<User> queryLikeByName(String name);
}
```

**<span style="color:blue">2.UserMapper.xml配置文件</span>**

```xml
    <!--模糊查询-->
    <select id="queryLikeByName" parameterType="String" resultType="com.baizhi.mybatis.entity.User">
        <!-- include标签：用来引入SQL片段 -->
        <!-- Mysql中字符串拼接是通过concat()函数
             Oracle字符串拼接可以通过concat()函数 也可以通过 '%' ||#{name}||'%'
         -->
        <include refid="baseSql"/>
        where name like concat('%',#{name},'%')
    </select>
```

**<span style="color:blue">3.根据姓名进行模糊查询</span>**

```java
@Test
    public void queryByLikeNameTest(){
        //1.通过封装的工具类 获取SqlSession对象
        SqlSession sqlSession = GetAndCloseSqlSessionUtil.getSqlSession();
        //2.通过SqlSession获取UserMapper的实例
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        //3.调用查询全部的方法
        List<User> users = userMapper.queryLikeByName("o");
        //4.遍历输出
        users.forEach(System.out::println);
    }
```

**<span style="color:blue">4.运行结果</span>**

![image-20210501143045770](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210501143045770.png)

### 5.4.4分页查询

`select .. from .. limit 起始位置，每页的记录数`

**<span style="color:red">分页查询的重要公式：起始位置 =( 当前页 -1 ) * 每页记录数</span>**

**<span style="color:blue">1.UserMapper接口</span>**

```java
public interface UserMapper {

    /**
     * 分页查询
     * @param start  起始位置
     * @param size   每页的记录数
     * @return
     */
    List<User> queryPage(@Param("start") int start, @Param("size") int size);
   
}
```

**<span style="color:blue">2.UserMapper.xml配置文件</span>**

```xml
    <!--分页查询-->
    <select id="queryPage" resultType="com.baizhi.mybatis.entity.User">
        <include refid="baseSql"/>
        limit #{start},#{size}
    </select>
```

**<span style="color:blue">3.分页查询操作</span>**

```java
    //分页查询
    @Test
    public void queryPage(){
        //1.通过封装的工具类 获取SqlSession对象
        SqlSession sqlSession = GetAndCloseSqlSessionUtil.getSqlSession();
        //2.通过SqlSession获取UserMapper的实例
        UserMapper userMapper = sqlSession.getMapper(UserMapper.class);
        //3.分页
        // start：0 第一页
        // start：2 第二页
        List<User> users = userMapper.queryPage(2, 2);
        //4.遍历输出
        users.forEach(System.out::println);

    }
```

**<span style="color:blue">4.运行结果</span>**

![image-20210501163130847](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210501163130847.png)



### 5.5.5查询总条数

**<span style="color:blue">1.UserMapper接口</span>**

```java
//查询总条数
Long queryTotalCounts();
```



**<span style="color:blue">2.UserMapper.xml配置文件</span>**

```xml
<!--查询总条数-->
<select id="queryTotalCounts" resultType="Long">
  select count(id) from t_user
</select>
```

**<span style="color:blue">3.查询总条数操作</span>**

```java
//查询总条数
@Test
public void testQueryTotalCounts(){
  SqlSession sqlSession = MybatisUtil.getSqlSession();
  UserDAO userDAO = sqlSession.getMapper(UserDAO.class);
  Long counts = userDAO.queryTotalCounts();
  System.out.println("总记录数为: "+counts);
  MybatisUtil.close(sqlSession);
}
```

### 5.5.6 resultType和resultMap的区别

+ resultType：用来处理简单类型化的返回结果
+ resultMap：用来处理复杂的返回结果（一对一，一对多， 多对多）
+ resultMap使用

```XML
<!--查询所有
        resultType: list集合的泛型类型 com.baizhi.entity.User
    -->
    <!--sql标签:用来实现sql语句复用 id:相当于给sql标签中内容定义一个唯一标识-->
    <sql id="userQuery">
        select id, name as uname, age, bir
        from t_user
    </sql>
    <!--结果映射 id:resultMap标签起一个唯一标识 type:指定封装对象的类型  -->
    <resultMap id="userResultMap" type="com.baizhi.entity.User">
        <!--主键封装: id标签-->
        <id column="id" property="id"/>
        <!--普通列 result-->
        <result column="uname" property="name"/>
        <result column="age" property="age"/>
        <result column="bir" property="bir"/>
    </resultMap>
    <select id="queryAll" resultMap="userResultMap">
        <include refid="userQuery"/><!--include:包含那个sql片段 refid:包含片段的id-->
    </select>
```



# 六、Mybatis处理数据库中的关联管理（一对一，一对多，多对多）(重点)

+ 数据库关联关系
  + 一对一关联关系，一对多关联关系，多对多关联关系
+ 一对多关联关系
  + 用户信息  ----------->    身份信息
+ 一对多关联关系
  + 部门信息  ----------->    员工信息
+ 多对多关联关系
  + 老师信息  ----------->    学生信息
  + 学生信息  ----------->    课程信息

## 6.1 一对一关联关系处理

1.建表

```sql
CREATE TABLE t_person(
id INT PRIMARY KEY ,
NAME VARCHAR(40) ,
age INT ,
cardno VARCHAR(18) REFERENCES t_info(cardno) #外键约束
);

CREATE TABLE t_info(
id INT PRIMARY KEY AUTO_INCREMENT,
cardno VARCHAR(18),
address VARCHAR(100) 
);
```

### 6.1.1 Mybatis一对一关联关系保存操作：在保存用户信息同时保存身份信息

**<span style="color:blue">1.身份信息保存</span>**

**Info实体类**

```java
public class Info {
    private Integer id;
    private String cardno;
    private String address;
}
```

**InfoMapper接口**

```java
public interface InfoMapper {
    //保存Info信息
    int save(Info info);
}
```

**InfoMapper.xml配置文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace:命名空间，指定当前mapper配置文件对应哪个Mapper接口-->
<mapper namespace="com.baizhi.mybatis.mapper.InfoMapper">

    <insert id="save" parameterType="com.baizhi.mybatis.entity.Info">

        insert into t_info values(#{id},#{cardno},#{address})

    </insert>

</mapper>
```

**在Mybatis主配置文件中注册InfoMapper.xml**

```xml
<mapper resource="mapper/InfoMapper.xml"/>
```

**保存Info身份信息操作**

```java
@Test
    public void insertTest() {
        SqlSession sqlSession = GetAndCloseSqlSessionUtil.getSqlSession();
        InfoMapper infoMapper = sqlSession.getMapper(InfoMapper.class);
        try {
            Info info = new Info();
            info.setCardno("123456789012345678");
            info.setAddress("上海浦东");
            int count = infoMapper.save(info);
            //增删改操作需要手动提交事务
            sqlSession.commit();
        } catch (Exception e) {
            sqlSession.rollback();
            e.printStackTrace();
        } finally {
            GetAndCloseSqlSessionUtil.closeSqlSession(sqlSession);
        }
    }
```

**<span style="color:blue">2.用户信息保存</span>**

```java
public class Person {
    private Integer id;
    private String name;
    private Integer age;
    private String cadrno;
}
```

**PersonMapper接口**

```java
public interface PersonMapper {
    //保存用户
    int save(Person person);
}
```

**PersonMapper.xml配置文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace:命名空间，指定当前mapper配置文件对应哪个Mapper接口-->
<mapper namespace="com.baizhi.mybatis.mapper.PersonMapper">

    <insert id="save" parameterType="com.baizhi.mybatis.entity.Info">

        insert into t_person values(#{id},#{name},#{age},#{cardno})
    </insert>
</mapper>
```

**将PersonMapper.xml注册到Mybatis主配置文件中**

```xml
<mapper resource="mapper/PersonMapper.xml"/>
```

**保存Person操作**

```java
    @Test
    public void saveTest() {
        SqlSession sqlSession = GetAndCloseSqlSessionUtil.getSqlSession();
        PersonMapper personMapper = sqlSession.getMapper(PersonMapper.class);

        try {
            Person person = new Person();
            person.setAge(28);
            person.setName("陈sir");
            person.setCardno("123456789012345678");
            personMapper.save(person);
            sqlSession.commit();
        } catch (Exception e) {
            sqlSession.rollback();
            e.printStackTrace();
        } finally {
            sqlSession.close();
        }
    }
```

### <span style="color:red">6.1.2 Mybatis处理一对一关联关系中的查询（重点掌握）</span>

根据用户信息并将他的身份信息一并查询出来

**在用户信息实体类中添加身份信息实体类对象**

```java
public class Person {

    private Integer id;
    private String name;
    private Integer age;
    private String cardno;
    //外键信息
    private Info info;
    // get/set省略
}
```

**PersonMapper接口**

```java
public interface PersonMapper {
    //查询所有
    List<Person> queryAll();
}
```

**PersonMapper.xml配置文件**

```xml
 <!--用来处理一对一关联关系的结果封装-->
    <resultMap id="personInfoResultMap" type="com.baizhi.mybatis.entity.Person">
        <id column="id" property="id"/>
        <result column="name" property="name"/>
        <result column="age" property="age"/>
        <result column="cardno" property="cardno"/>
        <!--
        association标签：用来处理一对一关系的
        property属性：在Person实体类中定义的外键属性名
        javaType属性：指定这个外键属性的类型

        -->
        <association property="info" javaType="com.baizhi.mybatis.entity.Info">
            <id column="iid" property="id"/>
            <result column="icardno" property="cardno"/>
            <result column="address" property="address"/>
        </association>
    </resultMap>


 <!--查询所有-->
    <select id="queryAll" resultMap="personInfoResultMap">

        SELECT t_person.id,NAME,age,t_person.cardno ,i.`id` iid,i.`cardno` icardno,i.`address`
        FROM t_person  LEFT JOIN t_info i ON t_person.`cardno` = i.`cardno`
    </select>
```

**一对一关联查询操作**

```java
    @Test
    public void queryAllTest(){

        SqlSession sqlSession = GetAndCloseSqlSessionUtil.getSqlSession();
        PersonMapper personMapper = sqlSession.getMapper(PersonMapper.class);
        List<Person> people = personMapper.queryAll();
        people.forEach(System.out::println);
        sqlSession.close();
    }
```

**运行结果：**

![image-20210503102621589](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210503102621589.png)



## 6.2 一对多关联关系处理

**建表**：部门表和员工表

**<span style="color:red">注意：在进行一对多建表建立关联关系的时候，外键最好在多的一方建立。</span>**

```sql
--部门表
create table t_dept(
	id int(6) primary key auto_increment,
  name varchar(40)
);

--员工表
create table t_emp(
	id int(6) primary key auto_increment,
  name varchar(40),
  age int(3),
  bir timestamp,
  deptid int(6) references t_dept(id)
);
--  注意: 在处理一对多关联关系时外键最好在多的一方
```

### 6.2.1 一对多：根据一的一方查询多的一方

+ **<span style="color:red">查询部门并且同时查询到部门下的所有员工</span>**

**部门实体类、员工实体类**

```java
public class Dept {
    private Integer id;
    private String name;
    //一对多关联属性：Emp
    private List<Emp> emps;
}

public class Emp {

    private Integer id;
    private String name;
    private Integer age;
   
}
```

**DeptMapper接口**

```java
public interface DeptMapper {
    //查询所有部门以及部门下的员工信息
    List<Dept> queryAll();
}
```

**DeptMapper.xml配置文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace:命名空间，指定当前mapper配置文件对应哪个Mapper接口-->
<mapper namespace="com.baizhi.mybatis.mapper.DeptMapper">

    <!--resultMap用来封装一对多查询的结果-->
    <resultMap id="deptEmpResultMap" type="com.baizhi.mybatis.entity.Dept">
        <id column="id" property="id"/>
        <result column="name" property="name"/>
        <!--collection用来封装一对多中多的一方
            property属性：关系属性名
            javaType属性：关系属性类型
            ofType属性：关系属性中泛型的类型

        -->
        <collection property="emps" javaType="java.util.List" ofType="com.baizhi.mybatis.entity.Emp">
            <id column="eid" property="id"/>
            <result column="ename" property="name"/>
            <result column="age" property="age"/>
        </collection>
    </resultMap>

    <select id="queryAll" resultMap="deptEmpResultMap">

        SELECT
              d.id,
              d.name,
              e.`id` eid,
              e.`name` ename,
              e.`age`
        FROM
              t_dept d
        LEFT JOIN t_emp e
        ON d.`id` = e.`deptid`

    </select>
</mapper>
```

**将DeptMapper.xml配置文件注册到Mybatis的主配置文件中**

```xml
    <mappers>
        <mapper resource="mapper/DeptMapper.xml"/>
    </mappers>
```

**通过一的一方查询多的一方(查询部门以及部门下的的所有员工信息)操作**

```java
    @Test
    public void testQueryAll(){
        SqlSession sqlSession = GetAndCloseSqlSessionUtil.getSqlSession();
        DeptMapper deptMapper = sqlSession.getMapper(DeptMapper.class);
        List<Dept> depts = deptMapper.queryAll();
        depts.forEach(System.out::println);
        sqlSession.close();
    }
```

### 6.2.2一对多：根据多的一方查询

+ **<span style="color:red">查询员工并查询每个员工的部门</span>**

**部门实体类、员工实体类**

```java
public class Emp {

    private Integer id;
    private String name;
    private Integer age;
    //每个员工的部门信息 
    private Dept dept;
}

public class Dept {
    private Integer id;
    private String name;
}
```

**EmpMapper接口**

```java
public interface EmpMapper {
    //查询每个员工的部门信息
    List<Emp> queryAll();
}
```

**EmpMapper.xml配置文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace:命名空间，指定当前mapper配置文件对应哪个Mapper接口-->
<mapper namespace="com.baizhi.mybatis.mapper.EmpMapper">

    <resultMap id="EmpDeptResultMap" type="com.baizhi.mybatis.entity.Emp">
        <id column="id" property="id"/>
        <result column="name" property="name"/>
        <result column="age" property="age"/>
        <!--association:封装一对一关联关系-->
        <association property="dept" javaType="com.baizhi.mybatis.entity.Dept">
            <id column="did" property="id"/>
            <result column="dname" property="name"/>
        </association>
    </resultMap>

    <select id="queryAll" resultMap="EmpDeptResultMap">
        select e.id ,e.name,e.age ,d.id did,d.name dname
        from t_emp e
        left join t_dept d
        on e.deptId = d.id
    </select>
</mapper>
```

**将EmpMapper.xml配置文件注册到Mybatis主配置文件中**

```xml
    <mappers>
        <mapper resource="mapper/EmpMapper.xml"/>
    </mappers>
```

**查询操作**

```java

    @Test
    public void testQueryAll(){
        SqlSession sqlSession = GetAndCloseSqlSessionUtil.getSqlSession();
        EmpMapper empMapper = sqlSession.getMapper(EmpMapper.class);

        List<Emp> emps = empMapper.queryAll();

        emps.forEach(System.out::println);
    }
```

## 6.3 多对多关联关系

以学生和课程为例

**创建学生表、课程表**

```sql
CREATE TABLE t_student (
id INT(10) PRIMARY KEY AUTO_INCREMENT,
NAME VARCHAR(20)
);


CREATE TABLE t_course(
id INT(10) PRIMARY KEY AUTO_INCREMENT,
NAME VARCHAR(5) 
);


CREATE TABLE t_student_course(
id INT PRIMARY KEY AUTO_INCREMENT,
sid INT(10) REFERENCES t_student(id),
cid INT(10) REFERENCES t_course(id)
);
```

### 6.3.1 查询学生并查询学生选择的课程

**创建学生实体类 、课程实体类**

```java
public class Student {

    private Integer id;
    private String name;

    private List<Course> courses;
}

public class Course {

    private Integer id;
    private String name;

}
```

**新建StudentMapper接口**

```java
public interface StudentMapper {

    //查询学生信息以及学生所选的课程信息
    Student queryById(Integer id);
}
```

**StudentMapper.xml配置文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--namespace:命名空间，指定当前mapper配置文件对应哪个Mapper接口-->
<mapper namespace="com.baizhi.mybatis.mapper.StudentMapper">

    <resultMap id="studentCourseResultMap" type="com.baizhi.mybatis.entity.Student">
        <id column="id" property="id"/>
        <result column="name" property="name"/>
        <collection property="courses" javaType="java.util.List" ofType="com.baizhi.mybatis.entity.Course">
            <id column="cid" property="id"/>
            <result column="cname" property="name"/>
        </collection>
    </resultMap>


    <select id="queryById" parameterType="int" resultMap="studentCourseResultMap">
            SELECT
              s.id,
              s.name,
              c.`id` cid,
              c.`name` cname
            FROM
              t_student s
              LEFT JOIN t_student_course sc
                ON s.`id` = sc.`sid`
              LEFT JOIN t_course c
                ON sc.`cid` = c.`id`
            where s.id=#{id}
    </select>
</mapper>
```

**将StudentMapper.xml配置文件注册到Mybatis的主配置文件中**

```xml
    <mappers>
        <mapper resource="mapper/StudentMapper.xml"/>
    </mappers>
```

**查询学生以及学生选择的课程操作**

```java
    @Test
    public void testQueryStuAndCourse(){
        SqlSession sqlSession = GetAndCloseSqlSessionUtil.getSqlSession();
        StudentMapper studentMapper = sqlSession.getMapper(StudentMapper.class);
        Student student = studentMapper.queryById(1);

        System.out.println(student);

        GetAndCloseSqlSessionUtil.closeSqlSession(sqlSession);
    }
```

### 6.3.2 查询课程信息以及选择该课程的学生情况

这个跟6.3.1的做法很类似，具体做法参照6.3.2