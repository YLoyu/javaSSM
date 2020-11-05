# javaSSM
# SSM框架简介

SSM框架，是Spring+Spring MVC + MyBatis的缩写，这个是继SSH之后，目前比较主流的JavaEE 企业级框架，适用于搭建各种大型企业级应用系统。



## 1.Spring简介

1.  Spring是一个开源框架
2.  Spring为简化企业级开发而生，使用Spring,javaBean就可以实现很多以前要靠EJB才能实现的功能。同样的功能，在EJB中要通过繁琐的配置和复杂的代码才能够实现，而在Spring中却非常的优雅和简介
3.  Spring是一个IOC（DI）和AOP容器框架。
4.  Spring的优良特性：
    - 非侵入式：基于Spring开发的应用中的对象可以不依赖于Spring的API。
    - 依赖注入：DI——Dependency Injection,反转控制（IOC）最经典的实现。
    - 面向切面编程：Aspect Oriented Programming----AOP
    - 容器：Spring是一个容器，因为它包含并且管理应用对象的生命周期
    - 组件化：

## 2.Spring MVC简介

​    Spring MVC属于Spring Framework的后续产品，已经融合在Spring Web Flow里面，它原生支持的Spring特性，让开发变得非常简单规范。Spring MVC 分离了控制器、模型对象、分派器以及处理程序对象的角色，这种分离让它们更容易进行定制。

## 3.MyBatis简介

​    MyBatis本是apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。MyBatis是一个基于Java的持久层框架。iBATIS提供的持久层框架包括SQL Maps和Data Access Objects（DAO）MyBatis消除了几乎所有的JDBC代码和参数的手工设置以及结果集的检索。MyBatis使用简单的XML或注解用于配置和原始映射，将接口和Java的POJOs（Plain
Old Java Objects，普通的 Java对象）映射成数据库中的记录。可以这么理解，MyBatis是一个用来帮你管理数据增删改查的框架。
