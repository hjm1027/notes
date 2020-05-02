# Redis简介

## Redis是什么
Redis全名为REmote DIctionary Server，它是完全开源免费的，遵守BSD协议，是一个高性能的key-value型，支持网络，可基于内存亦可持久化的日志型数据库。

Redis可以支持多种键值类型，其中常见的有5种： String字符类型 map散列类型 list列表类型 set集合类型 sortedset有序集合类型

Redis是非关系型数据库，它不使用表，他的数据库不会预定义或者强制去要求用户对Redis存储的不同数据进行关联

数据库的工作模式按存储方式可分为：硬盘数据库和内存数据库。Redis 将数据储存在内存里面，读写数据的时候都不会受到硬盘 I/O 速度的限制，所以速度极快。

（1）硬盘数据库的工作模式：

![](https://user-gold-cdn.xitu.io/2018/8/22/1656042975b51bbe?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

（2）内存数据库的工作模式：

![](https://user-gold-cdn.xitu.io/2018/8/22/16560429758ecf00?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Redis官网：https://redis.io/
