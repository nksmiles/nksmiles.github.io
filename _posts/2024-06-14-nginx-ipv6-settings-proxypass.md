---
layout: mypost
title: Nginx IPv6 Server设置笔记
categories: [Linux]
---

## 介绍

近期发现服务器的IPv4地址访问延迟比IPv6还大，所以打算用一个单独的子域名启用IPv6访问，顺便将之前为青龙面板启用的https套用过来。

## nginx 配置文件内容 

将/etc/ngxin/config.d目录下的default.conf复制一份，重命名为v6.conf：

```bash
cp /etc/nginx/config.d/default.conf /etc/nginx/config.d/v6.conf
```

修改v6.conf文件内容：

```
server {
    server_name         v6.example.com;
    listen              [::]:443 ssl;
    ssl_certificate     /root/.acme.sh/example.com_ecc/example.com.cer;
    ssl_certificate_key /root/.acme.sh/example.com_ecc/example.com.key;
    ssl_session_timeout 5m;
    ssl_protocols       TLSv1.3;
    http2               on;

    root /usr/share/nginx/html;

    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        #反向代理
        proxy_ssl_server_name on;
        proxy_pass http://v6.example.com:5700;
        proxy_set_header Accept-Encoding '';
        #过滤器模块
        sub_filter "v6.example.com:5700" "v6.example.com";
        sub_filter_once off;
    }
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}

server {
    listen [::]:80;
    server_name v6.example.com;
    return 301 https://$host$request_uri;
}
```

## 说明

不同的域名配置要放到单独的Server{}中，否则可能没法正常识别。

可以使用lsof -i:443 查看是否已经启用IPv6的端口监听。

```
$ lsof -i:443
COMMAND     PID  USER   FD   TYPE   DEVICE SIZE/OFF NODE NAME
nginx   4142424  root    6u  IPv4 17904885      0t0  TCP *:https (LISTEN)
nginx   4142424  root   11u  IPv6 18110384      0t0  TCP *:https (LISTEN)
nginx   4184819 nginx    6u  IPv4 17904885      0t0  TCP *:https (LISTEN)
nginx   4184819 nginx   11u  IPv6 18110384      0t0  TCP *:https (LISTEN)
nginx   4184820 nginx    6u  IPv4 17904885      0t0  TCP *:https (LISTEN)
nginx   4184820 nginx   11u  IPv6 18110384      0t0  TCP *:https (LISTEN)
```

## 参考

[Nginx 监听 IPv6 地址端口的正确操作方法](https://www.librehat.com/the-right-way-to-setup-nginx-monitor-ipv6-address-port/)


