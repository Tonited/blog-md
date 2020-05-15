---
titile: TCP三次握手四次挥手
author: Tonited
date: 2020.04.05
keywords: TCP三次握手四次挥手
img: https://img-blog.csdnimg.cn/20200405115748128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70
categories: 编程知识
---

## TCP数据报

![示意图](https://img-blog.csdnimg.cn/20200405112825785.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
为了理解TCP握手和挥手，我们可以简单的认为，数据报就是每次握手/挥手时相互传送的信息，每次都传送一个数据报，需要传送的信息就存在数据报当中，其中比较重要的字段有这样的几个：

- 序号：随即生成的，用来唯一标志当前数据报，可以理解成数据报的ID
- 确认号：确认号=刚才接收到的对方的序号+1，具体功能通过下面的握手/挥手过程就可以明白
- 标志位：用来标志当前数据报的功能，可以同时标志很多功能，Tcp的握手/挥手用到以下标志功能功能（它还可以标志其他功能，感兴趣可以自行搜索）：
	- ACK：确认序号有效
	- RST：重置连接
	- SYN：建立连接
	- FIN：断开连接

## 三次握手
TCP的三次握手指的是过程一共会发送三个数据报，简单描述为

1. A对B说：我要建立连接
2. B对A说：好的我同意了
3. A对B说：那我建立连接了

![示意图](https://img-blog.csdnimg.cn/20200405115748128.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
1. 客户端在未建立连接前处于`CLOSED`状态，这表示客户端无连接
	服务端连接前要处于`LISTEN`状态，可以通过服务端调用`listen()`函数实现，这种状态服务端会监控是否有客户端建立连接的请求
2. 客户端首先向服务端发送一个数据报，标志位为`SYN`标志这是要请求建立连接的数据报；数据报的序号是随机生成的；发送完之后客户端进入`SYN-SEND`状态，等待服务端的反馈
3. 服务端收到数据报之后，自己产生一个数据报，标志位为`SYN+ACK`，表示服务端确认了建立连接的序号有效；并将 收到的数据报 的`序号+1` 并存入 自己数据报 的`确认号`中，然后将数据报发回给客户端，自己进入`SYN-RECV`状态，等待客户端的回应；
4. 客户端收到了反馈的数据报，先检查 收到的数据报的 确认号，看它是不是等于自己上次发送数据报序号+1，如果是，自己产生数据报，标志位`ACK`，将 收到数据报的`序号+1 `存入 自己的数据报的确认号 中，发回给服务端，发送之后自己进入`ESTABLISEND`状态
5. 服务端收到数据报，自己进入`CLOSED`状态，三次握手完成

## 四次挥手
断开连接时要发送四次数据报，所以被称为四次挥手，三次握手和四次挥手过程类似，就不细讲了，大致过程为：
1. A对B说：我要断开连接
2. B对A说：好的我知道了，我准备一下
3. 过了一会，B对A说：我准备好了，我们断开吧
4. A对B说：好的，我断开了

![示意图](https://img-blog.csdnimg.cn/20200405123227125.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)他没有直接`CLOSED`而是先进入`TIME_WAIT`状态，等待一段时间再`CLOSED`关闭连接，原因是数据报的发送是**只管发送不管是否接收的**，确认是否接受要靠对方发来的数据包确认，如果超时没有收到反馈数据报就再发一次
如果最后的数据报客户端发送过去就关闭了，但是服务端没有收到数据报，服务端的连接就关闭不了，所以客户端需要等待一会

数据报在网络中存在由最长生命的，过了这个时间就会自动销毁并通知发送端，报文最大生存时间（MSL，Maximum Segment Lifetime）

`TIME_WAIT`会等待两个MSL，如果客户端最后发送的数据报没有被服务端接收到，服务端超时，就会重新发给客户端最后的数据报，客户端这时正在等待，等待过程中又收到了服务端的数据报，客户端就知道刚刚的数据报服务端没有接收到，就会重发一次数据报

<hr/>
参考资料：[Socket技术讲解](https://www.jianshu.com/p/066d99da7cbd)