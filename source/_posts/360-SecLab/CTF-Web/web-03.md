---
title: CTF-Web-03
date: 2021-04-15 16:28:53
categories:
- [360-SecLab, 360-CTF-Web篇]
tags: CTF
---
:::info

360CTF实训平台：http://172.18.25.55

仅限内网访问

:::

# 360-SecLab-Web-03

## 捡到银行卡

### 题目描述

使用burp爆破密码

题目网址：[http://10.2.0.3/22/](http://10.2.0.3/22/)

### Write-up

密码是银行卡密码，那么密码就是6位纯数字的密码，我们用Burpsuite爆破一下

1. 先设置代理为`127.0.0.1:8080`，这里以Firefox浏览器为例

   ![127.0.0.1:8080](https://i.vgy.me/u8Z4xx.png)

2. 启动Burpsuite，在网页先随便输入一个密码，点确定提交，Burpsuite抓到数据，在`Proxy`下显示抓包信息

   ![test](https://i.vgy.me/DzbYmd.png)

3. 可以看到输入的密码为6个1，点击`Action`-->`Send Intruder`

   ![Send Intruder](https://i.vgy.me/EZH6Me.png)

4. `Intruder`下的`Target`是攻击目标，`Positions`是攻击方式，这里默认就好，选中`Submit=`后一对`§`括起来的字符串，点击右侧的`Clear§`按钮

   ![Intruder_Positions](https://i.vgy.me/fWaZy6.png)

5. 只留下一个`password=`后面带有`§`标记的，这个标记括起来的当作变量一会进行爆破

   ![Clear§](https://i.vgy.me/5YbdRM.png)

6. 切换到`Payloads`，修改密码参数，密码6位建议从中间500000开始

   ![set_payloads](https://i.vgy.me/mNnOVq.png)

7. 切换到`Options`，可以修改线程数，默认是5线程，我这里设置20线程，线程设置当然不是说越多越好，经过测试我的电脑20线程是最佳，然后点击`Start attack`开始爆破

   ![payload_threads](https://i.vgy.me/tr1oXL.png)

8. 看返回的`Length`不一样的，最大的那个就是密码

   ![passwd_result](https://i.vgy.me/nOvk0K.png)

9. 关闭Burpsuite或者代理，输入爆破出来的密码，得到flag

![result](https://i.vgy.me/KAMDcY.png)

## 找找看看

### 题目描述

看url，先下载图片winhex打开url解密得到,在源码中还有url解密,可以看到是百度网盘的分享码,得到flag

题目网址：[http://10.2.0.3/23/](http://10.2.0.3/23/)

### Write-up

1. 根据提示先下载图片用winhex打开，寻找类似url编码的东西，拉到最底

   ![found_urlencode](https://i.vgy.me/SkBbuy.png)

2. 复制下来去url解码，得到一个百度网盘链接，再看提示，源码里也有url解密，得到网盘分享密码

   ![passwd_urldecode](https://i.vgy.me/I7vvT5.png)

3. 输入密码打开网盘分享链接，得到flag文件，下载下来即拿到flag

   ![result](https://i.vgy.me/Nzna45.png)

## 只能上传图片

### 题目描述

php进行上传限制，绕过黑名单

题目网址：[http://10.2.0.3/24/](http://10.2.0.3/24/)

### Write-up

1. 先随便上传一个php文件试试

   ![test](https://i.vgy.me/HiGmUl.png)

2. 弹出提示，显示的文件后缀的文件都不能上传

   ![warn_tip](https://i.vgy.me/tV7aUI.png)

3. 把PHP文件的后缀改成`.pHp`，再上传试试？

   ![result](https://i.vgy.me/T0qYNu.png)

   黑名单限制文件后缀的大小写不敏感，成功拿到flag