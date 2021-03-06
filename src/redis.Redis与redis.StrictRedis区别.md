---
layout: post
title: redis.Redis与redis.StrictRedis区别
slug: redis
date: 2019-7-17 20:34
status: publish
author: 一灰
categories: 
  - Python
tags: 
  - 博客
  - Redis
excerpt: redis-py提供两个类Redis和StrictRedis用于实现Redis的命令...
---

redis-py提供两个类Redis和StrictRedis用于实现Redis的命令，StrictRedis用于实现大部分官方的命令，并使用官方的语法和命令（比如，SET命令对应与StrictRedis.set方法）。Redis是StrictRedis的子类，用于向后兼容旧版本的redis-py。 简单说，官方推荐使用StrictRedis方法。

不推荐Redis类，原因是他和咱们在redis-cli操作有些不一样，主要不一样是下面这三个方面。

·LREM：参数 ‘num’ 和 ‘value’ 的顺序交换了一下，cli是 lrem queueName 0 ‘string’ 。 这里的0时所有的意思。 但是Redis这个类，把控制和string调换了。

·ZADD：实现时 score 和 value 的顺序不小心弄反了，后来有人用了，就这样了

·SETEX: time 和 value 的顺序反了

.Pool: 连接池

Redis的连接池的方法：

```
pool = redis.ConnectionPool(host=‘localhost‘, port=6379, db=0)

r = redis.Redis(connection_pool=pool)
```
StrictRedis的连接池的实现方式:
```
In [4]: pool = redis.ConnectionPool(host=‘127.0.0.1‘, port=6379)

In [5]: r = redis.StrictRedis(connection_pool=pool)
```

官方的创建redis的时候，都可以添加什么参数。

>class redis.StrictRedis(host=‘localhost‘, port=6379, db=0, password=None, socket_timeout=None, connection_pool=None, charset=‘utf-8‘, errors=‘strict‘, decode_responses=False, unix_socket_path=None)
Implementation of the Redis protocol.
This abstract class provides a Python interface to all Redis commands and an implementation of the Redis protocol.
Connection and Pipeline derive from this, implementing how the commands are sent and received to the Redis server

redis的对于有些编码入库的问题，redis的连接附加的参数里面，默认编码是utf-8，但是如果你非要用GBK那就需要指明你的chardet和decode_responses为True 。

原文地址(还是我的博客): [https://www.yifeilu.cn/index.php/archives/32/](https://www.yifeilu.cn/index.php/archives/32/)