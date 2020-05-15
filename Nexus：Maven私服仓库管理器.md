---
titile: Nexus：Maven私服仓库管理器
author: Tonited
date: 2020.02.20
keywords: Nexus：Maven私服仓库管理器
img: https://img-blog.csdnimg.cn/20200220135751229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70
categories: 框架
---

## 什么是Nexus

![示意图](https://img-blog.csdnimg.cn/20200220135751229.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
在多人协作开发时，每个人写的子项目之间可能jar包需要互相依赖，两个人也许可以可以直接文件传输jar包，但是人数一多就会变得复杂

那么，如果所有人用一共公共仓库，每个人能上传自己的jar包，Maven能直接从这个公共的仓库下载下来，而且能检测其他队友项目的更新并获取下来，这不是很棒吗

这个公共仓库不同于Maven官方的仓库，是自己团队使用的，我们叫做Maven私服

Nexus就是一个强大的私服仓库管理器，极大地简化了私服仓库的维护和外部仓库的访问
<!--more-->

## 依赖的下载
有了Maven私服，Maven构建时如果本地没有依赖包，那么它会去私服找，私服如果没有，就会去Maven官方找，从官方下载到私服，私服再下载到本机
![示意图](https://img-blog.csdnimg.cn/20200220135528826.png)
## Docker 安装 Nexus
如果不会使用Docker和docker-compose的建议补一下

1. 创建`docker-compose.yml`文件内容如下
	```yml
	version: '3.1'
	services:
	  nexus:
	    restart: always
	    image: sonatype/nexus3
	    container_name: nexus
	    ports:
	      - 8081:8081
	    volumes:
	      - /usr/local/docker/nexus/data:/nexus-data
	```
2. 安装时，`/usr/local/docker/nexus/data`目录可能会没有权限，输入下行命令赋予权限（[chmod数字权限设定法](https://editor.csdn.net/md/?articleId=104324943)）
	```shell
	chmod 777 /usr/local/docker/nexus/data:/nexus-data
	```
3. 登录控制台`地址：http://ip:port/ 用户名：admin `，新版密码不再是admin123，密码需要查看`/usr/local/docker/nexus/data/admin.password`文件
![示意图](https://img-blog.csdnimg.cn/2020022014041134.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

其实`docker-compose.yml`可以这样写
```yml
version: '3.1'
services:
  nexus:
    restart: always
    image: sonatype/nexus3
    container_name: nexus
    ports:
      - 8081:8081
    volumes:
      - nexus-data:/nexus-data
nexus-data:
```
`nexus-data`是自己起的名字，我们可以用它指定卷组的路径，如果向上面这样不写地址的话，默认路径就在`/var/lib/docker/volumes/文件夹名`,文件夹名取决于你放`docker-compose.yml`的文件夹的名字

## 为私服的`maven-central`仓库设置阿里云镜像
镜像地址：`http://maven.aliyun.com/nexus/content/repositories/central/`
![示意图](https://img-blog.csdnimg.cn/20200220145139100.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)![示意图](https://img-blog.csdnimg.cn/20200220145218389.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
## 在项目中使用 Maven 私服
### 配置验证信息
在 Maven `settings.xml` 中添加 Nexus 认证信息(servers 节点下)：
```xml
<server>
  <id>nexus-releases</id>
  <username>admin</username>
  <password>密码</password>
</server>

<server>
  <id>nexus-snapshots</id>
  <username>admin</username>
  <password>密码</password>
</server>
```
### 配置自动化部署（上传）

在 pom.xml 中添加如下代码：
```xml
<distributionManagement>  
	<!-- 上传RELEASE版本的仓库 -->
  <repository>  
    <id>nexus-releases</id>  
    <name>Nexus Release Repository</name>  
    <url>http://127.0.0.1:8081/repository/maven-releases/</url>  
  </repository>  
  <!-- 上传SNAPSHOT版本的仓库 -->
  <snapshotRepository>  
    <id>nexus-snapshots</id>  
    <name>Nexus Snapshot Repository</name>  
    <url>http://127.0.0.1:8081/repository/maven-snapshots/</url>  
  </snapshotRepository>  
</distributionManagement> 
```
注意事项：
- ID 名称必须要与 settings.xml 中 Servers 配置的 ID 名称保持一致。
-  发布时会根据项目版本号中是否有`SNAPSHOT `，发布到Nexus的不同仓库下
### 配置代理仓库（下载）
在Nexus设置界面我们可以看到，`maven-public`仓库其实是结合了三个仓库，所以我们可以直接设置从`maven-public`下载，它会自动去下属的仓库中找
![示意图](https://img-blog.csdnimg.cn/20200220144657424.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
```xml
<!-- 下载RELEASE和SNAPSHOT版本的仓库 -->
<repositories>
    <repository>
        <id>nexus</id>
        <name>Nexus Repository</name>
        <url>http://127.0.0.1:8081/repository/maven-public/</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <enabled>true</enabled>
        </releases>
    </repository>
</repositories>
<!-- 下载插件的仓库 -->
<pluginRepositories>
    <pluginRepository>
        <id>nexus</id>
        <name>Nexus Plugin Repository</name>
        <url>http://127.0.0.1:8081/repository/maven-public/</url>
        <snapshots>
            <enabled>true</enabled>
        </snapshots>
        <releases>
            <enabled>true</enabled>
        </releases>
    </pluginRepository>
</pluginRepositories>
```
### 上传到私服仓库
```maven
mvn deploy
```
#### 上传第三方 JAR 包
##### 最新版Nexus支持页面上传
![示意图](https://img-blog.csdnimg.cn/20200220142841872.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
##### Nexus 3.0 不支持页面上传，可使用 maven 命令：
如第三方JAR包：名字-版本号.jar
```maven
mvn deploy:deploy-file 
  -DgroupId=JAR包的groupId
  -DartifactId=JAR包的artifactId
  -Dversion=版本号
  -Dpackaging=jar 
  -Dfile=D:\名字-版本号.jar
  -Durl=私服仓库地址
  -DrepositoryId= settings.xml 中 Servers 配置的 ID 名称（授权）
```
注意事项：

- 建议在上传第三方 JAR 包时，创建单独的第三方 JAR 包管理仓库，便于管理有维护。（maven-3rd）