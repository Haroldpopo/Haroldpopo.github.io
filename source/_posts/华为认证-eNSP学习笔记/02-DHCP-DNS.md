---
title: DHCP和DNS的配置
date: 2022-10-17 10:00:00
categories: 华为认证 eNSP学习笔记
tags: eNSP
---

# DHCP和DNS的配置

## 基本步骤

1. 启动DHCP服务

   `dhcp enable`

2. 进入接口选择使用DHCP服务

   `dhcp select interface`

3. 添加DNS服务的IP地址

   `dhcp server dns-list DNS服务的IP`

## 配置DHCP

```shell eNSP command:("<Huawei>":1||"[Huawei]":3||"[Router]":4,8,10||"[Router-GigabitEthernet0/0/0]":5,7,11-12)
system-view 
Enter system view, return user view with Ctrl+Z.
sysname Router
interface GigabitEthernet 0/0/0
ip address 192.168.1.1 255.255.255.0
Oct 18 2022 23:29:30-08:00 Router %%01IFNET/4/LINK_STATE(l)[0]:The line protocol IP on the interface GigabitEthernet0/0/0 has entered the UP state. 
quit
dhcp enable
Info: The operation may take a few seconds. Please wait for a moment.done.
interface g0/0/0
dhcp select interface
quit
```

## 配置DNS

```shell eNSP command:("[Router]":1||"[Router-GigabitEthernet0/0/0]":2-3)
interface g0/0/0
dhcp server dns-list 192.168.1.100
quit
```
