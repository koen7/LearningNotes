# 1、权限管理

## 1.什么是权限管理

基本上涉及到用户参与的系统都要进行权限管理，权限管理属于系统安全的范畴，**权限管理实现对用户访问系统的控制**，按照安全规则或者安全策略控制用户可以访问且只能访问自己被授权的资源。

权限管理包括**身份认证**和**授权**两个部分，简称**认证授权**。对于需要访问控制的资源用户首先经过身份认证，认证通过后用户具有该资源的访问权限方可访问。

## 1.2 什么是身份认证

**身份认证**，就是判断一个用户是否为合法用户的处理过程。最常用的简单身份认证方式是系统通过核对用户输入的用户名和口令，判断其是否与系统中存储的用户名和口令一致，来判断用户身份是否正确。对于采用指纹等系统，则出示指纹，对于硬件Key等刷卡系统，则需要刷卡。

## 1.3 什么是授权

授权，即控制用户可以访问哪些系统资源。主体进行身份认证后需要分配权限方可访问系统的资源，对于某些资源没有权限是无法访问的。

# 2、什么是Shiro

```
Apache Shiro™ is a powerful and easy-to-use Java security framework that performs authentication, authorization, cryptography, and session management. With Shiro’s easy-to-understand API, you can quickly and easily secure any application – from the smallest mobile applications to the largest web and enterprise applications.  

Shiro 是一个功能强大且易于使用的Java安全框架，它执行身份验证、授权、加密和会话管理。使用Shiro易于理解的API，您可以快速轻松地保护任何应用程序—从最小的移动应用程序到最大的web和企业应用程序。
```

Shiro是apache旗下的一个开源框架，它将软件系统的安全认证相关功能抽取出来，实现用户身份认证、权限授权、加密、会话管理等功能，组成了一个通用的安全认证框架。

# 3.shiro的核心架构

![img](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/ShiroArchitecture.png)



## 3.1 Subject

**<span style="color:red">Subject即主体</span>**，外部应用与subject进行交互，subject记录了当前操作用户，将用户的概念理解为当前操作的主体，可能是一个通过浏览器请求的用户，也可能是一个运行的程序。**<span style="color:red">Subject在shiro中是一个接口</span>**，接口中定义了很多认证授权相关的方法，外部程序通过Subject进行认证授权，而subect是通过SecurityManager安全管理器进行认证授权

## 3.2 SecurityManager

**<span style="color:red">SecurityManager即安全管理器</span>**，它是shiro的核心。**通过`SecurityManager`可以完成subject的认证、授权**等，实质上SecurityManager是通过`Authenticator`进行认证，通过`Authorizer`进行授权，通过`SessionManager`进行会话管理。

`SecurityManager`是一个接口，继承了`Authenticator、Authorizer、SessionManager`这三个接口。

## 3.3 Authenticator

**<span style="color:red">Authenticator即认证器</span>**，对用户的身份进行认证，**Authenticator是一个接口，shiro提供`ModularRealmAuthenticator`实现类，通过该实现类可以满足大多数需求、也可以自定义认证器。**

## 3.4 Authorizer

**<span style="color:red">Authorizer即授权器</span>**，用户通过认证器认证通过后，在访问功能时需要通过授权器判断用户是否有此功能的操作权限。

## 3.5 Realm

**<span style="color:red">Realm即领域</span>**，相当于datasource数据源，`securityManager`进行安全认证需要通过Realm获取用户权限数据，比如：如果用户身份数据在数据，那么realm需要从数据库获取用户身份信息。

​	**注意：**不要把realm理解成只是从数据源取数据，在realm中还有认证授权校验的相关的代码。

## 3.6 SessionManager

**<span style="color:red">SessionManager即会话管理</span>**，shiro框架定义了一套会话管理，它不依赖于web容器的session，所以shiro可以使用在非web应用上，也可以将分布式应用的会话集中在一点管理，此特性可使它实现单点登录。

## 3.7 SessionDAO

**<span style="color:red">SessionDAO即会话dao，是对session会话操作的一套接口，</span>**比如要将session存储到数据库，可以通过jdbc将会话存储到数据库。

## 3.8 CacheManager

**<span style="color:red">CacheManager即缓存管理，将用户权限数据存储在缓存中，</span>**这样可以提高性能。

## 3.9 Cryptography

**<span style="color:red">Cryptography即密码管理</span>**，shiro提供了一套加密/解密的组件，方便开发。比如提供常用的散列，加/解密等功能。

# 4、shiro中的认证代码实现

## 4.1 认证

身份认证，就是判断一个用户是否为合法用户的处理过程。最常用的简单身份认证是系统通过核对用户输入的用户名和口令，看其是否与系统中存储的该用户的用户名与口令一致，来判断用户身份是否正确。

## 4.2 shiro中<span style="color:red">认证</span>的关键对象

+ **subjcet：主体**
  + 访问系统的用户，主体可以是用户、程序等，进行认证的都称为主体
+ **principal：身份信息**
  + 是主体(subject)进行身份认证的标识，**<span style="color:red">标识必须具有唯一性</span>**，如用户名、手机号、邮箱地址等，一个主体可以有多个身份，但是必须有一个主身份（Primary Principal）
+ **credential：凭证信息**
  + 是只有主体自己知道的安全信息，如密码，证书等。

## 4.3 认证的流程

![image-20210621115517363](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210621115517363.png)

## 4.4 认证—代码实现

**1.创建项目并引入依赖**

```xml
  <!--引入shiro核心包-->
    <dependency>
      <groupId>org.apache.shiro</groupId>
      <artifactId>shiro-core</artifactId>
      <version>1.5.3</version>
    </dependency>
```

**2.创建shiro配置文件shiro.ini并加入用户信息**

```ini
[users]
leo=123
zhangsan=123456
lisi=123
```

**3.用户身份认证-代码**

![image-20210621123559248](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210621123559248.png)

```java
public class TestAuthenticator {
    public static void main(String[] args) {
        //1.创建安全管理器
        DefaultSecurityManager securityManager = new DefaultSecurityManager();

        //2.创建realms对象
        IniRealm iniRealm = new IniRealm("classpath:shiro.ini");

        //2.给安全管理器设置realms
        securityManager.setRealm(iniRealm);

        //3.通过SecurityUtils 安全工具类设置默认的安全管理器
        SecurityUtils.setSecurityManager(securityManager);

        //4.通过SecurityUtils 安全工具类获取当前的主体(用户)
        Subject subject = SecurityUtils.getSubject();

        //5.创建认证时需要的UsernamePasswordToken token对象
        UsernamePasswordToken token = new UsernamePasswordToken("leo", "123");

        //6.通过主体对象进行认证操作
        try {
            System.out.println("认证状态:" + subject.isAuthenticated());
            subject.login(token);
            System.out.println("认证状态:" + subject.isAuthenticated());
        }catch (UnknownAccountException e1){
            e1.printStackTrace();
            System.out.println("用户名不存在");
        }catch (IncorrectCredentialsException e2){
            e2.printStackTrace();
            System.out.println("用户名密码不正确~");
        }
    }
}
```

**<span style="color:red">需要注意</span>**：进行`subject.login(token);`登录时，如果认证失败，会抛出异常的，并且`login()`方法没有返回值

+ `UnknownAccountException`：用户名不正确抛出的异常
+ `IncorrectCredentialsException`：密码不正确抛出的异常
+ `ExcessiveAttemptsException`：登录失败次数过多
+ `ExpiredCredentialsException`：凭证过期

## 4.5 自定义Realm

上边的程序使用的是Shiro自带的IniRealm，IniRealm从ini配置文件中读取用户的信息，大部分情况下需要从系统的数据库中读取用户信息，所以需要自定义Realm。

**1.shiro提供的Realm**

![image-20200521212728541](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200521212728541.png)
**2.根据认证源码得知：认证使用的是`SimpleAccountRealm`**

![image-20200521213451998](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200521213451998.png)

`SimpleAccountRealm`的部分源码中有两个方法：**一个是认证，一个是授权**

```java
public class SimpleAccountRealm extends AuthorizingRealm {
		//.......省略
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        UsernamePasswordToken upToken = (UsernamePasswordToken) token;
        SimpleAccount account = getUser(upToken.getUsername());

        if (account != null) {

            if (account.isLocked()) {
                throw new LockedAccountException("Account [" + account + "] is locked.");
            }
            if (account.isCredentialsExpired()) {
                String msg = "The credentials for account [" + account + "] are expired";
                throw new ExpiredCredentialsException(msg);
            }

        }

        return account;
    }

    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        String username = getUsername(principals);
        USERS_LOCK.readLock().lock();
        try {
            return this.users.get(username);
        } finally {
            USERS_LOCK.readLock().unlock();
        }
    }
}
```

**<span style="color:red">3.自定义realm</span>**

```java
/**
 * 自定义realm
 */
public class CustomerRealm extends AuthorizingRealm {
    /**
     * 自定义授权方法
     * @param principalCollection
     * @return
     */
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    /**
     * 自定义认证方法
     * @param authenticationToken
     * @return
     * @throws AuthenticationException
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        //1.根据token获取token中的用户
        String principal = (String) authenticationToken.getPrincipal();
        // 模拟从数据库查询出来数据
        if ("leo".equals(principal)){
            return new SimpleAuthenticationInfo("leo","123",this.getName());
        }
        return null;
    }
}
```

**<span style="color:red">4.使用自定义realm进行身份认证</span>**

```java
public class TestCustomerRealmForAuthenticator {

    public static void main(String[] args) {
        //1.创建安全管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        //2.给安全管理器设置自定义realm
        defaultSecurityManager.setRealm(new CustomerRealm());
        //3.通过给安全工具类设置安全管理器
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        //4.通过安全工具类SecurityUtils获取主体
        Subject subject = SecurityUtils.getSubject();
        //5.创建usernamepasswordToken对象
        UsernamePasswordToken token = new UsernamePasswordToken("leo", "123");
        //6.通过主体对象实现认证login
        try {
            subject.login(token);
        } catch (AuthenticationException e) {
            e.printStackTrace();
        }
    }
}
```

## 4.6 使用MD5和Salt

实际应用是将烟和山裂后的值存在数据库中，realm从数据库取出盐和加密后的值由shiro完成密码校验

**<span style="color:red">1.自定义Shiro自带的md5的realm</span>**

```java
public class CustomerMd5Realm extends AuthorizingRealm {

    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    // 认证功能
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        String principal = (String) authenticationToken.getPrincipal();
        if ("leo".equals(principal)){
            //第一个参数：用户名
            //第二个参数：明文使用md5加密后的结果。不带salt盐 和 散列次数
            //第三个参数：使用的盐
            //第四个参数：获取realm的名字
            return new SimpleAuthenticationInfo(principal,"202cb962ac59075b964b07152d234b70", this.getName());
        }
        return null;
    }
}
```

```java
public class TestCustomerMd5RealmForAuthenticator {
    public static void main(String[] args) {
        //1.创建安全管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        //2.创建自定义realm对象
        CustomerMd5Realm customerMd5Realm = new CustomerMd5Realm();
        //2.1创建MD5算法的密码校验器
        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher("md5");
        //2.2给自定义realm设置MD5算法的密码校验器
        customerMd5Realm.setCredentialsMatcher(credentialsMatcher);
        //3.给安全管理器设置自定义realm对象
        defaultSecurityManager.setRealm(customerMd5Realm);
        //4.给安全工具类SecurityUtils设置安全管理器
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        //5.通过安全工具类SecurityUtils获取主体
        Subject subject = SecurityUtils.getSubject();
        //6.创建UsernamepasswordToken对象
        UsernamePasswordToken token = new UsernamePasswordToken("leo", "123");
        try {
            System.out.println("认证状态:" + subject.isAuthenticated());
            subject.login(token);
            System.out.println("认证状态:" + subject.isAuthenticated());
        } catch (UnknownAccountException e) {
            e.printStackTrace();
            System.out.println("用户名不存在");
        } catch (IncorrectCredentialsException e) {
            e.printStackTrace();
            System.out.println("密码错误！");
        }
    }
}
```

**<span style="color:red">2.自定义Shiro中的md5+盐进行认证</span>**

**只需要修改`CustomerMd5Realm`类的`AuthenticationInfo()`**

```java
public class CustomerMd5Realm extends AuthorizingRealm {

    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    // 认证功能
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        String principal = (String) authenticationToken.getPrincipal();
        if ("leo".equals(principal)){
            String salt = "*ok1";
            //第一个参数：用户名
            //第二个参数：数据库中 明文 + salt 采用MD5加密后的结果
            //第三个参数：使用的盐
            //第四个参数：获取realm的名字
            return new SimpleAuthenticationInfo(principal,"46ba4f8431e8cdff13cbd6b2f0e21c10",ByteSource.Util.bytes(salt), this.getName());
        }
        return null;
    }
}
```

**<span style="color:red">3.自定义Shiro中的md5 + 盐值 + 散列次数 进行身份验证</span>**

```java
public class TestCustomerMd5RealmForAuthenticator {
    public static void main(String[] args) {
        //1.创建安全管理器
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        //2.创建自定义realm对象
        CustomerMd5Realm customerMd5Realm = new CustomerMd5Realm();
        //2.1创建MD5算法的密码校验器
        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher("md5");
        //2.2 ===新增的内容: 设置散列的次数 1024次 ======
        credentialsMatcher.setHashIterations(1024);
        //2.3给自定义realm设置MD5算法的密码校验器
        customerMd5Realm.setCredentialsMatcher(credentialsMatcher);
        //3.给安全管理器设置自定义realm对象
        defaultSecurityManager.setRealm(customerMd5Realm);
        //4.给安全工具类SecurityUtils设置安全管理器
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        //5.通过安全工具类SecurityUtils获取主体
        Subject subject = SecurityUtils.getSubject();
        //6.创建UsernamepasswordToken对象
        UsernamePasswordToken token = new UsernamePasswordToken("leo", "123");
        try {
            System.out.println("认证状态:" + subject.isAuthenticated());
            subject.login(token);
            System.out.println("认证状态:" + subject.isAuthenticated());
        } catch (UnknownAccountException e) {
            e.printStackTrace();
            System.out.println("用户名不存在");
        } catch (IncorrectCredentialsException e) {
            e.printStackTrace();
            System.out.println("密码错误！");
        }
    }
}
```

```java
public class CustomerMd5Realm extends AuthorizingRealm {

    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    // 认证功能
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        String principal = (String) authenticationToken.getPrincipal();
        if ("leo".equals(principal)){
            String salt = "*ok1";
            //第一个参数：用户名
            //第二个参数：数据库中 salt + md5 + 散列次数 采用MD5加密后的结果
            //第三个参数：使用的盐
            //第四个参数：获取realm的名字
            return new SimpleAuthenticationInfo(principal,"fa104b5d61dd782c659cca440c64b422",ByteSource.Util.bytes(salt), this.getName());
        }
        return null;
    }
}
```

# 5、Shiro中的授权

## 5.1 授权

**<span style="color:red">授权即访问控制，控制谁可以访问哪些资源。</span>**==主体进行身份认证后==需要分配权限方可访问系统的资源，对于某些资源没有权限是无法访问的。

## 5.2 关键对象

**授权可简单理解为who对what(which)进行How操作**

`Who，即主体(Subject)`：主体需要访问系统中的资源

`What，即资源(Resource)`：如系统菜单、页面、按钮、类方法、系统商品信息等。**<span style="color:red">资源包括资源类型和资源实例</span>**，比如**商品信息为资源类型**，类型为t01的商品为资源实例，**编号为001的商品信息也属于资源实例。**

`How，权限/许可(Permission)`：规定了主体对资源的操作许可，**权限离开资源没有意义**，如用户查询权限、用户添加权限、某个类方法的调用权限、编号为001用户的修改权限等，通过权限可知主体对哪些资源都有哪些操作许可。

## 5.3 授权流程

![image-20200521230705964](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200521230705964.png)

## 5.4 授权方式(重点)

+ **<span style="color:red">基于角色</span>的访问控制**
  + RBAC基于角色的访问控制（Role-Based Aceess Control）是以角色为中心进行访问控制

```java
if(subject.hasRole("admin")){
	//操作什么资源
}
```

+ **<span style="color:red">基于资源</span>的访问控制**
  + RBAC基于资源的访问控制（Resource-Based Access Control）

```java
if(subject.isPermission("user:update:01")){//资源实例
    //是否具有对01用户进行修改的权限
}

```

## 5.5 权限字符串(重点)

**<span style="color:red">权限字符串的规则是：资源标识符：操作：资源实例标识符</span>**，意思是对哪个资源的哪个实例具有什么操作，“：”是资源/操作/实例的分隔符，权限字符串也可以使用*通配符

例如：

+ 用户创建权限：`user：create或者user:create:*`
+ 用户修改实例001的权限：`user:update:001`
+ 用户实例001的所有权限：`user:*:001`

## 5.6 Shiro中授权的实现方式

+ **编程式**

```java
Subject subject = SecurityUtils.getSubject();
if(subject.hasRole(“admin”)) {
	//有权限
} else {
	//无权限
}
```

+ **注解方式**

```java
@RequiresRoles("admin")
public void hello() {
	//有权限
}
```

+ **标签方式**

```jsp
JSP/GSP 标签：在JSP/GSP 页面通过相应标签完成：
<shiro:hasRole name="admin">
    <!-- 有权限 -->
</shiro:hasRole>
注意：Thymeleaf 中使用shiro需要额外集成！
```

## 5.7 代码实现-授权

**1.realm的实现**

```java
public class CustomerRealm extends AuthorizingRealm {
    //认证方法
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        String primaryPrincipal = (String) principals.getPrimaryPrincipal();
        System.out.println("primaryPrincipal = " + primaryPrincipal);

        SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();

        simpleAuthorizationInfo.addRole("admin");

        simpleAuthorizationInfo.addStringPermission("user:update:*");
        simpleAuthorizationInfo.addStringPermission("product:*:*");


        return simpleAuthorizationInfo;
    }

    //授权方法
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
        String principal = (String) token.getPrincipal();
        if("xiaochen".equals(principal)){
            String password = "3c88b338102c1a343bcb88cd3878758e";
            String salt = "Q4F%";
            return new SimpleAuthenticationInfo(principal,password, 
                                                ByteSource.Util.bytes(salt),this.getName());
        }
        return null;
    }
}
```

+ `addRoles(Collection<String> roles)`：给当前认证过的用户授予多个角色
+ `addRoles(String role)`：给当前认证过的用户授予一个角色
+ `addStringPermission(String permission)`：给某个菜单分配对应的权限
+ `addStringPermissions(Collection<String> permissions)`：给多个菜单分配多个权限

**2.授权**

```java
public class TestAuthenticatorCusttomerRealm {
    public static void main(String[] args) {
        //创建securityManager
        DefaultSecurityManager defaultSecurityManager = new DefaultSecurityManager();
        //IniRealm realm = new IniRealm("classpath:shiro.ini");
        //设置为自定义realm获取认证数据
        CustomerRealm customerRealm = new CustomerRealm();
        //设置md5加密
        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
        credentialsMatcher.setHashAlgorithmName("MD5");
        credentialsMatcher.setHashIterations(1024);//设置散列次数
        customerRealm.setCredentialsMatcher(credentialsMatcher);
        defaultSecurityManager.setRealm(customerRealm);
        //将安装工具类中设置默认安全管理器
        SecurityUtils.setSecurityManager(defaultSecurityManager);
        //获取主体对象
        Subject subject = SecurityUtils.getSubject();
        //创建token令牌
        UsernamePasswordToken token = new UsernamePasswordToken("xiaochen", "123");
        try {
            subject.login(token);//用户登录
            System.out.println("登录成功~~");
        } catch (UnknownAccountException e) {
            e.printStackTrace();
            System.out.println("用户名错误!!");
        }catch (IncorrectCredentialsException e){
            e.printStackTrace();
            System.out.println("密码错误!!!");
        }
        //认证通过
        if(subject.isAuthenticated()){
            //基于角色权限管理
            boolean admin = subject.hasRole("admin");
            System.out.println(admin);

            boolean permitted = subject.isPermitted("product:create:001");
            System.out.println(permitted);
        }

    }
}
```

+ `isPermitted(Permission permission)`：判断当前用户是否具有指定的permisson权限
+ `boolean[] isPermitted(String... permissions)`：判断当前用户是否具有多个指定的permission权限
+ `boolean[] hasRoles(List<String> roleIdentifiers)`：判断当前用户是否具有对应的角色
+ `boolean hasAllRoles(Collection<String> roleIdentifiers)`：判断当前用户是否同时具有多个角色，同时具有时才会返回true，否则返回false
+ `boolean hasRole(String roleIdentifier);`：判断当前用户是否具有单个角色

# 6、SpinrgBoot整合Shiro实战

## 6.1 整合思路

![image-20200525185630463](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200525185630463.png)

## 6.2 创建SpringBoot项目

![image-20210622181534307](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210622181534307.png)

## 6.3 引入shiro依赖

```xml
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-spring-boot-starter</artifactId>
  <version>1.5.3</version>
</dependency>
```

## 6.4 引入Shiro环境

### 1.创建Shiro配置类ShiroConfig

+ 在ShiroConfig配置类中主要配置三个方法
  + **`public ShiroFilterFactoryBean getShiroFilterFactoryBean`：该方法中主要指定哪些请求是公共资源，哪些请求是受限资源，并且需要设置`SecurityManager`安全管理器。**
  + **`public DefaultWebSecurityManager getDefaultWebSecurityManager`：主要将DefaultWebSecurityManager注入到容器中，并且指定自定义Realm**
  + **`public Realm getReaml()`：主要配置自定义Realm源。**

```java
/**
 * ShiroFilter配置类
 */
@Configuration
public class ShiroFilter {

    /**
     * 注入ShiroFilterFactoryBean,并且指定哪些请求是公共资源，哪些请求是受限制的资源
     * @param defaultWebSecurityManager
     * @return
     */
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(DefaultWebSecurityManager defaultWebSecurityManager ){
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();
        shiroFilterFactoryBean.setSecurityManager(defaultWebSecurityManager);
        //配置公共资源
        //配置受限资源
        Map<String, String> map = new HashMap<>();
        map.put("/index.jsp","authc");
        shiroFilterFactoryBean.setFilterChainDefinitionMap(map);
        return shiroFilterFactoryBean;
    }

    // 注入SecurityManager
    @Bean
    public DefaultWebSecurityManager getDefaultWebSecurityManager(Realm realm){
        DefaultWebSecurityManager defaultWebSecurityManager = new DefaultWebSecurityManager();
        defaultWebSecurityManager.setRealm(realm);
        return defaultWebSecurityManager;
    }

    // 注入自定义Realm
    @Bean(name = "realm")
    public Realm getRealm(){
        CustomerRealm customerRealm = new CustomerRealm();
        return customerRealm;
    }
}
```

### 2.创建自定义Realm

![image-20210622191932190](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210622191932190.png)

```java
/**
 * 自定义Realm：进行认证和授权相关操作
 */
public class CustomerRealm extends AuthorizingRealm {
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        return null;
    }
}
```

### 3.启动项目 访问：http://localhost:8080/index.jsp

![image-20210622192212538](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210622192212538.png)

+ **<span style="color:red">注意：默认在配置好shiro环境后，并没有对项目中的任何资源进行权限控制，所以项目中的所有资源都可以进行访问。</span>**

### 4. 加入权限控制

在`public ShiroFilterFactoryBean getShiroFilterFactoryBean`方法中加入授权的规则

```java
    /**
     * 注入ShiroFilterFactoryBean,并且指定哪些请求是公共资源，哪些请求是受限制的资源
     * @param defaultWebSecurityManager
     * @return
     */
    @Bean
    public ShiroFilterFactoryBean getShiroFilterFactoryBean(DefaultWebSecurityManager defaultWebSecurityManager ){
        ShiroFilterFactoryBean shiroFilterFactoryBean = new ShiroFilterFactoryBean();
        shiroFilterFactoryBean.setSecurityManager(defaultWebSecurityManager);
//        配置公共资源
//        配置受限资源
//       ====加入授权规则====
        Map<String, String> map = new HashMap<>();
        map.put("/index.jsp","authc");
        shiroFilterFactoryBean.setFilterChainDefinitionMap(map);
        return shiroFilterFactoryBean;
    }
```

![image-20210622192535352](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210622192535352.png)

/index.jsp：代表拦截项目中的`/index.jsp`请求，**authc**代表shiro中的一个filter的别名，详细内容看文档的shirofilter列表

### 5.重启项目重新访问http://localhost:8080/index.jsp

访问http://localhost:8080/index.jsp该地址会自动跳转到/login.jsp，这是因为shiro中配置了默认的访问失败的地址。

![image-20210622192836414](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210622192836414.png)

## 6.5 常见过滤器

shiro提供了多个默认的过滤器，我们可以使用这些过滤器来配置控制指定的url的权限

| 配置缩写          | 对应的过滤器                   | 功能                                                         |
| ----------------- | ------------------------------ | ------------------------------------------------------------ |
| anon              | AnonymousFilter                | 指定url可以匿名访问                                          |
| authc             | FormAuthenticationFilter       | 指定url需要form表单登录，默认会从请求中获取`username、password、rememberme`等参数并尝试登录，如果登录不了就跳转到loginUrl配置的路径。我们也可以使用这个过滤器做默认的登录逻辑，但是一般都是我们自己在控制器写登录逻辑的，自己写的话出错返回的信息都可以定制 |
| authcBasic        | BasicHttpAuthenticationFilter  | 指定url需要basic登录                                         |
| logout            | LogoutFilter                   | 登出过滤器，配置指定url就可以实现退出功能，非常方便          |
| noSessionCreation | NoSessionCreationFilter        | 禁止创建会话                                                 |
| perms             | PermissionsAuthorizationFilter | 需要指定权限才能访问                                         |
| port              | PortFilter                     | 需要指定端口号才能访问                                       |
| rest              | HttpMethodPermissionFilter     | 将http请求方法转化为相应的动词来构造一个权限字符串，这个感觉意义不大，有兴趣自己看源码的注释 |
| roles             | RolesAuthorizationFilter       | 需要指定角色才能访问                                         |
| ssl               | SslFilter                      | 需要https请求才能访问                                        |
| user              | UserFilter                     | 需要已登录或“记住我”的用户才能访问                           |

## 6.6 认证实现

### 1.在login.jsp页面添加登录表单

```html
<body>
<h1>用户登录</h1>
<form action="${pageContext.request.contextPath}/user/login">
    用户名:<input type="text" name="username">
    密码:<input type="text" name="password">
    <input type="submit" value="登录">
</form>
</body>
```

### 2.创建Controller，提供登录和退出功能

**登录功能：**

```java
@Controller
@RequestMapping("/user")
public class UserController {


    /**
     * 登录操作
     * @param username 前端传来的username
     * @param password 前端传来的password
     * @return
     */
    @RequestMapping("/login")
    public String login(String username,String password){
        //1.获取主体对象
        Subject subject = SecurityUtils.getSubject();
        try {
            //登录
            subject.login(new UsernamePasswordToken(username,password));
            //登录成功 重定向到主页
            return "redircet:/index.jsp";
        }catch (UnknownAccountException e){
            e.printStackTrace();
            System.out.println("用户名错误！");
        }catch (IncorrectCredentialsException e){
            e.printStackTrace();
            System.out.println("密码错误！");
        }
        //登录失败,跳转到登录页面
        return "redirect:/login.jsp";
    }
}
```

**退出功能方法：**

```java
    @RequestMapping("/logout")
    public String logout(){
        //获取主体对象
        Subject subject = SecurityUtils.getSubject();
        //调用退出方法
        subject.logout();
        return "redirect:/login.jsp";
    }
```

### 3.在自定义Realm的`doGetAuthenticationInfo`方法中进行身份认证

```java
/**
 * 自定义Realm：进行认证和授权相关操作
 */
public class CustomerRealm extends AuthorizingRealm {
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    /**
     * 身份认证功能
     * @param authenticationToken 带有身份信息的token
     * @return
     * @throws AuthenticationException
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {
        //1.从token中获取身份信息（用户）
        String principal = (String) authenticationToken.getPrincipal();
        //2.与数据库中的身份信息进行比较，这里写死数据。
        if ("leo".equals(principal)){
            //用户名一致
            //3.返回AuthenticationInfo接口的实现类
            new SimpleAuthenticationInfo(principal,"123",this.getName());
        }
        //数据库中没有该用户，认证失败
        return null;
    }
}
```

### 4. 启动项目，访问http://localhost:8080/index.jsp进行测试

![image-20210622233230893](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210622233230893.png)

### 5.登录成功，进入到主页

![image-20210622234425566](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210622234425566.png)

### 6.点击退出按钮，退出到登录页，并且如果要再次访问资源，需要重新登录

```html
<h1>主页</h1>
<ul>
    <li><a href="">用户管理</a></li>
    <li><a href="">角色管理</a></li>
    <li><a href="">课程管理</a></li>
    <li><a href="">订单管理</a></li>
    <li><a href="${pageContext.request.contextPath}/user/logout">退出</a></li>
</ul>
```

![image-20210622234440568](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20210622234440568.png)

## 6.7 使用数据库中的数据进行MD5+salt+散列实现认证功能

在使用MD5+salt+散列进行认证时，那在注册阶段也需要进行md5+salt

### 1.注册页面

```html
<body>
<h1>用户注册</h1>
<form action="${pageContext.request.contextPath}/user/register">
    用户名:<input type="text" name="username">
    密码:<input type="text" name="password">
    <input type="submit" value="注册">
</form>
</body>
```

### 2.创建数据库

```sql
CREATE TABLE `t_user` (
  `id` int(6) NOT NULL AUTO_INCREMENT,
  `username` varchar(20) DEFAULT NULL,
  `password` varchar(20) DEFAULT NULL,
  `salt` varchar(20) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

### 3.引入数据库相关依赖

```xml
		<!--添加mysql-->
		<dependency>
			<groupId>mysql</groupId>
			<artifactId>mysql-connector-java</artifactId>
		</dependency>

		<!--添加mybatis-->
		<dependency>
			<groupId>org.mybatis.spring.boot</groupId>
			<artifactId>mybatis-spring-boot-starter</artifactId>
			<version>2.1.2</version>
		</dependency>

		<!--Druid-->
		<dependency>
			<groupId>com.alibaba</groupId>
			<artifactId>druid</artifactId>
			<version>1.0.9</version>
		</dependency>
```

### 4.配置application.properties配置文件

```properties
#新增
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.datasource.type=com.alibaba.druid.pool.DruidDataSource
        
mybatis.mapper-locations=classpath:/mapper/*.xml
mybatis.type-aliases-package=com.baizhi.springbootshirojsp.entity
```

### 5.创建User实体类

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class User {
    private Integer id;
    private String username;
    private String password;
    private String salt;
}
```

### 6.创建UserMapper接口

```java
@Mapper
public interface UserMapper {
    public void save(User user);
}
```

### 7.创建UserMapper.xml配置文件

```xml
<insert id="save" parameterType="com.baizhi.springbootshirojsp.entity.User" keyProperty="id" useGeneratedKeys="true">
        INSERT into t_user VALUES (#{id},#{username},#{password},#{salt})
</insert>
```

### 8.UserService接口

```java
public interface UserService  {

    //注册
     void register(User user);
}
```

### 9.创建Salt工具类

```java
/**
 * Salt工具类
 */
public class SaltUtil {

    public static String getSalt(int n){

        String str = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz123456789!@#$%^&*()";
        char[] chars = str.toCharArray();
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < n; i++) {
            char ch = chars[new Random().nextInt(chars.length)];
            sb.append(ch);
        }
        return sb.toString();
    }
}
```

### 10.UserServiceImpl类

```java
@Service
@Transactional
public class UserServiceImpl implements UserService {

    @Autowired
    private UserMapper userMapper;

    // 注册功能
    @Override
    public void register(User user) {
        //1.使用工具类生成随机盐
        String salt = SaltUtil.getSalt(8);
        user.setSalt(salt);
        //2.通过md5Hash进行加密
        Md5Hash md5Hash = new Md5Hash(user.getUsername(), salt, 1024);
        user.setPassword(md5Hash.toHex());
        //3.添加操作
        userMapper.save(user);
    }
}
```

### 11.开发注册Controller

```java
  /**
     * 注册功能
     * @param user
     * @return
     */
    @RequestMapping("/register")
    public String register(User user) {
        try {
            userService.register(user);
            return "redirect:/login.jsp";
        }catch (Exception e){
            e.printStackTrace();
            return "redirect:/register.jsp";
        }
    }
```

### 12.编写UserMapper，根据用户身份信息认证的方法

```java
@Mapper
public interface UserMapper {

    User findByUsername(String username);
}
```

### 13.编写对应的UserMapper.xml

```sql
    <select id="findByUsername" resultType="com.baizhi.springbootshirojsp.entity.User">
        select id,username,password,salt from t_user where username=#{username}
    </select>
```

### 14.在realm进行认证操作

```java
/**
 * 自定义Realm：进行认证和授权相关操作
 */
public class CustomerRealm extends AuthorizingRealm {

    @Autowired
    private UserMapper userMapper;

    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principalCollection) {
        return null;
    }

    /**
     * 身份认证功能
     *
     * @param authenticationToken 带有身份信息的token
     * @return
     * @throws AuthenticationException
     */
    @Override
    protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken authenticationToken) throws AuthenticationException {

        //1.从token中获取身份信息（用户）
        String principal = (String) authenticationToken.getPrincipal();
        User user = userMapper.findByUsername(principal);
        //2.与数据库中的身份信息进行比较，这里写死数据。
        if (user != null) {
            //用户名一致
            //3.返回AuthenticationInfo接口的实现类
            return new SimpleAuthenticationInfo(user.getUsername(), user.getPassword(), ByteSource.Util.bytes(user.getSalt()), this.getName());
        }
        //数据库中没有该用户，认证失败
        return null;
    }
}
```

### 15.配置realm的凭证信息校验器

```java
@Bean
public Realm getRealm(){
  CustomerRealm customerRealm = new CustomerRealm();
  //设置hashed凭证匹配器
  HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
  //设置md5加密
  credentialsMatcher.setHashAlgorithmName("md5");
  //设置散列次数
  credentialsMatcher.setHashIterations(1024);
  customerRealm.setCredentialsMatcher(credentialsMatcher);
  return customerRealm;
}
```

## 6.8授权实现

+ 授权实现主要有三种方式：
  + 第一种：使用**页面标签**实现
  + 第二种：使用**代码方式**实现
  + 第三种：使用**注解方式**实现

### 1.页面标签授权

```html
<%@taglib prefix="shiro" uri="http://shiro.apache.org/tags" %>

<shiro:hasAnyRoles name="user,admin">
        <li><a href="">用户管理</a>
            <ul>
                <shiro:hasPermission name="user:add:*">
                <li><a href="">添加</a></li>
                </shiro:hasPermission>
                <shiro:hasPermission name="user:delete:*">
                    <li><a href="">删除</a></li>
                </shiro:hasPermission>
                <shiro:hasPermission name="user:update:*">
                    <li><a href="">修改</a></li>
                </shiro:hasPermission>
                <shiro:hasPermission name="user:find:*">
                    <li><a href="">查询</a></li>
                </shiro:hasPermission>
            </ul>
        </li>
        </shiro:hasAnyRoles>
        <shiro:hasRole name="admin">
            <li><a href="">商品管理</a></li>
            <li><a href="">订单管理</a></li>
            <li><a href="">物流管理</a></li>
        </shiro:hasRole>
```

### 2.代码方式授权

```java
@RequestMapping("save")
public String save(){
  System.out.println("进入方法");
  //获取主体对象
  Subject subject = SecurityUtils.getSubject();
  //代码方式
  if (subject.hasRole("admin")) {
    System.out.println("保存订单!");
  }else{
    System.out.println("无权访问!");
  }
  //基于权限字符串
  //....
  return "redirect:/index.jsp";
}
```

### 3.方法调用授权

+ **`@RequiresRoles：`基于角色授权**
+ **`@RequiresPermissions`：基于权限授权**

```JAVA
@RequiresRoles(value={"admin","user"})//用来判断角色  同时具有 admin user
@RequiresPermissions("user:update:01") //用来判断权限字符串
@RequestMapping("save")
public String save(){
  System.out.println("进入方法");
  return "redirect:/index.jsp";
}
```

![image-20200527203415114](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200527203415114.png)

### 4.授权数据持久化

![image-20200527204839080](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200527204839080.png)

```sql
SET NAMES utf8mb4;
SET FOREIGN_KEY_CHECKS = 0;

-- ----------------------------
-- Table structure for t_pers
-- ----------------------------
DROP TABLE IF EXISTS `t_pers`;
CREATE TABLE `t_pers` (
  `id` int(6) NOT NULL AUTO_INCREMENT,
  `name` varchar(80) DEFAULT NULL,
  `url` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
-- Table structure for t_role
-- ----------------------------
DROP TABLE IF EXISTS `t_role`;
CREATE TABLE `t_role` (
  `id` int(6) NOT NULL AUTO_INCREMENT,
  `name` varchar(60) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
-- Table structure for t_role_perms
-- ----------------------------
DROP TABLE IF EXISTS `t_role_perms`;
CREATE TABLE `t_role_perms` (
  `id` int(6) NOT NULL,
  `roleid` int(6) DEFAULT NULL,
  `permsid` int(6) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

-- ----------------------------
-- Table structure for t_user
-- ----------------------------
DROP TABLE IF EXISTS `t_user`;
CREATE TABLE `t_user` (
  `id` int(6) NOT NULL AUTO_INCREMENT,
  `username` varchar(40) DEFAULT NULL,
  `password` varchar(40) DEFAULT NULL,
  `salt` varchar(255) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8;

-- ----------------------------
-- Table structure for t_user_role
-- ----------------------------
DROP TABLE IF EXISTS `t_user_role`;
CREATE TABLE `t_user_role` (
  `id` int(6) NOT NULL,
  `userid` int(6) DEFAULT NULL,
  `roleid` int(6) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

SET FOREIGN_KEY_CHECKS = 1;
```

### 5.创建dao方法

```java
 //根据用户名查询所有角色
User findRolesByUserName(String username);
//根据角色id查询权限集合
List<Perms> findPermsByRoleId(String id);
```

### 6.mapper实现

```xml
<resultMap id="userMap" type="User">
  <id column="uid" property="id"/>
  <result column="username" property="username"/>
  <!--角色信息-->
  <collection property="roles" javaType="list" ofType="Role">
    <id column="id" property="id"/>
    <result column="rname" property="name"/>
  </collection>
</resultMap>

<select id="findRolesByUserName" parameterType="String" resultMap="userMap">
  SELECT u.id uid,u.username,r.id,r.NAME rname
  FROM t_user u
  LEFT JOIN t_user_role ur
  ON u.id=ur.userid
  LEFT JOIN t_role r
  ON ur.roleid=r.id
  WHERE u.username=#{username}
</select>

<select id="findPermsByRoleId" parameterType="String" resultType="Perms">
  SELECT p.id,p.NAME,p.url,r.NAME
  FROM t_role r
  LEFT JOIN t_role_perms rp
  ON r.id=rp.roleid
  LEFT JOIN t_perms p ON rp.permsid=p.id
  WHERE r.id=#{id}
</select>
```

### 7.service接口

```JAVA
//根据用户名查询所有角色
User findRolesByUserName(String username);
//根据角色id查询权限集合
List<Perms> findPermsByRoleId(String id);
```

### 8.service接口实现

```java
@Override
public List<Perms> findPermsByRoleId(String id) {
  return userDAO.findPermsByRoleId(id);
}

@Override
public User findRolesByUserName(String username) {
  return userDAO.findRolesByUserName(username);
}
```

### 9.修改自定义realm

```java
public class CustomerRealm extends AuthorizingRealm {
    @Override
    protected AuthorizationInfo doGetAuthorizationInfo(PrincipalCollection principals) {
        //获取身份信息
        String primaryPrincipal = (String) principals.getPrimaryPrincipal();
        System.out.println("调用授权验证: "+primaryPrincipal);
        //根据主身份信息获取角色 和 权限信息
        UserService userService = (UserService) ApplicationContextUtils.getBean("userService");
        User user = userService.findRolesByUserName(primaryPrincipal);
        //授权角色信息
        if(!CollectionUtils.isEmpty(user.getRoles())){
            SimpleAuthorizationInfo simpleAuthorizationInfo = new SimpleAuthorizationInfo();
            user.getRoles().forEach(role->{
                simpleAuthorizationInfo.addRole(role.getName());
                //权限信息
                List<Perms> perms = userService.findPermsByRoleId(role.getId());
                if(!CollectionUtils.isEmpty(perms)){
                    perms.forEach(perm->{
                        simpleAuthorizationInfo.addStringPermission(perm.getName());
                    });
                }
            });
            return simpleAuthorizationInfo;
        }
        return null;
    }
}
```

![image-20200527213821611](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200527213821611.png)

### 9.启动测试



## 6.9 使用CacheManager

### 1.Cache的作用

+ Cache缓存：计算机内存中的一段数据
+ 作用：用来减轻DB的访问压力，从而提高系统的查询效率
+ 流程：

![image-20200530090656417](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200530090656417.png)

### 2.使用shiro中默认的EhCache实现缓存

#### 1.引入依赖

```xml
<!--引入shiro和ehcache-->
<dependency>
  <groupId>org.apache.shiro</groupId>
  <artifactId>shiro-ehcache</artifactId>
  <version>1.5.3</version>
</dependency>
```

#### 2.开启缓存

```java
//3.创建自定义realm
    @Bean
    public Realm getRealm(){
        CustomerRealm customerRealm = new CustomerRealm();
        //修改凭证校验匹配器
        HashedCredentialsMatcher credentialsMatcher = new HashedCredentialsMatcher();
        //设置加密算法为md5
        credentialsMatcher.setHashAlgorithmName("MD5");
        //设置散列次数
        credentialsMatcher.setHashIterations(1024);
        customerRealm.setCredentialsMatcher(credentialsMatcher);

        //开启缓存管理器
        customerRealm.setCachingEnabled(true);
        customerRealm.setAuthorizationCachingEnabled(true);
        customerRealm.setAuthorizationCachingEnabled(true);
        customerRealm.setCacheManager(new EhCacheManager());
        return customerRealm;
    }
```

![image-20200529173859939](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200529173859939.png)

#### 3.启动刷新页面进行测试

**注意：如果控制台没有任何sql展示就说明缓存已经开启**

### 3.shiro中使用redis进行缓存

#### 1.引入依赖

```xml
<!--redis整合springboot-->
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

#### 2.配置redis连接

```properties
spring.redis.port=6379
spring.redis.host=localhost
spring.redis.database=0
```

![image-20200530084616799](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200530084616799.png)

#### 3.启动redis服务

```sh
➜  bin ls
dump.rdb        redis-check-aof redis-cli       redis-server    redis.conf
redis-benchmark redis-check-rdb redis-sentinel  redis-trib.rb
➜  bin ./redis-server redis.conf
```

![image-20200530081954871](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200530081954871.png)

#### 4.开发RedisCacheManager

```java
public class RedisCacheManager implements CacheManager {
    @Override
    public <K, V> Cache<K, V> getCache(String cacheName) throws CacheException {
        System.out.println("缓存名称: "+cacheName);
        return new RedisCache<K,V>(cacheName);
    }
}
```

#### 5.RedisCache实现

```java
public class RedisCache<K,V> implements Cache<K,V> {

    private String cacheName;

    public RedisCache() {
    }

    public RedisCache(String cacheName) {
        this.cacheName = cacheName;
    }

    @Override
    public V get(K k) throws CacheException {
        System.out.println("获取缓存:"+ k);
        return (V) getRedisTemplate().opsForHash().get(this.cacheName,k.toString());
    }

    @Override
    public V put(K k, V v) throws CacheException {
        System.out.println("设置缓存key: "+k+" value:"+v);
        getRedisTemplate().opsForHash().put(this.cacheName,k.toString(),v);
        return null;
    }

    @Override
    public V remove(K k) throws CacheException {
        return (V) getRedisTemplate().opsForHash().delete(this.cacheName,k.toString());
    }

    @Override
    public v remove(k k) throws CacheException {
        return (v) getRedisTemplate().opsForHash().delete(this.cacheName,k.toString());
    }

    @Override
    public void clear() throws CacheException {
        getRedisTemplate().delete(this.cacheName);
    }

    @Override
    public int size() {
        return getRedisTemplate().opsForHash().size(this.cacheName).intValue();
    }

    @Override
    public Set<k> keys() {
        return getRedisTemplate().opsForHash().keys(this.cacheName);
    }

    @Override
    public Collection<v> values() {
        return getRedisTemplate().opsForHash().values(this.cacheName);
    }

    private RedisTemplate getRedisTemplate(){
        RedisTemplate redisTemplate = (RedisTemplate) ApplicationContextUtils.getBean("redisTemplate");
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        return redisTemplate;
    }


    //封装获取redisTemplate
    private RedisTemplate getRedisTemplate(){
        RedisTemplate redisTemplate = (RedisTemplate) ApplicationContextUtils.getBean("redisTemplate");
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        return redisTemplate;
    }
}
```

#### 6.启动项目测试发现报错

![image-20200530100948598](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200530100948598.png)

+ 错误解释: **由于shiro中提供的simpleByteSource实现没有实现序列化,所有在认证时出现错误信息**

+ 解决方案: **需要自动salt实现序列化**

  + 自定义salt实现序列化

  ```java
  //自定义salt实现  实现序列化接口
  public class MyByteSource extends SimpleByteSource implements Serializable {
      public MyByteSource(String string) {
          super(string);
      }
  }
  ```

  + 在realm中使用自定义salt

  ```java
   @Override
  protected AuthenticationInfo doGetAuthenticationInfo(AuthenticationToken token) throws AuthenticationException {
    System.out.println("==========================");
    //根据身份信息
    String principal = (String) token.getPrincipal();
    //在工厂中获取service对象
    UserService userService = (UserService) ApplicationContextUtils.getBean("userService");
    User user = userService.findByUserName(principal);
    if(!ObjectUtils.isEmpty(user)){
      return new SimpleAuthenticationInfo(user.getUsername(),user.getPassword(), 
                                        new MyByteSource(user.getSalt()),this.getName());
    }
    return null;
  }
  ```

  ![image-20200530101301543](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200530101301543.png)

#### 7.再次启动测试，发现可以成功放入redis缓存

![image-20200530101617692](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200530101617692.png)



# 7.shiro整合thymeleaf之权限控制

## 1.引入依赖

```xml
<dependency>
    <groupId>com.github.theborakompanioni</groupId>
    <artifactId>thymeleaf-extras-shiro</artifactId>
    <version>2.0.0</version>
</dependency>
```

## 2.页面引入命名空间

xmlns:shiro="http://www.pollix.at/thymeleaf/shiro"

```html
<html xmlns="http://www.w3.org/1999/xhtml" xmlns:th="http://www.thymeleaf.org"
      xmlns:shiro="http://www.pollix.at/thymeleaf/shiro">
......
```

## 3.常见权限控制标签使用

```html
<!-- 验证当前用户是否为“访客”，即未认证（包含未记住）的用户。 -->
<p shiro:guest="">Please <a href="login.html">login</a></p>


<!-- 认证通过或已记住的用户。 -->
<p shiro:user="">
    Welcome back John! Not John? Click <a href="login.html">here</a> to login.
</p>

<!-- 已认证通过的用户。不包含已记住的用户，这是与user标签的区别所在。 -->
<p shiro:authenticated="">
    Hello, <span shiro:principal=""></span>, how are you today?
</p>
<a shiro:authenticated="" href="updateAccount.html">Update your contact information</a>

<!-- 输出当前用户信息，通常为登录帐号信息。 -->
<p>Hello, <shiro:principal/>, how are you today?</p>


<!-- 未认证通过用户，与authenticated标签相对应。与guest标签的区别是，该标签包含已记住用户。 -->
<p shiro:notAuthenticated="">
    Please <a href="login.html">login</a> in order to update your credit card information.
</p>

<!-- 验证当前用户是否属于该角色。 -->
<a shiro:hasRole="admin" href="admin.html">Administer the system</a><!-- 拥有该角色 -->

<!-- 与hasRole标签逻辑相反，当用户不属于该角色时验证通过。 -->
<p shiro:lacksRole="developer"><!-- 没有该角色 -->
    Sorry, you are not allowed to developer the system.
</p>

<!-- 验证当前用户是否属于以下所有角色。 -->
<p shiro:hasAllRoles="developer, 2"><!-- 角色与判断 -->
    You are a developer and a admin.
</p>

<!-- 验证当前用户是否属于以下任意一个角色。  -->
<p shiro:hasAnyRoles="admin, vip, developer,1"><!-- 角色或判断 -->
    You are a admin, vip, or developer.
</p>

<!--验证当前用户是否拥有指定权限。  -->
<a shiro:hasPermission="userInfo:add" href="createUser.html">添加用户</a><!-- 拥有权限 -->

<!-- 与hasPermission标签逻辑相反，当前用户没有制定权限时，验证通过。 -->
<p shiro:lacksPermission="userInfo:del"><!-- 没有权限 -->
    Sorry, you are not allowed to delete user accounts.
</p>

<!-- 验证当前用户是否拥有以下所有角色。 -->
<p shiro:hasAllPermissions="userInfo:view, userInfo:add"><!-- 权限与判断 -->
    You can see or add users.
</p>

<!-- 验证当前用户是否拥有以下任意一个权限。  -->
<p shiro:hasAnyPermissions="userInfo:view, userInfo:del"><!-- 权限或判断 -->
    You can see or delete users.
</p>
<a shiro:hasPermission="pp" href="createUser.html">Create a new User</a>
```

## 4.加入shiro的方言配置

**页面标签不起作用一定要记住加入方言处理**

```java
@Bean(name = "shiroDialect")
public ShiroDialect shiroDialect(){
  return new ShiroDialect();
}
```

![image-20200601210335151](Shiro%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0.assets/image-20200601210335151.png)













