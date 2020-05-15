---
titile: Spring入门笔记
author: Tonited
date: 2019.08.15
keywords: Spring入门笔记
img: https://www.w3cschool.cn/attachments/image/20170802/1501662448352296.png
categories: 框架
---



## 前言

picture-resources: imooc

IDE: IDEA2019.2

java-version: 11.0.3

date: 2019.08.15

这篇笔记是本人零基础看Spring入门网课时整理的笔记，同时加入了一些自己的理解，方便大家学习和使用，内容可能存在错误或者在理解上有一定问题，如果有任何问题、意见、见解或关于侵权请联系我，QQ710260712，申请理由请写“Spring入门笔记勘误”。

 ——Tonited

## 一. 概述

### 1.Spring的概述

#### （1）什么是Spring

- Spring是一个开源框架
- Spring为简化企业级应用开发而生，使用Spring可以使简单的JavaBean实现以前只有EJB才能实现的功能
- Spring是JavaSE/EE的一站式框架
- 方便解耦，简化开发
  - Spring就是一个大工厂，可以将所有对象创建和依赖关系维护交给Spring管理
- AOP编程的支持
  - Spring提供面向切面编程，可以方便的实现对程序进行权限拦截、运行监控等功能
- 声明事务的支持
  - 只通过配置就可以完成对事务的管理，而无需手动编程

#### （2）Spring的优点

- 方便程序的测试
  - Spring对Junit4支持，可以通过注解方便的测试Spring程序
- 方便集成各种优秀框架
  - Spring不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如：Struts、Hibernate、MyBatis等）的直接支持
- 降低JavaEE API的使用难度
  - Spring对JavaEE开发中非常难用的一些API（JDBC、JavaMail、远程调用等），都提供了封装，使这些API应用程度大大降低

### 2. Spring IOC快速入门案例

#### 1）配置流程

##### 一般配置

1. 下载Spring最新开发包
2. 复制Spring开发jar到工程
3. 理解IOC控制反转和DI依赖注入
4. 编写Spring核心配置文件
5. 在程序中读取Spring配置文件，通过Spring框架获得Bean，完成相应操作
6. 官方下载Spring 4.X 最新开发版本
7. Spring 4.2 版本目录结构

![1565947086285](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565947086285.png)

8. 导入Spring核心开发包到创建工程

![1565947143247](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565947143247.png)

##### Maven pom依赖

- 使用maven创建spring项目时需要在pom中引入以上依赖
- 还要引入单元测试依赖

![单元测试依赖](https://gitee.com//tonited/Spring-Notes/raw/master/assets/%E5%8D%95%E5%85%83%E6%B5%8B%E8%AF%95%E4%BE%9D%E8%B5%96.png)

- **<font color="red">本笔记所记录的示例均为使用maven方式创建和管理spring项目，以下记录均为maven记录</font>**

#### 2）HelloTest类中使用UserServive类对象

##### · 传统方式

![1565947678639](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565947678639.png)

##### · Spring方式实现

![1565947765945](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565947765945.png)

##### · 读取磁盘中的配置文件

#### 3）IOC（Inverse of Control 控制反转）的概念

- 就是将原本在程序中手动创建UserService对象控制权，交由Spring框架管理
- 简单说，就是创建UserService对象控制权被反转到了Spring框架

### 3. 提问

- 关于IOC说法
  - 控制反转
- SpringIOC底层用那种设计模式完成的
  - 工厂模式
  - 反射模式
- Spring中依赖注入的目的
  - 在代码之外管理组件之间的依赖关系

## 二. Spring Bean管理

### 1. Spring的工厂类

![1565948243266](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565948243266.png)

- 工厂类

  ![1565948894592](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565948894592.png)



- 传统方式工厂类：BeanFactory

  ![1565948950620](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565948950620.png)

### 2. Spring的Bean管理（XML方式）

#### 1） 三种实例化Bean的方式

##### · ApplicationContext——XML配置

![1565949609472](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565949609472.png)

##### · 使用类构造器实例化（默认无参数）

ApplicationContext在创造时调用类的构造函数

![1565949489230](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565949489230.png)

##### · 使用静态工厂方法实例化（简单工厂模式）

创建工厂类，ApplicationContext在创造时通过工厂生成Bean

**这个Bean由静态方法创建，只会创造一个Bean，获取时获取的是同一个Bean**

![1565949695517](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565949695517.png)

##### · 使用实例工厂方法实例化（工厂方法模式）

与上图简单工厂模式相同，区别是工厂方法创建的Bean不是单例

![1565953301428](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565953301428.png)

#### 2）Bean的配置

![1565953718273](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565953718273.png)

- id和name
  - 一般情况下，装配一个Bean时，通过指定一个id属性作为Bean的名称
  - id属性在IOC容器中必须是唯一的，name可以不唯一
  - （id和name的作用相同）
  - 如果Bean的名称中含有特殊字符，就需要使用name属性
- calss
  - class用于设置一个类的完全路径名称，主要作用时IOC容器生成类的实例

#### 3） Bean的作用域

|   类别    |                             说明                             |
| :-------: | :----------------------------------------------------------: |
| singleton | 在SpringIOC容器中仅存在一个Bean实例，Bean一单实例的方式存在（单例模式） |
| prototype |   每次调用getBean()时都会返回一个新的实例（既非单例模式）    |
|  request  | 每次HTTP请求都会创建一个新的Bean，该作用域仅适于WebApplicationContext环境 |
|  session  | 同一个HTTP Session共享一个Bean，不同的HTTP Session使用不同的Bean。该作用域仅适于WebApplicationContext环境 |

#### 4）Spring中Bean的生命周期

##### （1. Bean创造拆卸生命周期

Spring初始化Bean或销毁Bean时，有时需要作一些处理工作，因此spring可以在创建和拆卸Bean的时候调用Bean的两个生命周期方法

![1565957255381](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565957255381.png)

<font color="red">注意：destory只有在scope=singleton时有效</font>

##### （2.  Bean完整11个生命周期

顺序：

1. instantiate （bean对象实例化

   - **调用构造函数**

     

   ![1565957786821](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565957786821.png)

   ![1565958219380](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565958219380.png)

   

2. populate properties （封装属性

   - **根据AccountContext.xml配置文件封装属性**

     

   ![1565958185028](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565958185028.png)

   ![1565957999293](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565957999293.png)

   ![1565958233217](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565958233217.png)

   

3. 如果Bean实现BeanNameAware接口，执行接口的**setBeanName**方法

   - **告诉被创造的Bean自己在ApplicationContext.xml中的名字（id/name）**

     

   ![1565958367139](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565958367139.png)

   ![1565958063051](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565958063051.png)

   ![1565958449383](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565958449383.png)

   

4. 如果Bean实现BeanFactoryAware或者ApplicationContextAware，执行 设置工厂**setBeanFactory**方法 或者上 下文对象**setApplicationContext**方法

   - **使被创建的Bean了解工厂信息**

   

   ![1565959056700](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565959056700.png)

   ![1565959071715](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565959071715.png)

   

5. 如果**存在类**实现 BeanPostProcessor 接口<font color="red">（Bean后处理器）</font>，执行**postProcessBeforeInitialization **方法

   - **实现Bean后处理器接口的类并不是被创建的Bean**

   - **该生命周期会在<font color="red">每个Bean实例化都会被调用</font>**

     

   ![1565959721679](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565959721679.png)

   ![1565959878144](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565959878144.png)

   

6. 如果Bean实现 InitalizingBean 接口，执行 **afterPropertiesSet**方法

   

   ![1565960127024](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565960127024.png)

   ![1565960206398](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565960206398.png)

   

7. 调用 <bean init-method="init"> 指定初始化方法 **init** 

   

   ![1565961292569](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565961292569.png)

   ![1565961333428](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565961333428.png)

   ![1565961368912](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565961368912.png)

   

8. 如果存在类实现 BeanPostProcessor接口 <font color="red">（Bean后处理器）</font>，执行 **postProcessAfterInitialization** 方法

   - 与第5步相同，为Bean后处理器

   - 实现Bean后处理器接口的类并不是被创建的Bean

   - 该生命周期会在<font color="red">每个Bean实例化都会被调用</font>

     

     ![1565961394714](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565961394714.png)

     ![1566007903579](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566007903579.png)

     

9. **执行业务逻辑**

   

   ![1565961840790](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565961840790.png)

   ![1565961795496](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565961795496.png)

   ![1565961864209](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565961864209.png)



10. 如果 Bean 实现 DisposableBean 执行 destroy

    - 关闭才会执行

    

    ![1565962296381](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565962296381.png)

    

11. 调用 <bean destory-method="customerDestory"> 指定的销毁方法 **customerDestory**

    

    ![1565962006316](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565962006316.png)

    ![1565962338706](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1565962338706.png)

##### （3. beanpostprocessor的作用（Java动态代理示例）

- 可以在创造Bean时产生代理

- 可以对Bean的方法增强

  

  - 下面的例子使用了Java的动态代理

    ![1566048608577](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566048608577.png)

### 3. Spring的几种属性注入

- 对于类成员变量，注入方式有三种
  - **构造函数注入**
  - **属性setter方法注入**
  - 接口注入
- **Spring支持前两种**

#### 1）Spring属性注入-构造方法注入

- 通过构造方法注入Bean的属性值或依赖的对象，它保证了Bean实例在实例化之后就可以使用

- 构造器注入在<constructor-arg>元素里生命的属性

  

  ![1566038373815](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566038373815.png)

  ![1566038330789](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566038330789.png)

  

#### 2）Spring属性注入-set方法注入

- 在Spring配置文件ApplicationContext.xml中，通过<propert>设置注入的属性

  

  ![1566038655792](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566038655792.png)

  ![1566038677234](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566038677234.png)



#### 3）Spring属性注入-p名称空间

- 为了简化XML文件配置，Spring从2.5开始引入一个新的p名称空间

  - 要在<bean>中加入

    ```xml
    xmlns:p="http://www.springframework.org/schema/p"
    ```

- p:<属性名>="XXX"引入常量值

- p:<属性名>-ref="XXX"引用其他Bean对象

- **需要set**

  

  ![1566039292638](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566039292638.png)

  ![1566039310780](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566039310780.png)



#### 4）Spring属性注入-SpEL注入

- SpEL：spring expression language，spring表达式语言，对依赖注入进行简化
- **不需要set**
- 语法：#{表达式}
- <bean id="" value="#{表达式}">
  - #{}
  - #{ 'hello' } ：使用字符串
  - #{topicId3}：使用另一个bean
  - #{topic4.content.toUpperCase()}：使用指定名属性，并使用方法
  - {T(java.lang.Math).PI}：使用静态字段或方法

![1566040113107](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566040113107.png)



#### 5）Spring属性注入-复杂类型的属性注入

- 写在<bean></bean>之间

  - 数组类型的属性注入

    ![1566040386774](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566040386774.png)

    

  - List集合类型的属性注入

    ![1566040416008](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566040416008.png)

    

  - Set集合类型的属性注入

    ![1566040434186](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566040434186.png)

    

  - Map集合类型的属性注入

    ![1566040456028](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566040456028.png)

    

  - Properties类型的属性注入

    ![1566040506230](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566040506230.png)

    

### 4. Spring的Bean管理（注解方式）

#### 0）必要配置

- 使用注解时

  - <font color="red">想使用注解要引入依赖**spring-aop ** </font>

  - 替换ApplicationContext.xml头<bean>为

    ```xml
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
                               http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd">
    ```

  - **开启注解扫描**

    - 这将自动开启属性注入

    ```xml
    <context:component-scan base-package="要扫描的包名"/>
    ```

    

#### 1）使用注解定义Bean

- Spring2.5引入使用注解去定义Bean

  - @Component 描述Spring框架中的Bean

    ![1566042351014](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566042351014.png)

- 除了@Component外，Spring提供了3个功能基本和@Component等效的注解

  - @Repository 用于对DAO实现类进行标注
  - @Service 用于对Service实现类进行标注
  - @Controller 用于对Controller实现类进行标注
  - <font color="red">这三个注解是为了让标注类本身的用途清晰，Spring在后续会对其增强</font>

- @Value可以直接注入属性

  - 如果属性提供了get，@Value就要写在set上
  - 如果没提供get，@Value就要写在属性上

  ![1566042577114](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566042577114.png)



#### 2）Spring的属性注入-注解方式

- 使用@Autowired进行自动注入

- **@Autowired按照默认类型进行注入**

  - **如果存在两个Bean类型相同，则按照名称注入**

    - **<font color="red">注入bean类型和被注入bean类型必须都被Spring管理（上面被注解@Component或另外三个）</font>**（图中粉色框）
    - **<font color="red">按照名称注入指的是：被注入的变量的名称（图1红色框）和注入类的注解命名一致</font>**（图2红色框）

    ![1567073068797](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1567073068797.png)

    ![1567073274762](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1567073274762.png)

  

  **@Autowired注入针对成员变量或者set方法都可以**

  ​	

  

- 通过@Autowired的required属性，设置一定要找到匹配的Bean

  - @Qualifier指定注入Bean的名称，即实现了required属性

    ![1566043796181](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566043796181.png)

  - **使用@Qualifier 指向Bean名称时，Bean必须指定相同名称**

    ![1566043924048](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566043924048.png)

    

- Spring提供对 JSR-250中定义@Resource标准注解的支持

  - @Resource相当于@Autowried和@Qualifier的组合形式

  - <font color="red">想要使用@Resource要引入依赖</font>

    ```xml
    <dependency>    
        <groupId>javax.annotation</groupId>
        <artifactId>jsr250-api</artifactId>
        <version>1.0</version>
    </dependency>
    ```

    ![1566044706954](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566044706954.png)



#### 3) Spring的其他注解

- Spring初始化bean或销毁bean时，有时需要做一些处理工作，因此Spring可以在创建和拆卸bean的时候调用bean的两个生命周期方法

  ![1566045146960](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566045146960.png)

  ![1566045401127](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566045401127.png)

  

#### 4）Bean的作用范围

- @Scope注解用于指定Bean的作用范围

- 使用注解配置的Bean和<bean>配置的一样，默认作用范围都是singleton

  ![1566045425549](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566045425549.png)



### 5. 传统XML配置和注解配置的混合使用

- XML方式的优势

- 结构清晰，易于阅读

- 注解方式的优势

  - 开发敏捷，属性注入方便

- XML与注解的整合开发

  - 引入context命名空间：修改ApplicationContext.xml文件<beans>为

    ```xml
    <beans xmlns="http://www.springframework.org/schema/beans"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xmlns:context="http://www.springframework.org/schema/context"
           xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
                               http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd">
    ```

  - 开启属性注入

    - 在配置文件中添加

      ```xml
      <context:annotation-config/>
      ```

- 组合开发

  - ApplicationContext.xml中只需配置<bean>的 id 和 class ，不需要配置属性了

    

    ![1566047680902](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566047680902.png)

    

  - 属性通过注解注入，不需要set方法了

    

    ![1566047495583](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566047495583.png)

    

  - 因为在ApplicationContext.xml中已经配置了bean的id，所以不需要每个类上面用@Component等标注id了

    ![1566047918257](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566047918257.png)

![1566047854274](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566047854274.png)

### 6. 提问

- 在Spring中，加载类路径下的配置文件使用的是
  - ApplicationContext
- 在Spring中，已知Student类的定义，下方式属于无参构造器的方式来实例化Bean
  - <bean id="bean1" class="io.github.Tonited.Student"/>
- 在Spring中，Bean在配置时，初始化和销毁方法配置在 <bean ./> 中的标签名是
  - init-method
  - destory-method
- 在Spring中，假设某个bean要使用某种类型的资源，那么一般情况下，应该把资源的释放放到bean生命周期中的阶段为
  - 销毁阶段
- 关于p名称空间的说法
  - 形如 p:age="18" 引入常量值
  - 形如 p:test-ref="test" 引入Bean对象
- 以下SpEL表达式语法使用错误
  - 语法：${表达式}
- 在SpringBean注入中，如果需要对Map进行数据注入，需要用到以下标签
  - property
  - map
  - entry
- Spring2.5以后引入了注解定义Bean，那么定义Bean可以使用下列注解
  - @Repository
- 在Spring的Bean注入中，如果使用注解的方式对属性进行注入，则有以下说法
  - 类中如果有setter方法，那么注解需要加到setter方法上方



## 三. Spring AOP

### 1. AOP的概述

#### 0）纵向继承和AOP的横向抽取机制

假设现在我们有以下实现了一个名为UserDao接口的类，我们想在save()方法被调用前做权限校验，可以在类中写一个用于权限校验的方法checkPrivilege()，并在save()的逻辑执行前调用这个类的checkPrivilege()方法。

![1](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1.png)



如果现在我们有100实现了UserDao接口的类，想在每一个类中的某个方法被调用之前都进行一步权限调用，就需要给100个类中的每一个类都写一个checkPrivilege()方法。

这时你可能想到：要将权限校验的方法checkPrivilege()写在接口中，让每一个继承它的类都实现这一方法。![纵向继承](https://gitee.com//tonited/Spring-Notes/raw/master/assets/%E7%BA%B5%E5%90%91%E7%BB%A7%E6%89%BF.png)

这种做法就是所谓的**纵向继承**，”纵向“指的是从接口继承方法这种上下父子关系

可是你也能很容易的发现，这样写的代码量很大，最重要的是：会出现大量重复代码

为了解决这一问题，出现了AOP，关于AOP的具体概念稍后会具体讲解。



AOP采用的是**横向抽取机制**

- 所谓的横向抽取机制是指将可以复用的代码抽取出来以便复用
- AOP横向抽取机制采用的是代理机制，可以通过访问代理达到访问目标对象的目的，同时通过执行代理中的复用代码段解决代码服用问题

![AOP横向抽取机制](https://gitee.com//tonited/Spring-Notes/raw/master/assets/AOP%E6%A8%AA%E5%90%91%E6%8A%BD%E5%8F%96%E6%9C%BA%E5%88%B6.png)





#### 1）什么是AOP

- AOP Aspect Oriented Programing 面向切面编程
- AOP采取横向抽取机制，取代了传统纵向继承体系重复性代码（性能检测、事务管理、安全检查、缓存）
- Spring AOP底层使用纯Java实现，不需要专门的编译过程和类加载器，在运行期间通过代理方式向目标类织入增强代码



#### 2）AOP的相关术语

![AOP相关术语](https://gitee.com//tonited/Spring-Notes/raw/master/assets/AOP%E7%9B%B8%E5%85%B3%E6%9C%AF%E8%AF%AD.png)

- Joinpoint（连接点）：所谓连接点是指那些可以被拦截到的点。在Spring中，这些点指的是方法，**因为Spring只支持方法类型的连接点**
- Pointcut（切入点）：所谓切入点是指我们要对哪些Jointpoint进行拦截的定义
- Advice（通知/增强）：所谓通知是指拦截到Joinpoint之后所要做到的事情就是通知
  - 通知分为：前置通知，后置通知，异常通知，最终通知，环绕通知（切面要完成的功能）
- Introduction（引介）：引介是一种特殊的通知，在不修改代码的前提下，Introduction可以在运行期为类动态地添加一些方法或者Field
  - **Spring并不支持，可先不考虑**
- Targrt（目标对象）：代理的目标**对象**
- Weaving（织入）：是指把增强应用到目标对象来创建新的代理对象的过程
  - Spring采用动态代理织入
  - **AspectJ（一种织入工具，后面会讲到）采用编译器织入和类装载器织入的方式**
- Proxy（代理）：一个类被AOP织入增强后，就会产生一个结果代理类
- Aspect（切面）：切入点和通知（引介）的结合

### 2. AOP的底层实现

#### 1）JDK动态代理

##### （1 使用方法

- **JDK的动态代理只能对实现了接口的类进行代理**

- 首先我们要创建一个代理对象，用于稍后传给 InvocationHandler 的 invoke() 函数，创建对象使用 Proxy.newProxyInstant 函数，它需要三个参数

  - 被代理类的类加载器

  - 被代理类所实现的接口

  - 执行代理的类，这个类需要实现 InvocationHandler 接口的 invoke() 方法

    ![1566193384737](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566193384737.png)

    - 本例让此代理类实现了此接口，同时作为代理类和执行代理类，所以这时参数为 this

    ```java
    public Object createProxy(){
            Object proxy = Proxy.newProxyInstance(
                    userDao.getClass().getClassLoader(),
                    userDao.getClass().getInterfaces(),
                    this
            );
            return proxy;
        }
    ```

    

- InvocationHandler接口的 invoke(Object proxy, Method method, Object[] args) 

  - 它包含三个参数

    - Object proxy：代理对象，刚刚已创建过
    - Method method：被执行的方法
    - Object[] args：方法所带的参数

    ```java
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {}
    ```

  - 返回值需要返回“方法的执行：method.invoke()”， 它包含两个参数

    - 被代理的类实例

    - - invoke参数列表中的args（args内部是method方法执行时的参数）

        ```java
        return method.invoke(userDao,args);
        
        ```



##### （2. 一个JDK动态代理完整代码示例

- 代理类

  ```java
  public class MyJdkProxy implements InvocationHandler{
      
      // UserDao是接口
      private UserDao userDao;
  
      public MyJdkProxy(UserDao userDao){
          this.userDao = userDao;
      }
  
      public Object createProxy(){
          Object proxy = Proxy.newProxyInstance(
                  userDao.getClass().getClassLoader(),
                  userDao.getClass().getInterfaces(),
                  this);
          return proxy;
      }
  
      public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
          if("save".equals(method.getName())){
              System.out.println("权限校验..");
              return method.invoke(userDao,args);
          }
          return method.invoke(userDao,args);
      }
  }
  
  ```

- 使用

  ```java
  public class SpringDemo1 {
      public void demo1(){
          // UserDaoImpl是实现UserDao接口的类
          UserDao userDao = new UserDaoImpl();
  
          UserDao proxy = (UserDao)new MyJdkProxy(userDao).createProxy();
          proxy.save();  //UserDao中的方法
      }
  }
  
  ```



#### 2）使用CGLIB生成代理

##### （1. CGLIB原理

- **对于不使用接口的业务类，无法使用JDK动态代理**
- CGlib采用非常底层的字节码技术，可以为一个类创建子类，解决无接口代理问题

##### （2. 使用

- 首先创造生成代理函数，经历以下四步

  1. 创建核心类：创建Enhancer核心类，接下来的三步设置都是对这个核心类操作

     ```java
     //1. 设置核心类
     Enhancer enhancer = new Enhancer();
     
     ```

  2. 设置被代理类：用 Enhancer 的 setSuperClass() 方法告诉enhancer所要代理的类，参数为被代理类

     - 本质上是设置父类，因为CGlib生成的代理实际上是继承了这个类

     ```java
     // 2，设置父类
     enhancer.setSuperclass(productDao.getClass());
     
     ```

  3. 设置回调：用 Enhancer 的 setCallBack() 设置代理执行类，这个类要实现 MethodInterceptor 接口，参数为代理执行类

     - 本例中的代理类实现了 MethodInterceptor 接口，既是代理类也是代理执行类，所以这里的参数为 this

       ```java
       // 3.设置回调
       enhancer.setCallback(this);
       
       ```

  4. 生成代理，前三步完成了对代理的设置，现在使用 Enhancer 的 create() 方法创建代理并将代理返回

     ```java
     // 4.生成代理
     Object proxy = enhancer.create();
     return proxy;
     
     ```

- 创造生成代理函数完整代码示例

  ```java
  public Object createProxy(){
          // 1.创建核心类
          Enhancer enhancer = new Enhancer();
          // 2.设置父类
          enhancer.setSuperclass(productDao.getClass());
          // 3.设置回调
          enhancer.setCallback(this);
          // 4.生成代理
          Object proxy = enhancer.create();
          return proxy;
      }
  
  ```

  

- MethodInterceptor接口的intercept() 方法

  - 需要四个参数

    - Object proxy：代理对象，刚刚已创建过
    - Method method：被执行的方法，**获取方法信息时使用**
    - Object[] args：方法所带的参数
    - MethodProxy methodProxy：方法代理类，**执行时使用**

  - 返回值需要返回“方法代理执行父类：methodProxy.invokeSuper()”， 它包含两个参数

    - 参数列表中的代理
    - 参数列表中参数数组

  - intercept完整代码示例

    ```java
    public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
            if("save".equals(method.getName())){
                System.out.println("权限校验===================");
                return methodProxy.invokeSuper(proxy,args);
            }
            return methodProxy.invokeSuper(proxy,args);
        }
    
    ```

##### （3. CGLIB代理完整示例

- 代理类

  ```java
  public class MyCglibProxy implements MethodInterceptor{
  
      private ProductDao productDao;
  
      public MyCglibProxy(ProductDao productDao){
          this.productDao = productDao;
      }
  
      public Object createProxy(){
          // 1.创建核心类
          Enhancer enhancer = new Enhancer();
          // 2.设置父类
          enhancer.setSuperclass(productDao.getClass());
          // 3.设置回调
          enhancer.setCallback(this);
          // 4.生成代理
          Object proxy = enhancer.create();
          return proxy;
      }
  
      public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
          if("save".equals(method.getName())){
              System.out.println("权限校验===================");
              return methodProxy.invokeSuper(proxy,args);
          }
          return methodProxy.invokeSuper(proxy,args);
      }
  }
  
  ```

- 使用

  ```java
  public class SpringDemo2 {
  
      public void demo1(){
          ProductDao productDao = new ProductDao();
  
          ProductDao proxy = (ProductDao) new MyCglibProxy(productDao).createProxy();
          proxy.save();
          proxy.update();
          proxy.delete();
          proxy.find();
      }
  }
  
  ```

#### 3） <font color="red">代理知识总结</font>

- **Spring在运行期，生成动态代理对象，不需要特殊的编译器**
- **Spring AOP的底层就是通过JDK动态代理或CGLib动态代理技术为目标Bean执行横向织入**
  - **若目标对象实现了若干接口，Spring使用JDK的java.lang.reflect.Proxy类代理**
  - **若目标对象没有实现任何接口，spring使用CGlib库生成目标对象的子类**
- **程序中应优先对接口创建代理，便于程序解耦维护**
- **标记为final的方法，不能被代理，因为无法进行覆盖**
  - **JDK动态代理，是针对接口生成子类，接口中方法不能使用final修饰**
  - **CGLib是针对目标类生成子类，因此类或方法不能使用final**
- **Spring只支持方法连接点，不提供属性连接**



### 3. Spring的传统AOP

#### 1）Spring AOP 增强类型

- AOP不是Spring特有的，AOP联盟提出AOP，为通知Advice定义了Advice——org.aopalliance.aop.Interface.Advice

- 增强类型也称通知类型

- Spring按照“通知Advice在目标类方法的连接点位置”来分为五类

  - 前置通知 org.springframework.aop.MethodBeforeAdvice

    - 在目标方法执行前实施增强

      ```java
      public class MyBeforeAdvice implements MethodBeforeAdvice {
          public void before(Method method, Object[] args, Object target) throws Throwable {
              System.out.println("前置增强======================");
          }
      }
      
      ```

      

  - 后置通知 org.springframework.aop.AfterReturningAdvice

    - 在目标方法执行后实施增强

  - 环绕通知 org.aopalliance.intercept.MethodInterceptor

    - 在目标方法执行前后实施增强

      ```java
      public class MyAroundAdvice implements MethodInterceptor {
          public Object invoke(MethodInvocation invocation) throws Throwable {
              System.out.println("环绕前增强===================");
      
              Object obj = invocation.proceed();
      
              System.out.println("环绕后增强===================");
              return obj;
          }
      }
      
      ```

      

  - 异常抛出通知 org.springframework.aop.ThrowsAdvices

    - 在方法抛出异常之后实施增强

  - 引介通知 org.springframework.aop.IntroductionInterceptor

    - 在目标类中添加一些新的方法和属性、

- **增强类要实现这些接口**



#### 2）Spring AOP 切面类型

- Advisor：代表一般切面，Advice本身就是一个切面，对目标类所有方法进行拦截
- PointcutAdvisor：代表具有切点的切面，可以指定拦截目标类哪些方法
- IntroductionAdvisor：代表引介切面，针对引介同通知而使用切面（不要求掌握）

#### 3）切面案例

##### （0. 引入依赖

- Aop联盟：aoppalliance

  ![SpringAop一般切面案例-Aop联盟](https://gitee.com//tonited/Spring-Notes/raw/master/assets/SpringAop%E4%B8%80%E8%88%AC%E5%88%87%E9%9D%A2%E6%A1%88%E4%BE%8B-Aop%E8%81%94%E7%9B%9F.png)

- Spring AOP：spring-aop

  ![SpringAop一般切面案例-SpringAOP](https://gitee.com//tonited/Spring-Notes/raw/master/assets/SpringAop%E4%B8%80%E8%88%AC%E5%88%87%E9%9D%A2%E6%A1%88%E4%BE%8B-SpringAOP.png)



##### （1. Advisor切面案例

- 配置目标类

- 配置切面

  - 当通知类中方法对目标类的全部方法拦截时，通知类就是切面（不带有切入点的切面）

  - 否则就要单独配置切面（带有切入点的切面）

    - 这时的切面其实就是一个只有id/name和class的Bean

    - class必须是org.springframework.aop.support.RegexpMethodPointcutAdvisor

      - 属性pattern：使用正则限制切入点

        - <font color="red">**注意：切入点的限制在切面配置中,而不是在产生代理ProxyFactoryBean中**</font>

        ```xml
        <property name="patterns" value=".*save.*,.*delete.*"/>
        
        ```

      - 属性advice：所需要使用的切面

        ```xml
        <property name="advice" ref="myAroundAdvice"/>
        
        ```

- 配置产生代理——ProxyFactoryBean

  - 产生代理ProxyFactoryBean也是一个只有id/name和class的类

  - class为 org.springframework.aop.framework.ProxyFactoryBean

  - 常用属性

    - target：代理的目标对象

    - proxyInterfaces：代理实际要实现的接口

      - 如果多个接口使用以下格式

        ```xml
        <list>
        	<value></value>
            ..
        </list>
        
        ```

    - proxyTargetClass：是否对类代理而不是接口，即是否使用CGLib代理，值为true/false

    - interceptorName：需要织入目标的Advice

      - 当通知类中方法对目标类的全部方法拦截时，通知类就是切面（不带有切入点的切面）
      - 否则就要单独配置切面（带有切入点的切面），并且interceptor是切面这个单独配置的切面

    - singleton：返回代理是否为单例，默认为单例

    - optimize：设置为true时，强制使用CGLib

- Advisor切面完整配置示例

  ```xml
  <!--配置目标类============-->
      <bean id="customerDao" class="com.aop.demo4.CustomerDao"/>
  
      <!--配置通知============== -->
      <bean id="myAroundAdvice" class="com.aop.demo4.MyAroundAdvice"/>
  
      <!--一般的切面是使用通知作为切面的，因为要对目标类的某个方法进行增强就需要配置一个带有切入点的切面-->
      <bean id="myAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
          <!--pattern中配置正则表达式：.任意字符  *任意次数 -->
      	<!--<property name="pattern" value=".*save.*"/>-->
          <property name="patterns" value=".*save.*,.*delete.*"/>
          <property name="advice" ref="myAroundAdvice"/>
      </bean>
  
  <!-- 配置产生代理 -->
      <bean id="customerDaoProxy" class="org.springframework.aop.framework.ProxyFactoryBean">
          <property name="target" ref="customerDao"/>
          <property name="proxyTargetClass" value="true"/>
          <property name="interceptorNames" value="myAdvisor"/>
      </bean>
  
  ```

##### （2. 自动创建代理

- 前面的案例中，每个代理都是通过ProxyFactoryBean织入切面代理，在实际开发中，非常多的Bean每个都配置ProxyFactory开发维护量巨大
- **解决方案：自动创建代理**
  - BeanNameAutoProxyCreator 根据Bean名称创建代理
  - DefaultAdvisorAutoProxyCreator 根据Advisor本身包含信息创建代理
  - **AnnotationAwareAspectJAutoProxyCreator 基于 Bean 中的 AspectJ注解进行自动代理**
- 自动创建代理是ProxyFactory的替换
  - （切入点的限制在切面配置中,而不是在产生代理ProxyFactoryBean中）
  - BeanNameAutoProxyCreator存在属性beanNames，其value用来配置被代理Bean的名称，支持正则,，与ProxyFactory用法上大致相同，有些许不同
  - DefaultAdvisorAutoProxyCreator 会自动扫描切面和Bean，并根据切面的pattern进行代理

###### 1）BeanNameAutoProxyCreator 示例

- 对所有以DAO结尾Bean的所有方法使用代理

```xml
<!--配置目标类-->
<bean id="studentDao" class="com.aop.demo5.StudentDaoImpl"/>
<bean id="customerDao" class="com.aop.demo5.CustomerDao"/>

<!-- 配置增强-->
<bean id="myBeforeAdvice" class="com.aop.demo5.MyBeforeAdvice"/>
<bean id="myAroundAdvice" class="com.aop.demo5.MyAroundAdvice"/>

<bean class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator">
    <!--   .任意字符  *任意次数   -->
    <property name="beanNames" value="*Dao"/>
    <property name="interceptorNames" value="myBeforeAdvice"/>
</bean>

```

- 测试使用

  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration("classpath:applicationContext3.xml")
  public class SpringDemo5 {
      @Resource(name="studentDao")
      private StudentDao studentDao;
      @Resource(name="customerDao")
      private CustomerDao customerDao;
  
      @Test
      public void demo1(){
          studentDao.find();
          studentDao.save();
  
          customerDao.find();
          customerDao.save();
      }
  }
  
  ```



###### 2) DefaultAdvisorAutoProxyCreator 示例

- 配置环绕代理案例

```xml
<!--配置目标类-->
    <bean id="studentDao" class="com.aop.demo6.StudentDaoImpl"/>
    <bean id="customerDao" class="com.aop.demo6.CustomerDao"/>

    <!-- 配置增强-->
    <bean id="myBeforeAdvice" class="com.aop.demo6.MyBeforeAdvice"/>
    <bean id="myAroundAdvice" class="com.aop.demo6.MyAroundAdvice"/>

    <!--配置切面-->
    <bean id="myAdvisor" class="org.springframework.aop.support.RegexpMethodPointcutAdvisor">
        <property name="pattern" value="com\.aop\.demo6\.CustomerDao\.save"/>
        <property name="advice" ref="myAroundAdvice"/>
    </bean>

    <bean class="org.springframework.aop.framework.autoproxy.DefaultAdvisorAutoProxyCreator"/>

```

- 测试使用

  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration("classpath:applicationContext4.xml")
  public class SpringDemo6 {
      @Resource(name="studentDao")
      private StudentDao studentDao;
      @Resource(name="customerDao")
      private CustomerDao customerDao;
  
      @Test
      public void demo1(){
          studentDao.find();
          studentDao.save();
  
          customerDao.find();
          customerDao.save();
      }
  }
  
  ```

  

### 4. 问题

- 关于Spring AOP的说法
  - AOP是 Aspect Oriented Programming的英文缩写
  - AOP编程技术可以完成的功能：性能监视、事务管理以及安全检查、缓存
- 关于Spring AOP术语的说法
  - Joinpoint指的是可以被拦截的点，例如增删改查这些方法
  - 如果只想对update()方法做增强，那么update()方法就是Pointcut
- 在Spring AOP中，关于Introduction（引介）的说法
  - 引介是一种特殊的通知，在不修改类代码的前提下，引介可以在**运行期**为类动态的添加一些方法或Field
- 在Spring AOP中，以下关于Target（目标对象）和Weaving（织入）的说法**错误**
  - AspectJ 采用动态代理织入
  - Spring采用编译器织入和类装载器织入的方式
- 以下关于SpringAOP增强类型说法错误
  - SpringAOP按照通知Advice在目标类方法的连接点位置，可以分为4种通知类型，分别为前置通知、后置通知、环绕通知、引介通知
- 关于SpringAOP中对Advisor切面中属性有以下描述
  - interceptorNames：需要织入目标的Advice
  - optimize：设置为true时，强制使用CGLib

## 四. 基于AspectJ的AOP开发

### 1. AspectJ 简介

- AspectJ 是一个基于Java语言的AOP框架
- Spring2.0 以后新增了对AspectJ切点表达式的支持
  - 注意：切点表达式不是AspectJ特有的，而是Spring支持的
- @AspectJ 是 AspectJ1.5 新增功能，通过JDK5注解技术，允许直接在Bean类中定义切面
- 新版本Spring框架建议使用 AspectJ 方式开发AOP
- 使用AspectJ 需要导入Spring AOP 和 AspectJ 相关jar包（依赖）
  - **spring-aop**
  - （com.springsource.org.**aopalliance**)
  - (**spring-aspects**)
  - com.springsource.org.aspectj.weaver （**aspectjweaver**）

### 2. 基于AspectJ的注解AOP开发

#### 1）环境准备

- 配置Spring配置文件：applicationContext.xml

- 开启 AspectJ 自动代理  <aop:aspectj-autoproxy />

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
          http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
          http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
  
      <!--开启AspectJ自动代理-->
      <aop:aspectj-autoproxy />
      
      
  </beans>
  
  ```

#### 2) @AspectJ提供的通知类型

- @Before 前置通知，相当于 BeforeAdvice
- @AfterReturning 后置通知，相当于 AfterReturningAdvice
- @Around 环绕通知，相当于 MethodInterceptor
- @AfterThrowing 异常抛出通知，相当于ThrowAdvice
- @After 最终final通知，不管是否异常，该通知都会执行
- @DeclareParents 引介通知，相当于IntroductionInterceptor（不要求掌握）

#### 3）切点表达式——通知中通过value属性定义切点

- 通过execution函数，可以定义切点的方法切入

- 语法

  - “ * ”表示任意字符
  - “ . ” 表示任意参数

  ```xml
   execution( [<访问修饰符>] <返回类型> <方法名>([<参数>]) <异常> )
  
  ```

- 例如

  ```xml
  - 匹配所有类的public方法 
  	execution(public * *(.))
  
  - 匹配指定包下所有类方法（不包含子包）(第一个*是返回类型)
  	execution (* com.dao.*(.))
  - 匹配指定包下所有类方法（包含包、子包下的所有类）(在包名后变为两个点即可)
  	execution(* com.dao.*(.))
  
  - 匹配指定类所有方法 
  	execution(* com.service.UserService.*(.))
  
  - 匹配实现特定接口所有类方法
  	execution(* com.dao.GenericDAO+.*(.))
  
  - 匹配所有save开头的方法
  	execution(* save*(.))
  
  ```

#### 4）使用方法示例

- 使用@Aspect声明一个类为切面类

  ```java
  @Aspect
  public class MyAspectAnno {}
  
  ```

- 配置文件配置目标类，配置切面

  ```xml
  <!--开启AspectJ的注解开发，自动代理=====================-->
  <aop:aspectj-autoproxy/>
  
  <!--目标类===================-->
  <bean id="productDao" class="com.aspectJ.demo1.ProductDao"/>
  
  <!--定义切面-->
  <bean class="com.aspectJ.demo1.MyAspectAnno"/>
  
  ```

- 通知中通过value属性定义切点

  ```java
  @Aspect
  public class MyAspectAnno
  {
      @Before(value="execution(* com.aspectJ.demo1.ProductDao.save(.))")
      public void before(JoinPoint joinPoint){
          System.out.println("前置通知");
      }
  }
  
  ```

#### 5) 注解通知类型使用

- @Before 前置通知

  - 可以在方法中传入JoinPoint对象，用来获得切点信息

    ```java
    //要增强的代码
    @Before(value="execution(* com.aspectJ.demo1.ProductDao.save(.))")
    //通知
    public void before(JoinPoint joinPoint){
            System.out.println("前置通知");
        }
    
    ```

- @AfterReturning 后置通知

  - @AfterReturning 存在参数returning，值为字符串，作用是把切入点的返回值注入变量，变量名就是returning的值，这个变量作为通知函数的参数

    ```java
    @AfterRuning(
    	value="execution(* com.aspectJ.demo1.ProductDao.save(.))",
    	returning="resu"
    )
    public void afterReturning(Object resu){
        System.out.println("后置通知");
    }
    
    ```

- @Around 环绕通知

  - Around方法的返回值就是目标代理方法执行返回值
  - 参数为ProceedingJoinPoint，是目标方法
  - **要手动调用ProceedingJoinPoint的proceed()方法，才会执行目标方法，否则目标方法的执行就会被拦截**

  ```java
  @Around("execution(* com.aspectJ.demo1.ProductDao.save(.))")
  public Object around( ProceedingJoinPoint joinPoint )
  {
      System.out.println("==前环绕通知==");
      Object obj = joinPoint.proceed();
      System.out.println("==后环绕通知==");
      
      return obj;
  }
  
  ```

- @AfterThrowing 异常抛出通知

  - 通过设置throwing属性，可以设置发生异常对象参数

    ```java
    @AfterThrowing(
        value="execution(* com.aspectJ.demo1.ProductDao.save(.))",
        throwing = "e"
    )
        public void afterThrowing(Throwable e){
            System.out.println("异常抛出通知=============="+e.getMessage());
        }
    
    ```

- @After 最终通知

  - 无论是否出现异常，最终通知总是会被执行

  ```java
  @After(value="execution(* com.aspectJ.demo1.ProductDao.save(.))")
      public void after(){
          System.out.println("最终通知==================");
      }
  
  ```

#### 6）通过@Pointcut为切点命名

- 在每个通知内定义切点，会造成工作量大、不易维护，对于切点，可以使用@Pointcut进行定义
- 切点方法限制：private void 无参数方法 无内容 方法名为切点名
- 当通知多个切点时，可以使用||进行连接

```java
@Before(value="myPointcut1()")
    public void before(JoinPoint joinPoint){
        System.out.println("前置通知=================="+joinPoint);
    }


@Pointcut(value="execution(* com.aspectJ.demo1.ProductDao.save(.))")
    private void myPointcut1(){}

```

#### 7）注解AOP开发完整代码示例

- Spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--开启AspectJ的注解开发，自动代理=====================-->
    <aop:aspectj-autoproxy/>

    <!--目标类===================-->
    <bean id="productDao" class="com.aspectJ.demo1.ProductDao"/>

    <!--定义切面-->
    <bean class="com.aspectJ.demo1.MyAspectAnno"/>
</beans>

```



- 切面类

```java
/**
 * 切面类
 */
@Aspect
public class MyAspectAnno {

    @Before(value="myPointcut1()")
    public void before(JoinPoint joinPoint){
        System.out.println("前置通知=================="+joinPoint);
    }

    @AfterReturning(value="myPointcut2()",returning = "result")
    public void afterReturing(Object result){
        System.out.println("后置通知=================="+result);
    }

    @Around(value="myPointcut3()")
    public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("环绕前通知================");
        Object obj = joinPoint.proceed(); // 执行目标方法
        System.out.println("环绕后通知================");
        return obj;
    }

    @AfterThrowing(value="myPointcut4()",throwing = "e")
    public void afterThrowing(Throwable e){
        System.out.println("异常抛出通知=============="+e.getMessage());
    }

    @After(value="myPointcut5()")
    public void after(){
        System.out.println("最终通知==================");
    }


    @Pointcut(value="execution(* com.aspectJ.demo1.ProductDao.save(.))")
    private void myPointcut1(){}

    @Pointcut(value="execution(* com.aspectJ.demo1.ProductDao.update(.))")
    private void myPointcut2(){}

    @Pointcut(value="execution(* com.aspectJ.demo1.ProductDao.delete(.))")
    private void myPointcut3(){}

    @Pointcut(value="execution(* com.aspectJ.demo1.ProductDao.findOne(.))")
    private void myPointcut4(){}

    @Pointcut(value="execution(* com.aspectJ.demo1.ProductDao.findAll(.))")
    private void myPointcut5(){}
}


```

### 3. 基于AspectJ的XML方式的AOP开发

XML开发方式与注解方式只是用了不同给方式完成相同功能，两者使用上有相似之处



- 编写切面类（切面类上不使用注解）

- 完成目标类切面类的配置

  ```xml
  <!--配置目标类=================-->
  <bean id="customerDao" class="com.aspectJ.demo2.CustomerDaoImpl"/>
  
  <!--配置切面类-->
  <bean id="myAspectXml" class="com.aspectJ.demo2.MyAspectXml"/>
  
  ```

- 配置AOP完成增强

  - 关闭自动代理
  - AOP配置写在<aop: confing>标签中
  - 定义切入点：哪些类的哪些方法需要增强，使用标签 <aop: pointcut>
    - id属性
    - expression属性：使用切点表达式，与注解方法的value相同
  - 配置切面，使用标签 <aop: aspect>
    - ref属性，指向切面类的id/name
    - 切面种类标签
      - 包括<aop：before> <aop：after-returning> <aop：around> <aop：after-throwing> <aop：after>
      - method属性，值为切面种类
      - pointcut-ref属性，值为切入点的id/name，指向被增强的切入点
      - 其他属性：如afterThrowing的throwing属性、afterReturing的returing属性，使用方法与注解的同名属性相同

#### XML方式的AOP开发完整示例

- 切面类

```java
public class MyAspectXml {

    // 前置通知
    public void before(JoinPoint joinPoint){
        System.out.println("XML方式的前置通知=============="+joinPoint);
    }

    // 后置通知
    public void afterReturing(Object result){
        System.out.println("XML方式的后置通知=============="+result);
    }

    // 环绕通知
    public Object around(ProceedingJoinPoint joinPoint) throws Throwable {
        System.out.println("XML方式的环绕前通知==============");
        Object obj = joinPoint.proceed();
        System.out.println("XML方式的环绕后通知==============");
        return obj;
    }

    // 异常抛出通知
    public void afterThrowing(Throwable e){
        System.out.println("XML方式的异常抛出通知============="+e.getMessage());
    }

    // 最终通知
    public void after(){
        System.out.println("XML方式的最终通知=================");
    }
}

```



- Spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--XML的配置方式完成AOP的开发===============-->
    <!--配置目标类=================-->
    <bean id="customerDao" class="com.aspectJ.demo2.CustomerDaoImpl"/>

    <!--配置切面类-->
    <bean id="myAspectXml" class="com.aspectJ.demo2.MyAspectXml"/>

    <!--aop的相关配置=================-->
    <aop:config>
        <!--配置切入点-->
        <aop:pointcut id="pointcut1" expression="execution(* com.aspectJ.demo2.CustomerDao.save(.))"/>
        <aop:pointcut id="pointcut2" expression="execution(* com.aspectJ.demo2.CustomerDao.update(.))"/>
        <aop:pointcut id="pointcut3" expression="execution(* com.aspectJ.demo2.CustomerDao.delete(.))"/>
        <aop:pointcut id="pointcut4" expression="execution(* com.aspectJ.demo2.CustomerDao.findOne(.))"/>
        <aop:pointcut id="pointcut5" expression="execution(* com.aspectJ.demo2.CustomerDao.findAll(.))"/>
        <!--配置AOP的切面-->
        <aop:aspect ref="myAspectXml">
            <!--配置前置通知-->
            <aop:before method="before" pointcut-ref="pointcut1"/>
            <!--配置后置通知-->
            <aop:after-returning method="afterReturing" pointcut-ref="pointcut2" returning="result"/>
            <!--配置环绕通知-->
            <aop:around method="around" pointcut-ref="pointcut3"/>
            <!--配置异常抛出通知-->
            <aop:after-throwing method="afterThrowing" pointcut-ref="pointcut4" throwing="e"/>
            <!--配置最终通知-->
            <aop:after method="after" pointcut-ref="pointcut5"/>
        </aop:aspect>

    </aop:config>
</beans>

```

## 五. Spring JDBC Template

**——使用Spring组件JDBC Template简化持久化操作**

### 1. 简介

- 为了持久化操作，Spring再JDBC API之上提供了JDBC Template组件

  - 程序员代码 -> JDBC API -> JDBC驱动 -> 数据库
  - 程序员代码 -> JDBC Template -> JDBC API -> JDBC驱动 -> 数据库
  - JdbcTemplate是线程安全的

- JDBC Template 提供统一的模板方法，在保留代码灵活性的基础上，尽量减少持久化代码

  ```java
  // JDBC API
  Statement statement = conn.createStatement();
  ResultSet resultset = statement.executeQuery("select count(*) COUNT from student");
  if(resultSet.next()){
      Integer count = resultSet.getInt("COUNT");
  }
  
  // JDBC Template
  Integer count = jt.queryForObject("select count(*) from student", Integer.class);
  ```

### 2. 本章示例的数据库表结构

	![1566457053166](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566457053166.png)

### 3. 项目Spring配置及Maven依赖

- Maven

  - Mysql驱动

    ```xml
    <!-- mysql8以上版本使用8以上依赖-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.17</version>
    </dependency>
    ```

  - Spring组件：core、beans、context、aop

    ```xml
    <!--设置属性--> 
    <properties>
         <spring.version>4.2.4.RELEASE</spring.version>
    </properties>
    
    <!--配置依赖-->
    <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-core</artifactId>
         <version>${spring.version}</version>
    </dependency>
    
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
    
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aop</artifactId>
        <version>${spring.version}</version>
    </dependency>
    
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>${spring.version}</version>
    </dependency>
    ```

  - JDBC Template：jdbc、tx

    ```xml
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>${spring.version}</version>
    </dependency>
    
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
    </dependency>
    ```

  - 单元测试

    ```xml
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
    ```

- Spring 配置

  - 数据源配置

    ```xml
    <!--    数据源    -->
    <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        
        <property name="url" value="jdbc:mysql://localhost:数据库端口默认3306/数据库名"/>
        <!--使用Unicode需要在url后加上：?useUnicode=true&amp;characterEncoding=utf-8  -->
        <!--账号密码正确但连接失败(时区问题)在url后加上：&amp;serverTimezone=UTC  -->
        
        <property name="username" value="mysql的用户名"/>
        <property name="password" value="mysql的密码"/>
    </bean>
    ```

  - Jdbc Template

    ```xml
    <!--    JdbcTemplate    -->
    <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    	<property name="dataSource" ref="dataSource"/>
    </bean>
    ```

  - 不要忘记开启自动扫描，用于稍后注入JdbcTemplate

    ```xml
    <context:component-scan base-package="com.tonited.sc"/>
    ```

    

### 4. JDBC Template 基本使用

#### 1）execute方法

- execute方法可以直接执行sql语句，常用于执行对表的操作

  ```java
  public void testExecute(){
         jdbcTemplate.execute("create table user1(id int, name varchar(20))");
  }
  
  ```

#### 2）update和batchUpdate方法

- update

  - 对数据进行增删改查操作，返回值为被影响的行数

  - sql为sql语句字符串

  - args为参数

    ```java
    int update(String sql, Object[] args)
    int update(String sql, Object.. args)//不定参数
    
    ```

  - 重载一

    ```java
    public void testUpdate(){
        String sql = "insert into student(name,sex) value(?,?)";
        jdbcTemplate.update(sql, new Object[]{"张飞","男"});
    }
    
    ```

  - 重载二

    ```java
    public void testUpdate2(){
        String sql = "update student set sex=? where id=?";
        jdbcTemplate.update(sql, "男",1);
    }
    
    ```

- batchUpdate方法

  - 批量增删改查，返回值为每个操作影响的行数

    ```java
    int[] batchUpdate(String[] sql)
    int[] batchUpdate(String sql, List<Object[]> args)
    
    ```

  - 重载一

    - 这一重载以sql字符串数组为参数，执行数组中的每一条sql语句

    ```java
    public void testBatchUpdate(){
        String[] sqls = {
            "insert into student(name, sex) value('关羽', '女')",
            "insert into student(name, sex) value('刘备','男')",
            "update student set sex='男' where id=2"
        };
        jdbcTemplate.batchUpdate(sqls);
    }
    
    ```

  - 重载二

    - 这一重载执行多次同构sql语句，参数 list 中 Object[] 是同构sql语句中的占位符的值

    ```java
    public void testBatchUpdat2(){
        String sql = "insert into selection(student, course) value(?,?)";
        List<Object[]> list = new ArrayList<>();
        list.add(new Object[] {3,1});
        jdbcTemplate.batchUpdate(sql,list);
    }
    
    ```

#### 3）query和queryXXX方法

##### （1 查询单一数据

- 获取一条数据

  - type为返回值的类型

  ```java
  T queryForObject (String sql, Class<T> type)
  T queryForObject (String sql, Object[] args, Class<T> type)
  T queryForObject (String sql, Class<T> type, Object.. arg)
  
  ```

  - 重载一

    ```java
    public void testQuerySimple(){
        String sql = "select count(*) from student";
        int count = jdbcTemplate.queryForObject(sql, Integer.class);
        System.out.println(count);
    }
    
    ```

- 获取多条数据

  - type为返回值的类型

  ```java
  List<T> queryForList(String sql, Class<T> type)
  List<T> queryForList(String sql, Object[] args, Class<T> type)
  List<T> queryForList(String sql, Class<T> type, Object.. args)
  
  ```

  - 重载三

    ```java
    public void testQuerySimple2(){
        String sql = "select name from student where sex=?";
        List<String> names = jdbcTemplate.queryForList(sql,String.class,"女");
        System.out.println(names);
    }
    
    ```

##### （2 查询复杂对象（多字段）——封装为Map

- 查询一条数据

  - 返回值中，键为字段名

  ```java
  Map queryForMap(String sql)
  Map queryForMap(String sql, Object[] args)
  Map queryForMap(String sql, Object.. args)
  
  ```

- 查询多条数据

  - 返回值中，键为字段名

  ```java
  List< Map<String,Object> > queryForList(String sql)
  List< Map<String,Object> > queryForList(String sql, Object[] args)
  List< Map<String,Object> > queryForList(String sql, Object.. args)
  
  ```

  - 重载三

    ```java
    public void testQueryMap1(){
        String sql = "select name,sex from student where sex=?";
        List<Map<String,Object>> stu = jdbcTemplate.queryForList(sql,"男");
        System.out.println(stu);
    }
    
    ```

##### （3 查询复杂对象（多字段）——封装为实体对象

- 参数中的RowMapper需要实现接口RowMapper

  - 有时会采用匿名内部类方法
  - 本例中使用内部类方法，单独提取出封装数据的RowMapper

  ```java
  private class StudentRowMapper implements RowMapper<Student>{
      public Student mapRow(ResultSet resultSet, int i) throws SQLException {
          Student stu = new Student();
          stu.setId(resultSet.getInt("id"));
          stu.setName(resultSet.getString("name"));
          stu.setSex(resultSet.getString("sex"));
          stu.setBorn(resultSet.getDate("born"));
          return stu;
      }
  }
  
  ```

- 获取一条数据

  ```java
  T queryForObject(String sql, RowMapper<T> mapper)
  T queryForObject(String sql, Object[] args ,RowMapper<T> mapper)
  T queryForObject(String sql, RowMapper<T> mapper, Object.. args)
  
  ```

  - 重载三

    ```java
    public void testQueryEntity1(){
        String sql = "select * from student where id = ?";
        Student stu = jdbcTemplate.queryForObject(sql, new StudentRowMapper(), 1004);
        System.out.println(stu);
    }
    
    ```

- 获取多条数据

  ```java
  List<T> query(String sql, RowMapper<T> mapper)
  List<T> query(String sql, Object[] args ,RowMapper<T> mapper)
  List<T> query(String sql, RowMapper<T> mapper, Object.. args)
  
  ```

  - 重载三

    ```java
    public void testQueryEntity2(){
        String sql = "select * from student";
        List<Student> stus = jdbcTemplate.query(sql,new StudentRowMapper());
        System.out.println(stus);
    }
    
    ```

### 5. JDBC Template持久层示例

#### 1）创建实体类

![1566462959465](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566462959465.png)

```java
public class Course {
    private int id;
    private String name;
    private int score;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getScore() {
        return score;
    }

    public void setScore(int score) {
        this.score = score;
    }
}

```

#### 2) Dao

- 注入JdbcTemplate

  - 不要忘记开启包扫描

    ```xml
    <context:component-scan base-package="com.tonited.sc"/>
    
    ```

    

- 声明RowMapper

  ```java
  @Repository
  public class SelectionDaoImpl implements SelectionDao {
      @Autowired
      private JdbcTemplate jdbcTemplate;
  
      public void insert(List<Selection> seles) {
          String sql = "insert into selection values(?,?,?,?)";
          List<Object[]> list = new ArrayList<Object[]>();
          for(Selection sel:seles){
              Object[] args = new Object[4];
              args[0] = sel.getSid();
              args[1]=sel.getCid();
              args[2] = sel.getSelTime();
              args[3] =sel.getScore();
              list.add(args);
          }
          jdbcTemplate.batchUpdate(sql,list);
      }
  
      public void delete(int sid,int cid) {
          String sql = "delete from selection where student=? and course=?";
          jdbcTemplate.update(sql,sid,cid);
      }
  
      public List<Map<String, Object>> selectByStudent(int sid) {
          String sql = "select se.*,stu.name sname,cou.name cname from selection se " +
                  "left join student stu on se.student=stu.id " +
                  "left join course cou on se.course=cou.id" +
                  "where student=?";
          return jdbcTemplate.queryForList(sql,sid);
      }
  
      public List<Map<String, Object>> selectByCourse(int cid) {
          String sql = "select se.*,stu.name sname,cou.name cname from selection se " +
                  "left join student stu on se.student=stu.id " +
                  "left join course cou on se.course=cou.id" +
                  "where course=?";
          return jdbcTemplate.queryForList(sql,cid);
      }
  }
  ```



### 6. 优缺点分析

- 优点
  - 简单、灵活
    - Jdbc Template只是在JDBC的基础上进行的简单封装，所以使用起来就像使用JDBC一样灵活，且由于被封装，代码简单
- 缺点
  - SQL与java代码掺杂
    - 两种代码掺杂会需要程序员同时了解SQL和Java，难以做到专职专业
  - 功能不丰富
    - 我们可以看到很多复杂的查询（如多表查询）Jdbc Template并未进行封装，功能不够丰富，这并不是Jdbc Template没有考虑到，而是为了给其他框架更多的集成空间

### 7. 总结

- 持久化操作特点
  - 持久化操作是任何程序中必须
  - 持久化操作是很机械的增删改查

JDBC Template是Spring框架对JDBC操作的封装，简单、灵活，但是不够强大。

因此实际应用中还需要和其他ORM框架（Hibernate、MyBatis等）结合使用。

### 8. 问题

- 关于JDBC Template有以下说法
  - 为了简化持久化操作，**Spring**提供了JDBC Template组件
  - JDBC API是在JDBC Template基础上完成的这种说法是错误的
- 在JDBCTemplate中，创建table的sql语句应该写道execute()方法中
- 在JDBC Template中，查询复杂对象时，获取单条查询结果，使用下面的方法是错误的
  - T queryForObject(String sql, RowMapper<T> mapper, Object.. args)

## 六. 事务管理

- **所谓事务管理，个人理解就是防止事物之间发生交叉导致脏数据产生，事务管理所做的就是为每个事务设置不同的特性和行为，如是否自动提交、隔离级别是什么等等**



以下参考：

- 《Spring in actioin》
- [Spring事务机制详解](http://www.open-open.com/lib/view/open1350865116821.html)
- [Spring事务配置的五种方式](http://www.blogjava.net/robbie/archive/2009/04/05/264003.html)
- [Spring中的事务管理实例详解](http://www.jb51.net/article/57589.htm)
- [Trigl的博客：Spring事务管理（详解+实例）](https://blog.csdn.net/trigl/article/details/50968079)

### 1. 什么是事务

理解事务之前，先要理解日常生活中的一个常做的事：取钱

你去ATM机取1000块钱，大体有两个步骤银行卡扣除1000元、ATM出1000元

这两个步骤必须要么都执行、要么都不执行

所以，如果一个步骤成功另一个步骤却失败，对双方都不是好事

如果其中哪一个步骤出了问题可以取消所有操作，回退到未作任何操作的状态的话，这对双方都有利。

事务就是用来解决类似问题的。**事务是一系列操作，他们综合在一起才是一个完整的工作单元，这些动作必须全部完成，如果其中有任何失败的操作，事务就会回滚到最开始的状态。**

在企业级应用程序开发中，事务管理必不可少，用来确保数据的一致性与完整性

### 2. 事务的特性 ACID

|         特性          |                             含义                             |
| :-------------------: | :----------------------------------------------------------: |
|  原子性（Atomicity）  | 事务是一个原子操作，由一系列动作组成。事务的原子性确保动作要么全部完成，要么不起作用 |
| 一致性（Consistency） | 事务一旦完成，**不管成功还是失败**，系统必须确保它所建模的业务处于一致的状态，而不会是部分完成部分失败。在现实中的数据不应该被破坏 |
|  隔离性（Isolation）  | 可能有许多事务会同时处理相同的数据，因此每个事务都应该与其他事务隔离开，防止数据破坏 |
| 持久性（Durability）  | 一旦事务完成，无论发生什么系统错误，他的结果都不应该受到影响，这样就能从任何系统崩溃中恢复过来。通常情况下，事务的结果被写进持久化存储器中 |



### 3. Java事务实现模式

- Java事务的实现
  - 通过Java代码来实现对数据库的事务性操作
- Java事务类型
  - JDBC事务：用`Connection`对象控制的手动模式和自动模式
  - JTA（Java Transaction API）事务：与实现无关的，与协议无关的API
  - 容器事务：应用服务器提供的，且大多是基于JTA完成（通常基于JNDI的，相当复杂的API实现）
- 三种事务的差异
  - JDBC事务：控制的局限性在一个数据库连接内，但是使用比较简单
  - JTA事务：功能强大，可跨越多个数据库或多个DAO，使用比较复杂
  - 容器事务：主要指的是J2EE应用服务器提供的事务管理，局限于EJB应用使用

### 4. Spring事务管理

#### 0）Spring事务管理接口

![这里写图片描述](https://gitee.com//tonited/Spring-Notes/raw/master/assets/20160324011156424)

#### 1）事务管理器

- Spring并不管理事务，而是提供了多种事务管理器，他们将事务管理的职责委托给Hibernate或者JTA等持久化机制所提供的相关平台框架的事务来实现

- Spring事务管理器的接口是 **org.springframework.transaction.PlatformTransactionManager**，通过这个接口，Spring为各个平台如JDBC、Hibernate等都提供了对应的事务管理器，但是具体的实现就是平台自己的事情了

  - 接口代码如下

    ```java
    Public interface PlatformTransactionManager()..{  
        // 由TransactionDefinition得到TransactionStatus对象
        TransactionStatus getTransaction(TransactionDefinition definition) throws TransactionException; 
        // 提交
        Void commit(TransactionStatus status) throws TransactionException;  
        // 回滚
        Void rollback(TransactionStatus status) throws TransactionException;  
    } 
    
    ```

    从代码中可以看出

    - 具体的事物管理机制对Spring是透明的，他并不关心那些
    - 具体的管理事务由各个平台具体实现
    - Spring事务管理的一个优点就是为不同的事务API提供一致的编程模型，如JTA、JDBC、Hibernate、JPA

#### 2）各个平台框架实现事务管理的机制

##### （1 JDBC事务

- 如果应用程序中直接使用JDBC来进行持久化，DataSourceTransitionManager会为你处理事务边界（提交、回滚）

- 为了使用DateSourceTransactionManager，需要使用如下XMl装配到应用程序上下文定义中

  ```xml
  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  	<property name="dataSource" ref="dataSource" />
  </bean>
  
  ```

- 实际上，DataSourceTransitionManager是通过调用java.sql.Connection来管理事务，而java.sql.Connection是通过DataSource获得的

- 通过调用连接的commit()方法来提交事务，同样，事务失败则通过调用rollback()方法回滚



##### （2 Hibernate事务

- 如果应用程序的持久化是通过Hibernate实现，那么需要使用HibernateTransactionManager

- 对于Hibernate3，需要在Spring上下文定义中添加如下的`<bean>`声明：

  ```xml
  <bean id="transactionManager" class="org.springframework.orm.hibernate3.HibernateTransactionManager">
  	<property name="sessionFactory" ref="sessionFactory" />
  </bean>
  
  ```

- sessionFactory属性需要装配一个Hibernate的session工厂

- HibernateTransitionManager的实现细节是它将事务管理的职责委托给`org.hibernate.Transition`对象，而后者是从`HibernateSessioin`中获取到的

- 当事务成功完成时，`HibernateTransitionManager`将会调用`Transition`对象的`commite()`方法，反之调用`rollback()`方法



##### （3 Java持久化API事务（JPA）

- JPA： JPA 是一个**基于O/R映射的标准规范**。所谓规范即只定义标准规则（如注解、接口），不提供实现，软件提供商可以按照标准规范来实现，而使用者只需按照规范中定义的方式来使用，而不用和软件提供商的实现打交道

- Hibernate多年来一直是事实上的Java持久化标准，但是现在Java持久化API作为真正的Java持久化标准进入大家的视野

- **JPA和Hibernate之间的关系**

  - **可以简单的理解为JPA是标准接口，Hibernate是实现，并不是对标关系，**
  - **Hibernate属于遵循JPA规范的一种实现，但是JPA是Hibernate遵循的规范之一，Hibernate还有其他实现的规范**

  ![img](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1436045-20180817162031268-1675607816.png)

- 如果计划使用JPA，需要使用Spring的`JpaTransactionManager`来处理事务

  ```xml
  <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
  	<property name="sessionFactory" ref="sessionFactory" />
  </bean>
  
  ```

- `JpaTransactionManager`只需要装配一个JPA实体管理工厂（`javax.persistence.EntityManagerFactory`接口的任意实现）

- `JpaTransictionManager`将与由工厂所产生的`JPA EntityManager`合作构建事务



##### （4 java原生API事务

- 如果没有使用以上所述的事务管理，或者是跨越了多个事务管理源（比如两个或者多个不同的数据源），就需要使用`JtaTransactionManager`

  ```xml
  <bean id="transactionManager" class="org.springframework.transaction.jta.JtaTransactionManager">
  	<property name="transactionManagerName" value="java:/TransactionManager" />
  </bean>
  
  ```

- `JtaTransictionManager`将事务管理的责任委托给`javax.transaction.UserTransaction`和`javax.transaction.TransactionManager`对象

  - 其中事务成功完成通过`UserTransaction.commit()`方法提交
  - 事务失败通过`UserTransaction.rollback()`方法回滚

#### 3）基本事务属性

- 事务管理器接口`PlatformTransactionManager`通过`getTransaction(TransactionDefinition definition)`方法来得到事务，这个方法里面的参数是`TransactionDefinition`类

- `TransactionDefinition`类

  - 定义了一些基本事务属性，事务属性可以理解为事务的一些基本配置，包括五个方面

  ![这里写图片描述](https://gitee.com//tonited/Spring-Notes/raw/master/assets/20160325003448793)

  - `TransactionDefinition`接口代码

    ```java
    public interface TransactionDefinition {
        int getPropagationBehavior(); // 返回事务的传播行为
        int getIsolationLevel(); // 返回事务的隔离级别，事务管理器根据它来控制另外一个事务可以看到本事务内的哪些数据
        int getTimeout();  // 返回事务必须在多少秒内完成
        boolean isReadOnly(); // 事务是否只读，事务管理器能够根据这个返回值进行优化，确保事务是只读的
    } 
    
    ```

##### （1 传播行为

- 传播行为（propagation behavior），当事务方法被另一个事务方法调用时，必须指定事务应该如何传播
  - 例如：方法可能继续在现有的事务中运行，也可能开启一个新的事物并在自己的事务中运行
- Spring定义了七种传播行为

|         传播行为          |                             含义                             |
| :-----------------------: | :----------------------------------------------------------: |
|   PROPAGATION_REQUIRED    | 表示当前方法必须运行在事务中。如果当前事务存在，方法将会在该事务中运行。否则，会启动一个新的事务 |
|   PROPAGATION_SUPPORTS    | 表示当前方法不需要事务上下文，但是如果存在当前事务的话，那么该方法会在这个事务中运行 |
|   PROPAGATION_MANDATORY   | 表示该方法必须在事务中运行，如果当前事务不存在，则会抛出一个异常 |
| PROPAGATION_REQUIRED_NEW  | 表示当前方法必须运行在它自己的事务中。一个新的事务将被启动。如果存在当前事务，在该方法执行期间，当前事务会被挂起。如果使用JTATransactionManager的话，则需要访问TransactionManager |
| PROPAGATION_NOT_SUPPORTED | 表示该方法不应该运行在事务中。如果存在当前事务，在该方法运行期间，当前事务将被挂起。如果使用JTATransactionManager的话，则需要访问TransactionManager |
|     PROPAGATION_NEVER     | 表示当前方法不应该运行在事务上下文中。如果当前正有一个事务在运行，则会抛出异常 |
|    PROPAGATION_NESTED     | 表示如果当前已经存在一个事务，那么该方法将会在嵌套事务中运行。嵌套的事务可以独立于当前事务进行单独地提交或回滚。如果当前事务不存在，那么其行为与PROPAGATION_REQUIRED一样。注意各厂商对这种传播行为的支持是有所差异的。可以参考资源管理器的文档来确认它们是否支持嵌套事务 |



**注：以下具体讲解传播行为的内容参考自[Spring事务机制详解](https://www.open-open.com/lib/view/open1350865116821.html)**

###### 1）PROPAGATION_REQUIRED

- 如果存在一个事务，则支持当前事务。如果没有事务，则开启新的事务
- <font color="red">ROPAGATION_REQUIRED应该是我们首先的事务传播行为。它能够满足我们大多数的事务需求。 </font>

```java
//事务属性 PROPAGATION_REQUIRED
methodA{
    ……
    methodB();
    ……
}

```

```java
//事务属性 PROPAGATION_REQUIRED
methodB{
   ……
}

```

- **使用spring声明式事务，spring使用AOP来支持声明式事务，会根据事务属性，自动在方法调用之前决定是否开启一个事务，并在方法执行之后决定事务提交或回滚事务**



- 单独调用methodB方法

  ```java
  Main{ methodB(); }
  
  ```

  相当于

  ```java
  Main{ 
      Connection con=null; 
      try{ 
          con = getConnection(); 
          con.setAutoCommit(false); 
          //方法调用
          methodB(); 
          //提交事务
          con.commit(); 
      } Catch(RuntimeException ex) { 
          //回滚事务
          con.rollback();   
      } finally { 
          //释放资源
          closeCon(); 
      } 
  } 
  
  ```

  - Spring保证`methodB`方法中所有的调用都或得到相同的连接
  - 由于示例中在调用`methodB`时没有事务存在，根据传播行为`PROPAGATION_REQUIRED`的含义，它获得了一个新的连接，开启了一个新的事务



- 单独调用`methodA`时，`methodA`内部会调用`mehodB`

  执行效果相当于

  ```java
  Main{ 
      Connection con = null; 
      try{ 
          con = getConnection(); 
          methodA(); 
          con.commit(); 
      } catch(RuntimeException ex) { 
          con.rollback(); 
      } finally {    
          closeCon(); 
      }  
  } 
  
  ```

  - 调用`methodA`时，环境中没有事务，所以开启一个新的事务，当在`methodA`中调用`methodB`时，环境中已经有了刚刚由于`methodA`创建的事务，所以把`methodB`加入那个事务中



###### 2）PROPAGATION_SUPPORTS

- 如果存在一个事务，支持当前事务
- 如果没有事务，则非事务的执行方法
- 但是对于事务同步的事务管理器，PROPAGATION_SUPPORTS与不使用事务有少许不同

```java
//事务属性 PROPAGATION_REQUIRED
methodA(){
  methodB();
}

//事务属性 PROPAGATION_SUPPORTS
methodB(){
  ……
}

```

- 单纯的调用`methodB`时，`methodB`方法非事务的执行
- 当调用`methodA`时，`methodB`则加入`methodA`活动的事务中，并事务的执行



###### 3）PROPAGATION_MANDATORY

- 如果已经存在一个事物，则支持该事务
- 如果没有活动的事务，则抛出异常

```java
//事务属性 PROPAGATION_REQUIRED
methodA(){
    methodB();
}

//事务属性 PROPAGATION_MANDATORY
    methodB(){
    ……
}

```

- 当单独调用`methodB`时，因为无活动事务，则会抛出异常`throw new IllegalTransactionStateException(“Transaction propagation ‘mandatory’ but no existing transaction found”);`
- 当调用`methodA`时，`methodB`则加入到`methodA`的事务中，事务的执行



###### 4）PROPAGATION_REQUIRES_NEW

- 总是开启一个新的事务，如果一个事务已经存在，则将这个存在的事务挂起
- 使用`PROPAGATION_REQUIRES_NEW`，需要使用`JtaTransactionManager`作为事务管理器

```java
//事务属性 PROPAGATION_REQUIRED
methodA(){
    doSomeThingA();
    methodB();
    doSomeThingB();
}

//事务属性 PROPAGATION_REQUIRES_NEW
methodB(){
    ……
}

```

- 调用`methodA`相当于

  ```java
  main(){
      TransactionManager tm = null;
      try{
          //获得一个JTA事务管理器
          tm = getTransactionManager();
          tm.begin();//开启一个新的事务
          Transaction ts1 = tm.getTransaction();
          doSomeThing();
          tm.suspend();//挂起当前事务
          try{
              tm.begin();//重新开启第二个事务
              Transaction ts2 = tm.getTransaction();
              methodB();
              ts2.commit();//提交第二个事务
          } Catch(RunTimeException ex) {
              ts2.rollback();//回滚第二个事务
          } finally {
              //释放资源
          }
          //methodB执行完后，恢复第一个事务
          tm.resume(ts1);
          doSomeThingB();
          ts1.commit();//提交第一个事务
      } catch(RunTimeException ex) {
          ts1.rollback();//回滚第一个事务
      } finally {
          //释放资源
      }
  }
  
  ```

- 这里把`ts1`成为外层事务，`ts2`称为内层事务，`ts2`与`ts1`时两个独立的事务，互不相干。`ts2`是否成功并不依赖于`ts1`

  - 如果`methodA`方法在调用`methodB`方法之后的`doSomeThingB`方法失败了，而`methodB`方法所做的结果依然被提交，除了`methodB`之外的其他代码导致的结果却被回滚了

- 使用`PROPAGATION_REQUIRES_NEW`，需要使用`JtaTransactionManager`作为事务管理器

###### 5）PROPAGATION_NOT_SUPPORTED

- 总是非事务的执行，并挂起任何存在的事务
- 使用`PROPAGATION_NOT_SUPPORTED`，也需要`JtaTransactionManager`作为事务管理器（代码示例同上）

###### 6）PROPAGATION_NEVER

- 总是非事务地执行，如果存在一个活动事务，则抛出异常

###### 7）PROPAGATION_NESTED

- 如果没有活动事务，按照`TransactionDefinition.PROPAGATION_REQUIRED`属性执行
- 如果一个活动的事务存在，则运行在一个嵌套的事务中
- 嵌套事务一个非常重要的概念就是内层事务依赖于外层事务。
  - 外层事务失败时，会回滚内层事务所做的动作。
  - 而内层事务操作失败并不会引起外层事务的回滚。
- 这是一个嵌套事务，使用JDBC3.0驱动时，仅仅支持`DataSourceTransactionManager`作为事务管理器
  - 需要JDBC的驱动`java.sql.Savepoint`类
  - 有一些JTA的事务管理器实现可能提供了相同的功能
- 使用`PROPAGATION_NESTED`，还需要把`PlatformTransactionManager`的`nestedTransactionAllowed`属性设为`true`；而 `nestedTransactionAllowed`属性值默认为`false`。

```java
//事务属性 PROPAGATION_REQUIRED
methodA(){
    doSomeThingA();
    methodB();
    doSomeThingB();
}

//事务属性 PROPAGATION_NESTED
methodB(){
    ……
}

```

- 如果单独调用`methodB`方法，则按照`REQUIRED`属性执行

- 如果调用`methodA`方法，相当于下面效果

  ```java
  Main(){
      Connection con = null;
      Savepoint savepoint = null;
      try{
          con = getConnection();
          con.setAutoCommit(false);
          doSomeThingA();
          savepoint = con2.setSavepoint();
          try{
              methodB();
          } catch(RuntimeException ex) {
              con.rollback(savepoint);
          } finally {
              //释放资源
          }
          doSomeThingB();
          con.commit();
      } catch(RuntimeException ex) {
          con.rollback();
      } finally {
          //释放资源
      }
  }
  
  ```

  - 当`methodB`方法调用之前，调用`setSavePoint`方法，保存当前的状态到`savepoint`
  - 如果`methodB`方法调用失败，则恢复到之前保存的状态，值得注意的是：这时的事务并没有提交，如果后续的代码（`doSomeThingB()`方法）调用失败，则回滚包括`methodB`方法的所有操作

- PROPAGATION_NESTED 与PROPAGATION_REQUIRES_NEW的异同：

  - 他们非常类似，都像一个嵌套事务，如果不存在活动的事务，都会开启新的事务



|                      |                使用PROPAGATION_REQUIRES_NEW时                |                使用PROPAGATION_REQUIRES_NEW时                |
| :------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|   内外事务回滚关系   | 内层事务和外层事务就像两个独立的事务一样，一旦内层事务进行提交之后外层事务不能对其进行回滚 | 外层事务的回滚可以引起内层事务的回滚，内层事务的回滚不会导致外层事务的回滚 |
| 是否为真正的嵌套事务 |                              否                              |                              是                              |
|         支持         |                  它需要JTA事务管理器的支持                   | DataSourceTransactionManager使用savepoint支持。需要JDBC 3.0以上驱动及1.4以上的JDK版本支持。其它的JTA TrasactionManager实现可能有不同的支持方式。 |
|                      |                                                              |                                                              |



- `PROPAGATION_REQUIRES_NEW`创建一个新的、不依赖于环境的“内部”事务。
  - 这个事务将被完全`commited` 或 `rolled back` 而不是依赖于外部事务，它拥有自己的隔离范围、自己的锁等等
  - 当内部事务开始执行时，外部事务将被挂起，内部事务结束时，外部事务将继续执行
- `PROPAGATION_NESTED`开始一个“嵌套”事务，他是已经存在事务的真正的子事务
  - 嵌套事务开始执行时，他将取得一个`savepoint`，如果这个嵌套事务执行失败，我们将回滚到此`savepoint`
  - 嵌套事务是外部事事务的一部分，只有外部事务结束后它才会被提交
- `PROPAGATION_REQUIRES_NEW`和`PROPAGATION_NESTED`的最大区别在于
  - 前者完全是一个新的事务
  - 后者则是外部事务的子事务，如果外部事务`commit`，嵌套事务也会会被`commit`，这个规则同样适用于 `roll back`
- <font color="red">ROPAGATION_REQUIRED应该是我们首先的事务传播行为。它能够满足我们大多数的事务需求。</font>

##### （2 隔离级别

- 事务第二个维度就是隔离级别（isolation level）
- 隔离级别定义了一个事物可能受其他并发事务的影响程度

###### 1）并发事务引起的问题

- 在典型的应用程序中，多个事务并发运行，经常会操作相同的数据来完成各自的任务
- 并发虽然是必须的，但可能会导致以下的问题

| 问题       | 英文              | 含义                                                         |
| ---------- | ----------------- | ------------------------------------------------------------ |
| 脏读       | Dirty reads       | 脏读发生在一个事物读取了另一个事务“改写但尚未提交的数据”时，如果改写在稍后被回滚了，那么第一个事务获取的数据就是无效的 |
| 不可重复读 | Norepeatable read | 不可重复读发生在“一个事务执行相同的查询两次或两次以上，但是每次都得到不同的数据”时。这通常是因为另一个并发事务在两次查询期间进行了更新 |
| 幻读       | Phantom read      | 幻读与不可重复度类似，它发生在一个事务（T1）读取了几行数据，接着另一个并发事务（T2）插入了一些数据时。在随后的查询中，第一个事务（T1）就会发现多了一些原本不存在的记录 |

###### 2）不可重复读和幻读的区别

- <font color="red">不可重复读的重点是修改</font>	
- <font color="red">幻读的重点是插入删除数据</font>



- 不可重复读

  - 不可重复读的重点是修改：同样的条件，第一次读取和第二次读取的结果不一样了

  例如：在事务1种，Mary读取了自己的工资为1000，操作并没有完成

  ```sql
  con1 = getConnection();
  select salary from employee empid="Mary";
  
  ```

  在事务2中，这时财务人员修改了Mary的工资为2000，并提交了事务

  ```sql
  con2 = getConnection();
  update employee set salary=2000;
  con2.commit();
  
  ```

  在事务1中，Mary再次读取自己的工资时，工资变为了2000

  ```sql
  // con1
  select salary from employee empId="Mary";
  
  ```

  在一个事务中前后两次读取的结果并不一致，导致的不可重复读



- 幻读

  - 幻读的重点在于新增或者删除

  同样的条件，第一次第二次读出来的记录个数不一样

  例如：目前工资为1000的员工有10人。事务1，读取所有工资为1000的员工，读取结果为10条记录

  ```sql
  con1 = getConnection();
  select * from employee where salary=1000;
  
  ```

  这时另一个事务向employee表中插入了一条员工记录，工资也为1000

  ```sql
  con2 = getConnection();
  insert into employee(empid,salary) values("Lili",1000);
  con2.commit();
  
  ```

  事务1再次读取所有工资为1000的员工，这时读取到的记录数量是11，这就产生了幻读

  ```sql
  //con1
  select * from employee where salary=1000;
  
  ```

- 不同角度看待不可重复读与幻读的区别

  - 从总的结果来看，似乎不可重读读和幻读都表现为两次读取的结果不一致。
  - 但是从控制的角度来看，两者的区别就比较大
    - 对于前者，只需要锁住满足条件的记录
    - 对于后者，要锁住满足条件及其相近的记录

###### 3）隔离级别

|          隔离级别          |                             含义                             | 个人见解                                                     |
| :------------------------: | :----------------------------------------------------------: | ------------------------------------------------------------ |
|     ISOLATION_DEFAULT      |                 使用后端数据库默认的隔离级别                 |                                                              |
| ISOLATION_READ_UNCOMMITTED | 最低的隔离级别，允许读取尚未提交的数据变更，可能会导致脏读、幻读或不可重复读 | 查询时可以有任何事务对数据操作，可以查询没有提交的数据       |
|  ISOLATION_READ_COMMITTED  | 允许读取并发事务已经提交的数据，可以阻止脏读，但是幻读或不可重复读仍有可能发生 | 查询时只能查询已经提交的数据                                 |
| ISOLATION_REPEATABLE_READ  | 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，可以阻止脏读和不可重复读，但幻读仍有可能发生 | 在上面的基础上，查询时不能有修改被查询数据的事务对被查询数据操作 |
|   ISOLATION_SERIALIZABLE   | 最高的隔离级别，完全服从ACID的隔离级别，确保阻止脏读、不可重复读以及幻读，也是最慢的事务隔离级别，因为它通常是通过完全锁定事务相关的数据库表来实现的 | 在上面的基础上，查询时不能有插入删除被查询数据的事务对被查询数据操作 |



##### （3  只读

- 事务的第三个特性是它是否只读
- 如果事务只对后端的数据库进行该操作，数据库可以利用事务的制度特性来进行一些特定的优化
- 通过将事务设置为只读，你就可以给数据库一个机会，让它应用它认为合适的优化措施
- **注意：**
  - 事务的“只读”属性，不同的数据库厂商支持不同，通常而言只读属性的应用要参考厂商的具体支持说明，比如
    - <font color="red">Orical的`readOnly`不起作用，不影响其增删改查</font>
    - MySql的`readOnly`为`true`时只能查，增删改操作会抛出异常



##### （4 事务超时

- 为了使应用程序很好的运行，事务不能运行太长时间
  - 因为事务可能涉及对后端数据库的锁定，所以长时间的事务会不必要的占用数据库资源
- 事务超时就是一个定时器，在特定时间内事务如果没有执行完毕就会自动回滚，而不是一直等待其结束



##### （5 回滚规则

- 回滚规则定义了”哪些异常会导致事务回滚而哪些不会“
- 默认情况下，事务只有遇到运行期异常时才会回滚，而在遇到检查型异常时不会回滚
  - 这一行为和EJB的回滚行为是一致的
  - 检查型异常：编译器发现的异常，可能为语法错误
  - 运行时错误：程序运行时抛出的异常，编译器不会检查这类异常，比如数组越界、访问null对象，这种错误你自己是可以避免的，编译器不会强制你检查这种异常
- 可以设置事务在遇到特定的检查型对象时遇到运行期异常那样回滚，同样也可以设置遇到特定的异常不回滚（即使这些异常是运行期异常）



#### 4）事务状态

- 上面讲到的调用`PlatformTransactionManager`接口的`getTransaction()`的方法得到的是`TransactionStatus`接口的一个实现，这个接口如下

  ```java
  public interface TransactionStatus{
      boolean isNewTransaction(); // 是否是新的事物
      boolean hasSavepoint(); // 是否有恢复点
      void setRollbackOnly();  // 设置为只回滚
      boolean isRollbackOnly(); // 是否为只回滚
      boolean isCompleted; // 是否已完成
  } 
  
  ```

  - 可以发现这个接口描述的是一些为处理事务提供简单的控制事务执行和查询事务状态的方法，在回滚或提交的时候需要应用对应的事务状态

### 5. Spring 编程式事务与声明式事务

#### 1）编程式事务和声明式事务的区别

- Spring提供了对编程式事务和声明式事务的支持
- 编程式事务允许用户在代码中精确定义事务的边界
- 声明式事务有助于用户将操作与事务规则进行解耦
  - 基于AOP交由Spring容器实现
  - 实现关注点在业务逻辑上
- 编程式事务侵入到了业务代码里，但是提供了更加详细的事务管理
- 声明式事务由于基于AOP，所以既能起到事务管理的作用，又可以不影响业务代码的具体实现

#### 2）编程式事务

##### （1 模板事务方式：使用`TransactionTemplate`

- 此为Spring官方团队推荐的编程式事务管理方式

- 主要工具类为`JdbcTemplate`类

- 采用`TransactionTemplate`和采用其他Spring框架，如<font color="red">`JdbcTemplate`</font>和`Hibernate`是一样的方法

- 它使用回调函数（“`TransactionCallback`类或`TransactionCallbackWithoutResult`类”中的`doTransaction`函数），把应用程序从处理取得和释放资源中解脱出来

- 如同其他模板，`TransactionTemplate`是线程安全的

- 实现方法

  - 步骤
    - 获取事务模板（`TransactionTemplate`）对象
    - （可选）设置事务属性，如隔离级别、超时时间等
    - 选择事务结果类型、有无返回值
    - 业务数据操作处理
  - 实现代码片段

  ```java
  TransactionTemplate tt = new TransactionTemplate(); // 新建一个TransactionTemplate
  // 可设置事务属性，如隔离级别、超时时间等,如：
  // tt.setIsolationLevel(TransactionDefinition.ISOLATION_READ_UNCOMMITTED);
  Object result = tt.execute(
      new TransactionCallback(){  
          public Object doTransaction(TransactionStatus status){  
              updateOperation();  
              // 数据库操作1
              // JdbcTemplate jdbcTemplate = new JdbcTemplate(数据源);
              // jdbcTemplate.execute(sql);
              
              // 数据库操作2：简单模板化新增数据
              //SimpleJdbcInsert simpleInsert = new getSimpleJdbcTemplate(数据源);
              //simpleInsert.withTableName("books").usingColumns("isbn", "name", "price", "pubdate");
              //Map<String, Object> parameters = new HashMap<String, Object>();
              //parameters.put("isbn", book.getIsbn());
              //parameters.put("name", book.getName());
              //parameters.put("price", book.getPrice());
              //parameters.put("pubdate", book.getPubdate());
              //simpleInsert.execute(parameters);
              //System.out.println("新增数据成功！");
              
              // 数据库操作3；DAO数据操作模式：
              // BookDAO.save(book);
              
              return resultOfUpdateOperation();  
  	}  
  }); // 执行execute方法进行事务管理
  
  ```

  - 使用`TransactionCallback`可以返回一个值，使用`TransactionCallbackWithoutResult`无返回值

  

##### （2 平台事务管理器方式：使用`PlatformTransactionManager`

- 类似于`JTA UserTransaction API`方式，但异常处理更简洁

- 辅助类为`TransactionDefinition`和`TransactionStatus`

- 实现方法

  - 步骤

    - 获取事务管理器
    - 获取事务属性对象
    - 获取业务状态对象
    - 创建JDBC模板对象
    - 业务数据操作处理

  - 实现代码片段

    ```java
    // 编程式事务管理：事务管理器PlatformTransactionManager方式实现
    public void updateBookByIsbn(Book book) {
        //第一步：获取JDBC事务管理器
        DataSourceTransactionManager dtm = new DataSourceTransactionManager(数据源);
        // 第二步：创建事务管理器属性对象
        DefaultTransactionDefinition transDef = new DefaultTransactionDefinition(); // 定义事务属性
        // 根据需要，设置事务管理器的相关属性
        // 如设置传播行为属性
        transDef.setPropagationBehavior(DefaultTransactionDefinition.PROPAGATION_REQUIRED);
        // 第三步：获得事务状态对象(getTransaction会自动开启事务并返回该事务的状态对象)
        TransactionStatus ts = dtm.getTransaction(transDef);
        // 第四步：基于当前事务管理器,获取数据源，创建操作数据库的JDBC模板对象
        JdbcTemplate jt = new JdbcTemplate(dtm.getDataSource());
        try {//第五步：业务操作
            jt.update("update books set price="+book.getPrice()+",name='"+book.getName()
                      +"'  where isbn='"+book.getIsbn()+"' ");
            // 其它数据操作如增删
            //第六步：提交事务
            dtm.commit(ts); // 如果不commit，则更新无效果
        } catch (Exception e) {
            dtm.rollback(ts);
            e.printStackTrace();
        }
    }
    
    ```

    - `DataSourceTransactionManager.getTransaction()`方法会自动开启事务并返回该事务的状态对象

##### （3 小结

- 需要有效的数据源，具体数据源根据实际情况创建
- 创建编程事务管理对象：
  - 事务模板（`TransactionTemplate`）
  - 事务管理器（`PlatefromTransactionManager`）
- 业务逻辑处理
  - 基于`JdbcTemplate`完成业务处理

#### 3）声明式事务

##### （1 配置方式

根据代理机制不同，有以下五种Spring事务配置方式

- 独立代理：每个Bean都有一个代理
- 共享代理：所有Bean共享一个代理基类
- 拦截器
- tx拦截器
- 全注释：完全依靠注解（@Transactional）设置事务，配置文件中不做事务配置



- 本小节配置均为Hibernate示例
  - 需要配置`sessionFactory`
  - 如果是JDBC示例，需要配置数据源

###### 1）独立代理

- 每个Bean都有一个代理

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context-2.5.xsd
           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd">

     <!-- （Hibernate声明式的事务，如果是JDBC需要配数据源） --> 
    <bean id="sessionFactory" 
            class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"> 
        <property name="configLocation" value="classpath:hibernate.cfg.xml（数据库连接的相关配置文件）" /> 
        <property name="configurationClass" value="org.hibernate.cfg.AnnotationConfiguration" />
    </bean> 

    <!-- 定义事务管理器（Hibernate声明式的事务） --> 
    <bean id="transactionManager"
        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
        <property name="sessionFactory" ref="sessionFactory" />
    </bean>
    
    <!-- 定义事务管理器（JDBC声明式的事务） -->
    <!--
	<bean id="transactionManagerr"
        class="org.springframework.jdbc.datasource.DataSourceTransactionManager">  
        <property name="dataSource" ref="dataSource" />	
    </bean>  
	-->

    <!-- 配置DAO -->
    <bean id="userDaoTarget" class="com.bluesky.spring.dao.UserDaoImpl">
        <property name="sessionFactory" ref="sessionFactory" />
    </bean>

    <bean id="userDao" 
        class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean"> 
         <!-- 配置事务管理器 --> 
        <property name="transactionManager" ref="transactionManager" />   
        <!--target属性指向被代理的对象-->
        <property name="target" ref="userDaoTarget" /> 
        <property name="proxyInterfaces" value="com.bluesky.spring.dao.GeneratorDao" />
        <!-- 配置事务属性 --> 
        <property name="transactionAttributes"> 
            <props> 
                <!-- *对于所有方法 -->
                <prop key="*">PROPAGATION_REQUIRED</prop>
            </props> 
        </property> 
    </bean> 
</beans>

```

- 这种方法为每一个DAO（既上述xml中的DaoTarget）配置一个代理（既上述xml中的Dao），这个代理中完成事务管理器和事务属性的配置，并指向所代理的DAO（`DaoTarget`）



###### 2）共享代理

- 所有Bean共享一个代理基类

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context-2.5.xsd
           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd">

    <bean id="sessionFactory" 
            class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"> 
        <property name="configLocation" value="classpath:hibernate.cfg.xml（数据库连接的相关配置文件）" /> 
        <property name="configurationClass" value="org.hibernate.cfg.AnnotationConfiguration" />
    </bean> 

    <!-- 定义事务管理器（声明式的事务） --> 
    <bean id="transactionManager"
        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
        <property name="sessionFactory" ref="sessionFactory" />
    </bean>

    <bean id="transactionBase" 
            class="org.springframework.transaction.interceptor.TransactionProxyFactoryBean" 
            lazy-init="true" abstract="true"> 
        <!-- 配置事务管理器 --> 
        <property name="transactionManager" ref="transactionManager" /> 
        <!-- 配置事务属性 --> 
        <property name="transactionAttributes"> 
            <props> 
                <!--*设置所有方法都为REQUIRED-->
                <prop key="*">PROPAGATION_REQUIRED</prop> 
            </props> 
        </property> 
    </bean>   

    <!-- 配置DAO -->
    <bean id="userDaoTarget" class="com.bluesky.spring.dao.UserDaoImpl">
        <property name="sessionFactory" ref="sessionFactory" />
    </bean>

    <bean id="userDao" parent="transactionBase" > 
        <property name="target" ref="userDaoTarget" />  
    </bean>
</beans>

```

- 这种方法为每一个DAO（`DaoTarget`）创造一个代理（`Dao`），这个代理继承公共代理（`transactionBase`），完成事务管理器和事务属性的配置在公共代理中配置，而继承的代理只要指定代理目标和修改自身代理的特殊部分即可



###### 3）使用拦截器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context-2.5.xsd
           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd">

    <!--配置Hibernate需要使用的sessionFactory-->
    <bean id="sessionFactory" 
            class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"> 
        <property name="configLocation" value="classpath:hibernate.cfg.xml（数据库连接的相关配置文件）" /> 
        <property name="configurationClass" value="org.hibernate.cfg.AnnotationConfiguration" />
    </bean> 

    <!-- 定义事务管理器（HIbernate声明式的事务） --> 
    <bean id="transactionManager"
        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
        <property name="sessionFactory" ref="sessionFactory" />
    </bean> 

    <!--配置事务拦截-->
    <bean id="transactionInterceptor" 
        class="org.springframework.transaction.interceptor.TransactionInterceptor"> 
        <property name="transactionManager" ref="transactionManager" /> 
        <!-- 配置事务属性 --> 
        <property name="transactionAttributes"> 
            <props> 
                <prop key="*">PROPAGATION_REQUIRED</prop> 
            </props> 
        </property> 
    </bean>

    <!--根据名称自动代理-->
    <bean class="org.springframework.aop.framework.autoproxy.BeanNameAutoProxyCreator"> 
        <property name="beanNames"> 
            <list> 
                <!--自动代理所有以“Dao”结尾的Bean-->
                <value>*Dao</value>
            </list> 
        </property> 
        <property name="interceptorNames"> 
            <list> 
                <value>transactionInterceptor</value> 
            </list> 
        </property> 
    </bean> 

    <!-- 配置DAO -->
    <bean id="userDao" class="com.bluesky.spring.dao.UserDaoImpl">
        <property name="sessionFactory" ref="sessionFactory" />
    </bean>
</beans>

```



###### 4）tx标签配置的拦截器

- 使用Spring AOP切面、Spring的切点表达式

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context-2.5.xsd
           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd
           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd">

    <!--开启注解-->
    <context:annotation-config />
    <!--开启包扫描，用于自动注入-->
    <context:component-scan base-package="com.bluesky" />

    <bean id="sessionFactory" 
            class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"> 
        <property name="configLocation" value="classpath:hibernate.cfg.xml" /> 
        <property name="configurationClass" value="org.hibernate.cfg.AnnotationConfiguration" />
    </bean> 

    <!-- 定义事务管理器（声明式的事务） --> 
    <bean id="transactionManager"
        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
        <property name="sessionFactory" ref="sessionFactory" />
    </bean>

    <!--配置通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="*" propagation="REQUIRED" />
        </tx:attributes>
    </tx:advice>

    <aop:config>
        <!--配置切点-->
        <aop:pointcut id="interceptorPointCuts"
            expression="execution(* com.bluesky.spring.dao.*.*(.))" />
        	<!--切点表达式-->
        
        <!--配置切面-->
        <aop:advisor advice-ref="txAdvice"
            pointcut-ref="interceptorPointCuts" />
    </aop:config>     
</beans>

```



###### 5）全注解

- 完全依靠注解（@Transactional）设置事务，配置文件中不做事务配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:context="http://www.springframework.org/schema/context"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:tx="http://www.springframework.org/schema/tx"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans-2.5.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context-2.5.xsd
           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd
           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd">

    <!--开启注解-->
    <context:annotation-config />
    <!--开启包扫描-->
    <context:component-scan base-package="com.bluesky" />

    <!--开启tx注解（@Transactional）-->
    <tx:annotation-driven transaction-manager="transactionManager"/>

    <bean id="sessionFactory" 
            class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"> 
        <property name="configLocation" value="classpath:hibernate.cfg.xml" /> 
        <property name="configurationClass" value="org.hibernate.cfg.AnnotationConfiguration" />
    </bean> 

    <!-- 定义事务管理器（声明式的事务） --> 
    <bean id="transactionManager"
        class="org.springframework.orm.hibernate3.HibernateTransactionManager">
        <property name="sessionFactory" ref="sessionFactory" />
    </bean>
</beans>

```

- 然后在DAO上需要加上@Transactional注解

```java
@Transactional
@Component("userDao")
public class UserDaoImpl extends HibernateDaoSupport implements UserDao {

    public List<User> listUsers() {
        return this.getSession().createQuery("from User").list();
    }  
}

```

##### （2 全注解方式@Transactional属性

- @Transaction是全注解方式使用的事务配置

  - 用来注解是事务的方法

  - 可以设置事务属性

    |     属性      |                             含义                             |
    | :-----------: | :----------------------------------------------------------: |
    |  propagation  | 指定事务的传播行为，即当前的事务方法被另外一个事务方法调用时如何使用事务<br>* 默认取值为REQUIRED，即使用调用方法的事务<br>*也经常使用REQUIRES_NEW：使用自己的事务，调用的事务方法的事务被挂起 |
    |   isolation   |       指定事务的隔离级别，最常用的取值为READ_COMMITTED       |
    | noRollbackFor | 默认情况下 Spring 的声明式事务对所有的运行时异常进行回滚，也可以通过对应的属性进行设置。通常情况下，默认值即可 |
    |   readOnly    | 指定事务是否为只读。 表示这个事务只读取数据但不更新数据，这样可以帮助数据库引擎优化事务。若真的是一个只读取数据库值的方法，应设置readOnly=true |
    |    timeOut    |              指定强制回滚之前事务可以占用的时间              |

```java
@Transactional(propagation=Propagation.REQUIRES_NEW,
	isolation=Isolation.READ_COMMITTED,
	noRollbackFor={UserAccountException.class},
	readOnly=true, 
	timeout=3)
public void purchase() {
	someOperate();
}

```



##### （3 一个声明式事务的示例

注：该实例参考自[Spring中的事务管理实例详解](https://www.jb51.net/article/57589.htm)

- 首先是数据库表

  ```sql
  book(isbn, book_name, price)
  account(username, balance)
  book_stock(isbn, stock)
  
  ```

- XML配置_全注释

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:context="http://www.springframework.org/schema/context"
  xmlns:aop="http://www.springframework.org/schema/aop"
  xmlns:tx="http://www.springframework.org/schema/tx"
  xsi:schemaLocation="http://www.springframework.org/schema/beans
  http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
  http://www.springframework.org/schema/context
  http://www.springframework.org/schema/context/spring-context-3.0.xsd
  http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-2.5.xsd
  http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-2.5.xsd">
  
      <import resource="applicationContext-db.xml" />
  
      <context:component-scan
          base-package="com.springinaction.transaction">
      </context:component-scan>
  
      <tx:annotation-driven transaction-manager="txManager"/>
  
      <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
          <property name="dataSource" ref="dataSource" />
      </bean>
  
  </beans>
  
  ```

- 使用的类

  - BookShopDao

    ```java
    public interface BookShopDao {
        // 根据书号获取书的单价
        public int findBookPriceByIsbn(String isbn);
        // 更新书的库存，使书号对应的库存-1
        public void updateBookStock(String isbn);
        // 更新用户的账户余额：account的balance-price
        public void updateUserAccount(String username, int price);
    }
    
    ```

  - BookShopDaoImpl

    ```java
    @Repository("bookShopDao")
    public class BookShopDaoImpl implements BookShopDao {
    
        @Autowired
        private JdbcTemplate JdbcTemplate;
    
        @Override
        public int findBookPriceByIsbn(String isbn) {
            String sql = "SELECT price FROM book WHERE isbn = ?";
    
            return JdbcTemplate.queryForObject(sql, Integer.class, isbn);
        }
    
        @Override
        public void updateBookStock(String isbn) {
            //检查书的库存是否足够，若不够，则抛出异常
            String sql2 = "SELECT stock FROM book_stock WHERE isbn = ?";
            int stock = JdbcTemplate.queryForObject(sql2, Integer.class, isbn);
            if (stock == 0) {
                throw new BookStockException("库存不足！");
            }
            String sql = "UPDATE book_stock SET stock = stock - 1 WHERE isbn = ?";
            JdbcTemplate.update(sql, isbn);
        }
    
        @Override
        public void updateUserAccount(String username, int price) {
            //检查余额是否不足，若不足，则抛出异常
            String sql2 = "SELECT balance FROM account WHERE username = ?";
            int balance = JdbcTemplate.queryForObject(sql2, Integer.class, username);
            if (balance < price) {
                throw new UserAccountException("余额不足！");
            }       
            String sql = "UPDATE account SET balance = balance - ? WHERE username = ?";
            JdbcTemplate.update(sql, price, username);
        }
    }
    
    ```

  - BookShopService

    ```java
    public interface BookShopService {
         public void purchase(String username, String isbn);
    }
    
    ```

  - BookShopServiceImpl

    ```java
    @Service("bookShopService")
    public class BookShopServiceImpl implements BookShopService {
    
        @Autowired
        private BookShopDao bookShopDao;
    
        /**
         * 1.添加事务注解
         * 使用propagation 指定事务的传播行为，即当前的事务方法被另外一个事务方法调用时如何使用事务。
         * 默认取值为REQUIRED，即使用调用方法的事务
         * REQUIRES_NEW：使用自己的事务，调用的事务方法的事务被挂起。
         *
         * 2.使用isolation 指定事务的隔离级别，最常用的取值为READ_COMMITTED
         * 3.默认情况下 Spring 的声明式事务对所有的运行时异常进行回滚，也可以通过对应的属性进行设置。通常情况下，默认值即可。
         * 4.使用readOnly 指定事务是否为只读。 表示这个事务只读取数据但不更新数据，这样可以帮助数据库引擎优化事务。若真的是一个只读取数据库值的方法，应设置readOnly=true
         * 5.使用timeOut 指定强制回滚之前事务可以占用的时间。
         */
        @Transactional(propagation=Propagation.REQUIRES_NEW,
                isolation=Isolation.READ_COMMITTED,
                noRollbackFor={UserAccountException.class},
                readOnly=true, timeout=3)
        @Override
        public void purchase(String username, String isbn) {
            //1.获取书的单价
            int price = bookShopDao.findBookPriceByIsbn(isbn);
            //2.更新书的库存
            bookShopDao.updateBookStock(isbn);
            //3.更新用户余额
            bookShopDao.updateUserAccount(username, price);
        }
    }
    
    ```

  - Cashier

    ```java
    public interface Cashier {
        public void checkout(String username, List<String>isbns);
    }
    
    ```

  - CashierImpl：CashierImpl.checkout和bookShopService.purchase联合测试了事务的传播行为

    ```java
    @Service("cashier")
    public class CashierImpl implements Cashier {
        @Autowired
        private BookShopService bookShopService;
        @Transactional
        @Override
        public void checkout(String username, List<String> isbns) {
            for(String isbn : isbns) {
                bookShopService.purchase(username, isbn);
            }
        }
    }
    
    ```

  - BookStockException（继承`RuntimeException`）

  - UserAccountException（继承`RuntimeException`）

  - 测试（Test）类

#### 4）编程式事务和声明式事务的最佳实践

##### （1 两种事务的选择

- 小型应用，事务操作少：
  - 建议使用**编程式事务管理**实现：TransactionTemplate
  - 简单、显式操作、直观明显、可以设置事务名称
- 大型事务，事务操作量多：
  - 业务复杂度高、关系性紧密，建议**声明式事务管理**实现
  - 关注点聚焦到业务层面，实现业务和事务的解耦

### 6. 问题

- 关于事务的说法
  - 通过事务可以保证数据库中的数据从一种状态转化为另一种状态，保证数据操作全部成功或者全部失败
  - 事务正确提交之后，事务可以永久的保存在数据库中
- 在事务没有正确提交之前，其他事物不能获取它应显示的结果。这一特性被成为事务的
  - 隔离性
- 关于Java事务的说法
  - Java事务的范围是保证事务要不全部执行成功，要么撤销不执行
  - Java事务产生的原因是Java要操作数据库
  - Java事务的原理就是确保数据库事务的ACID特性
  - Java事务除了JDBC事务和容器事务之外，还有JTA事务
- 关于JTA事务的说法
  - JTA的全称是：Java Transaction API
  - JTA事务可以跨越多个数据库和DAO层，功能强大
  - JTA是与实现和协议都无关的API
- Spring事务的三个核心接口是
  - TransactionDefinition
  - PlatformTransactionManager
  - TransactionStatus
- 关于事务传播的说法
  - Spring提供了7种传播行为
  - 当事务方法被另一个方法调用时，必须指定事务应该如何传播
  - 在编写代码时，可以根据需要进行传播行为的设定
  - 传播行为PROPAGATION_REQUIRED表示当前方法必须运行在事务中
- 关于事务管理的说法
  - 声明式事务的事务管理代码不需要写在逻辑代码中（写在配置文件或注解中）
  - 声明式事务是基于AOP模式机制完成的
  - 声明式事务是对方法**前后**进行拦截的
  - 声明式事务的优点就是在逻辑代码中不需要掺杂事务代码
- 关于编程式事务和声明式事务的说法
  - 声明式事务有助于用户将操作和事务规则进行解耦
  - 编程式事务允许用户在代码中精确的定义事务的边界

## 七. Spring入门相关问题

- 关于IOC的理解

  - 控制反转
  - 对象被动的接受依赖项

- 在Spring中，已知Student类的定义，以下属于无参数构造器的方式实例化Bean

  - `<bean id="bean1" class="com.tonited.demo.Student">`

- 关于事务隔离的说法

  - 事务隔离级别定义了一个事务可能受其他并发事务影响的程度
  - `ISOLATION_SERIALIZABLE`是最高的隔离级别，可以防止脏读、不可重复读、幻读，也是最慢的事务隔离级别

- 在Spring中，Bean在配置时，初始化和销毁方法配置在`<bean ./>`中的标签名是`init-method`和`destory-method`

- 关于事务的说法

  - 小型应用、事务操作少时建议采用编程式事务的TransactionTeplate方式来完成
  - 大型应用，事务操作量最多，关联性紧密的应该采用声明式事务来完成
  - 声明式事务可以实现事务和业务的解耦

- Spring能够实现各个组件之间的解耦，涉及到的解耦原理是

  - 反射模式
  - 配置文件
  - 工厂模式

- 事务在处理时具有不可分割性，要么全部被执行、要么全部不执行，这一特性是事务的原子性

- 以关于Spring IOC定义的说法

  - 创建userService的控制权被反转到了Spring框架
  - IOC的作用就是将原本在程序中创建userService对象的控制权，交由Spring完成
  - IOC的英文全称是Inverse of Control，控制反转

- 已下载Spring开发版本，其中Spring的开发规范和API文档存在于`docs`目录下

- Spring事务的三个核心接口是

  - `TransactionDefinition`
  - `TransactionStatus`
  - `PlatformTransactionManager`

- 关于Spring的说法

  - Spring不是持久层的框架
  - Spring支持声明式事务的支持，通过配置就可以完成事务的管理
  - Spring支持面向切面编程，实现对程序的拦截与监控
  - Spring可以实现模块间的解耦，对象的创建可以直接由Spring来完成

- 在SpringBean注入中，如果需要对Map进行数据注入，需要用到的标签

  - `<entry>`
  - `<map>`
  - `<property>`

  ![1566040456028](https://gitee.com//tonited/Spring-Notes/raw/master/assets/1566040456028.png)

- 在Spring中，如果bean没有配置scope属性的值，那么默认的作用域是singleton

- 关于Bean的和id的name说法

  - 装配一个Bean时，通过指定一个id属性作为Bean的名称
  - name属性在IOC容器中可以不唯一
  - id属性在IOC容器中必须唯一
  - 通过name属性作为Bean名称时，可以包含特殊字符，id属性不可以

- 关于事务的说法

  - 通过事务可以保证数据库中的数据从一种状态转化为另一种状态，保证数据全部成功或者全部失败
  - 事务正确提交后，数据可以永久的保存在数据库中
  - 事务提交后，不可进行回滚

- 关于JDBC事务的说法

  - JDBC只能完成对数据库的事务操作
  - JDBC时通过Connection对象控制的
  - JDBC事务包括手动模式和自动模式两种情况
  - JDBC不能跨越多个数据库，JTA可以

- 事务属性范围包括

  - 只读
  - 事务超时
  - 回滚规则
  - 隔离规则
  - 传播行为

- 在Spring中，下列关于依赖注入的说法

  - 依赖注入的目的时在代码之外管理程序间的依赖关系

- 关于事务传播行为的说法

  - Spring提供了7种事务的传播行为
  - 当事务方法被另一个事务方法调用时，必须指定事务应该如何传播
  - 在编写代码时，可以根据需要进行传播行为的设定
  - 传播行为PROPAGATION_REQUIRED表示当前方法必须运行在事务中

- Spring IOC的底层是用反射模式和工厂模式来完成的