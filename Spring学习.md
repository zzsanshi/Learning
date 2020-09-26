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
+ 开发明确事项"
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

1. 注解通知的类型

   ![image-20200926174104556](E:\学习笔记\Learning\图片\image-20200926174104556.png)

## 8.3 切点表达式的抽取

同xml配置aop一样,我们可以将切点表达式抽取.抽取方式是在切面内定义方法,在该方法上使用@Pointcut注解定义切点表达式,然后在增强表达式中进行引用.