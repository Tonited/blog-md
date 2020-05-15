---
titile: Hikari连接池
author: Tonited
date: 2020.03.03
keywords: Hikari连接池
img: https://img-blog.csdnimg.cn/20200303173852646.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70
categories: 框架
---

## Hikari简介

后期之秀`Hikari`，其实是日语发音，名字翻译成“光”，号称目前最快的连接池，是在`BoneCP`基础上进行的修改

**目前已被Spring官方推荐并整合**

官网：https://github.com/brettwooldridge/HikariCP


## 使用
### POM
```POM
<dependency>
    <groupId>com.zaxxer</groupId>
    <artifactId>HikariCP</artifactId>
    <version>${hikaricp.version}</version>
</dependency>
```
### application.yml
```yml
spring:
  datasource:
    type: com.zaxxer.hikari.HikariDataSource
    username: 用户名
    password: 密码
    url: jdbc:数据库://IP地址:端口号/数据库名?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=UTC
    hikari:
      minimum-idle: 5
      idle-timeout: 600000
      maximum-pool-size: 10
      auto-commit: true
      pool-name: MyHikariCP
      max-lifetime: 1800000
      connection-timeout: 30000
      connection-test-query: SELECT 1
```

## 性能对比
它和前面提到过的连接池另一个连接池`Druid`相比，`Druid`注重监控，`HikariCP`注重速度
### 速度
![示意图](https://img-blog.csdnimg.cn/20200303173433245.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
### 稳定性
![示意图](https://img-blog.csdnimg.cn/20200303173852646.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
### 可靠性
（所有CP都配置了跟connectionTimeout类似的参数为5秒钟）
- HikariCP：等待5秒钟后，如果连接还是没有恢复，则抛出一个SQLExceptions 异常；后续的getConnection()也是一样处理；
- C3P0：完全没有反应，没有提示，也不会在“CheckoutTimeout”配置的时长超时后有任何通知给调用者；然后等待2分钟后终于醒来了，返回一个error；
- Tomcat：返回一个connection，然后……调用者如果利用这个无效的connection执行SQL语句……结果可想而知；大约55秒之后终于醒来了，这时候的getConnection()终于可以返回一个error，但没有等待参数配置的5秒钟，而是立即返回error；
- BoneCP：跟Tomcat的处理方法一样；也是大约55秒之后才醒来，有了正常的反应，并且终于会等待5秒钟之后返回error了；