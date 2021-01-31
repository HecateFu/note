> [Fping命令解析](https://blog.csdn.net/wz_cow/article/details/80967255)

# 简介
Fping类似于ping，但比ping强大。Fping与ping不同的地方在于，fping可以在命令行中**指定要ping的主机数量范围**，也可以**指定含有要ping的主机列表文件**。
与ping要等待某一主机连接超时或发回反馈信息不同，**fping给一个主机发送完数据包后，马上给下一个主机发送数据包，实现多主机同时ping**。如果某一主机ping通，则此主机将被打上标记，并从等待列表中移除，如果没ping通，说明主机无法到达，主机仍然留在等待列表中，等待后续操作。

# 命令格式
```bash
fping [options] [target1 target2 target3...]
```

# 常用选项
| 选项 | 说明 |
|---|---|
| `-b <num>`, --size=BYTES | ping 数据包的大小。（默认为56） |
| `-c <num>`, --count=N | ping每个目标的次数 (默认为1) |
| `-f <num>`, --file=FILE | 从文件获取目标列表( - 表示从标准输入)(不能与 -g 同时使用) |
| -g, --generate | 通过指定开始和结束地址来生成目标列表（例如：./fping –g 192.168.1.0 192.168.1.255）或者一个IP/掩码形式（例如：./fping –g 192.168.1.0/24） |
| `-H <num>`, --ttl=N | 指定ttl的数值 |
| `-I <网卡id>`, --iface=IFACE | 指定网卡 |
| -l, --loop | 一直循环ping |
| `-p <num>`, --period=MSEC | ping同一个目标的时间间隔，单位毫秒，默认1000ms |
| `-r <num>`, --retry=N | 失败重试次数,默认3次 |
| `-t <num>`, --timeout=MSEC | 每个目标的超时时间，默认500ms |
| -a, --alive | 只显示可连通的目标 |
| -A, --addr | 以ip显示连接目标 |
| -D, --timestamp | 每行ping结果输出前显示时间戳 |
| `-i <num>`, --interval=MSEC | ping不同目标的时间间隔，默认10ms |
| -q, --quiet | 不显示每个目标的单独的ping结果  |
| `-Q <num>`, --squiet=SECS | 和-q功能类似，但是每隔指定秒数输出一下摘要信息 |
| -s, --stats | 最后输出统计信息 |
| -u, --unreach | 只显示不可联通的目标 |
| -v, --version | 输出版本信息 |

# 输出结果

```bash
[hecatefu@fu-fedora ~]$ fping -c 4 -s -D www.baidu.com google.com qq.com
# 每个目标，每次ping的结果
# [时间戳] 目标名称 ： [序号]， 数据包大小， 当次响应时间 (平均响应时间，丢包率)
[1568193120.207587] www.baidu.com : [0], 84 bytes, 22.5 ms (22.5 avg, 0% loss)
[1568193120.234580] qq.com        : [0], 84 bytes, 28.3 ms (28.3 avg, 0% loss)
[1568193121.208364] www.baidu.com : [1], 84 bytes, 23.1 ms (22.8 avg, 0% loss)
[1568193121.248498] qq.com        : [1], 84 bytes, 41.3 ms (34.8 avg, 0% loss)
[1568193122.212356] www.baidu.com : [2], 84 bytes, 26.3 ms (23.9 avg, 0% loss)
[1568193122.226463] qq.com        : [2], 84 bytes, 18.5 ms (29.4 avg, 0% loss)
[1568193123.209962] www.baidu.com : [3], 84 bytes, 22.2 ms (23.5 avg, 0% loss)
[1568193123.228934] qq.com        : [3], 84 bytes, 19.2 ms (26.8 avg, 0% loss)
# 每个目标的ping结果
www.baidu.com : xmt/rcv/%loss = 4/4/0%, min/avg/max = 22.2/23.5/26.3
google.com    : xmt/rcv/%loss = 4/0/100%
qq.com        : xmt/rcv/%loss = 4/4/0%, min/avg/max = 18.5/26.8/41.3
# 所有目标的统计信息
       3 targets
       2 alive
       1 unreachable
       0 unknown addresses

       1 timeouts (waiting for response)
      12 ICMP Echos sent
       8 ICMP Echo Replies received
       0 other ICMP received

 18.5 ms (min round trip time)
 25.2 ms (avg round trip time)
 41.3 ms (max round trip time)
        4.026 sec (elapsed real time)

```
# 常用示例
> [Fping - Linux的高性能Ping工具](https://www.howtoing.com/ping-multiple-linux-hosts-using-fping)

Fping多个IP地址
```bash
fping 50.116.66.139 173.194.35.35 98.139.183.24
```

Fping IP地址范围
```bash
fping -s -g 192.168.0.1 192.168.0.9
```

ping整个网段，并将失败的重复一次（ -r 1 ）。
```bash
fping -g -r 1 192.168.0.0/24
```

从文件中读取目标列表
```bash
fping < fping.txt
```
或
```bash
fping -f fping.txt
```
