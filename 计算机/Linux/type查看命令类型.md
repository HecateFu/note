# type

> 参考资料
>
> [type命令及Linux命令类型](https://blog.csdn.net/liuyuan185442111/article/details/51530956)

## 作用

显示命令的类型信息。如果是在系统hash表中缓存的命令，会显示hash表中缓存内容。如果是 `alias `会显示别名所代表的具体命令，如果是外部命令会显示该命令的执行文件。如果是内建命令、函数、关键字会显示他们的类型。

当我们键入某个命令时, shell会按照 `alias->keyword->function->builtin->$PATH` 的顺序进行搜索， 本着“先到先得”的原则，就是说如果有如名为mycmd的命令同时存在于alias和function中的话，那么定会使用alias的mycmd命令。但是hash比它们的优先级都高。

## 概念

### 内建命令

shell内建命令是指bash（或其它版本）工具集中的命令。一般都会有一个与之同名的系统命令，比如bash中的echo命令与/bin/echo是两个不同的命令，尽管他们行为大体相仿。内建命令比系统论命令有比较高的执行效率，外部命令执行时往往需要fork一个子进程，而内建命令一般不用。

使用 `help` 命令可查看bash内建命令

### hash

linux系统下会有一个hash表，当你刚开机时这个hash表为空，每当你执行过一条命令时，hash表会记录下这条命令的路径，就相当于缓存一样。第一次执行命令shell解释器默认的会从PATH路径下寻找该命令的路径，当你第二次使用该命令时，shell解释器首先会查看hash表，没有该命令才会去PATH路径下寻找。输入 `hash` 可以查看hash表的内容，`hash -p a b` 添加一项a改名为b，执行b时实际会执行a命令。

## 用法

```
type [-afptP] 命令名称 [命令名称 ...]
```

## 选项说明

| 选项 | 作用 |
| --- | ---- |
| -a | 输出一个命令所有可能的类型，很多内建命令既是内建命令又有同名的外部命令，如echo、kill、cd ... |
| -f | 阻止函数搜索（不清楚具体什么效果） |
| -p | 如果是类型是file，输出它的执行文件路径，如果不是file，无输出，对于kill、cd这类的有两个类型的并不会输出路径，对于别名类型也不会输出路径 |
| -t | 输出命令类型，`alias` 别名，`keyword` shell保留字，`function` shell方法，`builtin` shell内部命令，`file` 外部命令，无输出表示命令不存在 |
| -P | 强制搜索命令的执行文件路径，两种类型的kill、cd，alias类型的ls都可以输出文件路径 |

## 举例

1. 别名

  ```shell
  type ls
  ```
  输出：ls is aliased to \`ls --color=tty'

  ```shell
  type -t ls
  # 输出
  alias

  type -p ls
  #无结果

  type -P ls
  # 输出
  /usr/bin/ls
  ```

2. 内建命令

  ```shell
  type -a cd
  # 输出
  cd is a shell builtin
  cd is /usr/bin/cd

  type -p cd
  # 无结果

  type -P cd
  #输出
  /usr/bin/cd

  type -a help
  #输出
  help is a shell builtin

  type -p help
  # 无结果

  type -P help
  # 无结果
  ```
3. 外部命令

  ```shell
  # 当前会话中未执行过
  type java
  # 输出
  java is /usr/bin/java

  type -t java
  # 输出
  file

  # 当前会话中已执行过
  type java
  java is hashed (/usr/bin/java)

  type -p java
  # 输出
  /usr/bin/java
  ```

4. 关键字
  ```shell
  type if
  # 输出
  if is a shell keyword

  type -at if
  # 输出
  keyword
  ```
