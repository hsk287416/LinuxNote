# 1. 安装Redis

redis是C语言开发的，因此安装redis时需要gcc环境：

```shell
yum install -y gcc-c++
```

安装包：redis-<版本号>.tar.gz
解压之后，将目录切换到redis目录下
然后执行以下命令进行安装：
```shell
make && make install
```

安装完毕之后，会在/usr/local/bin/目录下生成以下文件

- redis-benchmark：性能测试工具
- redis-check-aof：AOF文件修复工具
- redis-check-dump：RDB文件检查工具（快照持久化文件）
- redis-cli：命令行客户端
- redis-server：redis服务器启动命令

# 2. 运行/关闭Redis

## 2.1 运行服务端

首先要修改配置文件redis.conf如下：
```text
port 6379 => port [自定义端口号]
daemonize no => daemonize yes
bind 127.0.0.1 => #bind 127.0.0.1
protected-mode yes => protected-mode no
```

然后启动服务端：
```text
# 启动服务器时，要加上配置文件的路径（可以是相对也可以是绝对）
redis-server redis.conf
```

## 2.2 运行客户端
```text
redis-cli
```

## 2.3 关闭服务器
关闭服务器要通过客户端进行：
```text
redis-cli shutdown
```

# 3. Redis命令

## 3.1 String命令

字符串类型是Redis中最为基础的数据存储类型，它在Redis中是 **二进制安全** 的，这便意味着该类型存入和获取的数据相同。在Redis中字符串类型的Value最多可以容纳的数据长度是512M。

> 什么是二进制安全？
```text
首先，关系型数据库都是二进制不安全的，因为数据在传入关系型数据库的时候，会按照进行数据库所在服务器的编码格式进行一次编码，因此如果客户端和服务器的编码格式不同的话，则会出现乱码。
而Redis，数据在传入服务器的时候不会进行编码，而是直接以2进制的形式存储，因此不会出现乱码，这就是二进制安全。
```

| 命令类型 | 命令代码 | 说明 |
| -- | -- | -- |
| 赋值 | set key value | 设定key持有指定的字符串value，如果该key存在则进行覆盖操作。<br>因此执行结果总是返回“OK” |
| 取值 | get key | 获取key的value。如果该key关联的value不是String类型，Redis将返回错误信息，因为get命令只能用于获取String。<br>如果该key不存在，返回(nil) |
| 删除 | del key | 如果成功删除，返回（integer）1；如果删除对象不存在，则返回（integer）0 |
| 赋值&取值 | getset key value | 将新值赋给key，将旧值返回 |
| 增值 | incr num | 使num加1 |
| 减值 | decr num | 使num减1 |
| 增加指定值 | incrby num 10 | 使num加10 |
| 减去指定值 | decrby num 10 | 使num减10 |
| 字符串拼接 | append name "123" | 在name后面追加123 |

## 3.2 哈希命令

哈希的数据结构如下：
![avatar](https://raw.githubusercontent.com/hsk287416/LinuxNote/master/imgs/2018-08-11_081318.png)

| 命令类型 | 命令代码 | 说明 |
| -- | -- | -- |
| 赋值 | hset user:1 name hushukang | 为user:1的name属性赋值为hushukang |
| 取值 | hget user:1 name | 获取user:1中name属性的值 |
| 删除 | hdel user:1 name | 删除user:1中的name属性 |
| 判断指定key中属性是否存在 | hexists user:1 address | 判断user:1中是否存在address属性 |
| 获取属性数量 | hlen user:1 | 获取user:1中的属性数量 |

## 3.3 列表命令

列表的数据结构如下：
![avatar](https://raw.githubusercontent.com/hsk287416/LinuxNote/master/imgs/2018-08-11_084239.png)

| 命令类型 | 命令代码 | 说明 |
| -- | -- | -- |
| 插入值 | rpush key value1 value2 ... valueN | 向列表的右端插入一个值 |
| 插入值 | lpush key value1 value2 ... valueN | 向列表的左端插入一个值 |
| 插入值 | linsert key before|after value newValue | 在list指定的值前|后插入newValue |
| 弹出元素 | lpop key | 从列表左侧弹出一个元素 |
| 弹出元素 | rpop key | 从列表右侧弹出一个元素 |
| 删除元素 | lrem key count value | 根据count值，从列表中删除所有value相等的元素。<br>1). count > 0，从左到右，删除最多的count个value相等的元素<br>2). count < 0，从右到左，删除最多Math.abs(count)个value相等的元素<br>3). count = 0，删除所有value相等的元素 |
| 删除范围 | ltrim key start end | 截取索引范围内的列表并删除，索引从0开始 |
| 获取范围 | lrange key start end | 获取列表指定索引范围内的所有item |
| 获取索引 | lindex key index | 获取列表指定索引的item |
| 获取列表长度 | llen key | 获取列表长度 |
| 重新赋值 | lset key index newValue | 设置列表指定索引值为newValue |

## 3.4 集合命令

集合的数据结构如下：
![avatar](https://raw.githubusercontent.com/hsk287416/LinuxNote/master/imgs/2018-08-11_095849.png)

| 命令类型 | 命令代码 | 说明 |
| -- | -- | -- |
| 添加 | sadd key item | 向集合key中添加一个item，如果item已经存在则添加失败 |
| 删除 | srem key item | 从集合key中删除一个item |
| 随机取出一个元素 | srandmember key | 从集合key中随机取出一个元素 |
| 随机弹出一个元素 | spop key | 从集合key中随机取出并删除一个元素 |
| 取出所有元素 | smembers key | 取出集合key中所有的元素 |
| 算出集合中的元素个数 | scard key | 算出集合key中的元素个数 |
| 判断存在 | sismember key item | 判断集合key中时候有指定元素 |
| 集合间操作--差集 | sdiff key1 key2 | 获取两个集合的差集 |
| 集合间操作--并集 | sinter key1 key2 | 获取两个集合的并集 |
| 集合间操作--合并 | sunion key1 key2 | 合并两个集合（去除重复元素） |

# 4. Jedis

安装Maven依赖：
```xml
<!-- https://mvnrepository.com/artifact/redis.clients/jedis -->
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
</dependency>
```

# 5. pipeline

当我们从Redis中存取数据时，所消耗的时间等于两次网络通信时间（来回各一次）+Redis命令执行时间。如果我们要存入一万条数据，那么就需要20000 * 网络通信时间 + 10000Redis命令执行时间。我们知道，Redis命令执行速度是非常快的，可以在微秒单位内做出响应。然而网络通信时间是一个不可控的因素。因此，如果一万条数据要逐条保存的话，需要消耗大量的时间，而如果我们能把多个Redis命令打包起来，一次性发给Redis服务器，那么就会节省很多的网络通信时间。

没有pipeline
```java
// 执行10000次hset命令时，我们逐个执行
// 消耗时间：50s
Jedis jedis = new Jedis("127.0.0.1", 6379);
for (int i = 0; i < 10000; i++) {
    jedis.hset("hashkey:" + i, "field" + i, "value" + i);
}
```

使用pipeline
```java
// 执行10000次hset命令时，我们将每100条命令打成一个包，一起发给服务器
// 消耗时间：0.7s
Jedis jedis = new Jedis("127.0.0.1", 6379);
for (int i = 0; i < 100; i++) {
    Pipeline pipeline = jedis.pipelined();
    for (int j = 1 * 100; j < (i + 1) * 100; j++) {
        pipeline.hset("hashkey:" + j, "field" + j, "value" + j);
    }
    pipeline.syncAndReturnAll();
}
```

# 6. 主从复制

修改从节点的配置如下：
```text
slaveof ip port
slave-read-only yes
pidfile /var/run/redis-[自己的port].pid
dbfilename dump-[自己的port].rdb
```

# 7. Redis Sentine架构

# 7.1 主要配置
```shell
port [sentinel port]
daemonize yes
protected-mode no
dir "..."
logfile "..."
# 2的意思是，至少有2各sentinel发现当前master出现故障，才触发故障转移
sentinel monitor mymaster [hostname] [port] 2
sentinel down-after-milliseconds mymaster 5000
sentinel parallel-syncs mymaster 1
sentinel failover-timeout mymaster 180000
```

