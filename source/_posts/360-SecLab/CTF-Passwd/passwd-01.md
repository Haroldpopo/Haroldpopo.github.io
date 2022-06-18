---
title: CTF-Passwd-01
date: 2021-04-19 12:50:11
categories:
- [360-SecLab, 360-CTF-密码学篇]
tags: CTF
---

:::info

360CTF实训平台：http://172.18.25.55

仅限内网访问

:::

# 360-SecLab-Passwd-01

## 用脑子想想

### 题目描述

web中Brainfuck源码密文解密。

题目网址：[http://10.2.0.3/31/](http://10.2.0.3/31/)

### Write-up

1. 通过Brainfuck密文解密获取key

   打开题目网址，发现一大堆这样的符号

   ![Brainfuck_code](https://i.vgy.me/9txEsz.png)

2. 提示说Brainfuck解密，这里认得Brainfuck密码的样子，下次直接找Brainfuck在线解密网站解密

   [https://www.splitbrain.org/services/ook](https://www.splitbrain.org/services/ook)

   ![decode](https://i.vgy.me/SbPYvY.png)

   点击`Brainfuck Text`，得出flag

   ![result](https://i.vgy.me/lKOsva.png)

## 仔细找找呗。。

### 题目描述

源码获取base64加密的key解密

题目网址：[http://10.2.0.3/32/](http://10.2.0.3/32/)

### Write-up

1. 查看网页源码，发现Flag:接了一串Base64的编码

   ![found_base64](https://i.vgy.me/m3BBWi.png)

2. 复制下来去Base64在线解密网站解密，得到flag

   ![decode](https://i.vgy.me/baPSYS.png)

## rar啦

### 题目描述

工具破解rar文件

题目网址：[http://10.2.0.3/57/](http://10.2.0.3/57/)

### Write-up

1. 文件下载下来，用ARCHPR工具暴力破解密码

   密码一般是4位，这里密码范围选择所有数字和所有小写字母

   ![solve](https://i.vgy.me/zkgSqJ.png)

   

2. 等待爆破出密码

   ![result-pre](https://i.vgy.me/mJzOQ1.png)

3. 输入密码，拿到flag

   ![result](https://i.vgy.me/0nN5aq.png)