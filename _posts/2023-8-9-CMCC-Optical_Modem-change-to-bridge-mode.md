---
layout: mypost
title: 移动光猫修改桥接模式
categories: [Network]
---

需要先登录光猫，记录原有联网参数，然后修改或新建网络连接。

使用以下管理用户名和密码登录移动光猫： 

用户名: ```CMCCAdmin``` 

密码: ```aDm8H%MdA``` 


登陆后，查看“网络-宽带设置”，选择名称中含有“INTERNET”的连接名称，截图记录原有INTERNET设置：

![line](CMCC1.png)

截图记录后，删除此配置。

参考原有配置，新建一个INTERNET连接，并设为桥接模式： 

![line](CMCC2.png)

VLAN、802.1p等信息保持跟原有一致。 

剩下的就是修改路由器WAN口为PPPoE拨号。如果不知道拨号密码，需要提前找移动客服索要。。

