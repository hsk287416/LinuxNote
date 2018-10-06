# 1. 安装

执行命令：
```shell
yum -y install vsftpd
```

**注意：**
- rpm -qa | grep vsftpd 可以检查是否已经安装vsftpd
- 默认配置文件在/etc/vsftpd/vsftpd.conf


# 2. 创建虚拟用户

创建ftp文件夹：mkdir [path]/ftpfile，如/ftpfile

然后添加匿名用户：
```shell
useradd ftpuser -d /ftpfile -s /sbin/nologin
```

修改ftpfile权限
```shell
chown -R ftpuser.ftpuser /ftpfile
```

重设ftpuser的密码：
```shell
echo "12345" | passwd --stdin ftpuser
```

# 3. 配置vsftpd

```shell
cd /etc/vsftpd
echo "ftpuser" > chroot_list # ftpuser是之前创建的虚拟用户
```

修改/etc/selinux/config，修改为
```text
SELINUX=disabled
```
**注意：修改完SELINUX，需要重启服务器**

# 4. 常用配置项
| 配置项 | 值 | 含义 |
| -- | -- | -- |
| local_root | /ftpfile | 当本地用户登入时，将被更换到定义的目录下，默认值为各用户的Home目录 |
| anon_root | /ftpfile | 使用匿名用户登入时，所登入的目录 |
| use_localtime | YES | 默认是GMT事件，改为使用本机系统时间 |
| anonymous_enable | NO | 不允许匿名用户登录 |
| local_enable | YES | 允许本地用户登录 |
| write_enable | YES | 本地用户可以在local_root目录中进行读写操作 |
| local_umask | 022 | 本地用户新增文件时的umask值 |
| dirmessage_anable | YES | 如果启动这个选项，那么使用者在第一次进入一个目录时，会检查该目录下是否有.message这个文件。如果有的话，则会出现该文件的内容，通常这个文件会放置欢迎话语，或者是对该目录的说明。默认是YES |
| xferlog_enable | YES | 是否启用上传/下载日志记录。如果启用，则上传与下载的信息将会完成记录在xfrelog_file所定义的文件中。预设为开启 |
| connect_from_port_20 | YES | 制定FTP使用20端口进行数据传输，默认是YES |
| xferlog_std_format | YES | 如果启用，则日志文件将会写成xferlog的标准格式 |
| ftpd_banner | 欢迎标语 | 这里用来定义欢迎标语的字符串 |
| chroot_local_user | YES | 是否将所有用户限制在主目录,YES为启用 NO禁用.(该项默认值是NO,即在安装vsftpd后不做配置的话，ftp用户是可以向上切换到要目录之外的) |
| chroot_list_enable | YES | 设置是否启用chroot_list_file配置项指定的用户列表文件 |
| chroot_list_file | /etc/vsftpd/chroot_list | 用于指定用户列表文件 |
| pasv_min_port | 61001 | 被动模式使用端口范围最小值 |
| pasv_max_port | 62000 | 被动模式使用端口范围最大值 |
| pasv_enable | TRUE | 启用被动模式 |

> 被动模式和主动模式
```text
FTP协议有两种工作方式，PORT方式和PASV方式，中文意思为主动模式和被动模式。

1. PORT（主动模式）的连接过程是：客户端项服务器的FTP端口（默认是21）发送连接请求，服务器接受连接，建立一条命令链路。当需要传送数据时，客户端在命令链路上用PORT命令告诉服务器：“wo打开了***端口，你过来连接我”。于是服务器从20端口向客户端的***端口发送连接请求，建立一条数据链路来传递数据。

2. PASV（被动模式）的连接过程是：客户端项服务器的FTP端口（默认是21）发送请求连接，服务器接受连接，建立一条命令链路。当需要传送数据时，服务器在命令链路上用PASV命令告诉客户端：“我打开了***端口，你过来连接我”。于是客户端项服务器的***端口发送连接请求，建立一条数据链路来传递数据。

从上面可以看出，两种方式的命令链路连接方法是一样的，而数据链路的建立方法就完全不同。
```