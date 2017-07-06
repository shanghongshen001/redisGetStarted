![image](https://redis.io/images/redis-white.png)

# [Redis](https://redis.io/)


# Redis 目录

## 删除这些讨厌的东西

让我不解的是安装好的目录下面有很多的pdb文件，这些调试文件会很影响性能，我不知道开发团队为什么要把这些文件打包到安装包中去，作为一个强迫症患者，我建议删掉这些文件。删除的时候提示删不掉，请先停止redis服务。

![image](https://ws1.sinaimg.cn/large/006aR3cagy1fh8yeae02rj30nl0aqdh5.jpg)

## 简单介绍目录

![image](https://ws1.sinaimg.cn/large/006aR3cagy1fh8ysr6u70j30kt07rgmf.jpg)

按类型排个序可以看到 有用的就是
- 2个conf配置文件
- 4个exe
- 1个dll

### conf

打开着两个conf可以看到熟悉的linux配置文件的写法。这两个配置文件的内容基本相差不大，根据名字猜测，第一个应该是默认的配置文件，文件里面有个

```
# Redis configuration file example
```
第二个我猜测应该是安装后的服务默认使用的配置文件。

### 4个exe

- redis-server.exe redis服务端
- redis-cli.exe redis客户端
- redis-check-aof.exe 应该是AOF工具 [AOF介绍](http://redisbook.readthedocs.io/en/latest/internal/aof.html)
- redis-benchmark.exe 性能测试工具

### 1个dll
这个dll根据命名猜测应该是日志相关的。

# Redis服务

我下载的是MSI版本的安装包，在安装之前我猜想这种安装包可能或帮我配置好环境，安装后发现它还把Redis server 注册成了服务。

![image](https://ws1.sinaimg.cn/large/006aR3cagy1fh8yfe47mnj30qg01omxc.jpg)

## 两种服务端启动方法

- 直接启动服务
- cd到redis安装目录  然后执行 
```
redis-server.exe redis.windows.conf 
```
- 需要注意的是由于端口只能独占，如果以服务方式启动后，就不能已命令方式启动了。


# Redis客户端

以下两种方式启动
- cd到redis安装目录 输入 redis-cli 
- 双击redis-cli.exe

由于双击启动的方式是自动连接到本机的redis默认端口，如果想连接别的redis服务器（外网或局域网其他电脑）， 显然只能使用命令的方式连接了
连接命令

```
默认直接连接  远程连接-h 192.168.1.20 -p 6379
```
更多redis-cli [请参考](http://www.cnblogs.com/silent2012/p/5368925.html)


# 简单测试

**启动服务端**

![image](https://ws1.sinaimg.cn/large/006aR3cagy1fh8zkm93eyj30rl0efjs3.jpg)

![image](https://ws1.sinaimg.cn/large/006aR3cagy1fh8zlgosdhj30rl0efq37.jpg)



---
初次体检redis 感觉redis的安装使用都较为简洁，命令简单易懂。