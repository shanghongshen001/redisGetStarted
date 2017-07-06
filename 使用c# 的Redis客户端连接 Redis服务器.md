![image](https://redis.io/images/redis-white.png)

# [Redis](https://redis.io/)

# c# redis客户端

既然redis是c/s架构，我们可以使用redis-cli客户端去连接服务端，那么我们也可以使用其他客户端区连接服务端。

c#中比较知名的两个redis客户端库有：ServiceStack.Redis 和 StackExchange.Redis。遗憾的是ServiceStack.Redis在4.0后选择闭源商业化发展，离开了开源的怀抱。

---

**目前C# 用得最多的应该是StackExchange.Redis**

打开nuget 按照下载量排序，可以看到StackExchange.Redis 下载量雄踞第一

![image](https://ws1.sinaimg.cn/large/006aR3cagy1fh901trbcwj30p00f740v.jpg)

# 使用StackExchange.Redis连接redis服务端

[github地址](https://github.com/StackExchange/StackExchange.Redis/)

```
假设你已经安装好了redis 并且配置好了环境。
如果你还没有安装redis请先安装redis。
```


## 1.新建一个控制台应用
![image](https://ws1.sinaimg.cn/large/006aR3cagy1fh906oza7tj311f0hhdh3.jpg)

## 2.nuget安装StackExchange.Redis
![image](https://ws1.sinaimg.cn/large/006aR3cagy1fh907lzsxaj30p00f775s.jpg)

## 基本使用

### 3.1获得一个ConnectionMultiplexer 


```
ConnectionMultiplexer redis = ConnectionMultiplexer.Connect("127.0.0.1:6379");
```


![image](https://ws1.sinaimg.cn/large/006aR3cagy1fha4zqdu91j310t0g43zo.jpg)


```
原文:
Once you have a ConnectionMultiplexer, there are 3 main things you might want to do:

access a redis database (note that in the case of a cluster, a single logical database may be spread over multiple nodes)
make use of the pub/sub features of redis
access an individual server for maintenance / monitoring purposes

大概意思：
如果你已经获得了一个ConnectionMultiplexer 对象，那么你接下来需要做这3件事情

1 访问redis数据库
2 使用redis 的  pub/sub（pubish-subscribe的缩写） 发布/订阅 功能
3 访问单个服务器进行维护/监控
```

### 3.1 访问单个redis数据库


```
IDatabase db = redis.GetDatabase();

```
![image](https://ws1.sinaimg.cn/large/006aR3cagy1fha6c1de6bj311y0kqmzp.jpg)

### 3.2 string操作

```
string value = "abcdefg";
db.StringSet("mykey", value);
...
string value = db.StringGet("mykey");
Console.WriteLine(value); // writes: "abcdefg"

```

![image](https://ws1.sinaimg.cn/large/006aR3cagy1fha6f0c9fjj311y0jj40j.jpg)


---

---

**Over**