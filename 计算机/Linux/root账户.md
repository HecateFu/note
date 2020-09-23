# 设置root账户密码

安装完Ubuntu后默认是没有主动设置root密码的，也就无法进入根用户。

执行以下操作：

1. 在终端输入命令 `sudo passwd`，输入当前用户的密码然后回车；
2. 提示输入新密码，输入完成后回车；
3. 提示再输入一次新密码以确认，然后回车，设置成功。

```bash
hecate@FU-PC:~$ sudo passwd
[sudo] password for hecate:
New password:
Retype new password:
passwd: password updated successfully
```

注意：这个新密码就是root的密码，可以与当前用户的密码不同。

# 切换root账户

在终端中输入 `su root` 或 `su` 或 `sudo -i`，然后输入root的密码，验证成功即可切换到root用户。在root用户下做完操作后，用exit命令即可退出root用户，退回当前登陆用户。

# 禁用root账户

`passwd` 命令：设置账户密码、锁定、解锁账户、设置密码过期时间

锁定账户

```bash
$ sudo passwd -l root
```

解锁账户

```bash
$ sudo passwd -u root
```

