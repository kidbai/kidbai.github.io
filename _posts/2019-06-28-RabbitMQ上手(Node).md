# 大纲
- 介绍
- 安装以及编码

## 介绍

RabbitMQ 是一个消息代理，其主要思想比较简单。允许接收和转发消息。你可以把RabbitMQ想成是一个邮局，当你发一份邮件到邮箱，你相当确定邮递员最终会把这封信送到你的收件人那边。
使用这个比喻，RabbitMQ可以是一个邮箱，或一个邮局，或一个邮递员。
其与邮局主要的不同是实际上RabbitMQ并不处理内容，而是接收，存储然后转发二进制数据--消息（message）

在RabbitMQ的消息传递上，我们用一些术语来解释：

- 生产（Producing） 意味着只做发送的事情。一个发送消息（message）的程序可以作为一个生产者（producer）.图如下，代号‘P’

   ![image](https://www.rabbitmq.com/img/tutorials/producer.png)
   
- 队列（queue）是一个邮箱的名字，存在在 RabbitMQ中。尽管消息（messages）是在RabbitMQ和你的应用中传递，但是它们只能被存到一个队列中。这个队列没有任何限制，可以存储你想要的任何多的消息，实际上，它就是一个无限的Buffer，大量生产者可以把消息发送到一个
队列中，消费者们可以尝试从一个队列中去接收数据。一个队列可以画成下图所示：上方是它的名字

![image](https://www.rabbitmq.com/img/tutorials/queue.png)

- 消费（Consuming）和接收的意思类似。一个等待接收消息的程序是一个消费者（Consumer）。如下图所示，代号“C”：

![image](https://www.rabbitmq.com/img/tutorials/consumer.png)

- 生产者-队列-消费者

![image](https://www.rabbitmq.com/img/tutorials/python-one.png)


## 安装以及编码

- 安装部署RabbitMQ
- amqplib （node）

### 安装部署RabbitMQ

通过docker安装

#### 获查询镜像
```
docker search rabbitmq:management
```
可以查到：
```
Youngbye➜  ~  ᐅ  docker search rabbitmq:management

NAME                                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
macintoshplus/rabbitmq-management   Based on rabbitmq:management whit python and…   3                                       [OK]
transmitsms/rabbitmq-sharded        Fork of rabbitmq:management with sharded_exc…   0
xiaochunping/rabbitmq               xiaochunping/rabbitmq:management   2018-06-30   0
```
拉取镜像
```
docker pull rabbitmq:management
```
运行镜像
```
docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmq rabbitmq:management

```
访问后台
```
http://localhost:15672/#/
```

### 用例

创建一个消费者 
```
// consumer.js
// 使用callback API
var amqp = require('amqplib/callback_api')

// 链接rabbitmq server
amqp.connect('amqp://localhost', function (err, conn) {
  //创建通道
  conn.createChannel(function (err, ch) {
    // 声明一个队列
    var q = 'queue'
    ch.assertQueue(q, {durable: false})
    console.log(' [*] 等待消息 in %s [对列名]. To exit press CTRL+C', q)
    // 消费
    ch.consume(q, function(msg) {
      console.log(msg)
      console.log(' [x] 收到消息 %s', msg.content.toString())
      ch.ack(msg)
    })
  })
})
```

生产者:
```
// send.js
var amqp = require('amqplib/callback_api')

amqp.connect('amqp://localhost', function (err, conn) {
  conn.createChannel(function (err, ch) {
    var q = 'queue'
    ch.assertQueue(q, {durable: false})
    // 发送消息到队列中
    ch.sendToQueue(q, new Buffer('hello world'))
    console.log('[x] send Hello world')
  })
  setTimeout(function () {
    // 关闭通道
    conn.close()
    process.exit(0)
  }, 500)
})

```

启动消费者
```
node consumer.js
```
发送被消费的消息
```
node send.js
```# 大纲
- 介绍
- 安装以及编码

## 介绍

RabbitMQ 是一个消息代理，其主要思想比较简单。允许接收和转发消息。你可以把RabbitMQ想成是一个邮局，当你发一份邮件到邮箱，你相当确定邮递员最终会把这封信送到你的收件人那边。
使用这个比喻，RabbitMQ可以是一个邮箱，或一个邮局，或一个邮递员。
其与邮局主要的不同是实际上RabbitMQ并不处理内容，而是接收，存储然后转发二进制数据--消息（message）

在RabbitMQ的消息传递上，我们用一些术语来解释：

- 生产（Producing） 意味着只做发送的事情。一个发送消息（message）的程序可以作为一个生产者（producer）.图如下，代号‘P’

   ![image](https://www.rabbitmq.com/img/tutorials/producer.png)
   
- 队列（queue）是一个邮箱的名字，存在在 RabbitMQ中。尽管消息（messages）是在RabbitMQ和你的应用中传递，但是它们只能被存到一个队列中。这个队列没有任何限制，可以存储你想要的任何多的消息，实际上，它就是一个无限的Buffer，大量生产者可以把消息发送到一个
队列中，消费者们可以尝试从一个队列中去接收数据。一个队列可以画成下图所示：上方是它的名字

![image](https://www.rabbitmq.com/img/tutorials/queue.png)

- 消费（Consuming）和接收的意思类似。一个等待接收消息的程序是一个消费者（Consumer）。如下图所示，代号“C”：

![image](https://www.rabbitmq.com/img/tutorials/consumer.png)

- 生产者-队列-消费者

![image](https://www.rabbitmq.com/img/tutorials/python-one.png)


## 安装以及编码

- 安装部署RabbitMQ
- amqplib （node）

### 安装部署RabbitMQ

通过docker安装

#### 获查询镜像
```
docker search rabbitmq:management
```
可以查到：
```
Youngbye➜  ~  ᐅ  docker search rabbitmq:management

NAME                                DESCRIPTION                                     STARS               OFFICIAL            AUTOMATED
macintoshplus/rabbitmq-management   Based on rabbitmq:management whit python and…   3                                       [OK]
transmitsms/rabbitmq-sharded        Fork of rabbitmq:management with sharded_exc…   0
xiaochunping/rabbitmq               xiaochunping/rabbitmq:management   2018-06-30   0
```
拉取镜像
```
docker pull rabbitmq:management
```
运行镜像
```
docker run -d -p 5672:5672 -p 15672:15672 --name rabbitmq rabbitmq:management

```
访问后台
```
http://localhost:15672/#/
```

### 用例

创建一个消费者 
```
// consumer.js
// 使用callback API
var amqp = require('amqplib/callback_api')

// 链接rabbitmq server
amqp.connect('amqp://localhost', function (err, conn) {
  //创建通道
  conn.createChannel(function (err, ch) {
    // 声明一个队列
    var q = 'queue'
    ch.assertQueue(q, {durable: false})
    console.log(' [*] 等待消息 in %s [对列名]. To exit press CTRL+C', q)
    // 消费
    ch.consume(q, function(msg) {
      console.log(msg)
      console.log(' [x] 收到消息 %s', msg.content.toString())
      ch.ack(msg)
    })
  })
})
```

生产者:
```
// send.js
var amqp = require('amqplib/callback_api')

amqp.connect('amqp://localhost', function (err, conn) {
  conn.createChannel(function (err, ch) {
    var q = 'queue'
    ch.assertQueue(q, {durable: false})
    // 发送消息到队列中
    ch.sendToQueue(q, new Buffer('hello world'))
    console.log('[x] send Hello world')
  })
  setTimeout(function () {
    // 关闭通道
    conn.close()
    process.exit(0)
  }, 500)
})

```

启动消费者
```
node consumer.js
```
发送被消费的消息
```
node send.js
```