---
title: 计网：http报文结构
Date: 2020-05-14
tags: [计网]
categories: 计网
comments: true
---

### http报文的产生
HTTP通信过程包括客户端往服务器端发送请求以及服务器端给客户端返回响应两个过程。在这两个过程中就会产生请求报文和响应报文。

### 什么是HTTP报文呢？
HTTP报文是用于HTTP协议交互的信息，HTTP报文本身是由多行数据构成的字符串文本。客户端的HTTP报文叫做请求报文，服务器端的HTTP报文叫做响应报文。

### 报文结构
- HTTP报文由报文首部和报文主体构成，中间由一个空行分隔。
- 报文首部是客户端或服务器端需处理的请求或响应的内容及属性， 可以传递额外的重要信息。
- 报文首部包括请求行和请求头部。
- 报文主体主要包含应被发送的数据。
- 通常，不一定有报文主体。

### http请求报文
一个HTTP请求报文由请求行、请求头部、空行和请求数据4个部分构成。

![image](https://img-blog.csdn.net/20180828215741663?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2tvbmdtaW5fMTIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

请求行数据格式由三个部分组成：请求方法、URI、HTTP协议版本，他们之间用空格分隔。
```
// 该部分位于数据首行，基本格式为：
GET /index.html HTTP/1.1
```
该部分的请求方法字段给出了请求类型，URI给出请求的资源位置(/index.html)。HTTP中的请求类型包括:GET、POST、HEAD、PUT、DELETE。一般常用的为GET和POST方式。最后HTTP协议版本给出HTTP的版本号。

### http响应报文
HTTP响应报文由状态行（HTTP版本、状态码（数字和原因短语））、响应头部、空行和响应体4个部分构成。

![image](https://img-blog.csdn.net/20180828215835558?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2tvbmdtaW5fMTIz/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

状态行主要给出响应HTTP协议的版本号、响应返回状态码、响应描述，同样是单行显示。格式为：
```
HTTP/1.1 200 OK
```
状态码告知从服务器端返回的请求的状态，一般由一个三位数组成,分别以整数1～5开头组成。

### 报文首部
#### 结构
- 由首部字段名和字段值构成的，中间用冒号“:”分割。
- 首部字段格式： 首部字段名:字段值。

#### 类型
- 通用首部字段：请求报文和响应报文两方都会使用的首部。
- 请求首部字段：从客户端向服务器端发送请求报文时使用的首部。补充了请求的附加内容、客户端信息、响应内容相关优先级等信息。
- 响应首部字段：从服务器端向客户端返回响应报文时使用的首部。补充了响应的附加内容，也会要求客户端附加额外的内容信息。
- 实体首部字段：针对请求报文和响应报文的实体部分使用的首部。补充了资源内容更新时间等和实体有关的信息。


