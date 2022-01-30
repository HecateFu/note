# 远程连接

> [sqlplus连接远程数据库](https://www.cnblogs.com/zhou__zhou/archive/2010/03/19/sqlplus.html)

```
sqlplus 用户名/密码@ip[:端口]/服务名 [as sysdba]
```

eg:

```
sqlplus eom/eomhzwq#2018@192.168.11.54:1521/orcl
```

使用默认1521端口时可省略输入

使用管理员账号时才需要添加  as sysdba

如何保证客户端机器连接到oracle数据库呢？ 引用：http://www.cnoug.org/viewthread.php?tid=15661

A． 客户端

1. 在客户端机器上安装ORACLE的Oracle Net通讯软件，它包含在oracle的客户端软件中。 
2. 正确配置了sqlnet.ora文件
3. 正确配置了tnsname.ora文件

B． 服务器端

1. 保证listener已经启动 lsntctl start
2. 保证数据库已经启动。 sql>startup