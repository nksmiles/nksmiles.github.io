---
layout: mypost
title: Almalinux 安装 nginx stable 或者 mainline 最新版
categories: [Linux]
---

Almalinux 自带软件源中的 nginx 版本较低，不支持http2，改 nginx 官网的软件源，安装 stable 或者 mainline 的最新版。


## 安装先决条件：

```bash
yum install yum-utils
```

## 配置软件源

要设置 yum 资源库，请创建名为 `/etc/yum.repos.d/nginx.repo` 的文件，内容如下：

```
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true
```

默认情况下，使用 stable 的 nginx 软件包仓库。如果想使用 mainline 的 nginx 软件包，请运行以下命令：
```
yum-config-manager --enable nginx-mainline
```

## 安装 nginx

要安装 nginx，请运行以下命令：
```
yum install nginx
```

当提示接受 GPG 密钥时，验证指纹是否与 `573B FD6B 3D8F BC64 1079 A6AB ABF5 BD82 7BD9 BF62` 匹配，如果匹配，则接受它。


## 参考连接

[nginx: Linux packages](http://nginx.org/en/linux_packages.html#RHEL)
