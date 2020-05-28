---
titile: RabbitMQ部署
author: Lusifer
date: 2020.04.27
keywords: RabbitMQ部署
img: https://gitee.com/tonited/qfdmy/raw/master/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%B9%8B%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97-RabbitMQ/assert/4.RabbitMQ%E9%83%A8%E7%BD%B2/FsTit3gkO17GoekvBVz2uGsJPN6h@.webp
categories: 分布式架构及微服务
tags: 
- 达摩院
- 微服务解决方案之消息队列-RabbitMQ
---

## docker-compose.yml

```yaml
version: '3.1'
services:
  rabbitmq:
    restart: always
    image: rabbitmq:management
    container_name: rabbitmq
    ports:
      - 5672:5672
      - 15672:15672
    environment:
      TZ: Asia/Shanghai
      RABBITMQ_DEFAULT_USER: rabbit
      RABBITMQ_DEFAULT_PASS: 123456
    volumes:
      - ./data:/var/lib/rabbitmq
```

## RabbitMQ WebUI

### 访问地址

http://ip:15672

### 首页

![img](https://gitee.com/tonited/qfdmy/raw/master/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%B9%8B%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97-RabbitMQ/assert/4.RabbitMQ部署/FsTit3gkO17GoekvBVz2uGsJPN6h@.webp)

### Global counts

![img](https://gitee.com/tonited/qfdmy/raw/master/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%B9%8B%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97-RabbitMQ/assert/4.RabbitMQ部署/For2zYpy6D6Sp0T5uuMXw9voPrk1@.webp)

- Connections：连接数
- Channels：频道数
- Exchanges：交换机数
- Queues：队列数
- Consumers：消费者数

### 交换机页面

![img](https://gitee.com/tonited/qfdmy/raw/master/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%B9%8B%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97-RabbitMQ/assert/4.RabbitMQ部署/Fiul-X4nzz7-kU_3BLGLq38VVmgC@.webp)

### 队列页面

![img](https://gitee.com/tonited/qfdmy/raw/master/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%B9%8B%E6%B6%88%E6%81%AF%E9%98%9F%E5%88%97-RabbitMQ/assert/4.RabbitMQ部署/Fo5dfKgy_HVVC7TcJ94Dr2IGYt2H@.webp)

- Name：消息队列的名称，这里是通过程序创建的
- Features：消息队列的类型，durable:true 为会持久化消息
- Ready：准备好的消息
- Unacked：未确认的消息
- Total：全部消息
- 备注：如果都为 0 则说明全部消息处理完成