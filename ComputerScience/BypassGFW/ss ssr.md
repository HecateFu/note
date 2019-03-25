# SS
Windows客户端地址  
<https://github.com/shadowsocks/shadowsocks-windows>

Android客户端地址  
<https://github.com/shadowsocks/shadowsocks-android>

## Windows客户端教程

### 服务器配置
根据服务器的信息，在客户端配置好以下几项：  
![服务器配置](images/ss_server_config.png)  

### PAC配置
![PAC规则配置](images/pac_rule_config.png)  

- "编辑本地 PAC 文件..." ，打开本地的 pac.txt 文件所在目录（一般会和Shadowsock.exe在同一路径下），第一次会自动创建一份。rules中定义了所有的访问规则。  
![PAC.txt](images/pac_text.png)

- “从 GFWList 更新本地 PAC”，会用远程规则更新本地的gfwlist.js（在 ss_win_temp 目录下）和 pac.txt，如果之前修改过pac.txt，会覆盖之前修改。

- "编辑 GFWList 的用户规则"，打开本地的 user-rule.txt 文件所在目录（一般会和Shadowsock.exe在同一路径下），第一次会自动创建一份。

- 最终的pac 文件是根据 gfwlist.js 和 user-rule.txt 两个文件共同生成的，如果用户想要添加某些网站进入 PAC，最好的方式是写入 user-rule.txt 这个文件，而不是修改 gfwlist.js 或 pac.txt 这个文件，因为gfwlist.js这个文件会时不时的和 github 上做同步，可能会造成已有的修改会被覆盖掉。

- user-rule文件的语法规则  
详见 adblock 过滤规则 <https://adblockplus.org/en/filter-cheatsheet>
自定义代理规则的设置语法与pac.txt rules 中相同，但是 user-rule.txt 中每一行表示一个URL通配符。

1. 通配符支持，如 \*.example.com/\* 实际书写时可省略\* 如.example.com/ 意即*.example.com/*
2. 正则表达式支持，以\开始和结束， 如 \[\w]+:\/\/example.com\
3. 例外规则 @@，如 @@*.example.com/* 满足@@后规则的地址不使用代理
4. 匹配地址开始和结尾 |，如 |http://example.com、example.com|分别表示以http://example.com开始和以example.com结束的地址
5. || 标记，如 ||example.com 则http://example.com、https://example.com、ftp://example.com等地址均满足条件
6. 注释 ! 如 ! Comment
user-rule.txt中的规则并不能直接被shadowsocks使用，如要添加到user-rule.txt中的规则生效，你还要执行下面重要的一步：更新本地的PAC，更新后user-rule.txt中的自定义规则会添加到PAC.txt文件内。（备注：每次编辑完user-rule.txt后，均需执行“从GFWList更新本地PAC”，使本次规则也生效。）


# SSR
Windows客户端地址  
<https://github.com/HMBSbige/ShadowsocksR-Windows>

Android客户端地址  
<https://github.com/doio/Akkariiin-shadowsocksr-android>


