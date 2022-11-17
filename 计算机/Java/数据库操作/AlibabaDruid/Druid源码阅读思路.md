# Druid源码阅读思路

1. pool 模块是内核，都得看。其他的模块，你可以挑感兴趣的，以及你用得着的看。

2. 技巧：整体看大面的之外，，细节是写个demo，debug，跟着单步调试走。关注重点步骤：getConnection，，shrink，，，recycle，，，，，

3. druid实现主线去做源码的分析，怎么初始化连接的、如何拿连接、怎么保证连接线程之间不干扰等等，如果自己来设计应该怎么设计，需要考虑哪些点

# [druid关键问题清单]([druid关键问题清单.md (shimo.im)](https://shimo.im/docs/gXqmeww2wPSXbYqo))

连接如何创建的？ 

连接如何回收的？ 

哪些情况下会丢弃连接？

 哪些情况下会创建新连接？ 

如何实现断链重连的？ 

存在哪些可能的超时情况？ 

GC会如何影响druid的统计？ 

应用侧统计的慢SQL可能会有哪些坑？

跟数据库的统计有会什么差异？

 getConnection的过程是什么？ 

为什么要有holder？ 

testOnBorrow，testOnReturn，testWithIdle有什么区别？ 

removeAbandoned问题。怎么起作用的，线上能否打开？ 

Evict问题。 

keepAlive问题。 

unfair问题。  

如何检测连接泄露问题？ 

连接池配置有什么建议？ 

PSCache提升性能？ 

phyMaxUseCount有什么用？ 

常见错误问题？  

[Druid源码阅读笔记-冬天里的懒猫](https://www.jianshu.com/u/cca22173a13f)