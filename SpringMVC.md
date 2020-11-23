视频地址: [MVC](https://www.bilibili.com/video/BV1Bg4y1q7q2?p=109)

# 2020/9/28

# 1.SpringMVC简介

## 1.1概述

SpringMVC是一种基于Java的实现MVC设计模型的请求驱动类型的轻量级Web框架,属于SpringFrameWork的后续产品,已经融合在Spring Web Flow中.

通过一套注解,让一个类成为处理请求的控制器

开发步骤:

1. 导入SprignMVC包
2. 配置servlet
3. 编写Controller
4. 将Controller使用注解配置到Spring容器中
5. 配置Spring-mvc.xml文件(配置组件扫描)
6. 执行测试

# 2020/10/10

## 1.2组件解析

执行流程:

![image-20201010194526338](E:\学习笔记\Learning\图片\image-20201010194526338.png)

1. 用户发送请求至前端控制器DispatcherServlet
2. DispatcherServlet收到请求调用HandlerMapping处理器映射器
3. 处理器映射器找到具体的处理器(可以根据xml配置,注解进行查找),生成处理器对象及处理器拦截器(如果有则生成)一并返回给DispatcherServlet
4. DispatcherServlet调用HandlerAdapter处理器调用器
5. HandlerAdapter经过适配调用具体的处理器(Controller,也叫后端控制器)
6. Controller执行完成返回ModelAndView
7. HandlerAdapter将controller执行结果ModelAndView返回给DispatcherServlet
8. DispatcherServlet将ModelAndView传给ViewReslover视图解析器
9. ViewReslover解析后返回具体View
10. DispatcherServlet根据View进行渲染视图(即将模型数据填充至视图中).DispatcherServlet响应用户

## 1.3 SpringMVC注解解析

+ @RequestMapping--请求映射

  **作用**:用于建立请求URL和处理请求方法之间的对应关系

  **位置**:1. 类上:请求URL的第一级访问目录.此处不写的话,就相当于应用的根目录

  ​         2.方法上:请求URL的第二级访问目录,于类上的是使用@ReqquestMapping标注的一级目录一起访问虚拟路径

+ 属性
  + value:用于指定请求的URL.它和属性的作用是一样的
  + method:用于指定请求的方式
  + params:用于指定限制请求参数的条件.它支持简单的表达式.要求参数的key和value必须和配置的一模一样
  + params={"accountName"},表示请求参数必须有accountName
  + params={"money!100"},表示请求参数中money不能是100

**Xml简单配置**

![image-20201010205456166](E:\学习笔记\Learning\图片\image-20201010205456166.png)

# 2020/10/11

# 2 数据响应

## 2.1 数据响应方式

1. 页面跳转
   + 直接返回字符串
   + 通过ModelAndView对象返回
2. 回写数据
   + 直接返回字符串
   + 返回对象或集合

## 2.2 页面跳转

1. **返回字符串形式**

   直接返回字符串:此种方式会将返回的字符串与视图解析器的前后缀拼接后跳转.

   ![image-20201011171536314](E:\学习笔记\Learning\图片\image-20201011171536314.png)

2. **返回ModelAndView对象**

   **Model**:模型 作用封装数据

   **View**:视图 作用展示数据

# 2020/11/12

## 2.3 回写数据

1. **直接返回字符串**

   @respionseBody--不进行跳转只进行数据回写

2. **返回对象集合**

![image-20201112165134223](E:\学习笔记\Learning\图片\image-20201112165134223.png)

# 3. SpringMVC获得请求数据

## 3.1 获得请求参数

+ 基本参数类型
  
  + Controller中的业务方法的参数名称要与请求参数的name一致,参数值会自动映射匹配.
+ POJO类型参数
  
  + Controller中的业务方法的POJO参数的属性名于请求参数的name一致,参数值会自动映射匹配
+ 数组类型参数
  
  + 数组名称与请求参数的name一致,参数值会自动映射匹配
+ 集合类型参数
  + 获得集合参数时,要将集合参数包装到一个POJO中才可以.
  + @RequestBody
  
    

## 3.2 请求数据乱码问题

当POST请求时,数据会出现乱码,可以设置过滤器进行编码过滤

![image-20201112191717008](E:\学习笔记\Learning\图片\image-20201112191717008.png)

## 3.3 参数绑定注解@RequestParam

当请求的参数名称与Controller的业务方法参数不一致时,就需要通过@RequestParam注解显示的绑定.

![image-20201112192255121](E:\学习笔记\Learning\图片\image-20201112192255121.png)

**参数**:

+ value:与请求参数名称
+ required:此在指定的请求参数是否必须包括,默认是true,提交时如果没有此参数则报错
+ defaultValue:当没有指定请求参数时,则使用指定的默认值赋值

## 3.4 获得Restful风格的参数

Restful是一种软件架构风格,设计风格,而不是标准,只是提供了一组设计原则和约束条件.主要用于客户端和服务器交互类的软件,基于这个风格设计的软件可以更简洁,更有层次,更易于实现缓存机制.

Restful风格的请求是使用"URL+请求方式"表示依次请求目的的,HTTP协议里面四个表示操作方式的动词如下:

+ GET:用于获取资源
+ POST:用于新建资源
+ PUT:用于更新资源
+ DELETE:用于删除资源

## 3.5 自定义类型转换器

SpringMVC默认已经提供了一些常用的类型转换器

**开发步骤**:

1. 定义转换器类实现Converter接口
2. 在配置文件中声明转换器
3. 在<annotation-driven>中引用转换器

## 3.6 获得请求头

1. @RequestHeader

   使用@RequestHeader可以获得请求头信息,相当于web阶段学习的request.getHeader(name)

   + value:请求头的名称
   + required:是否必须携带此请求头

2. @CookieValue

   使用@CookieValue可以获得指定Cookie的值

   + value:指定cookie的名称
   + required:是否必须携带此cookie

## 3.7 文件上传

1. **文件上传三要素**
   1. 表单项type="file"
   2. 表单的提交方式是post
   3. 表单的enctype属性是多部份表单格式,及enctype="multipart/form-data"

![image-20201118104608326](E:\学习笔记\Learning\图片\image-20201118104608326.png)

2. **上传原理**

   + 当form表单修改为多部份表单时,request.getparameter()将失效

   + enctype="application/x-www-form-urlencoded"时,form表单的正文内容格式是:

     key=value&key=value

   + 当form表单的enctype取值为Mutil/form-data时,请求正文内容就变成多部份形式:

     ![image-20201118105255662](E:\学习笔记\Learning\图片\image-20201118105255662.png)

3. 上传步骤

   1. 导入fileupload和io坐标

   2. 配置文件上传解析器

      ![image-20201118105856677](E:\学习笔记\Learning\图片\image-20201118105856677.png)

   3. 编写文件上传代码

# 2020/11/18

# 4. SpringMVC拦截器

## 4.1 拦截器(interceptor)作用

​	类似于Servlet开发中的过滤器Filter,用于对处理器进行预处理和后处理

​	将拦截器按照一定的顺序连结成一条链,这条链成为**拦截器链**.在访问被拦截的方法或字段时,拦截器链中的拦截器就会按其之前定义的顺序被调用.拦截器也是AOP思想的具体实现.

## 4.2 拦截器与过滤器的区别

![image-20201118203340509](E:\学习笔记\Learning\图片\image-20201118203340509.png)

## 4.3 快速入门

1.  创建拦截器类实现HandlerInterceptoru接口
2. 创建拦截器
3. 测试拦截器的拦截效果

## 4.4 总结

![image-20201118205638048](E:\学习笔记\Learning\图片\image-20201118205638048.png)

# 5. SpringMVC异常处理

## 5.1 异常处理思路

​	系统中异常包括两类:预期异常和运行时异常,前者通过捕获异常从而获取异常信息,后者主要通过规范代码开发,测试等手段减少运行时异常的发生.

​	由SpringMVC前端控制器交由异常处理器处理.

## 5.2 异常处理两种方式

1. 使用SpringMVC提供的简单异常处理器SimpleMappingExceptionResolver

   ![image-20201118212527215](E:\学习笔记\Learning\图片\image-20201118212527215.png)

2. 使用Spring的异常处理接口HandlerExceptionResolver**自定义**自己的异常处理器

