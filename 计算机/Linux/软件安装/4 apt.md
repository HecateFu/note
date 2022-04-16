apt 是 Debian 和 Ubuntu 中的软件包管理工具，是对 dpkg 封装，可以更加方便的管理 deb 包的仓库、下载、查找、依赖关系、安装、升级、卸载。

[apt vs apt-get](https://blog.csdn.net/liudsl/article/details/79200134)

最常用的 Linux 包管理命令都被分散在了 apt-get、apt-cache 和 apt-config 这三条命令当中。

apt 命令的引入就是为了解决命令过于分散的问题，它包括了 apt-get 命令出现以来使用最广泛的功能选项，以及 apt-cache 和 apt-config 命令中很少用到的功能。

# 主要命令

更新可用的安装包列表

```
apt update
```

通过安装或升级安装包来升级系统

```
apt upgrade
```

