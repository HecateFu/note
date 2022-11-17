> 参考资料
>
> [MySQL协议官方文档](https://dev.mysql.com/doc/dev/mysql-server/latest/PAGE_PROTOCOL.html)

# 1 简介

MySQL协议是用于MySQL客户端和服务端之间的通信协议，主要用于以下场景：

- MySQL客户端SDK，如MySQL的jdbc驱动mysql-connector-java
- MySQL代理，如Mycat、ShardingSphere Proxy
- MySQL主从服务器之间的通信

MySQL协议的主要特性：

- SSL透明加密
- 透明压缩
- 连接阶段（Connection Phase）进行客户端、服务端功能协商和认证数据交换
- 命令阶段（Command Phase）接收客户端命令并执行

# 2 连接生命周期

# 3 协议基础

# 4 协议包解析



