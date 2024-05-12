---
layout: mypost
title: OpenWrt ZeroTier 设置步骤
categories: [OpenWrt]
---

请确认已经初步上手 ZeroTier，并了解使用方式。

# 创建 ZeroTier 网络

请确认已经创建 ZeroTier 网络：

![image](01.png)

并将 Access Control 设置为 Private。

# OpenWrt 中加入 ZeroTier 网络

通过 OpenWrt 页面设置界面加入 ZeroTier 网络

![image](02.png)

通过终端软件连接到 OpenWrt，查看连接状态：
```bash
root@OpenWrt:~# zerotier-cli listnetworks
200 listnetworks <nwid> <name> <mac> <status> <type> <dev> <ZT assigned ips>
200 listnetworks 3efa5cb78ae7269c  9e:b1:62:73:48:ce ACCESS_DENIED PRIVATE ztrfylxeiw -
```

# ZeroTier 授权

![image](03.png)

![image](04.png)

# OpenWrt 设置

### 查看授权后的状态

```bash
root@OpenWrt:~# zerotier-cli listnetworks
200 listnetworks <nwid> <name> <mac> <status> <type> <dev> <ZT assigned ips>
200 listnetworks 3efa5cb78ae7269c Song 9e:b1:62:73:48:ce OK PRIVATE ztrfylxeiw 192.168.192.18/24
```

### 创建新接口

![image](05.png)

![image](06.png)

### 设置防火墙规则

![image](07.png)

![image](08.png)

### 设置防火墙通信规则

![image](09.png)

![image](10.png)

![image](11.png)

### 参考连接
[https://www.youtube.com/watch?v=pa0tkch3lss
](https://www.youtube.com/watch?v=pa0tkch3lss
)
