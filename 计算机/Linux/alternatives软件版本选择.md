同时安装同一个命令的多个版本时，可使用该命令配置使用指定版本

# 常用命令

## 添加命令可选版本

```
alternatives --install <link> <name> <path>
```
- link 命令软连接路径
- name 命令名称
- path 命令可执行文件路径

## 指定当前使用版本

```
alternatives --config <name>
```

- name 命令名称


# 安装多版本jdk

1. 查看 java 命令指向的路径

   ```
   [hecatefu@fedora ~]$ which java
   /usr/bin/java
   ```

2. 查看 `/usr/bin/java` 类型

   ```
   [hecatefu@fedora ~]$ ll /usr/bin/java
   lrwxrwxrwx. 1 root root 22 Oct  4 16:00 /usr/bin/java -> /etc/alternatives/java
   ```

3. 添加可选版本
   ```
   [hecatefu@fedora ~]$ sudo alternatives --install /usr/bin/java java /home/hecatefu/jdk1.8.0_301/bin/java 2
   ```

4. 配置版本
   ```
   [hecatefu@fedora ~]$ sudo alternatives --config java
   
   There are 2 programs which provide 'java'.
   
     Selection    Command
   -----------------------------------------------
   *+ 1           java-11-openjdk.x86_64 (/usr/lib/jvm/java-11-openjdk-11.0.12.0.7-4.fc34.x86_64/bin/java)
      2           /home/hecatefu/jdk1.8.0_301/bin/java
   
   Enter to keep the current selection[+], or type selection number: 2
   [hecatefu@fedora ~]$ java -version
   java version "1.8.0_301"
   Java(TM) SE Runtime Environment (build 1.8.0_301-b09)
   Java HotSpot(TM) 64-Bit Server VM (build 25.301-b09, mixed mode)
   ```
