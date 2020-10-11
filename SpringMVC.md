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

