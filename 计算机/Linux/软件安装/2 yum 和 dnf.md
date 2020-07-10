> [菜鸟教程-yum](http://www.runoob.com/linux/linux-yum.html)
> 
> [Linux命令大全-yum](http://man.linuxde.net/yum)
>
> [在linux下如何使用yum查看安装了哪些软件包](https://blog.csdn.net/wenwenxiong/article/details/51785221)

yum(Yellowdog Updater, Modified)是一个在Fedora和RedHat以及SUSE中的Shell前端软件包管理器。

管理RPM包的软件，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。

yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令。

DNF新一代的RPM软件包管理器。他首先出现在 Fedora 18 这个发行版中。而最近，他取代了YUM，正式成为 Fedora 22 的包管理器（它没有预装在 CentOS 和 RHEL 7 中, CentOS 8 开始预装）。

# 查看帮助
```
yum -h
yum help
yum help 子命令
```

# 管理软件包

**安装** 
```bash
yum install              #全部安装
yum install package1     #安装指定的安装包package1
yum groupinsall group1   #安装程序组group1
```

更新和升级
```bash
yum update               #全部更新
yum update package1      #更新指定程序包package1
yum check-update         #检查可更新的程序
yum upgrade package1     #升级指定程序包package1
yum groupupdate group1   #升级程序组group1
```

查找和显示
```bash
yum info              #列出所有软件包的信息
yum info installed    #列出所有已安装的软件包信息
yum info updates      #列出所有可更新的软件包信息
yum info extras       #列出所有已安装但不在 Yum Repository 内的软件包信息
yum info package1     #显示安装包信息package1(完整包名)
yum groupinfo group1  #显示程序组group1信息yum search string 根据关键字string查找安装包

yum list              #显示所有已经安装和可以安装的程序包
yum list installed    #列出所有已安装的软件包
yum list updates      #列出所有可更新的软件包
yum list extras       #列出所有已安装但不在 Yum Repository 内的软件包
yum list package1     #显示指定程序包安装情况package1

yum deplist package1  #查看程序package1依赖情况

yum search <keyword>  #查找软件包（关键字搜索，支持通配符）
```

删除程序
```bash
yum remove package1         #删除程序包package1
yum groupremove group1      #删除程序组group1
```
# 管理仓库

生成缓存
```bash
yum makecache
```

清除缓存
```
yum clean packages       #清除缓存目录下的软件包
yum clean headers        #清除缓存目录下的 headers
yum clean oldheaders     #清除缓存目录下旧的 headers
yum clean, yum clean all (= yum clean packages; yum clean oldheaders) #清除缓存目录下的软件包及旧的headers
```

**显示和配置仓库**
```
yum repolist        #列出现在启用的仓库
yum config-manager  #管理配置和仓库
```

repo 的信息保存在 `/etc/yum.repos.d/` 目录下，文件后缀是 `.repo`，repo文件中 `$releasever` 表示当前系统的发行版本， `$basearch` 系统硬件架构(CPU指令集)，就是我们常说的i386、x86_64...

添加repo
```bash
sudo yum config-manager --add-repo=http://repo.fdzh.org/FZUG/FZUG.repo
```

启用repo
```
yum config-manager --set-enabled 仓库name
```

禁用repo
```
yum config-manager --set-disabled 仓库name
```

删除repo

进入`/etc/yum.repo.d/`删除相应的文件


# 其他子命令

| 子命令 | 说明 |
|---|---|
| shell | 进入yum的shell提示符 |
| resolvedep | 显示rpm软件包的依赖关系 |
| localinstall | 安装本地的rpm软件包 |
| localupdate | 显示本地rpm软件包进行更新 |
| deplist | 显示rpm软件包的所有依赖关系 |
