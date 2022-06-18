---
title: sqlmap工具使用
categories: 
- [Kali从入门到入狱, 渗透工具使用]
tags: Kali
---

# SQLMAP

## SQLMAP简介

sqlmap是一款用来检测与利用SQL注入漏洞的免费开源工具，有一个非常棒的特性，即对检测与利用的自动化处理(数据库指纹、访间底层文件系统、执行命令）。

sqlmap是一个开放源码的渗透测试工具，它可以自动探测和利用SQL注入漏洞来接管数据库服务器。它配备了一个强大的探测引擎，为最终渗透测试人员提供很多的功能，可以拖库，可以访问底层的文件系统，还可以通过带外连接执行操作系统上的命令。

sqlmap支持五种SQL注入技术：

1. 基于布尔值得注入，基于时间的盲注，基于错误的盲注，UNION查询和stacked查询；

2. 检索DBMS的会话用户和数据库；

3. 枚举用户、哈希口令、权限和数据库、表、列；

4. 自动识别后台账户的哈希口令，并通过字典进行暴力破解；

5. 当数据库是MY-SQL、Postgre-SQL和Microsoft SQL Server时，支持上传和下载数据库文件。


## SQLMAP的基础命令

```bash
// 列举数据库
sqlmap -u “注入地址” -v 1 –-dbs

// 当前数据库
sqlmap -u “注入地址” -v 1 –-current-db

// 列数据库用户
sqlmap -u “注入地址” -v 1 –-users

// 当前用户
sqlmap -u “注入地址” -v 1 –-current-user

// 列举数据库的表名
sqlmap -u “注入地址” -v 1 -D “数据库” –-tables 

// 获取表的列名
sqlmap.py -u “注入地址” -v 1 -T “表名” -D “数据库” –-columns

// 获取表中的数据
sqlmap.py -u “注入地址” -v 1 -T “表名” -D “数据库” -C “字段” –-dump
```

其他sqlmap的高级命令，可参考相关资料进行更深入学习。
