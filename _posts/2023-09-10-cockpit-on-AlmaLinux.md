---
layout: mypost
title: AlmaLinux 9 安装使用 Cockpit
categories: [Linux]
---

Cockpit 是一款 Linux 支持的软件应用，可以让用户通过基于 web 的界面远程监控和管理 Linux 服务器的状态。

# 安装

要在 AlmaLinux 上安装和使用 Cockpit，我们可以参考以下步骤：

- 首先，确保我们的 AlmaLinux 系统是最新的，运行以下命令来更新所有的软件包：
```
dnf update
```
- 然后，安装并启用 EPEL 存储库，这样我们就可以安装 RHEL 支持的额外软件包：
```
dnf install epel-release
```
- 接下来，从 AlmaLinux 的 extras 存储库中安装 Cockpit 软件包：
```
dnf install cockpit
```
- 安装完成后，使用以下命令启动 Cockpit 服务：
```
systemctl start cockpit
```
- 为了让 Cockpit 服务在系统重启后仍然运行，使用以下命令使其自动启动：
```
systemctl enable --now cockpit.socket
```
- 然后，检查 Cockpit 服务的状态，确保它已经正常运行¹²³：
```
systemctl status cockpit.service
```
- 此外，还需要在防火墙中开放 Cockpit 的默认 HTTP 端口 9090¹²³：
```sh
firewall-cmd --permanent --zone=public --add-service=cockpit
firewall-cmd --reload
```
- 最后，可以通过浏览器访问 Cockpit 的 web 界面，输入我们的服务器的 IP 地址和端口号。例如：
[http://192.168.138.197:9090](http://192.168.138.197:9090)
- 在 web 界面上，我们需要输入我们的 AlmaLinux 用户名和密码来登录。登录后，我们就可以看到我们的服务器的各种信息和统计数据，并且可以执行一些管理操作。

# 可登录用户

不允许登录的用户，可以在 `/etc/cockpit/disallowed-users` 中管理。添加到文件中可以禁止该用户登录。

# 启用 HTTPS

对于有域名的服务器，使用 acme.sh 免费申请 HTTPS 证书后，可以通过在 `/etc/cockpit/ws-certs.d` 创建链接来启用 https：
```sh
ln /home/user/.acme.sh/nabuyiyang.top_ecc/nabuyiyang.top.cer /etc/cockpit/ws-certs.d/nabuyiyang.top.cert
ln /home/user/.acme.sh/nabuyiyang.top_ecc/nabuyiyang.top.key /etc/cockpit/ws-certs.d/nabuyiyang.top.key
```

# 参考

[INSTALL SSL CERTIFICATES ON COCKPIT](https://infotechys.com/install-ssl-certificates-on-cockpit/)
