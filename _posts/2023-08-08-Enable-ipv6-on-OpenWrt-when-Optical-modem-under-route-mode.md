---
layout: mypost
title:  光猫路由模式下（未开启桥接），OpenWrt启用ipv6的设置
categories: [OpenWrt,Network]
---

以下设置在 Lean OpenWrt 固件，移动、电信、联通宽带均测试成功。 

最新版的官方 OpenWrt，可以直接在设置中将LAN和WAN6均设置ipv6为中继模式。但 Lean 的 OpenWrt中，wan6的 Luci 界面无此设置选项。设置说明如下： 
1. 确保编译时已经添加ipv6helper。
2. 需要终端登录 OpenWrt，修改`/etc/config/dhcp`配置文件：
```bash
vi /etc/config/dhcp
```
修改以下配置：
```bash
 	config dhcp 'lan'
	        #……
	        #其他配置参数保持不变
	        option ra 'relay'
	        option dhcpv6 'relay'
	        option ndp 'relay'
	
	config dhcp 'wan6'
	        option interface 'wan6'
	        option dhcpv6 'relay'
	        option ra 'relay'
	        option ndp 'relay'
	        option master '1'
```

若原配置文件中无`config dhcp 'wan6'`部分，直接手动添加。

配置文件修改完成后重启dnsmasq服务：
```bash
/etc/init.d/dnsmasq restart
```
或者手动在网页中DHCP/DNS页面点“保存&应用”，也会自动重启dnsmasq服务。

