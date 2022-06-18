---
title: CTF-Passwd-03
date: 2021-04-21 23:05:11
categories:
- [360-SecLab, 360-CTF-密码学篇]
tags: CTF

---

:::info

360CTF实训平台：http://172.18.25.55

仅限内网访问

:::

# 360-SecLab-Passwd-03

## Brainfuck2

### 题目描述

Brainfuck编码加密解密

题目网址：[http://10.2.0.3/72/ok%EF%BC%9F.txt](http://10.2.0.3/72/ok%EF%BC%9F.txt)

### Write-up

1. 进入题目地址，显示的全是`Ook`，根据提示去Brainfuck网址解密

   *Brainfuck：*[https://www.splitbrain.org/services/ook](https://www.splitbrain.org/services/ook)

   粘贴进来，点击`Ook!_to_Text`

   ![Ook!_to_Text](https://i.vgy.me/SoIZYH.png)

2. 解码得到一个类似flag形式的编码，这里去凯斯密码解密一下，偏移量一般是13

   *凯撒密码解密：*[http://www.atoolbox.net/Tool.php?Id=778](http://www.atoolbox.net/Tool.php?Id=778)

   ![result](https://i.vgy.me/UM2fk2.png)

   解码得到flag

## Brainfuck3

### 题目描述

Brainfuck编码加密解密

题目网址：[http://10.2.0.3/73/classic.txt](http://10.2.0.3/73/classic.txt)

### Write-up

进入题目地址，显示了一堆符号，根据提示去[Brainfuck](http://www.atoolbox.net/Tool.php?Id=778)网址解密

粘贴进来，点击`Brainfuck to Text`

![Brainfuck_to_Text](https://i.vgy.me/eIKdOu.png)

得到一个JavaScript脚本语句，在网页中执行一下这个语句，输出flag

## rot13

### 题目描述

偏移量为13的凯撒密码

题目网址：[http://10.2.0.3/74/](http://10.2.0.3/74/)

### Write-up

1. 进入题目地址，得到一串编码

   ![code.txt](https://i.vgy.me/8zIKSA.png)

2. 根据提示，找个[凯撒密码解密](http://www.atoolbox.net/Tool.php?Id=778)的网址，解码得出flag

   ![result](https://i.vgy.me/PMM7oG.png)