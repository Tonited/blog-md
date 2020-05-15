---
titile: MyCat验证
author: funtl Tonited
date: 2020.04.27
keywords: MyCat验证
img: https://gitee.com/tonited/qfdmy/raw/master/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%B9%8B%E5%88%86%E5%BA%93%E5%88%86%E8%A1%A8-MyCat/assert/2.MyCat%E5%8E%9F%E7%90%86/FiICfHIK96Ao6C9exSiGl9pfAz0g@.webp
categories: 分布式架构及微服务
tags: 
- 达摩院
- 微服务解决方案之分库分表-MyCat
---

## 分别创建数据库

分别在 3 台 MySQL 数据库上创建 `myshop_1`、`myshop_2`、`myshop_3`，按照刚才创建 MySQL 容器的序号创建

## 通过 MyCat 操作数据库

使用 SQLyog 之类的客户端工具连接 MyCat 数据库，默认端口号为 `8066`

![img](https://gitee.com//tonited/qfdmy/raw/master/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%B9%8B%E5%88%86%E5%BA%93%E5%88%86%E8%A1%A8-MyCat/assert/4.MyCat验证/FielSTJtZRjHAmxQR6BSaZ8HVDOz@.webp)

![img](https://gitee.com//tonited/qfdmy/raw/master/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%B9%8B%E5%88%86%E5%BA%93%E5%88%86%E8%A1%A8-MyCat/assert/4.MyCat验证/Fq5rNmtcnXoevoWYRx8x68YSHnoz@.webp)

## 创建测试数据

- 建表语句

  ```mysql
  create table tb_admin (id int not null primary key,name varchar(100),sharding_id int not null);
  ```

- 测试数据

  ```mysql
  insert into tb_admin(id, name,sharding_id) values(1000000, 'lixiaohong', 0);
  insert into tb_admin(id, name,sharding_id) values(6000000, 'lixiaolu', 1);
  insert into tb_admin(id, name,sharding_id) values(7000000, 'pgone', 1);
  insert into tb_admin(id, name,sharding_id) values(11000000, 'jianailiang', 2);
  ```

## 检验测试结果

按照上面的配置规则

- `id` 为 `1000000` 的数据应该写入 `dataNode1` 即 `myshop_1`.`tb_admin` 表中
- `id` 为 `6000000` 的数据应该写入 `dataNode2` 即 `myshop_2`.`tb_admin` 表中
- `id` 为 `7000000` 的数据应该写入 `dataNode2` 即 `myshop_2`.`tb_admin` 表中
- `id` 为 `11000000` 的数据应该写入 `dataNode3` 即 `myshop_3`.`tb_admin` 表中

至此这说明分片成功了