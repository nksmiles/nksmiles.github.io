---
layout: mypost
title: 为青龙面板启用 HTTPS
categories: [Linux]
---

青龙面板本身不支持 HTTPS 访问，我们可以通过 nginx 反向代理实现：

- 首先，我们需要有一个域名和 SSL 证书，以便使用 HTTPS 协议访问青龙面板。SSL 证书可以通过 acme.sh 获取。
- 其次，我们需要在服务器上安装 Nginx，并配置反向代理，将域名指向青龙面板的端口（默认是 5700）。
- 然后，我们需要修改 Nginx 的配置文件，添加 proxy_ssl_server_name 以及 sub_filter 相关配置，用于替换青龙面板中的 ip 地址为域名。
- 最后，我们需要重启 Nginx 服务，使配置生效。

## 配置 HTTPS
使用 `nginx -t` 命令查看配置文件所在路径。
```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
然后编辑配置文件，修改或添加以下内容：

```
server {
    listen               443 ssl;
    ssl_certificate      /home/user/.acme.sh/example.com_ecc/example.com.cer;
    ssl_certificate_key  /home/user/.acme.sh/example.com_ecc/example.com.key;
    ssl_session_timeout  5m;
    ssl_protocols        TLSv1.3;
    http2                on;
    server_name          example.com;

    root                 /usr/share/nginx/html;

    location / {
        #反向代理
        proxy_ssl_server_name on;
        proxy_pass http://example.com:5700;
        proxy_set_header Accept-Encoding '';
        #过滤器模块
        sub_filter "example.com:5700" "example.com";
        sub_filter_once off;
    }
}
```
使用 `nginx -t` 命令确认配置文件无误后，使用：
```sh
nginx -s reload
```
重启 Nginx 服务使配置生效。

## 启用 HTTP 自动跳转 HTTPS

如果我们想要实现 HTTP 自动跳转 HTTPS 的功能，我们还可以在配置文件中添加一些重定向的规则：
```
server {
    listen       80;
    server_name  example.com;
    return       301 HTTPS://$host$request_uri;
}
```
注意这一部分要添加到 `server {    listen 443; ...}`后面。

使用 `nginx -t` 命令确认配置文件无误后，使用：
```sh
nginx -s reload
```
重启 Nginx 服务使配置生效。
