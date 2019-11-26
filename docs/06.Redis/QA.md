# 问题与简答

## Redis 篇

扩展阅读[《官方文档》](https://redis.io/documentation)

### Redis详解

扩展阅读[《Redis详解》](https://www.cnblogs.com/ysocean/category/1221478.html)

#### 1、redis的简介与安装

扩展阅读 [《redis的简介与安装》](https://www.cnblogs.com/ysocean/p/9074353.html)

#### 2、redis的配置文件介绍
扩展阅读 [《redis的配置文件介绍》](https://www.cnblogs.com/ysocean/p/9074787.html)

#### 3、redis的五大数据类型详细用法
扩展阅读 [《redis的五大数据类型详细用法》](https://www.cnblogs.com/ysocean/p/9080940.html)

#### 4、redis的底层数据结构

扩展阅读 [《redis的底层数据结构》](https://www.cnblogs.com/ysocean/p/9080942.html)

#### 5、redis的五大数据类型实现原理

扩展阅读 [《redis的五大数据类型实现原理》](https://www.cnblogs.com/ysocean/p/9102811.html)

#### 6、RDB 持久化

扩展阅读 [《RDB 持久化》](https://www.cnblogs.com/ysocean/p/9114268.html)

#### 7、AOF 持久化

扩展阅读 [《AOF 持久化》](https://www.cnblogs.com/ysocean/p/9114267.html)

#### 8、主从复制

扩展阅读 [《主从复制》](https://www.cnblogs.com/ysocean/p/9143118.html)

### Redis基础、高级特性与性能调优
原文地址[Redis基础、高级特性与性能调优](https://www.jianshu.com/p/2f14bc570563)

### Redis 与 Memcache 区别

|对比项|Redis|Memcache|
|-|-|-|
|数据结构|丰富数据类型|只支持简单 KV 数据类型|
|数据一致性|事务|cas|
|持久性|快照/AOF|不支持|
|网络IO|单线程 IO 复用|多线程、非阻塞 IO 复用|
|内存管理机制|现场申请内存|预分配内存|

扩展阅读[《redis和memcache区别，源码怎么说》](../14.对比篇/redis和memcache区别.md)

### 缓存穿透，缓存击穿，缓存雪崩解决方案分析

扩展阅读[《缓存穿透，缓存击穿，缓存雪崩》](https://www.cnblogs.com/sbj-dawn/p/11116673.html)

### 发布订阅

扩展阅读[《redis发布订阅》](http://doc.redisfans.com/topic/pubsub.html)

### 持久化策略 (RDB/AOF 持久化)

扩展阅读[redis持久化](http://doc.redisfans.com/topic/persistence.html)

### Redis 事务

扩展阅读[《redis事务》](http://doc.redisfans.com/topic/transaction.html)

```shell
redis> MULTI  #标记事务开始
OK
redis> INCR user_id  #多条命令按顺序入队
QUEUED
redis> INCR user_id
QUEUED
redis> INCR user_id
QUEUED
redis> PING
QUEUED
redis> EXEC  #执行
1) (integer) 1
2) (integer) 2
3) (integer) 3
4) PONG
```

> 在 Redis 事务中如果有某一条命令执行失败，其后的命令仍然会被继续执行

> 使用 DISCARD 可以取消事务，放弃执行事务块内的所有命令

### 如何实现分布式锁

#### 方式一

```text
tryLock() {
    SETNX Key 1 Seconds
}
release() {
    DELETE Key
}
```

缺陷：C<sub>1</sub> 执行时间过长未主动释放锁，C<sub>2</sub> 在 C<sub>1</sub> 的锁超时后获取到锁，C<sub>1</sub> 和 C<sub>2</sub> 都同时在执行，可能造成数据不一致等未知情况。如果 C<sub>1</sub> 先执行完毕，则会释放 C<sub>2</sub> 的锁，此时可能导致另外一个 C<sub>3</sub> 获取到锁

#### 方式二

```text
tryLock() {
    SETNX Key UnixTimestamp Seconds
}
release() {
    EVAL (
        //LuaScript
        if redis.call("get", KEYS[1] == ARGV[1]) then
            return redis.call("del", KEYS[1])
        else
            return 0
        end
    )
}
```

缺陷：极高并发场景下(如抢红包场景)，可能存在 UnixTimestamp 重复问题。分布式环境下物理时钟一致性，也无法保证，也可能存在 UnixTimestamp 重复问题

#### 方式三

```text
tryLock() {
    SET Key UniqId Seconds
}
release() {
    EVAL (
        //LuaScript
        if redis.call("get", KEYS[1]) == ARGV[1] then
            return redis.call("del", KEYS[1])
        else
            return 0
        end
    )
}
```

> 执行 `SET key value NX` 的效果等同于执行 `SETNX key value`

目前最优的分布式锁方案，但是如果在集群下依然存在问题。由于 Redis 集群数据同步为异步，假设在 Master 节点获取到锁后未完成数据同步情况下 Master 节点 crash，在新的 Master 节点依然可以获取锁，所以多个 Client 同时获取到了锁

### Redis 过期策略及内存淘汰机制

#### 过期策略

Redis 的过期策略就是指当 Redis 中缓存的 Key 过期了，Redis 如何处理

- 定时过期：每个设置过期时间的 Key 创建定时器，到过期时间立即清除。内存友好，CPU 不友好

- 惰性过期：访问 Key 时判断是否过期，过期则清除。CPU 友好，内存不友好

- 定期过期：隔一定时间，expires 字典中扫描一定数量的 Key，清除其中已过期的 Key。内存和 CPU 资源达到最优的平衡效果

#### 内存淘汰机制

```shell
[root]# redis-cli config get maxmemory-policy
1) "maxmemory-policy"
2) "noeviction"
```

- noeviction：新写入操作会报错
- allkeys-lru：移除最近最少使用的 key
- allkeys-random：随机移除某些 key
- volatile-lru：在设置了过期时间的键中，移除最近最少使用的 key
- volatile-random：在设置了过期时间的键中，随机移除某些 key
- volatile-ttl：在设置了过期时间的键中，有更早过期时间的 key 优先移除

### 为什么 Redis 是单线程的

Redis 是基于内存的操作，CPU 不是 Redis 的瓶颈，Redis 瓶颈最有可能是内存或网络。而且单线程容易实现，避免了不必要的上下文切换和竞争条件，不存在多线程切换消耗 CPU

### Redis单线程的优劣势

#### 单进程单线程优势
- 代码更清晰，处理逻辑更简单

- 不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗

- 不存在多进程或者多线程导致的切换而消耗CPU
 
#### 单进程单线程弊端

- 无法发挥多核CPU性能，不过可以通过在单机开多个Redis实例来完善

### Redis的高并发和快速原因
1、redis是基于内存的，内存的读写速度非常快；

2、redis是单线程的，省去了很多上下文切换线程的时间；

3、redis使用多路复用技术，可以处理并发的连接。非阻塞IO 内部实现采用epoll，采用了epoll+自己实现的简单的事件框架。epoll中的读、写、关闭、连接都转化成了事件，然后利用epoll的多路复用特性，绝不在io上浪费一点时间

### 如何利用 CPU 多核心

在单机单实例下，如果操作都是 O(N)、O(log(N)) 复杂度，对 CPU 消耗不会太高。为了最大利用 CPU，单机可以部署多个实例

### Redis为什么这么快

1、完全基于内存，绝大部分请求是纯粹的内存操作，非常快速。数据存在内存中，类似于HashMap，HashMap的优势就是查找和操作的时间复杂度都是O(1)；

2、数据结构简单，对数据操作也简单，Redis中的数据结构是专门进行设计的；

3、采用单线程，避免了不必要的上下文切换和竞争条件，也不存在多进程或者多线程导致的切换而消耗 CPU，不用去考虑各种锁的问题，不存在加锁释放锁操作，没有因为可能出现死锁而导致的性能消耗；

4、使用多路I/O复用模型，非阻塞IO；

5、使用底层模型不同，它们之间底层实现方式以及与客户端之间通信的应用协议不一样，Redis直接自己构建了VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求

### Redis单核CPU占用过高

#### 出现cpu过高的原因有：

1、连接数过多，通过redis-cli info | grep connected_clients查看

2、慢查询，因为redis是单线程，如果有慢查询的话，会阻塞住之后的操作，通过redis日志查 

3、value值过大？比如value几十兆，当然这种情况比较少，其实也可以看做是慢查询的一种

4、aof重写/rdb fork发生？瞬间会堵一下Redis服务器

#### 对应解决方案：
1、连接数过多解决：

1.1 关闭僵尸连接
采用redi-cli登录,采用client kill ip:port(redis远程连接的ip和端口)。 
需要采用脚本批量删除多个连接

1.2 修改redis timeout参数
采用redis-cli登录，采用config set timeout xx 设置redis的keepalive时间

1.3 修改redis进程的文件数限制
echo -n "Max open files  3000:3000" >  /proc/PID/limits

1.4 修改系统参数的最大文件数限制
/etc/security/limits.conf

2、对慢查询进行持久化，比如定时存放到mysql之类。（redis的慢查询只是一个list，超过list设置的最大值，会清除掉之前的数据，也就是看不到历史）

3、限制key的长度和value的大小

#### 使用redis的注意事项:
1、Master最好不要做任何持久化工作，包括内存快照和AOF日志文件，特别是不要启用内存快照做持久化。

2、如果数据比较关键，某个Slave开启AOF备份数据，策略为每秒同步一次。

3、为了主从复制的速度和连接的稳定性，Slave和Master最好在同一个局域网内。

4、尽量避免在压力较大的主库上增加从库

5、为了Master的稳定性，主从复制不要用图状结构，用单向链表结构更稳定，即主从关系为：Master<--Slave1<--Slave2<--Slave3.......， 这样的结构也方便解决单点故障问题，实现Slave对Master的替换，也即，如果Master挂了，可以立马启用Slave1做Master，其他不变

6、使用Redis负载监控工具：redis-monitor，它是一个Web可视化的 redis 监控程序