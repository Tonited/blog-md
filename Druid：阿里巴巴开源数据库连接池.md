---
titile: Druid：阿里巴巴开源数据库连接池
author: Tonited
date: 2020.02.21
keywords: Druid：阿里巴巴开源数据库连接池
img: https://img-blog.csdnimg.cn/20200221144120996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70
categories: 框架
---

## 概述
`Druid`，俗称德鲁伊，是阿里巴巴开源平台上的一个项目，整个项目包括数据库连接池、插件框架、SQL解析器组成，能够提供强大的监控和扩展功能，自带监控界面，可以方便监控数据

[点击查看官方中文文档](https://github.com/alibaba/druid/wiki/%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)

## 各种连接池性能对比[^1]

-   Druid 是性能最好的数据库连接池，tomcat-jdbc 和 druid 性能接近。
-  proxool 在激烈并发时会抛异常，完全不靠谱。
- c3p0 和 proxool 都相当慢，慢到影响 sql 执行效率的地步。
- bonecp 性能并不优越，采用 LinkedTransferQueue 并没有能够获得性能提升。
- 除了 bonecp，其他的在 JDK 7 上跑得比 JDK 6 上快
- jboss-datasource 虽然稳定，但是性能很糟糕

## Spring整合Druid（Mysql为例）
### POM
```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.6</version>
</dependency>
<!-- Mysql例子 -->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.46</version>
</dependency>
```
### 数据库连接配置
配置数据库文件`jdbc.properties`
```properties
# JDBC
# MySQL 8.x: com.mysql.cj.jdbc.Driver
jdbc.driverClass=com.mysql.jdbc.Driver
jdbc.connectionURL=jdbc:mysql://IP地址:3306/项目名称?useUnicode=true&characterEncoding=utf-8&useSSL=false
jdbc.username=用户名
jdbc.password=密码

# JDBC Pool
jdbc.pool.init=1
jdbc.pool.minIdle=3
jdbc.pool.maxActive=20
```
[^1]:数据来源：https://www.funtl.com/zh/mybatis/Druid-%E7%AE%80%E4%BB%8B.html#%E6%A6%82%E8%BF%B0

### Spring上下文集成
创建文件`spring-context-druid.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 加载配置属性文件 -->
    <context:property-placeholder ignore-unresolvable="true" location="classpath:jdbc.properties"/>

    <!-- 数据源配置, 使用 Druid 数据库连接池 -->
    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" init-method="init" destroy-method="close">
        <!-- 数据源驱动类可不写，Druid默认会自动根据URL识别DriverClass -->
        <property name="driverClassName" value="${jdbc.driverClass}"/>

        <!-- 基本属性 url、user、password -->
        <property name="url" value="${jdbc.connectionURL}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>

        <!-- 配置初始化大小、最小、最大 -->
        <property name="initialSize" value="${jdbc.pool.init}"/>
        <property name="minIdle" value="${jdbc.pool.minIdle}"/>
        <property name="maxActive" value="${jdbc.pool.maxActive}"/>

        <!-- 配置获取连接等待超时的时间 -->
        <property name="maxWait" value="60000"/>

        <!-- 配置间隔多久才进行一次检测，检测需要关闭的空闲连接，单位是毫秒 -->
        <property name="timeBetweenEvictionRunsMillis" value="60000"/>

        <!-- 配置一个连接在池中最小生存的时间，单位是毫秒 -->
        <property name="minEvictableIdleTimeMillis" value="300000"/>

        <!-- 配置监控统计拦截的filters -->
        <property name="filters" value="stat"/>
    </bean>
</beans>
```

## 配置Durid监控中心
`Durid`自带监控中心，可以监控数据，只要`web.xml`文件中配置一个Servlet就可进入
```xml
<servlet>
    <servlet-name>DruidStatView</servlet-name>
    <servlet-class>com.alibaba.druid.support.http.StatViewServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>DruidStatView</servlet-name>
    <url-pattern>/druid/*</url-pattern>
</servlet-mapping>
```
![示意图](https://img-blog.csdnimg.cn/20200221144120996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)