# 查看堆信息

jdk8

```
jmap -heap <pid>
```

jdk11，jdk8 之后不能直接使用上面的命令，需要使用 `jhsdb jmap` 代替

```
jhsdb jmap --heap --pid <pid>
```

Ubuntu中执行jmap出现类似错误

> [sun.jvm.hotspot.types.WrongTypeException](https://stackoverflow.com/questions/49516601/jstack-in-mixed-mode-wrongtypeexception-no-suitable-match-for-type-of-address)

```
hecatefu@FU-PC:~$ jhsdb jmap --heap --pid 790
Attaching to process ID 790, please wait...
Debugger attached successfully.
Server compiler detected.
JVM version is 11.0.15+10-Ubuntu-0ubuntu0.20.04.1
Exception in thread "main" sun.jvm.hotspot.types.WrongTypeException: No suitable match for type of address 0x00007efefc023a60
        at jdk.hotspot.agent/sun.jvm.hotspot.runtime.InstanceConstructor.newWrongTypeException(InstanceConstructor.java:62)
        at jdk.hotspot.agent/sun.jvm.hotspot.runtime.VirtualConstructor.instantiateWrapperFor(VirtualConstructor.java:80)
        at jdk.hotspot.agent/sun.jvm.hotspot.memory.Universe.heap(Universe.java:150)
        at jdk.hotspot.agent/sun.jvm.hotspot.tools.HeapSummary.run(HeapSummary.java:61)
        at jdk.hotspot.agent/sun.jvm.hotspot.tools.JMap.run(JMap.java:115)
        at jdk.hotspot.agent/sun.jvm.hotspot.tools.Tool.startInternal(Tool.java:260)
        at jdk.hotspot.agent/sun.jvm.hotspot.tools.Tool.start(Tool.java:223)
        at jdk.hotspot.agent/sun.jvm.hotspot.tools.Tool.execute(Tool.java:118)
        at jdk.hotspot.agent/sun.jvm.hotspot.tools.JMap.main(JMap.java:176)
        at jdk.hotspot.agent/sun.jvm.hotspot.SALauncher.runJMAP(SALauncher.java:369)
        at jdk.hotspot.agent/sun.jvm.hotspot.SALauncher.main(SALauncher.java:538)
```

安装jdk同版本的 `openjdk-<版本>-dbg`

```
sudo apt install openjdk-11-dbg
```

