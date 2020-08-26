> [Windows PowerShell基本语法及常用命令](https://blog.csdn.net/Mr_Pang/article/details/50571866)

# 基础语法

## 变量

- 定义：**PowerShell的变量无需预定义，可直接使用。当使用一个变量时，该变量被自动声明。**变量以 `$` 开头，不需指定变量类型。但是也可以强制指定变量类型，如 `[int] $b = 5` 则此变量只能包含整数值，如果包含其他类型会出错。 如 `[int] $b = "aaaa"` 就会报错。

- 赋值：使用 `=` 赋值，也可以使用  `set-variable` 赋值。

- 使用：使用 `$变量名` 参与运算。

- 输出：`$变量名` 将变量值输出到控制台，也可使用 `get-variable` 输出变量值。

- 使用范围：定义的变量如果不删除，在整个命令窗口打开的生命周期有效。

- 数据类型： 

  [int] 、[long]、[string] 、[char] 、[bool] 、[byte] 、[double] 、[decimal] 、[single]

  [array] ：数组对象

  [xml] ：XML对象

  [hashtable] ：哈希表对象，类似于一个字典对象

```powershell
# 给变量赋值
> $a =10
> $b = 1
> set-variable qwe123 qweqweqwe
# 使用变量
> $c = $a + $b
# 输出变量
> $a
10
> $b
1
> $c
11
> $qwe123
qweqweqwe
> get-variable qwe123
Name                           Value
----                           -----
qwe123                         qweqweqw
```

## 运算符

### 算数运算

+，-，*，/，%，++，--

### 比较运算

小于等于	-le

## 输出结果

write-host 结果输出到控制台

```powershell
> write-host $n,$total
0 16
> write-host $n-$total
0-16
```

## 流程控制

### 循环

#### while

```powershell
# 语法
while (条件)
{
循环体
}
# 实例
> $total = 16                                                                             > $n =0                                                                                   > while ($total -le 200)
>> {
>> $n++
>> $total = ($total + 7) * 1.13
>> write-host $n,$total
>> }                                                                                     1 25.99
2 37.2787
3 50.034931
4 64.44947203
5 80.7379033939
6 99.143830835107
7 119.942528843671
8 143.445057593348
9 170.002915080483
10 200.013294040946
```

# 常用命令

## 操作变量

```powershell
# 设置变量，变量值可以省略
set-variable 变量名 变量值
# 获取变量，支持通配符查找，可设置查找范围
get-variable 变量名
# 删除变量
remove-variable 变量名
```

## 输出

```powershell
# 控制台输出，可以自定义输入的文字颜色、文字背景色、分隔符等等
write-host 输出内容
```

## 获取帮助

```powershell
# 查看命令的语法和本地帮助
get-help 命令名称
# 联网查看命令的官方文档
get-help 命令名称 -online
```

## 清屏

```powershell
clear
```

## 启用可选功能

### WSL

> 参考资料
>
> [Running Redis on Windows 10 – Part I of III](https://redislabs.com/blog/redis-on-windows-10/)
>
> [Windows Subsystem for Linux Installation Guide for Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
>
> [Windows Insider Program](https://www.youtube.com/watch?v=oOlRvuEBG88)
>
> [Troubleshooting Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/troubleshooting)
>
> [Unable to upgrade WSL 1 - WSL 2 #5749](https://github.com/microsoft/WSL/issues/5749)

1. 启用 WSL 1，Windows Subsystem for Linux

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
   ```

   执行完该命令后重启系统后，通过 Microsoft Store 安装 Linux 发行版，然后可以使用 Linux 命令。

2. 升级 WSL 2，WSL 2 比 1 支持更全面的 Linux 命令，需要先启用 “Virtual Machine Platform”

   ```powershell
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```

   执行以下命令设置 WSL 默认版本
   
   ```powershell
   wsl --set-default-version 2
   ```
   
   如果 `wsl --set-default-version` 结果为无效命令，请输入 `wsl --help`。 如果 `--set-default-version` 未列出，则表示你的 OS 不支持它，你需要更新到版本 2004。更新完成后执行这个命令如果出现错误代码 `0x1bc`，或者启动从 Microsoft Store 安装的 Linux 发行版出现问题，访问“[更新 WSL 2 Linux 内核](https://aka.ms/wsl2kernel)”页面，点击 “[下载适用于 x64 计算机的最新 WSL2 Linux 内核](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)”下载 wsl_update_x64.msi 并安装，然后重试。

