# Redis简介


### 发展历史
2008年，意大利的一家创业公司Merzia推出了一款基于MySQL的网站实时统计系统LLOOGG，然而没过多久该公司的创始人 Salvatore Sanfilippo便 对MySQL的性能感到失望，于是他决定亲自为LLOOGG量身定做一个数据库，并于2009年开发完成，这个数据库就是Redis。 不过Salvatore Sanfilippo并不满足只将Redis用于LLOOGG这一款产品，而是希望更多的人使用它，于是在同一年Salvatore Sanfilippo将Redis开源发布，并开始和Redis的另一名主要的代码贡献者Pieter Noordhuis一起继续着Redis的开发，直到今天。

Salvatore Sanfilippo自己也没有想到，短短的几年时间，Redis就拥有了庞大的用户群体。Hacker News在2012年发布了一份数据库的使用情况调查，结果显示有近12%的公司在使用Redis。国内如新浪微博、街旁网、知乎网，国外如GitHub、Stack Overflow、Flickr等都是Redis的用户。

VMware公司从2010年开始赞助Redis的开发， Salvatore Sanfilippo和Pieter Noordhuis也分别在3月和5月加入VMware，全职开发Redis。
<br/><br/><br/>
### Redis是什么
Redis全名为REmote DIctionary Server，它是完全开源免费的，遵守BSD协议(五种开源协议之一)，是一个高性能的key-value型，支持网络，可基于内存亦可持久化的日志型数据库。

Redis可以支持多种键值类型，其中常见的有5种： String字符类型 map散列类型 list列表类型 set集合类型 sortedset有序集合类型

Redis是非关系型数据库，它不使用表，他的数据库不会预定义或者强制去要求用户对Redis存储的不同数据进行关联

Redis官网：https://redis.io/
<br/><br/><br/>
### Redis特性

* 性能极高 – Redis读的速度是110000次/s,写的速度是81000次/s 。
* 丰富的数据类型 – Redis支持二进制案例的 Strings, Lists, Hashes, Sets 及 Ordered Sets 数据类型操作。
* 原子性 – Redis的单个操作都是原子性的。多个操作也可以用MULTI和EXEC指令包起来来表现出原子性。
* 支持多种语言
* 功能丰富
* 主从复制
* 支持高可用和分布式
<br/><br/><br/>
### Redis性能高的原因
#### 1.完全基于内存
绝大部分请求是纯粹的内存操作，非常快速。数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)；

数据库的工作模式按存储方式可分为：硬盘数据库和内存数据库。Redis将数据储存在内存里面，读写数据的时候都不会受到硬盘 I/O 速度的限制，所以速度极快。

（1）硬盘数据库的工作模式：

![](https://user-gold-cdn.xitu.io/2018/8/22/1656042975b51bbe?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

（2）内存数据库的工作模式：

![](https://user-gold-cdn.xitu.io/2018/8/22/16560429758ecf00?imageView2/0/w/1280/h/960/format/webp/ignore-error/1)

Redis也提供了持久化的选项，这些选项可以让用户将自己的数据保存到磁盘上面进行存储，重启的时候再次加载进行使用。
<br/><br/>
#### 2.数据结构简单
Redis中的数据结构是专门进行设计的，对数据操作十分简单。
<br/><br/>
#### 3.采用单线程
避免了不必要的上下文切换和竞争条件，也不存在多进程或者多线程导致的切换而消耗 CPU，不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗；
<br/><br/>
#### 4.使用多路I/O复用模型，非阻塞IO

“多路”指的是多个网络连接，“复用”指的是复用同一个线程

多路I/O复用模型是利用 select、poll、epoll 可以同时监察多个流的 I/O 事件的能力，在空闲的时候，会把当前线程阻塞掉，当有一个或多个流有 I/O 事件时，就从阻塞态中唤醒，于是程序就会轮询一遍所有的流（epoll 是只轮询那些真正发出了事件的流），并且只依次顺序的处理就绪的流，这种做法就避免了大量的无用操作。

>下面举一个例子，模拟一个tcp服务器处理30个客户socket。假设你是一个老师，让30个学生解答一道题目，然后检查学生做的是否正确，你有下面几个选择：
>
>1. 第一种选择：按顺序逐个检查，先检查A，然后是B，之后是C、D...这中间如果有一个学生卡住，全班都会被耽误。这种模式就好比，你用循环挨个处理>socket，根本不具有并发能力。
>
>2. 第二种选择：你创建30个分身，每个分身检查一个学生的答案是否正确。 这种类似于为每一个用户创建一个进程或者线程处理连接。
>
>3. 第三种选择，你站在讲台上等，谁解答完谁举手。这时C、D举手，表示他们解答问题完毕，你下去依次检查C、D的答案，然后继续回到讲台上等。此时E、A又举手，然后去处理E和A。。。 这种就是IO复用模型，Linux下的select、poll和epoll就是干这个的。将用户socket对应的fd注册进epoll，然后epoll帮你监听哪些socket上有消息到达，这样就避免了大量的无用操作。
<br/><br/>
#### 5.使用底层模型不同
们之间底层实现方式以及与客户端之间通信的应用协议不一样，Redis直接自己构建了VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求。
<br/><br/><br/>

### Redis常用的五种数据结构

#### 1.字符串（String）
字符串是redis中最基础的数据结构，键都是字符串类型。

字符串类型的值实际可以是字符串(简单的字符串、复杂的字符串(例如JSON、XML))、数字(整数、浮点数),甚至是二进制(图片、音频、视频),但值最大不能超过512MB

```
redis 127.0.0.1:6379> SET asd redis
OK
redis 127.0.0.1:6379> GET asd
"redis"
```

##### 内部编码
* int：8个字节的长整型。
* embstr：<=39字节的字符串。
* raw：大于39个字节的字符串

#### 2.哈希（Hash）
Redis hash是一个string类型的field和value的映射表,每个hash可以存储 2的32次方-1键值对（40多亿）。

```
127.0.0.1:6379>  HMSET redishash name "redis hash" description "redis basic data structure" likes 20 visitors 23000
OK
127.0.0.1:6379>  HGETALL redishash
1) "name"
2) "redis hash"
3) "description"
4) "redis basic data structure"
5) "likes"
6) "20"
7) "visitors"
8) "23000"
```

##### 内部编码
内层的哈希使用的内部编码可以是压缩列表（ziplist）和哈希表（hashtable）两种；Redis的外层的哈希则只使用了hashtable。

1.ziplist（压缩列表）：压缩列表是Redis为了节约内存而开发的，是由一系列特殊编码的连续内存块(而不是像双端链表一样每个节点是指针)组成的顺序型数据结构；具体结构相对比较复杂。与双向链表相比，压缩列表可以节省内存空间，但是进行修改或增删操作时，复杂度较高；因此当节点数量较少时，可以使用压缩列表；但是节点数量多时，还是使用双向链表更好。

当哈希类型元素个数小于hash-max-ziplist-entries 配置(默认512个)、同时所有值都小于hash-max-ziplist-value配置(默认64 字节)时,Redis会使用ziplist作为哈希的内部实现

2.hashtable（哈希表）：当哈希类型无法满足ziplist的条件时,Redis会使 用hashtable作为哈希的内部实现,因为此时ziplist的读写效率会下降,而 hashtable的读写时间复杂度为O(1)

#### 3.
