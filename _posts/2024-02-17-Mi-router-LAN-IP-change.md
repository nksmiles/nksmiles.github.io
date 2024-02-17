---
layout: mypost
title: 小米路由器有线中继（AP）模式下更改IP地址
categories: [Router]
---

缸更换了小米 WiFi7 的新路由器 BE3600，发现将 BE3600 设置为 AP 模式时，无法设置LAN IP，以下方法可以让你自定义LAN IP。注意只能设置IP地址，不能设置DNS和网关。

1. 将 BE3600 设置为有线中继，并且能正常上网。
 
2. 在电脑上登陆 BE3600 管理后台。
 
3. 把网址栏中的apsetting改为setting后回车。
```
http://172.28.88.202/cgi-bin/luci/;stok=3456abcdef7890/web/apsetting/wan
```
类似上面的网址改为：
```
http://172.28.88.202/cgi-bin/luci/;stok=3456abcdef7890/web/setting/wan
```

5. 点击出现的局域网设置，把局域网IP地址改成自己想设置的地址并保存。

![LAN IP](LAN_IP.png)

6. 路由器会提示重启，如果没有提示重启，需要手动重启路由器。

![LAN IP_Reboot](LAN_IP_Reboot.png)

