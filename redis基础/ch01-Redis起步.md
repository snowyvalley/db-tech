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

### String

可以用于存储任何类型的数据：

- 文本（xml，json，html，或者原始文本）
- 整数
- 浮点数
- 二进制数（视频，图片，音频）

**注意：String类型的数据不能超过512M**

String类型可以用于以下场合：

- 缓存机制：将文本或者二进制数据缓冲到redis中。这些数据来自html页面上的任何内容以及通过接口调用得到的图片，视频等。

- 缓存自动过期：对于操作数据库时，查询时间较长，或者频繁的查询，为了提高性能将使用缓存机制，当长时间不再访问这些数据时，又需要这些缓存自动过期。

### 通过redis-cli使用String的例子

  相关命令：

  **MSET**：

  一次设置多个key和值，key-value对用空格隔开

  **MGET**：

  一次返回多个key名对应的值，key名用空格隔开

  *代码示例*

  C:\develop_tools\redis>redis-cli
  127.0.0.1:6379> mset student1 "jerry" student2 "petter"
  OK
  127.0.0.1:6379> mget student1 student2
  1) "jerry"
  2) "petter"
  127.0.0.1:6379>

**EXPIRE**

为给定的key添加一个过期时间（单位为秒），过期后自动删除给key，返回1表示设置成功，返回0表示该key不存在或者无法设置

**TTL**(time to live)

- 返回正数，表示给定key存活了多少秒
- -2 该key已经过期或者不存在
- -1 该key存在，但没有设置过期时间

*代码示例*

127.0.0.1:6379> set course "Redis Basic"
OK
127.0.0.1:6379> expire course 10
(integer) 1
127.0.0.1:6379> get course
"Redis Basic"
127.0.0.1:6379> ttl course
(integer) -2
127.0.0.1:6379> get course
(nil)
127.0.0.1:6379>

**INCR**

将指定key的值加1后，返回该值

**INCRBY**

将指定key加上给定整数后，并返回该值

**DECR、DECRBY**

和INCR,INCRBY刚好相反，只是减小值

**INCRBYFLOAT**

加上一个浮点数并返回

**注意：INCRBY,DECRBY,INCRBYFLOAT均可接收正数和负数**

*代码示例*

C:\develop_tools\redis>redis-cli
127.0.0.1:6379> set counter 100
OK
127.0.0.1:6379> incr counter
(integer) 101
127.0.0.1:6379> incrby counter 5
(integer) 106
127.0.0.1:6379> decr counter
(integer) 105
127.0.0.1:6379> decrby counter 50
(integer) 55
127.0.0.1:6379> incrbyfloat counter 5.5
"60.5"
127.0.0.1:6379> get counter
"60.5"
127.0.0.1:6379>

**注意：**

*Redis是单线程的，这就意味着每一时刻只会有一个客户端执行命令，不会存在多个客户端同时访问相同key的问题。*

### 使用Node.js建立投票系统

**需求**

1. 建立一个文章列表
2. 使用node.js为列表中的文章进行投票，票数加1
3. 减小一个文章的票数，票数减1
4. 显示文章及票数

**实现思路**

- 将文章列表存储到redis中
- 编写3个函数，upVote,downVote,showResults均将文章ID作为参数
- 文章key的命名规则    article:<id>:java
- 投票key的命名规则    article:<id>:votes

**实现步骤**

*step1：建立文章列表（假设有三篇文章）*

127.0.0.1:6379> set article:1001:java "what is JVM"
OK
127.0.0.1:6379> set article:1002:java "Java basic for beginer"
OK
127.0.0.1:6379> set article:1003:java "about the Runtime exception"
OK
127.0.0.1:6379>

*step2：编写投票的代码*

文件名：aritcles-popularity.js

```
var redis = require("redis");
var client = redis.createClient();
function upVote(id) {
    var key = "article:"+id+":votes";
    client.incr(key);
}
```

*step3：编写减小投票数的代码*

```
function downVote(id) {
    var key = "article:"+id+":votes";
    client.decr(key);
}
```

*step4：编写显示文章和投票数的代码*

```
function showResults(id) {
    var articleKey = "article:"+id+":java";
    var voteKey = "article:"+id+":votes";
    client.mget([articleKey,voteKey],function (err,replies) {
        console.log('The article- '+replies[0]+'] has',replies[1],'votes');
    });
}
```

【代码说明】

1. 为MGET命令传递一个key数组和一个回调函数。如果key不存在或者没有值将返回null

2. 在匿名函数中，参数replies是数组，索引0处放的是articleKey的值即文章标题，而索引1处放的是voteKey的值即票数

3. console.log的用法：

   console.log(object[, object, ...])
   使用频率最高的一条语句：向控制台输出一条消息。
   支持 C 语言 printf 式的格式化输出。当然，也可以不使用格式化输出来达到同样的目的：
   var animal='frog', count=10;
   console.log("The %s jumped over %d tall buildings", animal, count);
   console.log("The", animal, "jumped over", count, "tall buildings");

*step5:投票测试代码* 

```
upVote(1001);
upVote(1002);
upVote(1003);
upVote(1001);
upVote(1001);
upVote(1003);
downVote(1001);
showResults(1001);
showResults(1002);
showResults(1003);
client.quit();
```

*step6:运行结果*

The article[ what is JVM] has 2 votes
The article[ Java basic for beginer] has 1 votes
The article[ about the Runtime exception] has 2 votes

***

### List

- List类型在Redis中非常灵活，可以扮演简单集合，栈或者队列。
- List命令是原子性的。
- 操作list的命令是阻塞式的，当一个客户端建立了一个空列表，其他客户端需要等待该客户端向其中添加一个新项目。
- Redis中的list是Linked List

**List应用场合**

- 事件队列
- 存储用户最新帖子：比如Twitter

#### List示例

*LPUSH*

向List的开头插入数据（left push）

*RPUSH*

向List结尾插入数据（right push)

*LLEN*

List长度（List length）

*LINDEX*

返回指定list在给定索引处的值，左侧（开头）第一个元素为索引为0，负值表示从右侧（末尾）访问，-1表示右侧第一个元素，-2右侧第二个元素

redis-cli练习：

C:\develop_tools\redis>redis-cli
127.0.0.1:6379> lpush books "01:Java Basic"
(integer) 1
127.0.0.1:6379> lpush books "02:Oracle advice"
(integer) 2
127.0.0.1:6379> rpush books "03:redis basic"
(integer) 3
127.0.0.1:6379> llen books
(integer) 3
127.0.0.1:6379> lindex books 1
"01:Java Basic"
127.0.0.1:6379> lindex books 0
"02:Oracle advice"

*LRANGE*

返回给定索引范围内的元素构成的数组，索引值可正可负

redis-cli练习：

127.0.0.1:6379> lrange books 0 1
1) "02:Oracle advice"
2) "01:Java Basic"
127.0.0.1:6379> lrange books 0 -1
1) "02:Oracle advice"
2) "01:Java Basic"
3) "03:redis basic"

*LPOP*

移除并返回list第一个元素

*RPOP*

移除并返回list最后一个元素

**注意：Redis的list的不能从中间插入和删除数据**

redis-cli练习：

127.0.0.1:6379> lpop books
"02:Oracle advice"
127.0.0.1:6379> lrange books 0 -1
1) "01:Java Basic"
2) "03:redis basic"
127.0.0.1:6379> rpop books
"03:redis basic"
127.0.0.1:6379> lrange books 0 -1
1) "01:Java Basic"
127.0.0.1:6379>

#### 实现一个通用队列系统

*step1：编写queue函数*

建立queue.js，编写如下代码：

```
function Queue(queueName,redisClient) {//接收队列名和redis客户端对象作为参数
    this.queueName = queueName;//将对列名保存到属性中
    this.redisClient = redisClient;//保存redis客户端对象到属性中
    this.queueKey = 'queues:'+queueName;//设置Redis key名
    this.timeout = 0;//设置timeout属性为0，表示list命令执行后也不超时
}
```

