---
layout: post
title: "Golang使用ODBC链接MySql"
categories: golang
tags: golang
type: origin
---

> 系统环境:
>
> - Ubuntu 15.04
> - Go version 1.3.3

ODBC简介
-----------
ODBC（Open Database Connectivity，开放数据库互连）提供了一种标准的API（应用程序编程接口）方法来访问DBMS（Database Management System）。这些API利用SQL来完成其大部分任务。ODBC本身也提供了对SQL语言的支持，用户可以直接将SQL语句送给ODBC。ODBC的设计者们努力使它具有最大的独立性和开放性：与具体的编程语言无关，与具体的数据库系统无关，与具体的操作系统无关。

基本框架
--------
![go odbc](/images/go-odbc-components.png)

**go驱动**是使用go语言进行ODBC链接时必备的包<br>
**unixODBC**是ODBC驱动管理器，在linux中用go语言编写的ODBC包会尝试使用unixODBC来链接ODBC数据库。<br>
**ODBC驱动**是由数据库的厂商来提供的，负责数据库的链接。

环境配置
----
### 1. 安装mysql
`sudo apt-get install mysql-server mysql-client`

### 2. 安装unixODBC
`sudo apt-get install unixodbc`

### 3. 安装mysql odbc driver
在mysql官网下下载 [mysql-connector-odbc](https://dev.mysql.com/downloads/connector/odbc/)，解压并安装

```sh
tar zxf mysql-connctor-odbc.tar.gz
cd mysql-connector-odbc
sudo cp bin/mysql-installer /bin/
sudo cp lib/* /usr/lib/odbc/
```

### 4.配置数据源
> 配置文件说明
>
> - `/etc/odbcinst.ini` 系统驱动
> - `/etc/odbc.ini` 系统数据源
> - `$HOME/.odbcinst.ini` 用户驱动
> - `$HOME/.odbc.ini` 用户数据源

#### 方案一
编辑 `$HOME/.odbc.ini`(或`/etc/odbc.ini`) 内容如下

```ini
[testdsn]
Driver = /usr/lib/odbc/libmyodbc5a.so
Description = test dsn
Server = localhost
Port = 3306
Socket = /var/run/mysqld/mysqld.sock
Option = 3
```
#### 方案二
编辑 `$HOME/.odbcinst.ini`(或/etc/odbcinst.ini) 内容如下

```ini
[testdriver]
Description = test driver
Driver = /usr/lib/odbc/libmyodbc5a.so
Setup = /usr/lib/odbc/libmyodbc5S.so
```

编辑 `$HOME/.odbc.ini`(或/etc/odbc.ini) 内容如下

```ini
[testdsn]
Driver = testdriver
Description = test dsn
Server = localhost
Port = 3306
Socket = /var/run/mysqld/mysqld.sock
Option = 3
```

### 5.查看配置的驱动和数据源
可以用`myodbc-install`来查看驱动和数据源<br>

```sh
#查看数据源
myodbc-installer -d -l -c0

#查看驱动
myodbc-installer -s -l -c0
```

### 6.检测链接
```sh
isql -v user password
```
如果出现以下信息则说明链接成功

```
+---------------------------------------+
| Connected!                            |
|                                       |
| sql-statement                         |
| help [tablename]                      |
| quit                                  |
|                                       |
+---------------------------------------+
SQL>
```

go示例代码
----------
> 代码依赖谷歌的odbc包
>
> `go get code.google.com/p/odbc`

```go
package main

import (
	"database/sql"
	"fmt"

	_ "code.google.com/p/odbc"
)

func main() {
	db, err := sql.Open("odbc", "DSN=testdsn;UID=user;PWD=password;DATABASE=mysql")
	if nil != err {
		fmt.Println(err)
		return

	}
	rows, err := db.Query("select user,password,host from user")
	if nil != err {
		fmt.Println(err)
		return
	}
}
```
