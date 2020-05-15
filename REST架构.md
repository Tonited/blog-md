---
titile: REST架构
author: Tonited
date: 2020.03.19
keywords: REST架构
img: https://www.runoob.com/wp-content/uploads/2015/07/restful.gif
categories: 设计模式与编程思想
tags: 编程思想
---

## 什么是REST架构

详细的解释在网上大家都能搜到，我就不多说了

通俗的来说，REST是一种组织软件的风格，而不是规范，它不强制你必须使用什么样的格式，只是建议你使用什么样的格式

它提出了一系列的想法，按照这些想法来组织软件，按照这些想法来请求资源

REST提出的想法都可以通过现有技术的合理运用，没有提出新的技术

它提供了关于API格式的想法，也就是我们所说的RESTfulAPI

这些想法、这种风格，叫做REST，下面就是它具体提出了哪些想法

大家可以在看完全部之后再回过头来看这一段，就会有更深刻的理解

## 版本号
REST提出了关于版本的想法

假设我们的软件新版本修改了一个函数，需要最新的客户端支持，旧版本的无法运行

但是我们都知道，新版本发布时大部分人不会更新，就会导致这样的问题：
新旧版本客户端都调用这个函数，但是这个函数被改过，而且新旧客户端使用的都是同一个API请求资源，所以旧版本的客户端不能用了
![在这里插入图片描述](https://img-blog.csdnimg.cn/20200318205826850.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)![在这里插入图片描述](https://img-blog.csdnimg.cn/20200318210101724.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)
REST建议我们将函数最近的各个版本都留下来，给函数的每个版本赋予版本号，请求时带上版本号就可以了

![在这里插入图片描述](https://img-blog.csdnimg.cn/202003182107100.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzU1MzY5NA==,size_16,color_FFFFFF,t_70)

此时API就变成了这样：
```
【GET】  /v1/users/{user_id}  // 版本 v1 的查询用户列表的 API 接口
【GET】  /v2/users/{user_id}  // 版本 v2 的查询用户列表的 API 接口
```

这样新旧版本就都可以使用，我们需要同时维护函数的几个版本，不必太多，只维护最近的几个函数版本即可，废弃很少有人用的版本，并强制使用那些版本的用户升级

## 资源路径
RESTful API对于资源路径提出以下观点：

- RESTful API 是以“**资源**”为核心的，每一个URL代表的都是“**资源**”，所以URL应该只有名词，可以有形容词，但是尽量少用
- 无论资源是一个还是多个，名词都使用复数
- 使用小写字母
- 数字及下划线来区分多个单词
	例如：
	```
	【GET】  /v1/students/{student_id} 
	```
	```
	【POST】  /v1/schools/{school_id}/students/{student_id} // 添加学校的学生，这个大家看到可能会有些莫名的疑惑，后面我们会说明POST这一方式
	```
- 资源变化难以使用纯名词或形容词来命名时，可以考虑使用一些特殊的 actions 命名
	```
	/{resources}/{resource_id}/actions/{action}
	```
	例如密码修改：
	```
	【PUT】  /v1/users/{user_id}/password/actions/modify // 密码修改
	```

## 请求方式
REST认为，对于不同种类的操作，请求时要使用不同的请求方式
| 请求方式 | 使用                           |
| -------- | ------------------------------ |
| GET      | 用于查询资源                   |
| POST     | 用于创建新的资源               |
| PUT      | 用于更新服务端的资源的全部信息 |
| PATCH    | 用于更新服务端的资源的部分信息 |
| DELETE   | 用于删除服务端的资源。         |

比如
```
【GET】		/students/{student_id}		获取学生信息
【GET】		/students					获取所有学生列表
【POST】		/students					添加新学生
【PUT】		/students/{student_id}		修改该同学的所有信息
【PATCH】	/students/{student_id}	    修改该同学的部分信息
【DELETE】	/students/{student_id}		删除该同学
```

我们很少能看到除了【GET】和【POST】的其他请求方式，是因为并不是所有浏览器都支持这些请求方式，但是所有浏览器都支持【GET】【POST】

## 查询参数
[何时使用查询参数 何时使用路径参数](https://blog.csdn.net/weixin_43553694/article/details/104954820)

- 分页查询
	- offset 指定返回记录的开始位置
	- limit 指定返回记录的数量
	```
	【GET】  /{version}/{resources}/{resource_id}?offset={field}&limit={filed}
	```
- 排序方式`orderby`
	- asc升序，desc降序，自己规定其他排序方式
	```
	【GET】  /{version}/{resources}/{resource_id}[?orderby={filed}
	```
- 返回查询总数`count`
	```
	【GET】  /{version}/{resources}/{resource_id}?count=[true|false]
	```
- 其他个性化查询参数
	```
	【GET】  /v1/categorys/{category_id}/apps/{app_id}?enable=[1|0]&os_type={field}&device_ids={field,field,…}
	```
不要过度设计，只返回需要的数据，但同时还要尽量保证API的弹性，以方便未来软件扩展
为了提高效率，可以考虑适当在数据库添加索引

## 状态码
| 状态码 | 描述           |
| ------ | -------------- |
| 200    | 请求成功       |
| 201    | 创建成功       |
| 400    | 错误的请求     |
| 401    | 未验证         |
| 403    | 被拒绝         |
| 404    | 无法找到资源   |
| 409    | 资源冲突       |
| 500    | 服务器内部错误 |

## 异常响应
请求返回不是`2xx`的 HTTP 错误码响应时，说明请求出现异常，返回时要携带错误信息
RESTful API 返回的数据一般时json格式
```json
HTTP/1.1 400 Bad Request
Content-Type: application/json
{
    "code": "INVALID_ARGUMENT",
    "message": "{error message}",
    "cause": "{cause message}",
    "request_id": "01234567-89ab-cdef-0123-456789abcdef",
    "host_id": "{server identity}",
    "server_time": "2014-01-01T12:00:00Z"
}
```
这一点其实没必要，因为返回信息时会暴露很多信息，这些信息很可能会被人截获，用这些知道的信息对你的软件做些手脚

## 请求参数
- 对请求参数进行限制说明
	- 例如批量查询，要限制最多可以一次查询多少人
	```
	【GET】     /v1/users/batch?user_ids=1001,1002      // 批量查询用户信息
	参数说明
	- user_ids: 用户ID串，最多允许 20 个。
	```
- 说明参数的格式
	- 必填、选填、取值、长度等
	```
	【POST】     /v1/students                             // 创建新学生
	请求内容
	{
	    "id": "1805010413",				// 必填, 学号, max 10
	    "realname": "Tonited",			// 必填, 学生姓名, max 10
	    "password": "123456",			// 必填, 学生密码, max 32
	    "email": "tonited@tom.com",		// 选填, 电子邮箱, max 32
	    "sex": 1						// 必填, 学生性别[1-男 2-女 3-其他 0-未知]
	}
	```

## 响应参数
- 响应要符合请求方式的规范

| 请求方式   | 路径                                 | 解释                                                         |
| ---------- | ------------------------------------ | ------------------------------------------------------------ |
| 【GET】    | /{version}/{resources}/{resource_id} | 返回单个资源对象                                             |
| 【GET】    | /{version}/{resources}               | 返回资源对象的列表                                           |
| 【POST】   | /{version}/{resources}               | 返回新生成的资源对象                                         |
| 【PUT】    | /{version}/{resources}/{resource_id} | 返回完整的资源对象                                           |
| 【PATCH】  | /{version}/{resources}/{resource_id} | 返回完整的资源对象                                           |
| 【DELETE】 | /{version}/{resources}/{resource_id} | 状态码 200，返回完整的资源对象。<br/>状态码 204，返回一个空文档。 |
- 各种对象的返回方式
	- 一条数据：返回对象的信息
	```json
	HTTP/1.1 200 OK
	{
	    "id" : "1805010413",
	    "name" : "Tonited",
	    "created_time": 2018091513550084,
	    "updated_time": 2020041509120562,
	}
	```
	- 一组数据：返回一组数据的封装
	```json
	HTTP/1.1 200 OK
	{
	    "count":100,
	    "items":[
	        {
	            "id" : "1805010413",
			    "name" : "Tonited",
			    "created_time": 2018091513550084,
			    "updated_time": 2020041509120562,
	        },
	        ...
	    ]
	}
	```
## 完整案例
使用“获取学生列表”的案例
```json
【GET】     /v1/students?[&keyword=xxx][&enable=1][&offset=0][&limit=20] 获取学生列表
功能说明：获取学生列表
请求方式：GET
参数说明
- keyword: 模糊查找的关键字。[选填]
- enable: 启用状态[1-启用 2-禁用]。[选填]
- offset: 获取位置偏移，从 0 开始。[选填]
- limit: 每次获取返回的条数，缺省为 20 条，最大不超过 100。 [选填]
响应内容
HTTP/1.1 200 OK
{
    "count":100,
    "items":[
        {
            "id" : "1805010413",
			    "name" : "Tonited",
			    "created_time": 2018091513550084,
			    "updated_time": 2020041509120562
        },
        ...
    ]
}
失败响应
HTTP/1.1 403 UC/AUTH_DENIED
Content-Type: application/json
{
    "code": "INVALID_ARGUMENT",
    "message": "{error message}",
    "cause": "{cause message}",
    "request_id": "01234567-89ab-cdef-0123-456789abcdef",
    "host_id": "{server identity}",
    "server_time": "2014-01-01T12:00:00Z"
}
错误代码
- 403 UC/AUTH_DENIED    授权受限
```

<hr/>
本文API示例来源及参考：[《你怎么理解 RESTful》李为民](https://www.funtl.com/zh/apache-http-client/%E4%BD%A0%E6%80%8E%E4%B9%88%E7%90%86%E8%A7%A3-RESTful.html#%E5%93%8D%E5%BA%94%E5%8F%82%E6%95%B0)