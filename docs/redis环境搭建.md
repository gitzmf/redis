# Redis环境搭建

## 一、redis安装

**系统环境**

| 软件  | 版本  |
| ----- | ----- |
| Cenos | 7.6   |
| redis | 5.0.7 |

1. 在redis官方网站下载redis-5.0.7.tar.gz安装包，然后通过FTP上传到服务器/usr/local/zmf路径下

2. 解压安装包

   ```
   [root@localhost zmf]# tar -zxvf redis-5.0.7.tar.gz 
   ```

3. 进入解压包,启动redis

   ```
   [root@localhost zmf]# cd /usr/local/zmf/redis-5.0.7
   [root@localhost src]# ./redis-server /usr/local/zmf/redis-5.0.7/redis.conf 
   ```

## 二、过程中出现的问题

1. 问题1.redis desktop manager连接不上服务器的redis。cmd命令telnet ip port出现一下错误。

```
-DENIED Redis is running in protected mode because protected mode is enabled, no bind address was specified, no authentication passwo
rd is requested to clients. In this mode connections are only accepted from the loopback interface. If you want to connect from exter
nal computers to Redis you may adopt one of the following solutions: 1) Just disable protected mode sending the command 'CONFIG SET p
rotected-mode no' from the loopback interface by connecting to Redis from the same host the server is running, however MAKE SURE Redi
s is not publicly accessible from internet if you do so. Use CONFIG REWRITE to make this change permanent. 2) Alternatively you can j
ust disable the protected mode by editing the Redis configuration file, and setting the protected mode option to 'no', and then resta
rting the server. 3) If you started the server manually just for testing, restart it with the '--protected-mode no' option. 4) Setup
a bind address or an authentication password. NOTE: You only need to do one of the above things in order for the server to start acce
pting connections from the outside.
```

问题原因：redis默认bind只能是本机127.0.0.1，如果注释掉的话，会启动redis默认的保护机制，因此还需要配置密码或者保护机制设置为no

```
 1. 注释掉bind 127.0.0.1
 2. 设置密码,打开注释（#requirepass foobared）,并将foobared设置为需要的密码
 3. 关闭保护模式，设置protected-mode为no
```

2.问题2：redis文件配置好后不生效 问题原因：redis启动是需要指定配置文件的，否则会按照默认的，即使在配置文件中进行了配置。

```
    ./redis-server /usr/local/zmf/redis/redis.conf
```

注意：redis服务启动后会一直在终端显示，如果想要以守护进程的方式启动，修改配置文件daemonize属性即可