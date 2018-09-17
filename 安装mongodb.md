> 解压丶更名丶移动

```text
tar -zxvf mongodb-linux-x86_64-4.0.0.tgz
mv mongodb-linux-x86_64-4.0.0 mongodb
mv mongodb /usr/local
```

> 创建数据和日志存储路径

```text
cd /usr/local/mongodb
mkdir data && mkdir data/db
touch data/logs
```

> 编写配置文件

```text
cd /usr/local/mongodb/bin
vi mongodb.conf
```

添加内容如下：
```text
port=27017
dbpath=/usr/local/mongodb/data/db

logappend=true
fork=true
logpath=/usr/local/mongodb/data/logs
bind_ip=0.0.0.0
```

**注意：开启守护进程模式fork的时候，一定要设置log日志； 设置log日志时，logpath路径必须为文件路径； 
设置dbpath时，路径必须为文件夹路径；**

> 自启动配置

```text
cd /usr/lib/systemd/system
vi mongodb.service
```

编辑内容如下：
```text
[Unit]  
Description=mongodb
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
ExecStart=/usr/local/mongodb/bin/mongod -f /usr/local/mongodb/bin/mongodb.conf  
ExecReload=/bin/kill -s HUP $MAINPID  
ExecStop=/usr/local/mongodb/bin/mongod --shutdown -f /usr/local/mongodb/bin/mongodb.conf  
PrivateTmp=true  

[Install]  
WantedBy=multi-user.target
```

| 说明 | 命令 |
| -- | -- |
|开机启动|systemctl enable mongodb.service|
|启动|systemctl start mongodb.service|
|关闭|systemctl stop mongodb.service|




