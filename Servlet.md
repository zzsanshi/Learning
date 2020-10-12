[视频地址](https://www.bilibili.com/video/BV14D4y127p6?p=2)

# 2020/10/12

xml可扩展配置文件:Document,Element,Attribute,Text,Comment

web应用目录结构:

+ myapp:css,js,html,WEB-INF

JSP:动态网页

**web相关概念:**

+ 软件架构:C/S,B/S

+ 资源分类:静态资源,动态资源

+ 网络通信三要素:ip,端口-应用程序Zaibatsu计算机中的唯一标识,传输协议-规定数据传输的规则

**web服务器软件:**

+ 服务器:安装了服务器软件的电脑

+ 服务器软件:接受用户请求,处理请求,做出软件,如:web服务器软件

+ 常见软件:webLogic,webSphere,JBOSS,Tomcat(免费的)

  (JavaEE:企业中运用的语言规范)

**Java动态项目的目录结构:**

   ---项目的根目录

​	   ---WEB-INF

​			---web.xml:web项目的核心配置文件

​			---classes目录:放置字节码文件的目录

​			---lib目录:放置依赖的jar包

# 1.Servet

## 1.1 Servlet概念

+ 运行在服务器端的小程序--就是一个接口,定义了Java类被浏览器访问到规则.

+ 将来我们自定义一个类,实现Servlet接口,复写方法

## 1.2 Servet快速入门

1. 创建项目

2. 定义Servlet类

3. 实现接口中的抽象类

4. 配置Servlet,在web.xml中

   ```java
    	<servlet>
           <servlet-name>demo1</servlet-name>
           <servlet-class>web.servlet.ServletDemo1</servlet-class>
       </servlet>
       <servlet-mapping>//做一个映射
           <servlet-name>demo1</servlet-name>
           <url-pattern>/demo1</url-pattern>
       </servlet-mapping>
   ```

## 1.3 Servlet执行原理(过程)

1. 当服务器接收到客户端浏览器的请求后,会解析请求URL路径,获取访问的Servlet的资源路径
2. 查找web.xml文件,是否有对应的<url-pattern>便签内容
3. 如果有,则在找到对应的<servlet-class>全类名
4. tomcat会将字节码文件加载进内存,创建其对象
5. 调用方法

## 1.4 servlet方法,生命周期

+ **初始化**方法,在创建时执行,只会执行一次:public void **init**(ServletConfig servletConfig) throws ServletException 
+ **获取ServletConfig对象**,即配置对象 :public ServletConfig **getServletConfig**() {return null;}

+ **提供服务**的方法,每一次Servlet被访问时执行,执行多次:public void **service**(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {}

+ 获取Servlet的一些**信息**,版本,作者:public String **getServletInfo**() { return null;}

+ **销毁**方法,在Servlet被杀死时执行,即正常关闭,执行一次:public void **destroy**() {}

**生命周期**:

1. 被创建:执行init(),只执行一次,说明Serfvlet在内存中只存在一个对象为单例,可能存在线程安全问题

   ​				解决:方法不共享,尽量不要在Servlet中定义成员变量,即使定义了也不要赋值和修改

   ​				Servlet什么时候被创建:默认情况下,第一次访问时,servlet被创建

   ​				可以指定Servlet被创建的时机,如:

   ```java
   	<load-on-startup>0</load-on-startup>
       <!--负数:第一次被访问时创建;0或正数:服务器启动时-->
   ```

2. 提供服务:执行service方法,执行多次

3. 被销毁:执行destroy方法,只执行一此

   ​			被销毁时执行,服务器正常关闭时,非正常不会执行.先执行destory,后销毁Servlet.用于释放资源

## 1.5 Servlet3.0注解配置
**步骤**

1. 创建项目,Servlet3.0以上,可以不创建web.xml
2. 定义一个类,实现Servlet接口
3. 复写方法
4. 在类上使用**@WebServlet(urlPatterns = "/...")**注解

## 1.6 体系结构

1. Servlet--接口
   + GenericServlet:实现可Servlet接口时抽象类
     + 将Servlet接口中的其他方法默认空实现,之将Service方法作为抽象,实现service()
   + HttpServlet:抽象类,对Http协议的一种封装,简化操作
     1. 定义类集成HttpServlet
     2. 复写doGet/doPost方法

## 1.7 相关配置

1. urlPatterns:
   + urlPatterns={"1","2"}//多个访问路径
   + 路径定义规则:
     + /xxx,
     + /xxx/xxx.多层路径,目录结构
     + *.do,

# 2. Http

## 2.1概述

**超文本传输协议**:定义了客户端和服务端通信时,发送数据的格式

**特点:**

1. 基于TCP/IP的高级协议
2. 默认端口号:80
3. 基于请求响应/模型:一次请求对应一次响应
4. 无状态的:每次请求之间相互独立,不能交互数据

**历史版本:**

+ 1.0:每一次请求都会建立新的连接
+ 1.1:复用连接

## 2.2数据格式

+ **请求消息数据格式**

1. 请求行

   ​	请求方式 请求url 请求协议/版本:GET/*.html HTTP/1.1

   + 请求方式
     + GET:请求参数在请求行中,在url后;url长度**有限制**;**不安全**
     + POST:在请求体中；url长度**没有限制**；相对安全

2. 请求头

     请求头名称:请求头值

   + 常见的请求头

     + User-Agent：浏览器告诉服务器，我访问你使用的浏览器版本信息；可以在服务器端获得该头信息，解决浏览器的兼容性问题

     + Accept:哪些页面

     + Accept-language:语言环境

     + Reference:告诉服务器我(当前请求)从哪里来

       + 作用:1.防盗链:

         ​         2.统计工作:

     + Connection:连接关系

3. 请求空行

    空行,就是用于分割POST请求的请求头和请求体的

4. 请求体(正文):封装POST请求消息的请求参数的

   

+ **响应消息数据格式**

