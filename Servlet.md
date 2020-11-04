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

   ​	请求方式 请求url 请求协议/版本: GET/*.html HTTP/1.1

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

# 2020/10/13

# 3.Request--请求消息对象

## 3.1 request和response原理

request中封装请求消息数据;

response对象设置响应消息数据.

1. request和response是由服务器创建的
2. request获取请求消息,response设置响应消息

## 3.2 request对象的继承体系结构

**ServletRequest**: --接口

 	|继承

**HttpServletRequest**: --接口

​	 |实现

org.apache.catalina.connector.RequestFacade

## 3.3 Request功能

1. **获取请求消息数据**

   + 获取请求行数据

     + 获取请求方式: GET--String getMethod()
     + **获取虚拟目录**:String getContextPath()
     + 获取Servlet路径:String getServletPath()
     + 获取get方式的请求参数:String getQueryString()
     + **获取请求的URI**:String getRequestURI()--短一些
     + 获取协议及版本:Strng getProtocol()
     + 获取客户机IP地址:String getRemoteAddr()

   + 获取请求头数据

     + String getHeader(String name):通过请求头名称,获取请求头的值
     + Enumeration<String> getHeaderNames():获取所有请求头的名称

   + 获取请求体数据--只有POST请求方式,才有请求体,在请求体中封装了POST请求的请求参数

     + 步骤:1.获取流对象;getReader()--获取字符输入流,只能操作字符数据

       ​								getInputStream--获取字节输入流,可以操作所有数据类型

         		2.再从流对象中获取数据

**URL和URI的区别:**

+ URL:统一资源定位符

+ URI:统一资源标识符

2. **其他功能**

   + 获取请求参数通用方式:

     + String getParameter(String name):根据参数名称获取参数值
     + String[ ] getParameterValues(String name):根据参数名称获取参数值的数组
     + Enumeration<String>  getParameterNames():获取所有参数名称
     + Map<String,String[]> getParameterMap():获取所有参数的map集合

     **中文乱码问题:**post方式:获取参数前,设置流的编码,UTF-8

   + 请求转发:一种在服务器内部的资源跳转方式

     + 步骤:1.通过request对象获取请求转发器对象

       ​		 2.使用RequestDispatcher对象来进行转发

     + 特点:
       +  浏览器地址栏没有变化
       + 只能转发到服务器内部的资源中
       + 转发是一次请求

   + 共享数据:

     + 域对象:一个有作用范围的对象,可以在范围内共享数据
     + request域的范围:代表一次请求的范围,一般用于请求转发的多个资源中共享数据
     + 方法:
       1. void setAttribute(String name,Object obj):存储数据
       2. Object getAttitude(String name):通过键获取值
       3.  void removeAttribute(String name):通过键移除键值对

   + 获取ServletContext:

     Servletcontext getServletContext()

**JavaBean:**标准的Java类

**要求:**

1. 类必须被public修饰
2. 必须提供空参的构造器
3. 成员变量必须使用private修饰
4. 提供公共setter和getter方法

**功能:**封装数据

**概念:**

成员变量:

属性:setter和getter方法截取之后的产物---setHe--HE就是属性

**BeanUtils的方法**

1. setProperty():设置**属性值**
2. getProperty():获取**属性值**
3. **populate:**封装数据,将map集合的键值对信息,封装到对应的JavaBean对象中

# 2020/10/29

# 4.Response--响应消息对象

请求消息:客户端发送给服务器端的数据

​		数据格式:

						1. 请求行
   						2. 请求头
                        						3. 请求空行
                  						4. 请求体

响应消息:服务器端发送给客户端的数据

​		数据格式:

1. 响应行

   1. 组成:协议/版本 响应状态码 状态码描述
   2. 响应状态码:服务器告诉客户端浏览器本次请求和响应的一个状态
      1. 状态码都是三位数字
      2. 分类:
         1. 1XX:服务器接受客户端消息,但没有接受完成,等待一段时间后,发送1XX状态码
         2. 2XX:成功.代表:200
         3. 3XX:重定向.代表:302(重定向),304(访问缓存)
         4. 4XX:客户端错误.代表:404(路径没有对应资源);405(请求方式没有对应的do方法)
         5. 5XX:服务器端错误.代表:500(服务器内部出现异常)

2. 响应头

   1. 格式:头名称:值

   2. 常见的响应头:

      1. Content-Type:服务器告诉客户端本次响应体数据格式以及编码格式

      2. Content-disposition:服务器告诉客户端以什么格式打开响应体数据

         值:

         ​	in-line:默认值,在当前页面打开

         ​	attachment;filename=xxx:以附件形式打开响应体.文件下载

3. 响应空行

4. 响应体:真实的传输数据

## 4.1Response对象

功能:设置响应消息

1. 设置响应行

   1. 格式:HTTP/1.1 200 ok
   2. 设置状态码:setStatus(int sc)

2. 设置响应头:setHeader(String name,String value)

3. 设置响应体:

   使用步骤

    1. 获取输出流

       ​	字符输出流:PrintWriter getWriter()

       ​	字节输出流:ServletOutputStream getOutputStream()

   2. 使用输出流,将数据输出到客户端浏览器

## 4.2重定向--资源跳转的方式

代码实现:

1. 设置状态码为302:resp.setStatus(302)
2. 设置响应头location:resp.setHeader("location","/responseDemo2");

重定向特点:

1. 地址栏发生变化
2. 重定向可以访问其他站点(服务器)的资源
3. 重定向是两次请求

转发的特点:

1. 转发地址栏路径不变
2. 转发只能访问当前服务器下的资源
3. 转发是一次请求

路径写法:

1. 路径分类
   1. 相对路径:通过相对路径不可以确定唯一资源,
   2. 绝对路径:通过绝对路劲确定唯一资源

# 2020/11/3

# 5.ServletContext对象

## 5.1 概述

代表整个web应用,可以和程序的容器(服务器)来通信

**获取对象**

```java
request.getServletContext();//通过request对象获取
this.getServletContext();//通过httpservlet对象获取
```

## 5.2 功能

1. 获取MIME类型:
   1. 在互联网通信过程中定义的一种文件数据类型
   2. 格式:大类型/小类型   text/html    image/jpeg
   3. 获取:String getMimeType(String file)
2. 域对象:共享数据--范围:所有用户所有请求的数据
   1. setAttribute(String name,Object value)
   2. getAttribute(String name)
   3. removeAttribute(String name)
3. 获取文件真实(服务器)路径
   1. 方法:String getRealPath(String Path)

# 6. 会话技术

一次会话:浏览器第一次给服务器资源发送请求,会话建立,直到有一方断开为止.一次会话包含多次请求和响应

功能:在一次会话的范围内的多次请求间,共享数据

## 6.1 Cookie--客户端会话技术

将数据保存到客户端的技术

使用步骤:

1. 创建cookie对象,绑定数据

   new Cookie(String name, String value)

2. 发送cookie对象

   response.addCookie(Cookie cookie)

3. 获取cookie,拿到数据

   Cookie [] request.getCookies()

 **原理:**基于响应头**set-cookie**和请求头**cookie**实现的

**细节**:

1. 一次是否可以发送多个cookie--可以.可创建多个Cookie对象,使用response调用多次addcookie方法发送cookie即可

2. 保存时间

   1. 默认情况:浏览器关闭后,cookie即销毁

   2. 其他情况:设置生命周期

      setMaxAge(int seconds)

      ​	正数:将数据写到内存文件中.持久化存储.存活时间

      ​	负数:默认值

      ​	零:删除Cookie信息

3. 能不能存中文--能存

4. 获取范围多大--多个项目不能共享,setPath可改变这种需在同一个服务器下

**特点**

1. 存储数据在客户端浏览器
2. 浏览器对于单个cookie的大小有限制,对于同一个域名下的总cookie'数量也有限制

**作用:**

1. 存储少量不太敏感的数据 

2. 在不登陆的情况下,完成服务器对用户身份的识别

## 6.2 Session--服务端会话技术

**概述**:再一次会话的多次请求间共享数据,将数据保存在服务器端的对象中.HttpSession

**快速入门**:

​		**HttpSession对象:**

​					getAttribute(String name):

​					setAttribute(String name,object value):

**原理**:Session是**依赖于**cookie的

第一次获取Session,没有cookie会在内存中创建以一个新的Session对象

**细节**:

1. 当客户端关闭后,服务器不关闭,两次获取session不是同一个

2. 客户端不关闭,服务器关闭后,两次获取的session不是同一个,但是确保数据不丢失

   session的钝化:将session对象系列化到硬盘上

   活化:将session文件转化为内存中的session对象

3. Session失效时间: 

   1. 服务器关闭
   2. Session对象调用invalidate()方法
   3. session默认失效时间30分钟

**特点**:

1. 用于存储一次会话的多次请求的数据,存在服务器端
2. session可以存储任意类型,任意大小的数据

区别:

​	session没有大小限制,存储在服务器端,数据安全

# 2020/11/4

# 7. JSP入门教学--java server pages java服务器端页面

## 7.1 简介

**概念**:既可以定义html页面标签,也可以定义java代码--简化书写;本质是servlet

**原理**:

1. 寻找index.jsp文件
2. 转换为java文件,编译文件,生成.class字节码文件提供访问

**脚本**:声明java代码的方式

1. <%代码%>

   定义的java代码,在service方法中,可以定义**service方法**一样的东西

2. <%! 代码%>

   定义的代码,在jsp转换后的Java类的**成员变量**位置

3. <%=代码%>

   定义的代码,**定义输出语句**

## 7.2 指令

**作用**:用于配置jsp页面,导入资源文件

**格式**:

<%@ 指令名称 属性名1=属性值1 属性名2=属性值2 ... %>

**指令分类**:

1. **page**:配置jsp页面的

   **contetTpye**:等同于response.setContentType()

   	1. 设置响应体的mime类型以及字符集
    	2. 设置当前jsp页面的编码(只能是高级的IDE才能生效)

   **import**:导包

   **errorPage**:发生异常后,会自动跳转到指定的页面 

   **isErrorPage**:标记当前页面是否是错误页面

   ​			true:可以使用内置对象exception

   ​			false:默认值,不可以使用内置对象exception

2. **include**:页面包含.导入页面的资源文件

   ​			<%@include file="top.jsp"%>

3. **taglib**:导入资源

   ​			<%@ taglib prefix="c" uri="http://java...."%>

   ​						prefix:前缀,自定义的

   ​					

## 7.3 注释

1. html注释

   ​		<!--   -->:只能注释html代码

2. jsp特有注释

   ​		<%--   --%>:可以注释所有

## 7.4 JSP内置对象:

在jsp页面中不需要获取和创建,可以直接使用的对象

一共有九个**内置对象**:

1. request         一次请求访问的多个资源(转发)

2. response      响应对象

3. out:可以将数据输出到页面上,字符输出流对象,和response.getWriter()类似

   ​		out()和response.writer()**区别**:

   ​				先找response缓冲区,再找out缓冲区

   ​				response.writer输出在out.writer之前

4. pageContext        当前页面共享数据---还可获取其他八个对象

5. session                  一次会话的多个请求间

6. page                      当前页面(servlet)的对象,this

7. config                    servlet的配置对象

8. application           所有用户间共享数据

9. exception             异常对象

# 8. MVC开发模式

**M**:Model,模型.进行业务逻辑操作.JavaBean.如:查询数据库,封装对象

**V**:View,视图.展示数据.JSP,如:展示数据

**C**:Controller,控制器.调用模型进行业务操作.Servlet.如:获取用户的输入,调用模型,将数据交给视图进行展示

# 9. EL 表达式

## 9.1 概念

Expression Language表达式语言

## 9.2 作用

替换和简化jsp页面中java代码的编写

## 9.3语法

${表达式}

## 9.4 注意

jsp默认支持EL表示式

isELIgnore或  \    -->忽略EL表达式

## 9.5 使用

1. 运算

   1. 算数运算符:+-*/%

   2. 比较运算符:><= ==

   3. 逻辑运算符:&& || !

   4. 空运算符:empty

      **功能**:用于判断字符串,集合,数组对象是否为null并且长度是否为0

      S{empty list}

2. 获取值

   1. el表达式只能从域对象中获取值

   2. 语法

      1. ${域名称.键名}:从指定域中获取指定键的值

         域名称:

         1. pageScope  -->pageContext
         2. requestScope  -->request
         3. sessionScope -->session
         4. applicationScope  -->application(ServletContext)

      2. ${键名}:表示依次从最小的域中查找是否有该键值对应的值,直到找到为止.

         ​	${name}

      3. 获取对象,List集合,Map集合的值

         1. 对象:${域名称.键名.属性名}

            ​		本质上会去调用对象的getter方法

         2. List集合:${域名陈.键名[索引]}

         3. Map集合:${域名称.键名.key名称}以及${域名称.键名[key名称]}

# 10. JSTL标签

## 10.1 概念

JavaServer Pages Tag Library  JSP标准标签库

**作用**:用于简化和替换jsp页面上的java代码

## 10.2 使用步骤

1. 导入jstl相关jar包
2. 引入标签库:taglib指令:< %@taglib% >
3. 使用标签

## 10.3 常用的标签库

1. if                 相当于java代码的if语句
2. choose       相当于switch语句
3. foreach      相当于for语句

# 11.三层架构

## 11.1界面层web

用户看到的界面,可以通过界面上的组件和服务器进行交互

## 11.2 业务逻辑层service

处理业务逻辑的

## 11.3 数据访问层dao

操作数据存储文件

# 12. 过滤器 Filter

当访问服务器资源时,过滤器可以将请求拦截下来,完成一些特殊的功能

**作用**:

1. 一般完成通用的操作.如:登录验证

## 12.1 步骤

1. 定义一个类,实现接口Filter

2. 复写方法

3. 配置拦截路径

   1. web.xml

      ```java
      <filter>
          <filter-name>demo1</filter-name>
          <filter-class>web.Fliter.FilterDemo1</filter-class>
      </filter>
          <filter-mapping>
              <filter-name>demo1</filter-name>
              <url-pattern>/*</url-pattern>
          </filter-mapping>
      ```

   2. 注解配置

## 12.2 细节

1. 生命周期

   ​	init()---只执行依次

   ​	destroy()--在服务器关闭后,Filter对象被销毁.正常关闭会执行destory()

2. 配置详解

   1. 拦截路径

      1. 具体资源路径:/index.jsp  只有访问index.jsp时,过滤器才会被执行
      2. 目录拦截:/usr/*    访问/user下的所有资源,过滤器才会被执行
      3. 后缀名拦截:*.jsp   访问所有后缀名为jsp资源时,过滤器才会被执行
      4. 拦截所有资源:/*

   2. 拦截方式:资源被访问的方式

      1. 注解配置

         1. 设置dispatcherTypes属性
            1. request:默认值.浏览器直接请求资源
            2. forward:转发访问资源
            3. include:包含访问资源
            4. async:异步访问资源
            5. error:错误跳转资源

      2. web.xml配置

         ​			设置<dispatcher> </dispatcher>标签即可 

      3. 过滤器链(配置多个过滤器)

         1. 执行顺序:1->2->2->1(过滤器1和过滤器2)

         2. 顺序比较:

            注解配置:按照类名的字符串比较规则,值小的先执行.

            web.xml:谁定义在上面谁先执行.

