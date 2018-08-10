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
daemonize no => daemonize yes
```

然后启动服务端：
```text
redis-server redis.conf
```

## 2.2 运行客户端
```text
#启动服务器时，要加上配置文件的路径（可以是相对也可以是绝对）
redis-server redis.conf
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

哈希的结构示意图如下：
![avatar](https://raw.githubusercontent.com/hsk287416/LinuxNote/master/imgs/2018-08-11_081318.png)

| 命令类型 | 命令代码 | 说明 |
| -- | -- | -- |
| 赋值 | hset user:1 name hushukang | 为user:1的name属性赋值为hushukang |
| 取值 | hget user:1 name | 获取user:1中name属性的值 |
| 删除 | hdel user:1 name | 删除user:1中的name属性 |
| 判断指定key中属性是否存在 | hexists user:1 address | 判断user:1中是否存在address属性 |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |
|  |  |  |