---
titile: HTTP协议
author: Tonited
date: 2020.02.13
keywords: HTTP协议
img: https://img-blog.csdnimg.cn/20200203162523510.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70
categories: 编程知识
---



Https协议是一种在Http协议基础上上进行加密的协议

<!--more-->

浏览本篇博客之前请先了解[什么是公钥和私钥](https://blog.csdn.net/weixin_43553694/article/details/104158209)

## 1.什么是Http协议

Http（Hyper Text Transfer Protocol，超文本传输）协议，位于TCP/IP四层模型中的应用层
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162609480.png)


Http协议通过请求/响应的方式在客户端和服务端进行通讯


![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162614894.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
但是Http协议**不够安全**，它的信息传输以明文方式传输，不做任何加密，相当于数据在网络上”裸奔“

## 2.加密

![1575200246616](https://img-blog.csdnimg.cn/20200203162634457.png)

假设小明想给小红发送消息，因为http是明文传输，很有可能有人恶意截获信息甚至篡改信息，这种行为叫做**中间人攻击**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162652418.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

### 1）对称加密

#### ①加密方式

小明和小红可以约定一种**对称加密**，方式，使用随机生成的密钥进行加密，第一次先告诉对方要用的密钥，之后两个人的信息在发送的时候用密钥加密，接收的时候用密钥解密
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162715441.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162723493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

#### ②风险

虽然使用对称加密可以提高数据的安全性，但是第一次的对话还是明文传输的，中间人可以截获第一次的对话来获取密钥，从而使中间人可以破解之后的对话
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162737782.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

### 2）非对称加密

我们可以使用**非对称加密**，为密钥的传输做一层额外的保护

#### ①加密方式
小明访问小红

在通信之前，小红先把自己的公钥`Key红公`发送给小明

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162756832.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

小明拿到小红的公钥之后，自己生成一个用于*对称加密*的密钥`Key称`，并用接收到的`Key红公`对`Key称`进行加密，并传回给小红
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162803166.png)

小红拿到响应的这一大坨后，用自己的私钥将外层的`Key红公`解密开，拿到内部的`Key称`，之后就和对称加密后面一样，用`Key称`进行加密传输
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162804386.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)


#### ②风险

中间人虽然不知道小红的私钥，但是可以在第一次小红发给小明`Key红公`时将其截获，并将自己的公钥`Key中公`告诉小明

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020020316282891.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

这样小明就会以为小红告诉他加密的公钥是`Key中公`，那么他就会用`Key中公`进行加密，产生的就会是由`Key中公`加密的`key称`：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162840374.png)

小明响应给小红这一坨时再次被中间人截获，中间人用自己的私钥解开`Key中公`的加密，就成功获取到了`Key称`，他再将`Key称`用`key红公`加密发给小红

![1575207084650](https://img-blog.csdnimg.cn/20200203162857178.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

小明小红建立了正常的、以`Key称`为密钥的加密传输，中间人却在中间悄无声息的获得了密钥，他能随意的通过获得的`Key称`破解传输内容

### 3）证书

魔高一尺，道高一丈

出现上述风险的时候，有必要引入第三方，一个权威的证书颁发机构（CA）来解决
**<font color="red">数字证书可以保证数字证书里的公钥确实是这个证书的所有者(Subject)的，或者证书可以用来确认对方的身份</font>**
证书的部分关键信息如下，主要作用通过下面的流程大家就能明白了

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162910715.png)
#### 流程

作为服务端的小红把自己的公钥`Key红公`发给证书颁发机构（CA）来申请证书

![在这里插入图片描述](https://img-blog.csdnimg.cn/2020020316293188.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

CA用自己的私钥`Key书私`对`Key红公`进行加密，并且通过服务端网址等信息生成一个证书签名，并对证书签名也用`Key书私`加密，再将被加密过的`Key红公`和`Key书私`还有其他信息一起制作成证书

证书制作完成后，机构把证书发送给小红

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203162942518.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

这之后如果小明要访问小红，小红告诉小明的就不是自己的公钥`Key红公`了，而是把证书传过去，证书里有被`Key书私`加密过的`Key红公`

![1575209411194](https://img-blog.csdnimg.cn/20200203162947836.png)

小明拿到证书之后先检查证书的真伪，各大浏览器和操作系统已经维护了**所有权威证书机构的名称和公钥`Key书公`**，小明拿出对应CA的`Key书公`将证书解密，得到证书签名

接下来小明按照根据相同的签名规则，自己也生成一个证书，然后将两个证书对比，如果两个证书一致，则说明证书有效

证书验证有效后，小明就可以放心的使用`Key书公`解密证书，得到`Key红公`

后面的步骤与*非对称加密*的后续相同：

小明生成`Key称`，用`Key红公`将其加密，发给小红

小红用自己的私钥解密得到`Key称`，后续的传输小红小明用`Key称`加密自己传送的数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203163007895.png)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203163025322.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
#### 为什么中间人截取替换证书不起作用

假设中间人要做这样的事：小明要访问小红，小红会将证书发给小明，小红把证书发给小明的时候，中间人截取了小红的证书，然后用自己的公钥申请证书，替换掉小红的证书发给小明

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200203163037116.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

**但是没有什么卵用，因为证书的签名是由<font color="red">服务端</font>（这里也就是小红）的网址形成的**

小明拿到证书解析发现这的确是一个权威机构发的证书，可是证书中服务端的地址（中间人的地址）和小明要访问的地址（小红的地址）不匹配，小明就知道中间有人捣乱


## 3. HTTPS
上述的就是`HTTPS`的主体思想，`HTTPS`在`HTTP`基础上增加了`SSL`安全层，刚才的一系列认证过程就是在`SSL`层实现的
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200211172448404.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

<hr/>
感谢洋洋姐对证书工作原理的解释

参考：[程序员小灰-漫画：什么是HTTPS协议](https://mp.weixin.qq.com/s?__biz=MzIxMjE5MTE1Nw==&mid=2653197101&idx=1&sn=d1fe482561d3d079363032ec182c5b3b&chksm=8c99e1f7bbee68e10f8470453637a7d434751a9414ceeffbbb9601f5ae2ba64e26fa6a88a99b&scene=21#wechat_redirect)