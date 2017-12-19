# ch01 Redis起步

## Redis概述

- Redis代表：**RE** mote  **DI** ctionary   **S** erver

- Redis是NoSQL（not only sql），基于键值的数据存储。

- 具有强大的数据类型，包括：Strings, Hashes,Lists, Sets, Sorted Sets, Bitmaps, and HyperLogLogs

- 默认情况下，Redis将所有数据存储到内存中

- 开源网址: [https://github.com/antirez/redis](https://github.com/antirez/redis)

- 文档地址：[http://redis.io](http://redis.io)

  ​

## Redis 安装

### redis支持的平台

- Linux
- OS X
- OpenBSD
- NetBSD
- FreeBSD

*Redis并不官方支持windows，但是对windows平台的支持可以查看链接:[https://github.com/MicrosoftArchive/redis/releases](https://github.com/MicrosoftArchive/redis/releases)*

### Windows版本安装

下载地址：[https://github.com/MicrosoftArchive/redis/releases](https://github.com/MicrosoftArchive/redis/releases)

可下载文件：Redis-x64-3.0.504.msi或者Redis-x64-3.0.504.zip

**注意： 仅支持64位windows**

#### 安装与启动

1. 解压zip文件，并将文件夹内容拷贝到待安装位置

2. 通过命令行进入安装位置，运行命令启动redis服务

   C:\develop_tools\redis>redis-server

   **注意：这里没有指定配置文件，将采取默认配置启动服务**

    服务启动后输出信息：

   ![服务启动截图](ch01-resource\images\ch01-1.png)

   ​

   *说明：*

   *这里将输出程序启动的进程id，即pid:4500，以及客户端连接需要使用的端口，这里是port:6379*

   ## 连接到redis服务

   ### 使用redis客户端命令行接口连接redis服务

   C:\develop_tools\redis>redis-cli

   127.0.0.1:6379> set student_name 'jerry'
   OK
   127.0.0.1:6379> get student_name
   "jerry"
   127.0.0.1:6379>

   **说明：**

   - redis-cli，该命令启动redis客户端命令行接口
   - set存储一个键值信息
   - get根据key取值

   ### 获取帮助-使用help命令

   127.0.0.1:6379> help set

     SET key value [EX seconds] [PX milliseconds] [NX|XX]
     summary: Set the string value of a key
     since: 1.0.0
     group: string

   127.0.0.1:6379>

   ### 根据匹配模式查找key

   127.0.0.1:6379> keys u*
   (empty list or set)
   127.0.0.1:6379> keys stu*
   1) "student_name"
   127.0.0.1:6379>

**注意：redis-cli仅用于调试目的，实践中使用JavaScript脚本和node.js**

## node.js安装

node.js下载地址：[https://nodejs.org](https://nodejs.org)

可运行平台：Mac OS X, Windows, Linux

当前版本：v8.9.3

### 安装步骤

1. 双击安装，默认安装位置：C:\Program Files\nodejs\

2. 建立项目开发文件夹: D:\redisdemo\ch01

3. 进入项目开发文件夹运行npm命令，安装redis库（或称redis模块）

   D:\redisdemo\ch01>npm install redis

4. 安装完成后在ch01\node_modules下会产生redis的相关文件夹，这里就是redis的客户端安装位置

   **注意：不要在nodejs安装文件夹下运行npm install redis，这样会删除npm包，可以运行npm install redis -g 安装为全局方式。**

   ​


## 使用node.js操作redis

### 编写代码

在nodejs安装目录下建立hello.js并输入如下代码：

var redis = require("redis"); // 1
var client = redis.createClient(); // 2
client.set("my_key", "Hello World using Node.js and Redis"); // 3
client.get("my_key", redis.print); // 4
client.quit(); // 5

代码说明：

line1：表示node.js需要redis库

line2：建立redis客户端对象

line3：执行redis的set命令将一个字符串存储在my_key中

line4：执行redis的get命令取出my_key中的值并打印

line5：关闭到redis的服务端连接

### 运行代码：

在项目文件夹下运行命令行：

D:\redisdemo\ch01\>node hello.js
Reply: Hello World using Node.js and Redis

***

## 使用webstorm开发node.js

参考内容：[ch01-ref-使用webstorm编写node.js操作redis.md](ch01-ref-使用webstorm编写node.js操作redis.md)

## redis 数据类型

