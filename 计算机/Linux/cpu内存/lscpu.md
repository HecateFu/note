> 参考资料
>
> [Linux 查看系统硬件信息(实例详解)](https://www.cnblogs.com/ggjucheng/archive/2013/01/14/2859613.html)

# 查看CPU信息

`lscpu` 查看cpu的统计信息.

```
blue@blue-pc:~$ lscpu
Architecture:          i686            #cpu架构
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian   #小尾序
CPU(s):                4               #总共有4核
On-line CPU(s) list:   0-3
Thread(s) per core:    1               #每个cpu核，只能支持一个线程，即不支持超线程
Core(s) per socket:    4               #每个cpu，有4个核
Socket(s):             1               #总共有1一个cpu
Vendor ID:             GenuineIntel    #cpu产商 intel
CPU family:            6
Model:                 42
Stepping:              7
CPU MHz:               1600.000
BogoMIPS:              5986.12
Virtualization:        VT-x            #支持cpu虚拟化技术
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              6144K
```

