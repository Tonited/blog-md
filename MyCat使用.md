---
titile: MyCat使用
author: Lusifer
date: 2020.04.27
keywords: MyCat使用
img: https://gitee.com/tonited/qfdmy/raw/master/%E5%BE%AE%E6%9C%8D%E5%8A%A1%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88%E4%B9%8B%E5%88%86%E5%BA%93%E5%88%86%E8%A1%A8-MyCat/assert/2.MyCat%E5%8E%9F%E7%90%86/FiICfHIK96Ao6C9exSiGl9pfAz0g@.webp
categories: 分布式架构及微服务
tags: 
- 达摩院
- 微服务解决方案之分库分表-MyCat
---

## 环境准备

部署方式全部基于 Docker

### 部署 3 台 MySQL 容器

案例分片方案按照自定义数字范围分片方式（`auto-sharding-long`）进行数据库分片，其规则要求需要 3 台 MySQL。

`docker-compose.yml` 配置如下

- mysql-1

  ```yaml
  version: '3.1'
  services:
    mysql-1:
      image: mysql
      container_name: mysql-1
      environment:
        MYSQL_ROOT_PASSWORD: 123456
      command:
        --default-authentication-plugin=mysql_native_password
        --character-set-server=utf8mb4
        --collation-server=utf8mb4_general_ci
        --explicit_defaults_for_timestamp=true
        --lower_case_table_names=1
      ports:
        - 3306:3306
      volumes:
        - ./data:/var/lib/mysql
  ```

- mysql-2

  ```yaml
  version: '3.1'
  services:
    mysql-2:
      image: mysql
      container_name: mysql-2
      environment:
        MYSQL_ROOT_PASSWORD: 123456
      command:
        --default-authentication-plugin=mysql_native_password
        --character-set-server=utf8mb4
        --collation-server=utf8mb4_general_ci
        --explicit_defaults_for_timestamp=true
        --lower_case_table_names=1
      ports:
        - 3307:3306
      volumes:
        - ./data:/var/lib/mysql
  ```

- mysql-3

  ```yaml
  version: '3.1'
  services:
    mysql-3:
      image: mysql
      container_name: mysql-3
      environment:
        MYSQL_ROOT_PASSWORD: 123456
      command:
        --default-authentication-plugin=mysql_native_password
        --character-set-server=utf8mb4
        --collation-server=utf8mb4_general_ci
        --explicit_defaults_for_timestamp=true
        --lower_case_table_names=1
      ports:
        - 3308:3306
      volumes:
        - ./data:/var/lib/mysql
  ```

### 部署 MyCat 数据库中间件

- 克隆

  ```shell
  git clone https://github.com/dekuan/docker.mycat.git
  ```

- 构建

  ```shell
  cd docker.mycat
  docker-compose build
  ```

- 启动

  ```shell
  # 注意：配置完成后再启动
  docker-compose up -d
  ```

## 配置 MyCat 数据库分片

- 服务端用户名密码配置：`vi config/mycat/server.xml`，找到第 90 行，参考如下内容配置

  ```xml
  <mycat:server xmlns:mycat="http://io.mycat/">
      <user name="root">
          <property name="password">123456</property>       
          <property name="schemas">myshop</property>        
          <property name="usingDecrypt">0</property>
      </user>
  </mycat:server>
  ```

- 数据节点、数据库、分库分表配置：`vi config/mycat/schema.xml`，参考如下内容配置

  ```xml
  <?xml version="1.0"?>
  <!DOCTYPE mycat:schema SYSTEM "schema.dtd">
  <mycat:schema xmlns:mycat="http://io.mycat/">
  
          <schema name="myshop" checkSQLschema="true" sqlMaxLimit="100">
                  
                  <table />
          </schema>
  
          
          <dataNode name="dataNode1" dataHost="dataHost1" database="myshop_1" />
          <dataNode name="dataNode2" dataHost="dataHost2" database="myshop_2" />
          <dataNode name="dataNode3" dataHost="dataHost3" database="myshop_3" />
  
          
          <dataHost name="dataHost1" maxCon="1000" minCon="10" balance="0"
                            writeType="0" dbType="mysql" dbDriver="jdbc" switchType="-1" slaveThreshold="100">
                  <heartbeat>select user()</heartbeat>
                  
                  <writeHost
                                  host="192.168.141.206"
                                  url="jdbc:mysql://192.168.141.206:3306?useSSL=false&amp;serverTimezone=UTC&amp;characterEncoding=utf8"
                                  user="root" password="123456">
                          
                  </writeHost>
          </dataHost>
          <dataHost name="dataHost2" maxCon="1000" minCon="10" balance="0"
                            writeType="0" dbType="mysql" dbDriver="jdbc" switchType="-1" slaveThreshold="100">
                  <heartbeat>select user()</heartbeat>
                  <writeHost
                                  host="192.168.141.206"
                                  url="jdbc:mysql://192.168.141.206:3307?useSSL=false&amp;serverTimezone=UTC&amp;characterEncoding=utf8"
                                  user="root" password="123456">
                          
                  </writeHost>
          </dataHost>
          <dataHost name="dataHost3" maxCon="1000" minCon="10" balance="0"
                            writeType="0" dbType="mysql" dbDriver="jdbc" switchType="-1" slaveThreshold="100">
                  <heartbeat>select user()</heartbeat>
                  <writeHost
                                  host="192.168.141.206"
                                  url="jdbc:mysql://192.168.141.206:3308?useSSL=false&amp;serverTimezone=UTC&amp;characterEncoding=utf8"
                                  user="root" password="123456">
                          
                  </writeHost>
          </dataHost>
          
  </mycat:schema>
  ```

- 分片规则配置：`vi config/mycat/rule.xml`，分别查看第 32 行和第 105 行

  ```xml
  <tableRule name="auto-sharding-long">
      <rule>        
          <columns>id</columns>        
          <algorithm>rang-long</algorithm>
      </rule>
  </tableRule>
  ```

  ```xml
  <function name="rang-long" class="io.mycat.route.function.AutoPartitionByLong">    
      <property name="mapFile">autopartition-long.txt</property>
  </function>
  ```

- 自定义数字范围分片规则：`vi config/mycat/autopartition-long.txt`

  ```txt
  # range start-end ,data node index
  # K=1000,M=10000.
  # ID 0-5000000 保存在 dataNode1
  0-500M=0
  # ID 5000000-10000000 保存在 dataNode2
  500M-1000M=1
  # ID 10000000-15000000 保存在 dataNode3
  1000M-1500M=2
  ```

  





