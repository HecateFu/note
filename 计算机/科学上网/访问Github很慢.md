# 如何解决国内Github访问过慢的问题

> 参考资料 [国内加速访问Github的办法，超级简单](https://zhuanlan.zhihu.com/p/65154116)

## 问题描述

国内直接访问 Github 网站，页面加载速度非常慢，`raw.githubusercontent.com` 域名无法访问，网站文档中的图片资源经常加载失败。

## 解决思路

给 Github 网站用到的主要域名配置一下本地DNS。

不过该方法存在一个问题：域名对应的IP是否会经常变化？如果经常变化就需要经常修改配置。

## 具体操作

### Windows系统中具体配置方法

1. 找到hosts文件，Win10中位置 `C:\Windows\System32\drivers\etc` 目录下。

2. 找到 `github.com`、`assets-cdn.github.com`、`github.global.ssl.fastly.net`、`raw.githubusercontent.com` 这几个域名的IP，可以到 `ipaddress.com` 网站查找域名IP。

3. 将找到的IP添加到host文件中并保存，eg：

   ```
   140.82.112.3    github.com
   140.82.114.6    api.github.com
   185.199.108.153 assets-cdn.github.com
   185.199.110.153 assets-cdn.github.com
   185.199.111.153 assets-cdn.github.com
   199.232.69.194  github.global.ssl.fastly.net
   199.232.68.133  raw.githubusercontent.com
   199.232.68.133  avatars3.githubusercontent.com
   ```

4. 使用CMD刷新本地DNS缓存

   ```
   ipconfig /flushdns
   ```

完成以上4步后重新访问 GitHub 即可发现页面访问速度快了很多，而且文档中的图片可以正常显示。
