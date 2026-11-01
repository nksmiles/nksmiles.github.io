---
layout: mypost
title: Almalinux 安装cockpit、面板等
categories: [Linux]
---

Docker 是 Linux 虚拟化的一种封装，提供简单易用的容器使用接口。 它将应用程序与该程序的依赖，打包在一个文件里面。所以有了 Docker，就不用担心环境问题。

说到虚拟化，很容易想到虚拟器。Docker 和虚拟机有很大的区别，虚拟机一般是在系统上运行的另外一个系统，需要消耗大量的系统资源，而 Docker 可以理解为一个系统上的不同用户，他们都是运行的同一个系统上，性能消耗少，而且启动速度很快。

## Linux 下安装

`dnf install caddy`

`dnf install php-fpm`

`dnf install cockpit`


`nano /etc/caddy/Caddyfile`

```
# The Caddyfile is an easy way to configure your Caddy web server.
#
# https://caddyserver.com/docs/caddyfile


# The configuration below serves a welcome page over HTTP on port 80.  To use
# your own domain name with automatic HTTPS, ensure your A/AAAA DNS record is
# pointing to this machine's public IP, then replace `http://` with your domain
# name.  Refer to the documentation for full instructions on the address
# specification.
#
# https://caddyserver.com/docs/caddyfile/concepts#addresses

g.galileotp.com.cn {
    tls 327649070@qq.com

    root * /var/www/html
    php_fastcgi unix//run/php-fpm/www.sock
    file_server

    # xray-WebSocket / API 备用
    #reverse_proxy /myapi-loveEina* 127.0.0.1:16666

    # XHTTP 的反向代理配置，允许刷新缓冲，确保流式传输顺畅
    reverse_proxy /abcd-gavin-data-2026* 127.0.0.1:445 {
        flush_interval -1
    }

    # Cockpit 反向代理配置
    reverse_proxy /cockpit-bandwagonhost* https://127.0.0.1:9090 {
        transport http {
            tls_insecure_skip_verify
        }
    }

    # 3x-ui 面板后台代理
    reverse_proxy /xraypanel-bwghost* https://127.0.0.1:27 {
        transport http {
            tls_insecure_skip_verify
        }
    }

    log {
        output file /var/log/caddy/access.log
    }
}

# As an alternative to editing the above site block, you can add your own site
# block files in the Caddyfile.d directory, and they will be included as long
# as they use the .caddyfile extension.
import Caddyfile.d/*.caddyfile

```

cockpit本地环回访问：
`nano /etc/cockpit/cockpit.conf`

```
[WebService]
# 允许通过你的公网域名和本地环回地址访问
Origins = https://g.galileotp.com.cn wss://g.galileotp.com.cn https://127.0.0.1:9090
# 告知 Cockpit 前端代理使用的是 HTTPS 协议
ProtocolHeader = X-Forwarded-Proto
# 允许通过二级路径/子路径进行代理访问
UrlRoot = /cockpit-bandwagonhost/

```


cockpit用户登录：
`nano /etc/cockpit/disallowed-users`

```# List of users which are not allowed to login to Cockpit
# List of users which are not allowed to login to Cockpit
#root

```


php-fpm权限问题：
`nano /etc/php-fpm.d/www.conf`

```
listen.acl_users = apache,nginx,caddy

```


自动封禁IP
`nano /etc/fail2ban/jail.local`

```
[DEFAULT]
ignoreip = 127.0.0.1/8 192.168.1.0/24
bantime  = 30d
findtime = 1h
maxretry = 5
backend  = systemd
banaction = firewallcmd-ipset

[pam-generic]
enabled = true
backend = systemd

```
