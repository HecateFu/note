查看所有进程的cpu、内存使用情况

> [ps -aux命令详解](https://www.cnblogs.com/dion-90/articles/9048627.html)

```shell
ps aux
```

返回结果列

```
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
```

列表含义如下：

| 类名    | 含义                                                         |
| ------- | ------------------------------------------------------------ |
| USER    | 进程的属主；                                                 |
| PID     | 进程的ID；                                                   |
| %CPU    | 进程占用的CPU百分比；                                        |
| %MEM    | 占用内存的百分比；                                           |
| VSZ     | 进程使用的虚拟内存量（KB）；                                 |
| RSS     | 该进程占用的物理内存量（KB）（驻留中页的数量）；             |
| TTY     | 该进程在哪个终端上运行（登陆者的终端位置），若与终端无关，则显示（？）。若为pts/0等，则表示由网络连接主机进程 |
| STAT    | 状态位                                                       |
| START   | 该进程被触发启动时间；                                       |
| TIME    | 该进程实际使用CPU运行的时间；                                |
| COMMAND | 命令的名称和参数                                             |

STAT状态位常见的状态字符：

| 状态字符 | 含义                                                |
| -------- | --------------------------------------------------- |
| D        | 无法中断的休眠状态（通常 IO 的进程）；              |
| R        | 正在运行可中在队列中可过行的；                      |
| S        | 处于休眠状态；                                      |
| T        | 停止或被追踪；                                      |
| W        | 进入内存交换 （从内核2.6开始无效）；                |
| X        | 死掉的进程 （基本很少见）；                         |
| Z        | 僵尸进程；                                          |
| <        | 优先级高的进程                                      |
| N        | 优先级较低的进程                                    |
| L        | 有些页被锁进内存；                                  |
| s        | 进程的领导者（在它之下有子进程）；                  |
| l        | 多进程的（使用 CLONE_THREAD, 类似 NPTL pthreads）； |
| +        | 位于后台的进程组；                                  |



> [Linux终端查看最消耗CPU内存的进程](https://www.linuxprobe.com/linux-sort-max-mc.html)

CPU占用最多的前10个进程

```
ps auxw|head -1;ps auxw|sort -rn -k3|head -10
```

按CPU用量升序对进程排序

```
ps auxw --sort=%cpu
```



内存消耗最多的前10个进程

```
ps auxw|head -1;ps auxw|sort -rn -k4|head -10
```

按物理内存用量升序对进程排序

```
ps auxw --sort=rss
```



虚拟内存使用最多的前10个进程

```
ps auxw|head -1;ps auxw|sort -rn -k5|head -10
```

