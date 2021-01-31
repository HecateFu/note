> [Linux ping命令详解](https://blog.csdn.net/gechong123/article/details/80609598)

# 简介

**ping命令用来测试主机之间网络的连通性**。执行ping指令会使用ICMP传输协议，发出要求回应的信息，若远端主机的网络功能没有问题，就会回应该信息，因而得知该主机运作正常。

# 命令格式
```bash
ping [选项] ip地址/域名
```
如果不指定次数，会一直ping， `ctrl + c` 终止。

# 常用选项
| 选项 | 说明 |
|---|---|
| -A | 自适应ping，根据ping包往返时间确定ping的速度 |
| **-c** | **ping指定次数后停止ping** |
| -f | 极限检测，快速连续ping一台主机，ping的速度达到100次每秒 |
| -i | 设定间隔几秒发送一个ping包，默认一秒ping一次 |
| -n | 不要将ip地址转换成主机名 **?** |
| -q | 不显示任何传送封包的信息，只显示最后的结果 |
| -R | 记录ping的路由过程(IPv4 only)，注意：由于IP头的限制，最多只能记录9个路由，其他会被忽略 |
| -s | 指定每次ping发送的数据字节数，默认为“56字节”+“28字节”的ICMP头，一共是84字节，包头+内容不能大于65535，所以最大值为65507（linux:65507, windows:65500） |
| -t | 设置TTL(Time To Live)为指定的值。该字段指定IP包被路由器丢弃之前允许通过的最大网段数 |
| -W | 以毫秒为单位设置ping的超时时间 |

> [ping命令显示的TTL是什么意思](https://blog.csdn.net/huwei2003/article/details/53113874)
>
> TTL 是由发送主机设置的，以防止数据包不断在 IP 互联网络上永不终止地循环。转发 IP 数据包时，要求路由器至少将 TTL 减小 1。
>
> 如果同一服务器不同的ip,你ping这些 ip得到的**ttl越高（经过转发的路由器少），延时越小**，说明直连该ip会更快


# 输出格式
```bash
$ ping -Ac 4 www.baidu.com
PING www.a.shifen.com (61.135.169.121) 56(84) bytes of data.
64 bytes from 61.135.169.121 (61.135.169.121): icmp_seq=1 ttl=55 time=23.8 ms
64 bytes from 61.135.169.121 (61.135.169.121): icmp_seq=2 ttl=55 time=24.3 ms
64 bytes from 61.135.169.121 (61.135.169.121): icmp_seq=3 ttl=55 time=24.2 ms
64 bytes from 61.135.169.121 (61.135.169.121): icmp_seq=4 ttl=55 time=23.9 ms

--- www.a.shifen.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 605ms
rtt min/avg/max/mdev = 23.768/24.029/24.272/0.198 ms, ipg/ewma 201.676/23.877 ms
```
1. `www.a.shifen.com (61.135.169.121)` ping目标主机的域名和IP（ping会自动将域名转换为IP）
2. `56(84)` 不带包头的包大小和带包头的包大小（参考“-s”参数）
3. `icmp_seq=1` ping序列，从1开始
4. `ttl=55` 剩余的ttl
5. `time` 响应时间,数值越小，联通速度越快
6. `4 packets transmitted, 4 received, 0% packet loss, time 605ms` 发出去的包数，返回的包数，丢包率，耗费时间
7. `rtt min/avg/max/mdev = 23.768/24.029/24.272/0.198 ms` 最小/最大/平均响应时间和本机硬件耗费时间

# 常用示例
每隔0.6秒ping一次，一共ping 5次
```bash
ping -c 5 -i 0.6 qq.com
```

极限快速的使用大包ping,以最快的速度，使用最大的包进行ping，可用于测试目标主机的承压能力。
```bash
ping -f -s 65507 10.0.0.52
```
注意：此用法非常危险，65535（包头+内容）*100个包每秒=6.25MB，每秒发送6.25MB的数据，相当于50Mbps的带宽，完全可能导致目标主机拒绝服务。请勿用于非法用途，造成不良后果自负。

# 其他ping工具
> [Linux Ping工具汇总](https://www.jianshu.com/p/fb6ba54dbb14)

- [fping](fping.md)
- hping3
- nping
- arping
- netcat
- nmap
