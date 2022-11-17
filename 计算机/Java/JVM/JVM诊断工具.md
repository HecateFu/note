 [eclipse mat 使用说明](https://developer.ibm.com/zh/articles/os-cn-ecl-ma/)

[byte Http11OutputBuffer 内存泄露](https://juejin.cn/post/6844903834142113805)

[max-http-header-size 源码分析](https://www.jianshu.com/p/ab054620da64)

[eclipse mat 主要概念说明](https://blog.csdn.net/coslay/article/details/48182709)

[java 内存模型说明](https://blog.csdn.net/outsanding/article/details/102698525)

[tomcat 源码详解](https://www.cnblogs.com/fatmanhappycode/p/12348265.htmlhttps://www.cnblogs.com/fatmanhappycode/p/12348265.html)



jps 查找java进程

jinfo 获取java进程配置信息，包括java系统属性、虚拟机命令参数

jstat 虚拟机状态统计，可监控内存、编译、gc情况

-gc option

Garbage-collected heap statistics.

S0C: Current survivor space 0 capacity (kB).

S1C: Current survivor space 1 capacity (kB).

S0U: Survivor space 0 utilization (kB).

S1U: Survivor space 1 utilization (kB).

EC: Current eden space capacity (kB).

EU: Eden space utilization (kB).

OC: Current old space capacity (kB).

OU: Old space utilization (kB).

MC: Metaspace capacity (kB).

MU: Metacspace utilization (kB).

CCSC: Compressed class space capacity (kB).

CCSU: Compressed class space used (kB).

YGC: Number of young generation garbage collection events.

YGCT: Young generation garbage collection time.

FGC: Number of full GC events.

FGCT: Full garbage collection time.

GCT: Total garbage collection time.



jstack 查看java线程信息

jmap 获取java内存信息，-histo 查看类统计，-dump 转储内存信息文件

```
jmap -F -dump:format=b,file=heap.hprof <pid>

jmap -dump:live,format=b,file=heap.hprof <pid>
```

[Unable to open socket file: target process not responding or HotSpot VM not loaded](https://blog.csdn.net/ydk888888/article/details/108500935)

```
-XX:NativeMemoryTracking=[off | summary | detail]
jcmd <pid> VM.native_memory baseline
jcmd <pid> VM.native_memory detail scale=MB > a1.txt 
jcmd <pid> VM.native_memory detail.diff scale=MB > a2.txt
```

https://blog.csdn.net/longaiyunlay/article/details/102787768
https://blog.csdn.net/Developlee/article/details/100691997

-XX:MetaspaceSize=64m -XX:MaxMetaspaceSize=128m

[一次完整的JVM堆外内存泄漏故障排查记录](https://www.cnblogs.com/rude3knife/p/13570423.html)

-XX:+CMSClassUnloadingEnabled -XX:+UseConcMarkSweepGC

[What does JVM flag CMSClassUnloadingEnabled actually do?](https://stackoverflow.com/questions/3334911/what-does-jvm-flag-cmsclassunloadingenabled-actually-do)

[21 MOST IMPORTANT JAVA 8 VM OPTIONS FOR SERVERS](https://www.maknesium.de/21-most-important-java-8-vm-options-for-servers)

jhat

----

jconsole

jvisualvm

https://github.com/oracle/visualvm/issues/115

```
etc/visualvm.conf -J-Dsun.java2d.uiScale=1.0 --fontsize 15
```

[jvisualvm 连接 jstatd监控jvm_zuoshuaiax的专栏-CSDN博客](https://blog.csdn.net/zuoshuaiax/article/details/73849515)

jstatd -J-Djava.security.policy=/home/weblogic/jstatd.all.policy -J-Djava.rmi.server.hostname=192.168.11.70



jmc

jcmd

[jcmd oracle doc](https://docs.oracle.com/javase/8/docs/technotes/guides/troubleshoot/tooldescr006.html)

[find jcmd dump file default path](https://stackoverflow.com/questions/58519663/where-is-the-heap-dump-file-created-by-jcmd)

```
jcmd <pid> help
jcmd <pid> help <command> 
jcmd <pid> GC.heap_dump <fileName>
lsof -p 17352 | grep cwd
```



MAT

https://java.jiderhamn.se/2011/12/11/classloader-leaks-i-how-to-find-classloader-leaks-with-eclipse-memory-analyser-mat/

JProfiler

---

hprof

jmx

jfr