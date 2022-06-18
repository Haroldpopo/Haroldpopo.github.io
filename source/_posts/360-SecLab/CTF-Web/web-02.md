---
title: CTF-Web-02
date: 2021-04-13 21:45:54
categories:
- [360-SecLab, 360-CTF-Web篇]
tags: CTF
---

:::info

360CTF实训平台：http://172.18.25.55

仅限内网访问

:::

# 360-SecLab-Web-02

## 文件上传

### 题目描述

web代码修改上传验证

题目网址：http://10.2.0.18/17/

### Write-up

提示是web代码修改上传验证，题目应该是浏览器端的上传检测漏洞，先右键查看源码

1. 发现提交表单数据时有一个检测函数

   ```javascript
   <script>
           function check(){
   switch(document.getElementById("file1").value.substring(document.getElementById("file1").value.lastIndexOf('.')+1,document.getElementById("file1").length)){case"jpg":case"gif":return true;break;default:alert("文件格式不支持！");return false}
           }
   </script>
   ```

   上传检测方案是在客户端使用js对不合法图片进行检查，很容易在前端对js脚本操作使其无效从而绕过

2. 此为浏览器端的上传检测漏洞，在上传前把过滤函数`check()`删除即可实现检测绕过，直接把`return check()`删除就行

   ![code](https://i.vgy.me/UT4Em3.png)

3. 然后随便上传一个php文件，上传成功即出现key

   ![result](https://i.vgy.me/t9wvNz.png)

## CSS OR XSS

### 题目描述

源代码查看xss文件内容解析

题目网址：http://10.2.0.3/19/

### Write-up

题目提示是查看xss文件，又有css，那就找一下css样式表看看

![css](https://i.vgy.me/m6aSQc.png)

## 你抓住了吗？

### 题目描述

浏览器默认开启JavaScript挡住了flag

题目网址：http://10.2.0.3/21

### Write-up

根据提示关闭浏览器默认开启的JavaScript即可，但是好像又行不通了？？？

- [ ] 暂未复现