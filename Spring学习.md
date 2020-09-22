视频地址:[SSM教程](https://www.bilibili.com/video/BV1Bg4y1q7q2?p=40&t=486)

# 2020/9/2

##    1.Spring的重点配置

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

## 1.相关API

### 1.1ApplicationContex的继承体系

 接口类型 代表应用上下文,可以通过其实例获得Spring容器中的Bean对象

### 1.2ApplicationContex实现类

+ ClassPathXmlApplicationContext

  从类的根路径下加载配置文件推荐使用

+ FileSystemXmlApplicationContext

  从磁盘中加载配置文件

+ AnnotationConfigApplicationContext

  当使用注解配置容器对象时,需要使用此类来创建spring容器.它用来读取注解

### 1.3getBean()方法使用

+ id方式--有多个时用
+ 传字节码对象类型--只有一个时可以用

## 2.Spring配置数据源(连接池)

### 2.1数据源(连接池)的作用

+ 为提高程序性能出现的
+ 事先优化数据源,初始化部分连接资源
+ 使用连接资源时从数据源中获取
+ 使用完毕后将连接资源归还数据源
+ 常见的连接池:DBCP C3P0 Bonecp Druid等

### 2.2 Spring配置

   将Data的使用权交给Spring

### 2.3抽取配置JDBC配置文件

applicationContext.Xml加载jdbc.properties配置文件获得连接信息

首先,需要引入context命名空间和约束路径:

+ 命名空间:xmlns:context="http://..."
+ 约束路径:http://...

## 3.Spring注解开发--需要配置组件扫描

### 3.1Spring的原始注解

Spring是轻代码而重配置的框架,配置比较繁重,影响开发效率,所以注解开发是一种趋势,注解代替xml配置文件可以简化配置,提高开发配置.

原始注解:主要代替<bean>配置

<img src="E:\学习笔记\Learning\图片\image-20200920193533352.png" alt="image-20200920193533352 " style="zoom:60%;" />

### 3.2 Spring的新注解

旧注解不能全部代替xml配置文件,还需要使用注解替代的配置如下:

+ 非自定义的Bean的配置:<bean>
+ 加载properties文件的配置:<context:property-placeholder>
+ 组件扫描的配置:<context:componet-scan>
+ 引入其他文件:<import>

新注解:<img src="E:\学习笔记\Learning\图片\image-20200920204929289.png" alt="image-20200920204929289" style="zoom:70%;" />



# 2020/9/21

## 1.Spring集成Junit

### 1.1存在的问题

繁琐会有空指针异常

### 1.2集成步骤

1. 导入spring集成Junit的坐标
2. 使用@Runwith注解替换原来的运行期
3. 使用@ContextConfiguration指定配置文件或配置类
4. 使用@Autowired注入需要测试的对象
5. 创建测试方法进行测试

## 2.Spring的AOP

AOP--**面向切面编程**,通过预编译技术和运行期**动态代理**实现程序功能的统一维护的一种技术.

**作用**:在运行期间,不修改源码的情况下对方法进行增强

**优势**:减少代码重复,便于维护

### 2.1动态代理技术

+ JDK代理:基于**接口**的动态代理技术
+ cglib代理:基于**父类**的动态代理技术



![image-20200921164616028](E:\学习笔记\Learning\图片\image-20200921164616028.png)











