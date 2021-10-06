> 参考资料
>
> [nodeJS安装和环境变量的配置](https://www.cnblogs.com/kingchan/p/13195461.html)
>
> [nodejs安装及环境变量配置](https://zhuanlan.zhihu.com/p/86241466)

# 修改npm默认路径

npm全局模块默认安装路径是 `C:\Users\用户名\AppData\Roaming\npm`

npm默认缓存路径也在 `C:\Users\用户名\AppData\Roaming\npm-cache`

`npm config list` 查看关键配置，`npm config ls -l` 查看所有配置

设置 npm config 和 环境变量，把这个两个路径改到指定目录`F:\working\nodejs`

1. 执行以下命令

   ```
   npm config set prefix "F:\working\nodejs\node_global"
   npm config set cache "F:\working\nodejs\node_cache"
   ```

2. 环境变量

   系统环境变量中新建 变量名：`NODE_PATH`,变量值：`F:\working\nodejs\node_global\node_modules`

   用户环境变量的`path` 中， 将默认的 `C` 盘下 `APPData\Roaming\npm` 修改为 `F:\working\nodejs\node_global`
   
   系统环境变量的 `path` 中添加 `F:\working\nodejs\node_global`

# 安装淘宝镜像

npm默认的仓库地址是在国外网站，速度较慢，建议大家设置到淘宝镜像。

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

安装成功后可使用 `cnpm install` 安装组件

