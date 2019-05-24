# mysql常用命令
## 登录及退出
[使用mysql客户端连接mysql服务器](http://www.freeoa.net/osuport/db/using-mysql-client-connect-server_1342.html)

__进入mysql命令行__
```mysql
mysql -u用户名 -p密码 -h服务器ip -P端口
```

**退出mysql命令行**
```mysql
exit 
quit
```
MySQL客户端程序可以在命令行中被调用，在调用时可以定义相关参数，参数可以通过命令行指定，也可以使用参数文件来指定。MySQL客户端程序的选项有两种形式：
长选项：使用双“-”后跟单词选项。
短选项：使用单“-”后跟字母选项。

对于大多数选项，都带有长选项和短选项两种形式，有些选项可以带有选项值。例如在使用--host和-h来指定我们要连接到的MySQL服务器的主机地址时，你可以在后面加上主机地址的值。对于长选项，使用“=”来分隔选项值。对于短选项，直接在选项后面跟选项值或者选项后面跟一个空格，然后在跟选项值，如下命令：
```shell
shell> mysql --host=myhost.freeoa.net
shell> mysql -h myhost.freeoa.net
shell> mysql -hmyhost.freeoa.net
```

**连接参数选项**

要连接到MySQL服务器，我们需要知道MySQL服务器的主机名(地址)、端口号，同时为了认证我们还需要提供所需的用户名和密码信息。下面是建立到MySQL服务器连接所需要的参数：

| 参数名 | 含义 |
|-----------|--------|
|--protocol |    该连接所使用的协议|
|--host |   MySQL服务器的主机名 |
|--port |   TCP/IP协议中的端口号 |
|--shared-memory-base-name |   共享内存协议中的共享内存名称|
|--socket |    Unix下的socket名或windows下的命名管道名 |

下面对上面的参数做下描述：

`--host=host_name或-h host_name`
该选项指定了客户端要连接的MySQL服务器的主机地址，可以指定主机名或IP地址。默认值为localhost。

`--port=port_number或-P port_number`
该参数指定了连接到MySQL服务器的端口号。仅在协议为tcp时可用。默认值为3306。

`--shared-memory-base-name=memory_name`
用于windows环境下指定共享内存的名称。默认值为MYSQL，区分大小写。

`--socket=socket_name 或-S socket_name`
在Unix下用于指定socket名称。在Windows下用于指定管道名称。Unix下的默认值为：/tmp/mysql.sock，在Windows下为MYSQL。

MySQL服务器认证所需要的参数：

|参数名 | 含义 |
|----------|--------|
|--user |    MySQL用户名 |
|--password |   MySQL密码 |

下面对认证所需的参数做了描述：

`-–user=user_name 或 -u user_name`
该参数指定了MySQL账户的用户名信息。在Windows下该参数默认值ODBC，在Unix下该参数默认值是你的登录名。

`-–password=pass_value 或 -ppass_value`
该参数指定了MySQL账户的密码信息。该参数没有默认值。如果忽略了此参数，MySQL会要求你录入。如果使用短选项方式，那么-p和密码间不能有空格。

如果给定了–-protocol选项，则需要指定客户端连接到服务器端的协议类型，有效的协议类型包括：

|协议类型  |  连接协议  |  适应系统 |
|--|--|--|
|tcp  |  TCP/IP协议连接到本机或远程MySQL  |  所有系统|
|socket  |  Unix的socket文件连接到本机  |  Unix|
|pipe  |  本地命名管道连接到本机  |  Windows|
|memory  |  使用共享内存连接到本机  |  Windows|

从上表可以看出，tcp协议是使用的最多的协议，它可以连接到本机的MySQL或远程的MySQL，同时也适应于所有的操作系统，对于其他类型的协议只能连接到本机的MySQL上。

对于命名管道(pipe)协议，只能用于Windows上连接到本机MySQL。然而要使用命名管道，MySQL服务器必须使用`mysqld-nt或mysqld-max-nt`来启动，并使用`--enable-named-pipe`选项。

共享内存(memory)只能用于Windows上连接到本机MySQL，MySQL服务器启动需要增加`-–shared-memory`选项。

## 查看库表信息
显示所有数据库
```
show databases;
```

使用数据库

```mysql
use 数据库名;
```

查看当前使用的数据库

```mysql
select database();
```

显示当前数据库所有表

```mysql
show tables;
```

显示指定表的创建语句，\G表示将查询结果按列打印，用于美化输出，可省略
```mysql
SHOW CREATE TABLE 表名 \G;
```

查看表结构

```mysql
desc PEOPLE;
```

## 事务提交方式

查看当前会话autocommit模式，Value是ON表示开启自动提交
```mysql
show variables like 'autocommit';
select @@session.autocommit;
select @@autocommit;
```

关闭当前会话自动提交
```mysql
set autocommit = 0;
```

查看全局autocommit模式
```mysql
select @@global.autocommit;
```

关闭全局自动提交
```mysql
set global autocommit = 0;
```
## 事务隔离级别
查看当前当前会话事务隔离级别，Value有READ-UNCOMMITED,READ-COMMITED,R
EPEATABLE-READ,SERIALIZABLE
```mysql
show variables like 'tx_isolation';
select @@tx_isolation;
select @@session.tx_isolation;
```
设置当前会话事务隔离级别,0 READ-UNCOMMITED, 1 READ-COMMITED, 2 REPEATABLE-READ, 3 SERIALIZABLE
```mysql
set tx_isolation = {0|1|2|3};
```

查看全局事务隔离级别
```mysql
select @@global.tx_isolation;
```

设置全局事务隔离级别
```mysql
set global tx_isolation = {0|1|2|3};
```

## 查看系统变量值

查看所有系统变量名及变量值

```mysql
show variables;
```

查看特定变量值

```mysql
show variables like '变量名';
```

查看字符集

```mysql
show variables like '%char%'; 
```

查看时区

```mysql
show variables like '%time_zone%';
```

# 问题处理

## Windows中无法通过快捷方式打开 Mysql Command Line client窗口

打开mysql命令窗口的快捷方式目标如下
`
"C:\Program Files\MySQL\MySQL Server 5.7\bin\mysql.exe" "--defaults-file=C:\Program Files\MySQL\MySQL Server 5.7\my.ini" "-uroot" "-p" "--default-character-set=utf8"
`

其中`"--defaults-file=C:\Program Files\MySQL\MySQL Server 5.7\my.ini"`指定从my.ini中读取配置信息。

但是在Windows上使用msi安装的mysql中安装目录下不存在my.ini文件，导致无法正常打开命令窗口。

解决方法1：  
> 从`C:\ProgramData\MySQL\MySQL Server 5.7`复制一份my.ini到`C:\Program Files\MySQL\MySQL Server 5.7`

解决方法2:

> 修改快捷方式的启动命令，去掉`"--defaults-file=C:\Program Files\MySQL\MySQL Server 5.7\my.ini"`