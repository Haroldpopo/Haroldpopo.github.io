---
title: 静态路由
date: 2022-10-19 23:00:00
categories: 华为认证 eNSP学习笔记
tags: eNSP
---

# 静态路由

![Case Routing Static](https://i.vgy.me/2Lvn8J.png)

## 路由`AR1`配置

```shell eNSP command:("<Huawei>":1||"[Huawei]":3||"[AR1]":4,13-14||"[AR1-GigabitEthernet0/0/0]":5,8,12||"[AR1-GigabitEthernet0/0/1]":9)
system-view 
Enter system view, return user view with Ctrl+Z.
sysname AR1
interface g0/0/0
ip address 192.168.1.10 255.255.255.0
Oct 19 2022 22:13:06-08:00 AR1 %%01IFNET/4/LINK_STATE(l)[0]:The line protocol IP
 on the interface GigabitEthernet0/0/0 has entered the UP state. 
interface g0/0/1
ip address 192.168.2.1 255.255.255.0
Oct 19 2022 22:13:27-08:00 AR1 %%01IFNET/4/LINK_STATE(l)[1]:The line protocol IP
 on the interface GigabitEthernet0/0/1 has entered the UP state. 
quit
ip route-static 192.168.3.0 255.255.255.0 192.168.2.10
display ip routing-table 192.168.3.10
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Table : Public
Summary Count : 1
Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

    192.168.3.0/24  Static  60   0          RD   192.168.2.10    GigabitEthernet0/0/1
```

## 路由`AR2`配置

```shell eNSP command:("<Huawei>":1||"[Huawei]":3||"[AR2]":4,13-14||"[AR2-GigabitEthernet0/0/0]":5,8,12||"[AR2-GigabitEthernet0/0/1]":9)
system-view 
Enter system view, return user view with Ctrl+Z.
sysname AR2
interface g0/0/0
ip address 192.168.2.10 255.255.255.0
Oct 19 2022 22:16:37-08:00 AR2 %%01IFNET/4/LINK_STATE(l)[0]:The line protocol IP
 on the interface GigabitEthernet0/0/0 has entered the UP state. 
interface g0/0/1
ip address 192.168.3.1 255.255.255.0
Oct 19 2022 22:16:51-08:00 AR2 %%01IFNET/4/LINK_STATE(l)[1]:The line protocol IP
 on the interface GigabitEthernet0/0/1 has entered the UP state. 
quit
ip route-static 192.168.1.0 255.255.255.0 192.168.2.1
display ip routing-table 192.168.1.1
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Table : Public
Summary Count : 1
Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

    192.168.1.0/24  Static  60   0          RD   192.168.2.1     GigabitEthernet0/0/0
```

## 静态路由关键命令

`ip route-static 目标网段 目标网段子网掩码 下一跳地址`
