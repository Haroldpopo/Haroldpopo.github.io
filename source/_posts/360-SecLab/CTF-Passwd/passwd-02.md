---
title: CTF-Passwd-02
date: 2021-04-20 13:39:11
categories:
- [360-SecLab, 360-CTF-密码学篇]
tags: CTF
---

:::info

360CTF实训平台：http://172.18.25.55

仅限内网访问

:::

# 360-SecLab-Passwd-02

## base

### 题目描述

隐藏的摩斯密码信息

题目网址：[http://10.2.0.3/59/](http://10.2.0.3/59/)

### Write-up



## 爆吧

### 题目描述

爆破，base64 base32解码

题目网址：[http://10.2.0.3/69/](http://10.2.0.3/69/)

### Write-up

1. 进入题目网址，把文件下载下来，是一个rar压缩包，需要密码解压，提示说有爆破，先用RAR压缩包密码破解工具爆破试试。

   后面还需要解码，先试试最简单的4位纯数字密码爆破

   ![solve](https://i.vgy.me/rtNNYc.png)

2. 密码较简单，就是4位纯数字的密码，这里解压出来一个flag.txt文件，显示的是Base64编码

   ![flag.txt](https://i.vgy.me/473mPs.png)

3. 找个Base64解码网站解码一下

   *Base解码：*[https://www.qtool.net/baseencode](https://www.qtool.net/baseencode)

   ![base_decode](https://i.vgy.me/8A72Vc.png)

4. Base64解码后是Base32解码，换成base32解码，得到的便是flag

   ![result](https://i.vgy.me/qgbmsQ.png)

## 很多

### 题目描述

通过web获取的信息经过很多次编码，rot13，rabbit，16进制解密后找到其中的key或者flag

题目网址：[http://10.2.0.3/71/](http://10.2.0.3/71/)

### Write-up

1. 点击 <u>请下载此文件</u> 是一个文本文件

   ![txt](https://i.vgy.me/OinV0o.png)

2. 先进行rot13解码

   *rot13：*[https://www.qqxiuzi.cn/bianma/ROT5-13-18-47.php](https://www.qqxiuzi.cn/bianma/ROT5-13-18-47.php)

   ![rot13_decode](https://i.vgy.me/hcwzxi.png)

3. 解码之后还是密文，然后用rabbit解码

   *Rabbit：*[https://www.sojson.com/encrypt_rabbit.html](https://www.sojson.com/encrypt_rabbit.html)

   ![decode_error](https://i.vgy.me/va5g90.png)

   尝试了多个rabbit密码解密网址，都解密失败，wdf？？？

4. 试试不进行rot13解码，直接用rabbit解码，得到十六进制编码

   ![get_code](https://i.vgy.me/zfaW59.png)

5. 最后十六进制转字符串，得出flag，rot13解密就是个陷阱，根本用不到

   ![result](https://i.vgy.me/G9jw2X.png)

*<u>Tip：这里的flag只取值，且得到的flag里的 “O”可能是“0”（后台flag打错字）</u>*
