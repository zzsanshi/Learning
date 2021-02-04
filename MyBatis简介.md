视频地址:[MyBatis](https://www.bilibili.com/video/BV1Bg4y1q7q2?p=147)

# 2020/11/20

# 1. Mybatis简介

## 1.1 原始JDBC操作分析

![image-20201120195825506](E:\学习笔记\Learning\图片\image-20201120195825506.png)

## 1.2 什么是MyBatis

![image-20201120201245549](E:\学习笔记\Learning\图片\image-20201120201245549.png)

# 2020/11/23

## 1.3 快速入门

**开发步骤**:

![image-20201120202019542](E:\学习笔记\Learning\图片\image-20201120202019542.png)

## 1.4 MyBatis映射文件概述

![image-20201123191930810](E:\学习笔记\Learning\图片\image-20201123191930810.png)

## 1.5 增删查改操作

+ 增

  ![image-20201123194727169](E:\学习笔记\Learning\图片\image-20201123194727169.png)

+ 删

  ![image-20201123200324121](E:\学习笔记\Learning\图片\image-20201123200324121.png)

+ 查

+ 改

  ![image-20201123195403634](E:\学习笔记\Learning\图片\image-20201123195403634.png)

## 1.6 配置文件概述

1. environments标签

   ![image-20201123200859336](E:\学习笔记\Learning\图片\image-20201123200859336.png)

![image-20201123201111216](E:\学习笔记\Learning\图片\image-20201123201111216.png)



2. Mapper标签

   ![image-20201123202002749](E:\学习笔记\Learning\图片\image-20201123202002749.png)



3. properties标签

   ![image-20201123202346236](E:\学习笔记\Learning\图片\image-20201123202346236.png)



4. type.Aliases标签

   ![image-20201123202710713](E:\学习笔记\Learning\图片\image-20201123202710713.png)

## 1.7 相应API

1. SqlSession工厂构建器SqlSessionFactoryBuilder

   ![image-20201123203437136](E:\学习笔记\Learning\图片\image-20201123203437136.png)

2. SqlSession工厂对象SqlSessionFactoryFactory

   ![image-20201123203641440](E:\学习笔记\Learning\图片\image-20201123203641440.png)

3. SqlSession会话对象

   ![image-20201123204005658](E:\学习笔记\Learning\图片\image-20201123204005658.png)

# 2020/11/24

# 2. MyBatis的Dao层实现

## 2.1传统方式--手动实现

​		定义接口层

```java
        InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
        List<User> userList = sqlSession.selectList("userMapper.findAll");
        return userList;
```



## 2.2 代理开发方式

### 1.代理开发方式介绍

![image-20201124103228101](E:\学习笔记\Learning\图片\image-20201124103228101.png)

### 2.编写Mapper接口

![image-20201124103837872](E:\学习笔记\Learning\图片\image-20201124103837872.png)

```java
		InputStream resourceAsStream = Resources.getResourceAsStream("sqlMapConfig.xml");
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(resourceAsStream);
        SqlSession sqlSession = sqlSessionFactory.openSession();
   **** UserMapper mapper = sqlSession.getMapper(UserMapper.class);****
        List<User> all = mapper.findAll();
```

# 3.MyBatis映射文件深入

## 3.1 动态sql语句

### 1. < if >

![image-20201124190029939](E:\学习笔记\Learning\图片\image-20201124190029939.png)

### 2.< foreach >

![image-20201124191051395](E:\学习笔记\Learning\图片\image-20201124191051395.png)

## 3.2 Sql片段的抽取

![image-20201124191830667](E:\学习笔记\Learning\图片\image-20201124191830667.png)

# 4.核心配置文件深入

## 4 .1 typeHandlers标签

![image-20201124192411188](E:\学习笔记\Learning\图片\image-20201124192411188.png)

![image-20201124192704490](E:\学习笔记\Learning\图片\image-20201124192704490.png)

## 4.2 plugins标签

![image-20201124200057850](E:\学习笔记\Learning\图片\image-20201124200057850.png)

# 5.多表操作

1VS1,1VSn,nVSn

## 5.1 一对一查询

### 

![image-20201124203048557](E:\学习笔记\Learning\图片\image-20201124203048557.png)

## 5.2 一对多查询

![image-20201124210209943](E:\学习笔记\Learning\图片\image-20201124210209943.png)

## 5.3 多对多查询

![image-20201124210152308](E:\学习笔记\Learning\图片\image-20201124210152308.png)

## 5.4 总结

![image-20201124211227111](E:\学习笔记\Learning\图片\image-20201124211227111.png)

# 6.注解开发

## 1.1 Mybatis的常用注解开发

![image-20201125100729768](E:\学习笔记\Learning\图片\image-20201125100729768.png)

## 1.2 注解实现复杂映射开发

![image-20201125100816062](E:\学习笔记\Learning\图片\image-20201125100816062.png)

![image-20201125100938147](E:\学习笔记\Learning\图片\image-20201125100938147.png)