地址:[maven](https://www.bilibili.com/video/BV1Bg4y1q7q2?p=201)

# 2020/12/14

# 1. maven介绍

![image-20201214155302049](E:\学习笔记\Learning\图片\image-20201214155302049.png)

## 1.1 仓库类型

![image-20201214155445430](E:\学习笔记\Learning\图片\image-20201214155445430.png)

## 1.2 常用命令

![image-20201214155707967](E:\学习笔记\Learning\图片\image-20201214155707967.png)

## 1.3 坐标书写规范

![image-20201214155737249](E:\学习笔记\Learning\图片\image-20201214155737249.png)

## 1.4 依赖范围

![image-20201214155802279](E:\学习笔记\Learning\图片\image-20201214155802279.png)

# 2.maven依赖传递

## 2.1 什么是依赖传递

![image-20201214160334062](E:\学习笔记\Learning\图片\image-20201214160334062.png)

## 2.2 依赖冲突

![image-20201214160438294](E:\学习笔记\Learning\图片\image-20201214160438294.png)

## 2.3 解决冲突

![image-20201214160833990](E:\学习笔记\Learning\图片\image-20201214160833990.png)

## 2.4 第一声明者优先原则

![image-20201214160907037](E:\学习笔记\Learning\图片\image-20201214160907037.png)

## 2.5路径近者优先原则

![image-20201214161054455](E:\学习笔记\Learning\图片\image-20201214161054455.png)

## 2.6 排除依赖

可以使用exclusions标签将传递过来的依赖排除出去

​    ![image-20201214161422806](E:\学习笔记\Learning\图片\image-20201214161422806.png)

## 2.7 版本锁定

![image-20201214161607910](E:\学习笔记\Learning\图片\image-20201214161607910.png)

1. 在dependencyManagement标签中锁定依赖的版本

![image-20201214161753080](E:\学习笔记\Learning\图片\image-20201214161753080.png)

2. 导入jar包

![image-20201214161820536](E:\学习笔记\Learning\图片\image-20201214161820536.png)

# 3.分模块构建maven工程

![image-20201214170432386](E:\学习笔记\Learning\图片\image-20201214170432386.png)