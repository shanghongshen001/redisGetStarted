![image](https://redis.io/images/redis-white.png)

# [Redis](https://redis.io/)


key | value
---|---
github | [redis](https://github.com/MSOpenTech/redis)
开发者 | Salvatore Sanfilippo
许可 | BSD
存储位置 | 内存
可持久化 | 可持久化到硬盘
非关系型 | 是
存储类型 | 字符串，列表，散列表，有序集合
性能 | 主从备份，可以从任意从服务器发起访问

## 优点
- 读写性能优异
- 支持数据持久化，支持AOF和RDB两种持久化方式
- 支持主从复制，主机会自动将数据同步到从机，可以进行读写分离。(主服务器,主要负责写入数据,多台从服务器,负责数据的读取)
- 数据结构丰富：除了支持string类型的value外还支持string、hash、set、sortedset、list等数据结构。

## 缺点

- Redis不具备自动容错和恢复功能，主机从机的宕机都会导致前端部分读写请求失败，需要等待机器重启或者手动切换前端的IP才能恢复。
- 主机宕机，宕机前有部分数据未能及时同步到从机，切换IP后还会引入数据不一致的问题，降低了系统的可用性。
- redis的主从复制采用全量复制，复制过程中主机会fork出一个子进程对内存做一份快照，并将子进程的内存快照保存为文件发送给从机，这一过程需要确保主机有足够多的空余内存。若快照文件较大，对集群的服务能力会产生较大的影响，而且复制过程是在从机新加入集群或者从机和主机网络断开重连时都会进行，也就是网络波动都会造成主机和从机间的一次全量的数据复制，这对实际的系统运营造成了不小的麻烦。
- Redis较难支持在线扩容，在集群容量达到上限时在线扩容会变得很复杂。为避免这一问题，运维人员在系统上线时必须确保有足够的空间，这对资源造成了很大的浪费。


## Web中应用场景

### 1．在主页中显示最新的项目列表。

Redis使用的是常驻内存的缓存，速度非常快。LPUSH用来插入一个内容ID，作为关键字存储在列表头部。LTRIM用来限制列表中的项目数最多为5000。如果用户需要的检索的数据量超越这个缓存容量，这时才需要把请求发送到数据库。

### 2．删除和过滤。

如果一篇文章被删除，可以使用LREM从缓存中彻底清除掉。 

### 3．排行榜及相关问题。

排行榜（leader board）按照得分进行排序。ZADD命令可以直接实现这个功能，而ZREVRANGE命令可以用来按照得分来获取前100名的用户，ZRANK可以用来获取用户排名，非常直接而且操作容易。

### 4．按照用户投票和时间排序。

这就像Reddit的排行榜，得分会随着时间变化。LPUSH和LTRIM命令结合运用，把文章添加到一个列表中。一项后台任务用来获取列表，并重新计算列表的排序，ZADD命令用来按照新的顺序填充生成列表。列表可以实现非常快速的检索，即使是负载很重的站点。

### 5．过期项目处理。

使用unix时间作为关键字，用来保持列表能够按时间排序。对current_time和time_to_live进行检索，完成查找过期项目的艰巨任务。另一项后台任务使用ZRANGE...WITHSCORES进行查询，删除过期的条目。

### 6．计数。

进行各种数据统计的用途是非常广泛的，比如想知道什么时候封锁一个IP地址。INCRBY命令让这些变得很容易，通过原子递增保持计数；GETSET用来重置计数器；过期属性用来确认一个关键字什么时候应该删除。

### 7．特定时间内的特定项目。

这是特定访问者的问题，可以通过给每次页面浏览使用SADD命令来解决。SADD不会将已经存在的成员添加到一个集合。

### 8．实时分析正在发生的情况，用于数据统计与防止垃圾邮件等。

使用Redis原语命令，更容易实施垃圾邮件过滤系统或其他实时跟踪系统。

### 9．Pub/Sub。

在更新中保持用户对数据的映射是系统中的一个普遍任务。Redis的pub/sub功能使用了SUBSCRIBE、UNSUBSCRIBE和PUBLISH命令，让这个变得更加容易。 

### 10．队列。

在当前的编程中队列随处可见。除了push和pop类型的命令之外，Redis还有阻塞队列的命令，能够让一个程序在执行时被另一个程序添加到队列。你也可以做些更有趣的事情，比如一个旋转更新的RSS feed队列。

### 11．缓存。

Redis缓存使用的方式与memcache相同。

网络应用不能无休止地进行模型的战争，看看这些Redis的原语命令，尽管简单但功能强大，把它们加以组合，所能完成的就更无法想象。当然，你可以专门编写代码来完成所有这些操作，但Redis实现起来显然更为轻松。


# [MSOpenTech](https://github.com/MSOpenTech/redis)


---

Redis 官方没有提供windows下面安装的方法,windows下的Redis是微软官方的MSOpenTech团队fork官方的版本。
然后做了一些特殊的更改

**更改内容**
```
## Windows-specific changes
- There is a replacement for the UNIX fork() API that simulates the copy-on-write behavior using a memory mapped file on 2.8. Version 3.0 is using a similar behavior but dropped the memory mapped file in favor of the system paging file.
- In 3.0 we switch the default memory allocator from dlmalloc to jemalloc that is supposed to do a better job at managing the heap fragmentation.
- Because Redis makes some assumptions about the values of file descriptors, we have built a virtual file descriptor mapping layer. 
```


---

![image](https://ws1.sinaimg.cn/large/006aR3cagy1fh8umb8xmmj30rx0fs76o.jpg)

更多资料请参考[WIKI](https://github.com/MSOpenTech/redis/wiki/Memory-Configuration)

# windows下安装

## 1.下载

![image](https://ws1.sinaimg.cn/large/006aR3cagy1fh8vcm7bcqj30vb0futb6.jpg)

**我下载的msi安装版**

![image](https://ws1.sinaimg.cn/large/006aR3cagy1fh8vfftedtj30sa0hjjt6.jpg)

## 2.安装

**安装中需要注意的就是勾选把安装目录加入环境变量**


参考:

---

[redis对比其余数据库](http://www.cnblogs.com/jing99/p/6112055.html)

---

[redis优缺点总结](http://blog.csdn.net/oanqoanq/article/details/51281548)
