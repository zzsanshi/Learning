视频地址:[SSM教程](https://www.bilibili.com/video/BV1Bg4y1q7q2?p=40&t=486)

# 2020/9/2

# 1.Spring的重点配置
<bean>标签  
 - id属性:在容器中 Bean实例的唯一标志,不允许重复
 - class属性:要实例化的Bean的全限定名
 - scope属性:Bean的作用范围,常用是Singleton(默认)和prototype
   - <property>标签:属性注入 
      - name属性:属性名称
      - value属性:注入怕普通属性值
      - ref属性:注入的对象引用值
      -  <list>标签
       - `<map>`标签  
     - <properties>标签
   - ​	<constructor-arg>标签
+ <import>标签:导入其他的Spring的分文
# 2020/9/20

# 2.相关API
## 2.1ApplicationContex的继承体系

 接口类型 代表应用上下文,可以通过其实例获得Spring容器中的Bean对象

## 2.2ApplicationContex实现类

+ ClassPathXmlApplicationContext

  从类的根路径下加载配置文件推荐使用

+ FileSystemXmlApplicationContext

  从磁盘中加载配置文件

+ AnnotationConfigApplicationContext

  当使用注解配置容器对象时,需要使用此类来创建spring容器.它用来读取注解

## 2.3getBean()方法使用

+ id方式--有多个时用
+ 传字节码对象类型--只有一个时可以用

# 3.Spring配置数据源(连接池)

## 3.1数据源(连接池)的作用

+ 为提高程序性能出现的
+ 事先优化数据源,初始化部分连接资源
+ 使用连接资源时从数据源中获取
+ 使用完毕后将连接资源归还数据源
+ 常见的连接池:DBCP C3P0 Bonecp Druid等

## 3.2 Spring配置

   将Data的使用权交给Spring

## 3.3抽取配置JDBC配置文件

applicationContext.Xml加载jdbc.properties配置文件获得连接信息

首先,需要引入context命名空间和约束路径:

+ 命名空间:xmlns:context="http://..."
+ 约束路径:http://...

# 4.Spring注解开发--需要配置组件扫描

## 4.1Spring的原始注解

Spring是轻代码而重配置的框架,配置比较繁重,影响开发效率,所以注解开发是一种趋势,注解代替xml配置文件可以简化配置,提高开发配置.

原始注解:主要代替<bean>配置

<img src="E:\学习笔记\Learning\图片\image-20200920193533352.png" alt="image-20200920193533352 " style="zoom:60%;" />

## 4.2 Spring的新注解

旧注解不能全部代替xml配置文件,还需要使用注解替代的配置如下:

+ 非自定义的Bean的配置:<bean>
+ 加载properties文件的配置:<context:property-placeholder>
+ 组件扫描的配置:<context:componet-scan>
+ 引入其他文件:<import>

新注解:<img src="E:\学习笔记\Learning\图片\image-20200920204929289.png" alt="image-20200920204929289" style="zoom:70%;" />

# 2020/9/21


# 5.Spring集成Junit

## 5.1存在的问题

繁琐会有空指针异常

## 5.2集成步骤

1. 导入spring集成Junit的坐标
2. 使用@Runwith注解替换原来的运行期
3. 使用@ContextConfiguration指定配置文件或配置类
4. 使用@Autowired注入需要测试的对象
5. 创建测试方法进行测试

```java
@RunWith(.class)
@ContextConfigura(".xml")
public void test(){
   @Autowired
    private int num;
    @Test
    public void Test(){
        ...
    }
}
```



# 6.Spring的AOP

AOP--**面向切面编程**,通过预编译技术和运行期**动态代理**实现程序功能的统一维护的一种技术.

**作用**:在运行期间,不修改源码的情况下对方法进行增强

**优势**:减少代码重复,便于维护

## 6.1动态代理技术

+ JDK代理:基于**接口**的动态代理技术
+ cglib代理:基于**父类**的动态代理技术



![image-20200921164616028](E:\学习笔记\Learning\图片\image-20200921164616028.png)

# 2020/9/25

## 6.2 JDK的动态代理

```java
         //目标对象
        final Target target = new Target();
        Advice advice = new Advice();//获得增强对象
        //返回值为动态生成的代理对象
        TargetInterface proxy = (TargetInterface) Proxy.newProxyInstance(
                target.getClass().getClassLoader(),//目标对象类加载器
                target.getClass().getInterfaces(), //目标对象相同的接口字节码对象数组
                new InvocationHandler() {
                    //调用代理对象的任何方法,实质执行的都是invoke方法
                    @Override
                    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                        advice.before();//前置增强
                        method.invoke(target, args);//执行目标方法
                        advice.afterReturning();//后置增强
                        return null;
                    }
                }//
        );
        //调用代理对象的方法
        proxy.save();
```

## 6.3 cglib的动态代理

```java
  //目标对象
        final Target target=new Target();
        //获得增强对象
        final Advice advice=new Advice();
        //返回值就是动态生成的代理对象 基于cglib
        //1.创建增强器
        Enhancer enhancer=new Enhancer();

        //2.设置父类(目标)

        enhancer.setSuperclass(Target.class);
        //3.设置回调
        enhancer.setCallback(new MethodInterceptor() {
            @Override
            public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
               advice.before();//执行前置增强
               Object invoke=method.invoke(target,args);//目标
                advice.afterReturning();//后置
                return null;
            }
        });
        //4.生成代理对象
      Target proxy=(Target)enhancer.create();
       proxy.save();

```

## 6.4 AOP相关概念

+ Target(目标对象):代理的目标对象
+ Proxy(代理对象):一个类被AOP植入增强后,就产生一个结果代理类
+ JoinPoint(连接点):所谓连接点是指那些被拦截到的点.在Spring中,这些点指方法,因为Spring只支持方法类型的连接点--**可以被增强的方法叫做连接点**
+ **Pointcut**(切入点):所谓切入点是指我们要对哪些Joinpoint进行拦截的定义--**被增强的连接点叫做切入点**
+ **Advice(**通知/增强):所谓通知是指拦截到Joinpoint之后所要做的事情就是通知--
+ **Aspect**(切面):是切入点和通知(引介)的结合--**切点+通知**
+ Weaving(织入):是指把增强应用到目标对象来创建的代理对象的过程.spring采用动态代理织入,而AspectJ采用编译期织入和类装载期织入-**-将切点和通知/增强结合的过程就是织入**,即配置

## 6.5 AOP开发明确的事项

1. 需要编写的内容

   1. 编写业务核心代码(目标类的目标方法)--被增强的方法

   + 编写切面类,切面类中有通知(增强方法通知)
   + 在配置文件中,配置织入关系,即将哪些通知与哪些连接点进行结合

2. AOP技术实现的内容

Spring会监控切入点方法的执行.一旦**监控**到切入点方法被运行,使用代理机制,动态**创建**目标对象的代理对象,根据通知类别,在代理的对应位置,将通知对应的功能**织入**,完成完整的代码逻辑运行.

3. AOP底层使用哪种代理方式
   - 有接口:使用JDK
   - 没有接口:使用cglib

## 6.6 知识要点

+ aop:面向切面的编程
+ aop底层实现:基于JDK和基于Cglib的动态代理
+ aop的重点概念:
  + Pointcut切点:被增强的方法
  + Advice(通知/增强):封装增强业务逻辑的方法
  + Aspect(切面):切点+通知
  + Weaving(织入):将切点和通知结合的过程
+ 开发明确事项:
  + 谁是切点(切点表达式配置)
  + 谁是通知(切面类中的增强方法)
  + 将切点和通知进行织入配置

#  2020/9/26

# 7.基于XML的AOP开发

## 7.1 快速入门

1. 导入AOP相关坐标

2. 创建目标接口和目标类(内部有切点)

3. 创建切面类(内部有增强方法)

4. 将目标类和切面类的对象创建权交给spring

5. 在applicationContext.xml中配置织入对象

   ```java
   <!--    目标对象-->
       <bean id="Target" class="LearningCode0926.aop.Target"></bean>
   <!--        切面-->
       <bean id="myAspect" class="LearningCode0926.aop.MyAspect"></bean>
   <!--   配置织入,告诉spring哪些方法(切点)需要进行哪些增强(前置/后置) -->
   <aop:config>
   <!--    声名切面-->
       <aop:aspect ref="myAspect">
   <!--        切点+通知-->
           <aop:before method="before" pointcut="execution(public void LearningCode0926.aop.Target.save())"/>
       </aop:aspect>
   </aop:config>
   ```

   

6. 测试代码

## 7.2 XML配置AOP详解

1. 切点表达式的写法

表达式语法:execution([修饰符]返回值类型 包名.类名.方法名(参数))

+ 方法修饰符可以省略
+ 返回值类型,包名,类名,方法名可以使用星号*代表任意
+ 包名与类名之间一个点.代表当前包下的类,两个点..代表当前包及其子包下的类
+ 参数列表可以使用两个点..表示任意个数,任意类型的参数列表

2. 通知的类型

通知的配置语法:<aop:通知类型 method="切面类中方法名" pointcut="切点表达式"></aop:通知类型>

![image-20200926163646633](E:\学习笔记\Learning\图片\image-20200926163646633.png)

3. 切点表示式的抽取

当多个增强的切点表达式相同时,可以将切点表达式进行抽取,在增强中使用pointcut-ref属性代替pointcut属性来引用抽取后的切点表达式.

**优势**:好维护

# 8.基于注解的AOP开发

## 8.1 快速入门

开发步骤:

1. 创建目标接口和目标类(内部有切点)
2. 创建切面类(内部有增强方法)
3. 将目标类和切面类的对象创建权交给spring
4. 在切面类中使用注解配置织入关系
5. 在配置文件中开启组件扫描和AOP的**自动代理**

    ```java
    <!--组件扫描-->
    <context:component-scan base-package="LearningCode0926.annoation"/>
    <!--aop自动代理-->
    <aop:aspectj-autoproxy/>
    ```

6. 测试

## 8.2 注解配置AOP详解

注解通知的类型

![image-20200926174104556](E:\学习笔记\Learning\图片\image-20200926174104556.png)

## 8.3 切点表达式的抽取

同xml配置aop一样,我们可以将切点表达式抽取.抽取方式是在切面内定义方法,在该方法上使用@Pointcut注解定义切点表达式,然后在增强表达式中进行引用.

# 9.Spring JdbcTemplate基本使用

## 9.1概述

他是spring框架中提供的一个对象,是对原始Jdbc API对象的简单封装.Spring框架为我们提供了很多的操作模板类.例如,操作关系型数据的JdbcTemplate和HibernateTemplate.操作nosql数据库的RedisTemplate,操作消息队列的JmsTemplate等等.

## 9.2 JdbcTemplate开发步骤

1. 导入spring-jdbc和spring-tx坐标
2. 创建数据库和实体
3. 创建JdbcTemplate对象
4. 执行数据库操作

+ 基本数据库操作

```java
        //创建数据源对象
        ComboPooledDataSource dataSource=new ComboPooledDataSource();
        //dataSource.setDriverClass("com.sqlite.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:sqlite:D:\\Database\\username.db");
        //创建模板对象
        JdbcTemplate jdbcTemplate=new JdbcTemplate();
        jdbcTemplate.setDataSource(dataSource);//设置数据源对象,告诉数据位置
        int row=jdbcTemplate.update("insert into account values(?,?)"," Tom",4);//更新,插入数据,返回行数
```

+ Spring产生JdbcTemnplate对象

可以将JdbcTemplate的创建权交给Spring,将数据源Datasource的创建权也交给Spring.在Spring容器中将数据源DataSource注入到JdbcTemplate模板对象中.

```java
//xml配置
    <!--数据源对象-->
    <bean id="datasource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="jdbcUrl" value="jdbc:sqlite:D:\Database\username.db"/>
        //<property name="jdbcUrl" value="${jdbc.url}"/>//配置文件形式
    </bean>
    <!--Jdbc模板对象-->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="datasource"/>
    </bean>
```

```java
//Ioc
        //控制反转
        ApplicationContext app = new ClassPathXmlApplicationContext("DataSource.xml");
        JdbcTemplate jdbcTemplate = app.getBean(JdbcTemplate.class);
        //配置
        int row = jdbcTemplate.update("insert into account values(?,?)", " T1", 4);
```

# 2020/9/27

## 9.3 Jdbc基本操作

```java
        jdbcTemplate.update("insert into account values(?,?)", " T1", 4);//插入
        jdbcTemplate.update("update account set money=? where name=?", 2, "who");//更新操作
        jdbcTemplate.update("delete from account where name=?", "T1");//删除操作
```



```java
        //查询全部
        List<Account> accountList= jdbcTemplate.query("select * from account",new                         BeanPropertyRowMapper<Account>(Account.class));
        
        //查询单个对象
        Account account= jdbcTemplate.queryForObject("select * from account where name=?",new BeanPropertyRowMapper<Account>(Account.class),"Tom");
        
       //聚合查询
       Long count=jdbcTemplate.queryForObject("select count(*) from account",Long.class);
       
```

# 10. 事务控制

## 10.1 编程式事务控制

+ PlatformTransactionManager--接口只定义行为

spring的事务管理器,里面提供了常用的操作事务的方法

![image-20200927202131183](E:\学习笔记\Learning\图片\image-20200927202131183.png)

+ TransactionDedfinition

事务的定义信息对象

![image-20200927202258632](E:\学习笔记\Learning\图片\image-20200927202258632.png)

+ TransactionStatus--被动安装对象信息

提供事务具体的运行状态

![image-20200927202401195](E:\学习笔记\Learning\图片\image-20200927202401195.png)

## 10.2 基于XML的声名式事务控制

spring的声名式事务顾名思义就是采用声名的方式来处理事务.所谓声名,就是配置文件中声名,用在spring配置文件中声名式的处理事务来代替代码式的处理事务.

**作用:**

+ 事务管理不侵入开发的组件.具体来说,业务逻辑对象就不会到正在事务管理之中,事实上也应该如此,因为事务管理是属于**系统**层面的服务,而不是业务逻辑的一部分,如果想要改变事务管理策划的话,也只需要在定义文件中重新配置即可.--**解耦性**
+ 在不需要事务管理的时候,只要在设定文件上修改一下,即可移去事务管理服务,无需改变代码重新编译,这样维护起来极其方便.

**ps:**spring声明式事务控制底层就是AOP

## 10.3 基于XML的声名式事务控制的实现

**明确事项:**

+ 谁是切点
+ 谁是通知
+ 配置切面

```java
<!--配置平台事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="datasource"></property>
    </bean>
    <!--通知 事务的增强-->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <tx:method name="*"/>
    </tx:attributes>
</tx:advice>
<!--配置事务AOP的织入-->
    <aop:config>
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* LearningCode0927.service.impl.*.*(..))"></aop:advisor>
    </aop:config>
```

**事务参数配置:**

+ <tx:method>
  + name:切点方法名称
  + isolation:事务的隔离级别
  + propogation:事务的传播行为
  + timeout:超时时间
  + read-only:是否只读

**配置要点:**

+ 平台事务管理器配置(xml方法)
+ 事务通知的配置(xml或者@Transactional注解配置)
+ 事务aop织入的配置

## 10.4基于注解的声明式事务控制

```java
<!--配置平台事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="datasource"></property>
    </bean>

<!--事务的注解驱动-->
    <tx:annotation-driven transaction-manager="transactionManager"/>
```

**解析:**

1. 使用@Transactional在需要进行事务控制的类或是方法上修饰,注解可用的属性同XML配置方式,例如隔离级别,传播行为等.
2. 注解使用在类上,那么该类下的所有方法都使用同一套注解参数配置.
3. 使用在方法上,不同的方法可以采用不同的事务参数配置
4. Xml配置文件中要开启事务的注解驱动<tx:annotation-driven/>

# 2020/9/28

# 11. Spring集成Web环境

## 11.1 ApplicationContext应用上下文获取方式

+ **问题:**获取太多次,影响性能.

+ **解决:**在web项目中,可以使用ServletContextLinstener监听Web应用的启动,可以在Web启动时,就加载Spring的配置文件,创建应用上下文对象ApplicationContext,在将其存储到最大域servletContext域中,这样就可以在任意位置从域中获得应用上下文Application对象了.

+ 代码实例

  + 监听器 listenser

    ```java
    public class ContextLoaderListener implements ServletContextListener {
    
        @Override
        public void contextInitialized(ServletContextEvent servletContextEvent) {
            ApplicationContext app = new ClassPathXmlApplicationContext("application-0928.xml");
    
            //将Spring的应用上下文对象存储到最大的域--ServerContext中
            ServletContext servletContext = servletContextEvent.getServletContext();
            servletContext.setAttribute("app", app);
        }
    
        @Override
        public void contextDestroyed(ServletContextEvent servletContextEvent) {
    
        }
    }
    ```

  + web.xml配置

    ```java
    <!--配置监听器-->
        <listener>
            <listener-class>LearningCode0928.listener.ContextLoaderListener</listener-class>
        </listener>
    ```

  + 获取配置信息

    ```java
            //ServletContext servletContext= req.getServletContext();第一种
            ServletContext servletContext=this.getServletContext();//第二种
            ApplicationContext app= (ApplicationContext) servletContext.getAttribute("app");
            UserService userService=app.getBean(UserService.class);
    ```

## 11.2自定义监听器

+ 监听器 listenser

  ```java
   @Override
      public void contextInitialized(ServletContextEvent servletContextEvent) {
          //读取web.xml中的全全局参数
          ServletContext servletContext = servletContextEvent.getServletContext();
  
          String contextConfigLocation=servletContext.getInitParameter("contextConfigLocation");
          ApplicationContext app = new ClassPathXmlApplicationContext("contextConfigLocation");
  ```

+ web.xml

  ```java
   <!--    全局初始化参数-->
      <context-param>
          <param-name>contextConfigLocation</param-name>
          <param-value>application-0928.xml</param-value>
      </context-param>
  ```

## 11.3 Spring提供获取应用上下文的工具

Spring提供了一个监听器ContextLoaderListener对上述功能进行封装,该监听器内部加载Spring配置文件,创建应用上下文对象,并存储到ServletContext域中,提供了一个客户端工具WebApplicationUtils供使用者获得应用上下文对象.

1. 在web.xml中配置ContextLoaderListener监听器(导入spring-web坐标)
2. 使用WebApplicationContextUtils获得应用上下文对象ApplicationContext