# 推荐安装方式

> [Install WSL](https://docs.microsoft.com/en-us/windows/wsl/install)
>
> [Windows Insider Program](https://www.youtube.com/watch?v=oOlRvuEBG88)

系统版本要求： Windows 10 version 2004 and higher (Build 19041 and higher) or Windows 11

powershell 执行以下命令

```powershell
wsl --install
```

该命令会启用必须的的Windows功能（适用于 Linux 的 Windows 子系统 Windows Subsystem for Linux (WSL) 、虚拟机平台 Virtual Machine Platform  ），下载最新的linux内核，WSL 2 设为默认 WSL版本，安装默认linux发行版（默认 Ubuntu）。



```powershell
wsl -l -o
```

列出所有可安装的发行版



```powershell
wsl --install -d <发行版名称>
```

安装指定发行版，wsl 启用后，再直接执行 `wsl --install` 时，出现无法直接安装默认版本的情况，此时加上 `-d`  选项指定发行版名称即可安装。

# 老版本安装方式

> [Running Redis on Windows 10 – Part I of III](https://redislabs.com/blog/redis-on-windows-10/)
>
> [Manual installation steps for older versions of WSL](https://docs.microsoft.com/en-us/windows/wsl/install-manual)
>
> [Troubleshooting Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/troubleshooting)
>
> [Unable to upgrade WSL 1 - WSL 2 #5749](https://github.com/microsoft/WSL/issues/5749)

在 powershell 中执行以下命令启用 WSL

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

# wsl访问win10

> [wsl 访问 win10 文件](https://www.cnblogs.com/azureology/p/12889659.html)

比较简单，wsl已经默认将所有分区挂载在`/mnt`下，例如

```
/mnt/c/Users/xxx
/mnt/d/data
```