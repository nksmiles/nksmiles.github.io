---
layout: mypost
title: AlmaLinux 9 Firewalld 设置笔记
categories: [Linux]
---

## 介绍

Firewalld 最大的好处有两个：支持动态更新，不用重启服务；第二个就是加入了防火墙的 “zone” 概念。 
首先，将所有网络流量分为多个区域（zone），然后，根据数据包的源 IP 地址或传入的网络接口等条件将流量传入相应区域，
同时，每个区域都定义了自己打开或者关闭的端口和服务列表。


## 常用设置命令

使用以下命令开放端口范围：
```bash
firewall-cmd --zone=public --add-port=443/tcp --permanent
firewall-cmd --zone=public --add-port=4440-4442/tcp --permanent
# --permanent 永久生效，没有此参数重启后失效
```

使用以下命令重新加载配置：
```bash
firewall-cmd --reload
```

查看当前为哪些服务开放了哪些端口。

其实一个服务对应一个端口，每个服务对应 /usr/lib/firewalld/services 下面一个 xml 文件。
```
firewall-cmd --list-services
```

查看还有哪些服务可以打开
```
firewall-cmd --get-services
```

添加服务到firewalld以便开放对应的端口，如果修改了默认端口，则要提前编辑/usr/lib/firewalld/services 下面对应的 xml 文件。
```
firewall-cmd --add-service http
```

查看所有打开的端口： 
```
firewall-cmd --zone=public --list-ports
```

也可以通过配置文件 `/etc/firewalld/zones/public.xml` 查看端口设置。
对应的 Service 配置就是 /usr/lib/firewalld/services 目录下对应的 xml 文件。

```xml
<?xml version="1.0" encoding="utf-8"?>
<zone>
  <short>Public</short>
  <description>For use in public areas. You do not trust the other computers on networks to not harm your computer. Only selected incoming connections are accepted.</description>
  <service name="ssh"/>
  <service name="dhcpv6-client"/>
  <service name="cockpit"/>
  <service name="http"/>
  <service name="https"/>
  <port port="8443" protocol="tcp"/>
  <forward/>
</zone>
```

## 参考链接

[centos almalinux rocketLinux firewall-cmd 开放多个连续端口](https://blog.csdn.net/WonSafe/article/details/131536185)

[CentOS7 Firewall 常用命令汇总，开放端口及查看已开放的端口](https://blog.csdn.net/lvqingyao520/article/details/81075094)
