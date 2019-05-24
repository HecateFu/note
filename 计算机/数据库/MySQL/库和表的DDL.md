# 库操作

## 创建库

```mysql
CREATE SCHEMA 库名 DEFAULT CHARACTER SET 字符集 ;
```

或

```mysql
create database 库名;
```

## 修改库字符集

```mysql
ALTER DATABASE 库名 CHARACTER SET 字符集;
```

## 删除库

```mysql
drop database 库名;
```

# 表操作

## 创建表

### 创建新表

```mysql
create table `表名` (
	`主键名`int AUTO_INCREMENT PRIMARY KEY,
	`字段1` 类型 not null,
    `字段2` 类型 default 默认值,
    ……
	`字段n` 类型 COMMENT='字段注释'
)ENGINE=`存储引擎类型` auto_increment=指定自增起始值 DEFAULT CHARSET=`字符集` COMMENT='表注释';
```

[MySQL常见建表选项及约束](https://www.cnblogs.com/geaozhang/p/6786105.html)

`AUTO_INCREMENT`在字段定义中指定该字段为自增字段，在表定义中定义自增起始值

`comment`定义注释，可用于字段或表

`default`定义字段默认值，`default CURRENT_TIMESTAMP `设置默认值为字段创建时间，`DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP`设置默认值为字段修改时间；**5.6之前，使用 timestamp 类型，且默认值设为 now() 或 current_timestamp()，而且只能有一个使用默认值的 timestamp 字段**(参见[MySQL 日期类型及默认设置](https://blog.csdn.net/gxy_2016/article/details/53436865))，两个 用默认值的timestamp的字段会报以下错误 Error Code: 1293. Incorrect table definition; there can be only one TIMESTAMP column with CURRENT_TIMESTAMP in DEFAULT or ON UPDATE clause，如果想要两个时间字段有默认值，只能用触发器（参见[mysql  1293错误 建表两个timestamp](https://blog.csdn.net/cktmyh/article/details/49886495)）

`engine`默认引擎类型innodb

`PRIMARY KEY` 指定主键，也可以定义字段的时候不写，在字段定义好后使用`PRIMARY KEY(字段名)`或`constrain 主键名 primary key(字段名)`指定，括号中可指定多个字段来创建联合主键，主键名默认都是primary，constrain后面指定的主键名并不会生效

`not null`指定字段值非空

`unique`唯一约束，字段值不能重复，**可以为多个null**，也可在字段定义好后使用`unqiue(字段名)`或`constraint 约束名 unique(字段名)`指定，括号中可指定多个字段来创建组合唯一约束，不使用constraint 指定名称时默认使用字段名作为约束名

`foreign key (本表列名)  references  主表名 (列名)`指定外键，或`constraint 约束名 foreign key(本表列名) references 主表名 (列名)`，其后可跟`ON DELETE CASCADE`设置级联删除，或`ON DELETE SET NULL`当删除父表中的行时，如果子表中有依赖于被删除父行的子行存在，将子行的外键列设置为null

MySQL中check约束没有效果，可以使用**ENUM（enumeration，枚举）**和**SET（集合）**类型实现CHECK约束，使用ENUM`字段名 enum('可选值1','可选值2'...)`，只能选一个值，使用SET`字段名 set('可选值1','可选值2'...)`，可以选多个值

### 利用旧表创建新表

```mysql
create table 新表名 select * from 旧表名;
```

## 修改表

### 修改表字符集

```mysql
ALTER TABLE 表名 CONVERT TO CHARACTER SET 字符集;
```

### 增加字段

```mysql
alter table 表名 add column 字段名 字段类型 其他字段选项；
```

### 修改字段

```mysql
alter table 表名 MODIFY column 字段名 字段类型 其他字段选项 after 列名;
```

或

```mysql
alter table 表名 change 旧字段名 新字段名 字段类型 其他字段选项 after 列名;
```

change可以修改字段名，modify不行,after表示将新增字段放在哪个字段后面

### 删除字段

```mysql
alter table 表名 drop column 列名;
```

### 修改表名

```mysql
ALTER  TABLE 旧表名 RENAME TO 新表名;
```

或

```mysql
RENAME TABLE 旧表名 TO 新表名;
```

## 删除表

```mysql
drop table 表名;
```

直接删除表，不验证是否存在，如果表不存在会报错

或

```mysql
drop table if exists 表名;
```

如果表存在删除表