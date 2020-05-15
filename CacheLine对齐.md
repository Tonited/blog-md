---
titile: CacheLine对齐
author: Tonited
date: 2020.02.27
keywords: CacheLine对齐
img: https://img-blog.csdnimg.cn/20200227131956655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70
categories: 多线程与高并发
---

## CacheLine

总所周知，计算机将数据从主存读入Cache时，是把要读取数据附近的一部分数据都读取进来
这样一次读取的一组数据就叫做`CacheLine`，每一级缓存中都能放很多的`CacheLine`
![示意图](https://img-blog.csdnimg.cn/20200227131956655.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

## 多核CUP
![示意图](https://img-blog.csdnimg.cn/20200227130125483.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
- L1、L2、L3指一级缓存，二级缓存，三级缓存
- CUP中的每个核均可单独处理一个线程
- 每个核公用L3

## 超线程
一个核中有多套`PC和Register`，他们公用一个`ALU`，这样一个核可以处理多个线程
如四核八线程就由此而来

## Volatile的可见性
![示意图](https://img-blog.csdnimg.cn/20200227131858344.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

1. `x`被标记了`volatile`
2. 两个线程运算时是将缓存中**要被运算数所在的整条CacheLine**复制到线程自己的存储，并进行运算，运算之后写回缓存
3. 假设`线程1`修改了`x`并写回，但是`线程2`中的`x`还是未修改的`x`
4. 由于`x`被标记了`volatile`，在`线程1`写回`x`缓存时，`线程1`会通知`线程2`重新读取缓存中的`x`

## 伪共享

![示意图](https://img-blog.csdnimg.cn/20200227132025735.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
1. `线程1、2`公共使用**同一个**`CacheLine`
2. `x、y`在**同一个**`CacheLine`
3. `x、y`**都是**`volatile`
4. 如果`线程1`不断修改`x`，`线程2`不断修改`y`，那么修改的时候`线程1`就要不断通知`线程2`更新`x`、`线程2`就要不断通知`线程1`更新`y`
5. 这样的不断通知不断重新读取很浪费性能
6. 这就叫伪共享

## CacheLine对齐
多线程会有上面的伪共享的问题，如果在**缓存读取数据到`CacheLine`时**，两个`volatile`的数被读取到**不同的**`CacheLine`中的话，就不需要一直通知另一个线程更新数据了，因为另一个线程根本没有这个数据
![示意图](https://img-blog.csdnimg.cn/20200227133002321.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
那么如何让两个数据一定在不同的`CacheLine`呢，方法就是Cache Line对齐

一般一个`CacheLine`是`64`位，也就是`8`个`long`，我们可以把`x`定义为`long`，并**同时定义7个没有用的long变量**，这样这`8`个数就在同一个`CacheLine`中
之后再定义`y`，`y`自然也就在下一个CacheLine中了
![示意图](https://img-blog.csdnimg.cn/20200227133653368.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)这就叫**CacheLine对齐**，这样两线程就不会出现伪共享的现象了