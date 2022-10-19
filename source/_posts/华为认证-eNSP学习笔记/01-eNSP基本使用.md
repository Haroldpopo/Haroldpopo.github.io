---
title: eNSP基本命令
date: 2022-10-16 9:00:00
categories: 华为认证 eNSP学习笔记
tags: eNSP
---

# eNSP基本命令

## 进入系统视图修改设备名称

```shell eNSP command:("<Huawei>":1||"[Huawei]":3||"[SW]":4)
system-view
Enter system view, return user view with Ctrl+Z.
sysname SW
quit
```

## 查看IP表

```shell eNSP command:("[SW]":1)
display ip routing-table
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 2        Routes : 2        

Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface

      127.0.0.0/8   Direct  0    0           D   127.0.0.1       InLoopBack0
      127.0.0.1/32  Direct  0    0           D   127.0.0.1       InLoopBack0
```

## 查看vlan表

```shell eNSP command:("[SW]":1)
display vlan
The total number of vlans is : 1
--------------------------------------------------------------------------------
U: Up;         D: Down;         TG: Tagged;         UT: Untagged;
MP: Vlan-mapping;               ST: Vlan-stacking;
#: ProtocolTransparent-vlan;    *: Management-vlan;
--------------------------------------------------------------------------------

VID  Type    Ports                                                          
--------------------------------------------------------------------------------
1    common  UT:GE0/0/1(U)      GE0/0/2(U)      GE0/0/3(U)      GE0/0/4(U)      
                GE0/0/5(D)      GE0/0/6(D)      GE0/0/7(D)      GE0/0/8(D)      
                GE0/0/9(D)      GE0/0/10(D)     GE0/0/11(D)     GE0/0/12(D)     
                GE0/0/13(D)     GE0/0/14(D)     GE0/0/15(D)     GE0/0/16(D)     
                GE0/0/17(D)     GE0/0/18(D)     GE0/0/19(D)     GE0/0/20(D)     
                GE0/0/21(D)     GE0/0/22(D)     GE0/0/23(D)     GE0/0/24(U)     


VID  Status  Property      MAC-LRN Statistics Description      
--------------------------------------------------------------------------------

1    enable  default       enable  disable    VLAN 0001
```

## 保存配置

```shell eNSP command:("[SW]":1||"<SW>":2)
quit
save
The current configuration will be written to the device.
Are you sure to continue?[Y/N]y
Oct 19 2022 23:54:13-08:00 SW DS/4/DATASYNC_CFGCHANGE:OID 1.3.6.1.4.1.2011.5.25.191.3.1 configurations have been changed.
The current change number is 4, the change loop count is 0, and the maximum number of records is 4095.
Info: Please input the file name ( *.cfg, *.zip ) [vrpcfg.zip]:
Oct 19 2022 23:54:19-08:00 SW %%01CFM/4/SAVE(l)[50]:The user chose Y when deciding whether to save the configuration to the device.
Now saving the current configuration to the slot 0.
Save the configuration successfully.
```

