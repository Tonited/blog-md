---
titile: ABA问题
author: Tonited
date: 2020.03.22
keywords: ABA问题
img: https://img-blog.csdnimg.cn/20200320101432366.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70
categories: 多线程与高并发
---

**多图警告!**

## 简介
CAS会出现ABA问题

CAS如果比较发现`预期的数`和`被比较地址所存的值`相等，他就认为`被比较地址所存的值`没有发生变化，但是其实很有可能在比较时，`被比较地址所存的值`是变成了其他的值又变回来的

这就叫ABA问题，ABA这个名字就是说**A先变成B，又变回A**

## 具体问题出现的情况

1. 线程1进行CAS操作，首先线程1读取到`地址Ad`存的值是`D`
![示意图](https://img-blog.csdnimg.cn/20200320101432366.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
2. 线程1的CAS操作还没有进行到比较操作，线程2将数值`C`写入`地址Ad`
![示意图](https://img-blog.csdnimg.cn/20200320101519618.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
3. 线程2又将数值`D`写入`地址Ad`
![示意图](https://img-blog.csdnimg.cn/20200320101951330.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
4. 这时线程2才执行到CAS的比较操作，它将最初读取到的值`D`与`当前地址Ad所存的值`进行对比，发现两值相等，所以CAS认为这个值没有被其他线程修改过，就会继续执行下面的操作
![示意图](https://img-blog.csdnimg.cn/20200320102457398.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
其实这时的值是被修改成其他值后又变回来的
## ABA问题的后果
大家可能会觉得，反正都是同一个值，无所谓，其实这是很危险的问题，举个ABA出现问题的经典例子

存在一个栈，内有`A，B，C`三个值

1. 线程1进行CAS操作：首先读取栈顶的值，得到了`C`
	![示意图](https://img-blog.csdnimg.cn/20200320103603714.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
2. 线程1的CAS还没有执行到比较过程时，线程2接连将`C`和`B`弹出
	![示意图](https://img-blog.csdnimg.cn/20200320104028360.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
3. 此时栈中只剩下A，线程2又将`C`压入栈
![示意图](https://img-blog.csdnimg.cn/2020032010423235.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
4. 这时线程1才执行到CAS的比较操作，他比较之前读取的`C`和栈顶元素，发现相等，它就会认为这个栈没有发生变化，线程1就会继续执行其他操作，最后输出栈的时候发现，莫名奇妙少了一个值！
![示意图](https://img-blog.csdnimg.cn/20200320104521342.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
## 解决方法
可以给每个值定义一个版本，比较时也比较版本号，如果版本号和之前读取的版本号相同就继续进行操作，如果使用了这个方法，别忘了在更新值的时候也要对版本号进行更新

其实还有很多其他的例子，比如定义时间戳，JUC中提供了各种方法，感兴趣可以去研究一下