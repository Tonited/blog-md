---
titile: Java字符串常量池
author: Tonited
date: 2020.02.19
keywords: Java字符串常量池
img: https://img-blog.csdnimg.cn/20200219113902129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70
categories: 编程知识
---

Java中对字符串进行比较，`==`比较的是两个字符串的地址，叫做浅比较；`.equal`方法比较的是值，叫深比较

但是下面代码的结果确使`true`
```java
String s1 = "Hello";
String s2 = "Hello";
System.out.println(s1 == s2);
```
使用相同的字符常量创建的字符串（**即不是用new创建出来的**）会指向同一个地址的字符串

这其实是因为Java节约资源的一种方式**字符串常量池**

## 字符串常量池
JVM为了提高性能和减少内存开销，在实例化字符串常量的时候进行了一些优化

字符串类维护了一个字符串池，每当创建字符串常量时，JVM会先检查字符串常量池

如果字符串常量池中有字符串， 就返回池中的那个字符串实例；如果字符串不在池中，就实例化一个字符串并放到池中。
![示意图](https://img-blog.csdnimg.cn/20200219113902129.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

## new方法创建的字符串常量

**需要注意的是，使用`new`创建出来的字符串（如 `new String("Hello")`）不会加入字符串常量池**
但是`new`创造出来的字符串可以调用其`.intern`，如果字符串常量池没有该字符串，它会将该字符串添加进去
![示意图](https://img-blog.csdnimg.cn/2020021911361225.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

<hr/>
参考：http://www.xyzws.com/Javafaq/what-is-string-literal-pool/3